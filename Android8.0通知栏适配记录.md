### 必看郭神[文章](https://mp.weixin.qq.com/s/Ez-G_9hzUCOjU8rRnsW8SA)，看完你就懂了

#### 1、需要适配的版本

targetSdkVersion 为26或以上，android8.0及以上需要适配

#### 2、创建通知渠道

* 一定要进行判断系统版本的判断，低版本的没有通知渠道的功能，不做版本检查会在低版本手机上造成崩溃

  ```java
  if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
   	String channelId = "chat";                                 //渠道ID
  	String channelName = "聊天消息";                            //渠道名称
  	int importance = NotificationManager.IMPORTANCE_HIGH;      //重要等级
      createNotificationChannel(channelId, channelName, importance);}
  ```

* `createNotificationChannel()`创建通知渠道的方法需要三个参数：渠道ID，渠道名称，重要等级

  渠道ID：可随便定义，但要保证全局唯一

  渠道名称：给用户看，要表达清楚该渠道的用途

  重要等级：用户可随手更改每个渠道的重要等级

  ```java
  private void createNotificationChannel(String channelId, String channelName, int importance{
    NotificationChannel channel = new NotificationChannel(channelId, channelName, importance);
    channel.setShowBadge(true);      //允许该通知渠道下的通知显示角标
    NotificationManager notificationManager = (NotificationManager)  		      getSystemService(NOTIFICATION_SERVICE);
    notificationManager.createNotificationChannel(channel);	}
  ```

* 渠道的重要等级
  |名称（权重）|状态栏显示|允许通知|响铃|震动|||
  |:--:|:--:|:--:|:--:|:--:|:--:|:--:|
  |IMPORTANCE_MAX（5）|√|√|√|√|||
  |IMPORTANCE_HIGH（4）|√|√|√|√|||
  |IMPORTANCE_DEFAULT（3）|√|√|√|√|||
  |IMPORTANCE_LOW（2）|√|√|×|×|||
  |IMPORTANCE_MIN（1）|×|√|×|×|||
  |IMPORTANCE_NONE（0）|√|√|√|√|||

  目前并未发现其他的区别（后续补充，看其差别）

####3、发送通知

* 通知渠道关闭了要通知用户去设置页面开启该渠道权限

  ```java
  chanelName为你要判断的渠道，
  if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
     NotificationChannel channel = manager.getNotificationChannel(channelName);
     if (channel.getImportance() == NotificationManager.IMPORTANCE_NONE) {
         Intent intent = new Intent(Settings.ACTION_CHANNEL_NOTIFICATION_SETTINGS);
         intent.putExtra(Settings.EXTRA_APP_PACKAGE, getPackageName());
         intent.putExtra(Settings.EXTRA_CHANNEL_ID, channel.getId());
         startActivity(intent);
         Toast.makeText(this, "请手动将通知打开", Toast.LENGTH_SHORT).show();
  	}
  }
  ```

* **发送通知(最基本的要牢记)**

  ```java
  NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
  Notification notification = new NotificationCompat.Builder(this, channelName)//多了个通知渠道
                  .setContentTitle(contentTitle)
                  .setContentText(ContentText)
                  .setWhen(System.currentTimeMillis())
                  .setSmallIcon(R.drawable.item_pic)
                  .setLargeIcon(BitmapFactory.decodeResource(getResources(), R.drawable.app_icon))
                  .setAutoCancel(true)
                  .setNumber(1)                 //显示通知数目
                  .build();
  manager.notify(notifyNum, notification);
  ```
####4、通知的进阶技巧



####5、通知的高级功能






#### 6、相关文章

[关于Android 8.0后notification通知声音无法关闭或开启的问题](https://blog.csdn.net/fzkf9225/article/details/81119780)

[通知栏图标问题](https://blog.csdn.net/ruyi366/article/details/79081853)

[Android 8.0 升级笔记（适配图片、通知栏、ContentProvider、Receiver）](https://blog.csdn.net/moxiouhao/article/details/79209054)

[Android自定义通知栏显示](https://blog.csdn.net/wangzunkuan/article/details/79497874)

#### 7、小点记忆

* 小图标只能使用纯alpha图层的图片进行设置

* 通知的点击效果使用PendingIntent

  获取PendingIntent实例方法：getActivity()，getBroadcast()，getService()，三种方法所传参数相同

  ```java
  PendingIntent pendingIntent = PendingIntent.getActivity(this, 0, intent, 0);
  ```

  参数一：Context	参数二：通常传入0	参数三：Intent对象，表示意图	参数四：确定PendingIntent的行为，有FLAG_ONE_SHOT、FLAG_NO_CREATE、FLAG_CANCEL_CURRENT、FLAG_UPDATE_CURRENT，通常传入0即可

* Notification的setAutoCancel(true);当点击通知后自动消失

* 也可显示的通过NotificationManager的cancel(id);方法来取消指定id的通知

  


