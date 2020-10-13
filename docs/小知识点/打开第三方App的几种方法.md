1. 分类

   1. 显式调用
   2. 隐式调用
   3. URL Scheme

2. 显式调用方法1

   1. 调用方代码

      ```java
      Intent intent = new Intent();
      //包名+打开页面的全路径
      intent.setClassName("com.example.myapplication","com.example.myapplication.DbActivity");
      //传递参数
      intent.putExtra("extraKey", "刘备");
      startActivity(intent);
      ```

   2. 被调方获取传递的参数

      ```java
      String extraValue = getIntent().getStringExtra("extraKey");
      ```

3. 显式调用方法2

   1. 调用方代码

      ```java
      Intent intent2 = new Intent();
      ComponentName componentName = new ComponentName("com.example.myapplication","com.example.myapplication.DbActivity");
      intent2.setComponent(componentName);
      intent2.setAction("andropid.intent.action.MAIN");
      intent2.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
      intent2.putExtra("extraKey", "曹操");
      ```

   2. 被调方获取传递的参数

      ```java
      String extraValue = getIntent().getStringExtra("extraKey");
      ```

4. 隐式调用方法1

   1. 调用方代码

      ```java
      Intent intent3 = new Intent();
      intent3.setAction("app.intent.custom");
      intent3.addCategory("android.intent.category.DEFAULT");
      intent3.putExtra("extraKey", "孙权");
      startActivity(intent3);
      ```

   2. 被调方清单文件配置

      ```xml
      <intent-filter>
      	<!--自定义Action和category-->
      	<action android:name="app.intent.custom" />
      	<category android:name="android.intent.category.DEFAULT" />
      </intent-filter>
      ```

   3. 被调方获取传递的参数

      ```java
      String extraValue = getIntent().getStringExtra("extraKey");
      ```

5. URL Scheme调用方法1

   1. 调用方代码

      ```java
      //url =protocol + authority(host + port) + path + query
      //https://www.baidu.com:443/data?param=Tom&param2=Cat
      Intent intent4 = new Intent(Intent.ACTION_VIEW, Uri.parse("sanguo://jump.next:444/open?param=曹操&next=曹丕"));
      PackageManager packageManager = getPackageManager();
      List<ResolveInfo> activities = packageManager.queryIntentActivities(intent4, 0);
      if (activities.isEmpty()) {
      	Toast.makeText(this, "未安装", Toast.LENGTH_LONG).show();
      } else {
      	startActivity(intent4);
      }
      ```

   2. 被调方清单文件配置

      ```java
      <intent-filter>
          <action android:name="android.intent.action.VIEW" />
      
          <category android:name="android.intent.category.DEFAULT" />
          <category android:name="android.intent.category.BROWSABLE" />
      
          <!-- pathPrefix非必须-->
          <data
              android:host="jump.next"
              android:pathPrefix="/open"
              android:scheme="sanguo" />
      </intent-filter>
      ```

   3. 被调方获取传递的参数

      ```java
      Uri uri = getIntent().getData();
      if (uri != null) {
                  textView.setText("uri=" + uri.toString() + "\nScheme=" + uri.getScheme() + "\nHost=" + uri.getHost()
                          + "\nPort=" + uri.getPort() + "\nPath=" + uri.getPath() + "\nparam=" + uri.getQueryParameter("param")
                          + "\nnext=" + uri.getQueryParameter("next") + "\nQuery=" + uri.getQuery() + "\ngetPathSegments=" + uri.getPathSegments());
      }
      ```

      

6. URL Scheme调用方法2(未测试)

   ```html
   <a href="myapp://jump.app/open?param=Tom&param2=Cat"><button >打开第三方APP</button></a>
   ```

   