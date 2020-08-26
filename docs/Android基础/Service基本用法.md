### 服务Service

#### 概念

- 实现程序后台运行的解决方案，适合执行不需要和用户交互的长期运行的任务；
- 服务并不运行在单独的**进程**中，依赖于创建服务时所在的进程；
- 服务不会自动开启**线程**，任务默认运行在主线程。

#### 1、定义一个服务

- 继承Service；
- 重写`onCreate,onStartCommand,onDestroy`方法；
- 清单文件注册。

#### 启动和关闭服务

- 启动

  ```java
  Intent startIntent = new Intent(this, MyService.class);
  startSestarvice(startIntent);
  ```

- 关闭

  ```java
  Intent stopIntent = new Intent(this, MyService.class);
  stopService(stopIntent);
  ```

- Ps：现实情况是一旦服务开启就会和Activity失去联系的。

#### 2、服务和界面通信

- 在Service中继承Binder创建子类，子类中写和界面交互的方法；
- 在`onBind`中返回Binder子类的实例；
- 在界面中创建`ServiceConnection`，重写`onServiceConnected`和`onServiceDisconnected`方法，`onServiceConnected`中的`IBinder`向下转型为在Service中重写的Binder类实例，此实例负责通信；
- 调用`bindService`绑定界面和服务；
- 调用`unbindService`解绑服务和界面。

#### 生命周期

- `onCreate`：服务创建时调用，只要服务已经存在，即便再次`startSestarvice`也不会创建新的实例，此方法也不会再次被回调；
- `onStartCommand`：每次服务启动都会执行，即`startSestarvice`每执行一次，此方法会被回调一次，可以执行任务；
- `onDestroy`：`stopService`调用时，此方法会被回调，无论`startSestarvice`执行几次，只需要一次`stopService`即可关闭，用来回收资源；
- `onBind`：调用`bindService`时，此方法会被调用，`onBind`也只会被回调一次；
- `onServiceDisconnected`：此方法在正常关闭`stop`或者`unbind`服务时不会被回调，只有被系统杀死时才会回调；

#### 思考

| `startService` | `bindService`  | `unbindService` | `stopService`   | `onCreate->onStartCommand->onBind->onDestroy` |
| -------------- | -------------- | --------------- | --------------- | --------------------------------------------- |
| `bindService`  | `startService` | `unbindService` | `stopService`   | `onCreate->onBind->onStartCommand->onDestroy` |
| `startService` | `bindService`  | `stopService`   | `unbindService` | `onCreate->onStartCommand->onBind->onDestroy` |
| `bindService`  | `startService` | `stopService`   | `unbindService` | `onCreate->onBind->onStartCommand->onDestroy` |

#### 3、前台服务优点

- 提升优先级；

#### 使用

- 声明权限

  ```xml
  <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
  ```

- 在服务的`onCreate`方法中添加

  ```java
          Intent intent = new Intent(this, MainActivity.class);
          PendingIntent pendingIntent = PendingIntent.getActivity(this, 0, intent, 0);
          NotificationCompat.Builder builder = new NotificationCompat.Builder(this, "channel_id");
  
          if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
              NotificationManager notificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
              NotificationChannel channel = new NotificationChannel("channel_id", "channel_name", NotificationManager.IMPORTANCE_HIGH);
              notificationManager.createNotificationChannel(channel);
          }
  
          Notification notification = builder.setContentTitle("this is content title")
                  .setContentText("this is content text")
                  .setWhen(System.currentTimeMillis())
                  .setSmallIcon(R.mipmap.ic_launcher)
                  .setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.ic_launcher))
                  .setContentIntent(pendingIntent)
                  .build();
  
          startForeground(1, notification);
  ```

#### `4、IntentService`的优点

- 简单的创建一个异步的，执行完任务会自动停止的服务。

#### 使用

- 必须提供一个无参构造方法，在方法内部调用有参构造方法；
- `onHandleIntent`处理任务，已经运行在子线程，不会ANR；
- 不用主动关闭，任务执行完之后会自动销毁。