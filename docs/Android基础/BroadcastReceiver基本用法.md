## Broadcast Receiver

### 简介

Android中的每个应用程序都可以对自己感兴趣的广播进行注册，这样该程序就只会接收到自己所关心的广播内容。这些广播可能来自于系统，也可能来自其它应用程序。

### 广播的分类

- 标准广播：完全异步的广播，在广播发出后，所有的广播接收器几乎会在同一时刻接收到这条广播消息。效率高，无法被截断，接收没有先后顺序。
- 有序广播：同步执行的广播，同一时刻只有一个广播接收器能够收到这条广播消息，当这个广播接收器中的逻辑执行完毕后，广播才会继续传递。先后顺序，可被截断，优先级高的先收到。

### 接收系统广播

Android系统内置了很多系统级别的广播，我们可以在应用中通过监听这些广播来获得各种系统的状态信息。

#### 动态注册：

- 定义NetworkChangeReceiver

  ```java
  public class NetworkChangeReceiver extends BroadcastReceiver {
      @Override
      public void onReceive(Context context, Intent intent) {
  
          ConnectivityManager connectivityManager = (ConnectivityManager) context.getSystemService(Context.CONNECTIVITY_SERVICE);
          NetworkInfo networkInfo = connectivityManager.getActiveNetworkInfo();
          if (networkInfo != null && networkInfo.isAvailable()) {
              Toast.makeText(context, "network is available", Toast.LENGTH_LONG).show();
          } else {
              Toast.makeText(context, "network is unavailable", Toast.LENGTH_LONG).show();
          }
      }
  }
  ```

- 注册广播接收器

  ```java
  IntentFilter intentFilter = new IntentFilter();
  networkChangeReceiver = new NetworkChangeReceiver();
  intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
  registerReceiver(networkChangeReceiver, intentFilter);
  ```

- 取消注册：动态注册的广播接收器一定要取消注册才行，并且只能注册和取消只能成对出现，否则会崩溃。

  ```java
  unregisterReceiver(networkChangeReceiver);
  ```

优点：可以自由注册和取消，很灵活。

缺点：必须程序启动之后才能接收广播。

#### 静态注册

- 定义BootCompleteReceiver

  ```java
  public class BootCompleteReceiver extends BroadcastReceiver {
      @Override
      public void onReceive(Context context, Intent intent1) {
          Toast.makeText(context, intent1.getAction(), Toast.LENGTH_LONG).show();
      }
  }
  ```

- 增加权限

  ```java
  <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
  ```

- 在清单文件声明权限和其它属性

  ```java
          <receiver
              android:name=".BootCompleteReceiver"
              android:enabled="true"
              android:exported="true">
  
              <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
              </intent-filter>
          </receiver>
  ```

- 属性说明

  - enabled：是否启用这个广播接收器。
  - exported：是否允许这个广播接收器接收本程序以外的广播。

### 自定义广播

#### 自定义标准（无序）广播

- 自定义广播接收器

  ```java
  public class MyBroadcastReceiver extends BroadcastReceiver {
      @Override
      public void onReceive(Context context, Intent intent) {
          Toast.makeText(context, intent.getAction(), Toast.LENGTH_LONG).show();
      }
  }
  ```

- 广播接收器注册清单文件

  ```java
  <receiver android:name=".MyBroadcastReceiver">
              <intent-filter>
                  <!--这个action可以自定义，只要和发送Intent的action保持一致即可-->
                  <action android:name="com.wl.firstcode.MY_CUSTOM_WX_GB" />
              </intent-filter>
          </receiver>
  ```

- 发送标准广播

  ```java
   Intent intent = new Intent("com.wl.firstcode.MY_CUSTOM_WX_GB");
   //加上下面这一句，8.0的限制，不然收不到
   intent.setComponent(new ComponentName(getPackageName(), "com.wl.firstcode.MyBroadcastReceiver"));
   sendBroadcast(intent);
  ```

#### 自定义有序广播

- 定义广播接收器在此省略了，如上

- 清单文件可以简单一点声明，因为有序广播不可以在清单文件声明action之类的

  ```java
  <receiver android:name=".MyBroadcastReceiver" />
  <receiver android:name=".AnotherBroadcastReceiver" />
  ```

- 动态注册广播接收器

  ```java
          IntentFilter myBroadcastReceiverFilter = new IntentFilter();
          myBroadcastReceiverFilter.addAction("com.wl.firstcode.MY_CUSTOM_WX_GB");
          myBroadcastReceiverFilter.setPriority(500);//1000~-1000，越大越先收到
          registerReceiver(new MyBroadcastReceiver(), myBroadcastReceiverFilter);
  
          IntentFilter anotherBroadcastReceiverFilter = new IntentFilter();
          anotherBroadcastReceiverFilter.addAction("com.wl.firstcode.MY_CUSTOM_WX_GB");
          anotherBroadcastReceiverFilter.setPriority(200);
          registerReceiver(new AnotherBroadcastReceiver(), anotherBroadcastReceiverFilter);
  ```

- 发送有序广播

  ```java
  Intent intent = new Intent("com.wl.firstcode.MY_CUSTOM_WX_GB");
  sendOrderedBroadcast(intent, null);
  ```

### 本地广播

以上广播都是系统全局广播，可以被其它应用收到，也可以接收到来自其它应用的广播，容易引发安全性问题。Android引入了本地广播机制，只能接收来自本应用的广播，安全性提高。

- 使用本地广播，需要添加如下依赖

  ```java
    implementation 'androidx.legacy:legacy-support-v4:1.0.0'
  ```

- 广播接收器和以上没有任何区别

- 使用本地广播管理器注册，只能接收本地广播

  ```Java
  final LocalBroadcastManager localBroadcastManager = LocalBroadcastManager.getInstance(this);
  final IntentFilter localBroadcastFilter = new IntentFilter();
  localBroadcastFilter.addAction("com.wl.firstcode.MY_LOCAL_GB");
  localBroadcastManager.registerReceiver(new MyBroadcastReceiver(), localBroadcastFilter);
  ```

- 使用本地广播发送，只能被本地广播接收

  ```java
  Intent intent = new Intent();
  intent.setAction("com.wl.firstcode.MY_LOCAL_GB");
  intent.putExtra("key", "本地广播");
  localBroadcastManager.sendBroadcast(intent);
  ```

- 本地广播优点

  - 本地发送的广播不会被其它应用接收
  - 其它程序无法发送广播到我们程序内部
  - 更加高效

### 注意

- 不要在`onReceive`中执行耗时操作。
- 记得取消广播接收器。

