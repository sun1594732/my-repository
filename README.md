# my-repository
first attempt code
关于联系人的一些URI：

管理联系人的Uri：

ContactsContract.Contacts.CONTENT_URI 

管理联系人的电话的Uri：

ContactsContract.CommonDataKinds.Phone.CONTENT_URI 

管理联系人的Email的Uri：

ContactsContract.CommonDataKinds.Email.CONTENT_URI 

（注：Contacts有两个表，分别是rawContact和Data，

rawContact记录了用户的id和name,

其中id栏名称为：ContactsContract.Contacts._ID, name名称栏为ContactContract.Contracts.DISPLAY_NAME,

电话信息表的外键id为ContactsContract.CommonDataKinds.Phone.CONTACT_ID,

电话号码栏名称为：ContactsContract.CommonDataKinds.Phone.NUMBER.
data表中Email地址栏名称为：
ContactsContract.CommonDataKinds.Email.DATA
其外键栏为：ContactsContract.CommonDataKinds.Email.CONTACT_ID)

关于多媒体的一些URI：

存储在sd卡上的音频文件：

MediaStore.Audio.Media.EXTERNAL_CONTENT_URI 

存储在手机内部存储器上的音频文件：
MediaStore.Audio.Media.INTERNAL_CONTENT_URI 
SD卡上的图片文件内容：

MediaStore.Audio.Images.EXTERNAL_CONTENT_URI

手机内部存储器上的图片：

MediaStore.Audio.Images.INTERNAL_CONTENT_URI

SD卡上的视频：

MediaStore.Audio.Video.EXTERNAL_CONTENT_URI 

手机内部存储器上的视频：
MediaStore.Audio.Video.INTERNAL_CONTENT_URI 

(注：图片的显示名栏：Media.DISPLAY_NAME，

     图片的详细描述栏为：Media.DESCRIPTION

     图片的保存位置：Media.DATA

短信URI：

Content://sms

发送箱中的短信URI：

Content://sms/outbox

收信箱中的短信URI：

Content://sms/sent

草稿中的短信URI：

Content://sms/draft

常用代码：

// 查询通讯录中联系人？
 Cursor cursor = getContentResolver().query(ContactsContract.Contacts.CONTENT_URI, null, null, null, null);

// 获取手机号
 Cursor phone = cursor.query(ContactsContract.CommonDataKinds.Phone.CONTENT_URI, null, ContactsContract.CommonDataKinds.Phone.CONTACT_ID +"="+ ContactId, null, null);
// 复杂点：
 Cursor phone = cursor.query(ContactsContract.CommonDataKinds.Phone.CONTENT_URI, null, ContactsContract.CommonDataKinds.Phone.CONTACT_ID +"="+ ContactId +" AND "+ ContactsContract.CommonDataKinds.Phone.TYPE +"="+ ContactsContract.CommonDataKinds.Phone.TYPE_MOBILE, null, null);
ContactsContract.CommonDataKinds.Phone.TYPE 表示联系人电话的类型，

主要对应如下：
    TYPE_MOBILE ： 手机号码
    TYPE_HOME ： 住宅电话
    TYPE_WORK ： 公司电话

打开一个网页，类别是Intent.ACTION_VIEW
  	

Uri uri = Uri.parse("http://www.baidu.com/");

Intent intent = new Intent(Intent.ACTION_VIEW, uri);

打开地图并定位到一个点

  	

Uri uri = Uri.parse("geo:52.76,-79.0342");

Intent intent = new Intent(Intent.ACTION_VIEW, uri);

打开拨号界面，类型是Intent.ACTION_DIAL

  	

Uri uri = Uri.parse("tel:10086");

Intent intent = new Intent(Intent.ACTION_DIAL, uri);

直接拨打电话，与之不同的是，这个直接拨打电话，而不是打开拨号界面


	

Uri uri = Uri.parse("tel:10086");

Intent intent = new Intent(Intent.ACTION_CALL, uri);

卸载一个应用，Intent的类别是Intent.ACTION_DELETE

	

Uri uri = Uri.fromParts("package", "xxx", null);

Intent intent = new Intent(Intent.ACTION_DELETE, uri);

安装应用程序,Intent的类别是Intent.ACTION_PACKAGE_ADDED


	

Uri uri = Uri.fromParts("package", "xxx", null);

Intent intent = new Intent(Intent.ACTION_PACKAGE_ADDED, uri);

播放音频文件
  	

Uri uri = Uri.parse("file:///sdcard/download/everything.mp3");

Intent intent = new Intent(Intent.ACTION_VIEW, uri);

intent.setType("audio/mp3");

打开发邮件界面
  	

Uri uri= Uri.parse("mailto:admin@Android-study.com");

Intent intent = new Intent(Intent.ACTION_SENDTO, uri);

发邮件,不同是将邮件发送出去

  	

Intent intent = new Intent(Intent.ACTION_SEND);

String[] tos = { "admin@android-study.com" };

String[] ccs = { "webmaster@android-study.com" };

intent.putExtra(Intent.EXTRA_EMAIL, tos);

intent.putExtra(Intent.EXTRA_CC, ccs);

intent.putExtra(Intent.EXTRA_TEXT, "I come from http://www.android-study.com");

intent.putExtra(Intent.EXTRA_SUBJECT, "http://www.android-study.com");intent.setType("message/rfc882");

Intent.createChooser(intent, "Choose Email Client");

//发送带附件的邮件
  	

Intent intent = new Intent(Intent.ACTION_SEND);

intent.putExtra(Intent.EXTRA_SUBJECT, "The email subject text");

intent.putExtra(Intent.EXTRA_STREAM, "file:///sdcard/mysong.mp3");

intent.setType("audio/mp3");

startActivity(Intent.createChooser(intent, "Choose Email Client"));

发短信
  	

Uri uri= Uri.parse("tel:10086");

Intent intent = new Intent(Intent.ACTION_VIEW, uri);

intent.putExtra("sms_body", "I come from http://www.android-study.com");

intent.setType("vnd.Android-dir/mms-sms");

直接发短信
  	

Uri uri= Uri.parse("smsto://100861");

Intent intent = new Intent(Intent.ACTION_SENDTO, uri);

intent.putExtra("sms_body", "3g android http://www.android-study.com");

发彩信
  	

Uri uri= Uri.parse("content://media/external/images/media/23");

Intent intent = new Intent(Intent.ACTION_SEND);

intent.putExtra("sms_body", "3g android http://www.android-study.com");

intent.putExtra(Intent.EXTRA_STREAM, uri);

intent.setType("image/png");

# Market 相关


	

1 //寻找某个应用

Uri uri = Uri.parse("market://search?q=pname:pkg_name");

Intent it = new Intent(Intent.ACTION_VIEW, uri);

startActivity(it);

//where pkg_name is the full package path for an application

2 //显示某个应用的相关信息

Uri uri = Uri.parse("market://details?id=app_id");

Intent it = new Intent(Intent.ACTION_VIEW, uri);

startActivity(it);

//where app_id is the application ID, find the ID

//by clicking on your application on Market home

//page, and notice the ID from the address bar

路径规划
  	

Uri uri = Uri.parse("http://maps.google.com/maps?f=d&saddr=startLat%20startLng&daddr=endLat%20endLng&hl=en");

Intent it = new Intent(Intent.ACTION_VIEW, uri);

startActivity(it);

//where startLat, startLng, endLat, endLng are a long with 6 decimals like: 50.123456

安装指定apk
  	

public void setupAPK(String apkname){

    String fileName = Environment.getExternalStorageDirectory() + "/" + apkname;

    Intent intent = new Intent(Intent.ACTION_VIEW);

    intent.setDataAndType(Uri.fromFile(new File(fileName)),"application/vnd.android.package-archive");

    mService.startActivity(intent);

}

进入联系人页面


	

Intent intent = new Intent();

intent.setAction(Intent.ACTION_VIEW);

intent.setData(People.CONTENT_URI);

startActivity(intent);

查看指定联系人
  	

Uri personUri = ContentUris.withAppendedId(People.CONTENT_URI, info.id);// info.id联系人ID

Intent intent = new Intent();

intent.setAction(Intent.ACTION_VIEW);

intent.setData(personUri);

startActivity(intent);

调用相册
  	

public static final String MIME_TYPE_IMAGE_JPEG = "image/*";

public static final int ACTIVITY_GET_IMAGE = 0;

Intent getImage = new Intent(Intent.ACTION_GET_CONTENT);

getImage.addCategory(Intent.CATEGORY_OPENABLE);

getImage.setType(MIME_TYPE_IMAGE_JPEG);

startActivityForResult(getImage, ACTIVITY_GET_IMAGE);

调用系统相机应用程序，并存储拍下来的照片
  	

Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);

time = Calendar.getInstance().getTimeInMillis();

intent.putExtra(MediaStore.EXTRA_OUTPUT, Uri.fromFile(new File(Environment

.getExternalStorageDirectory().getAbsolutePath()+"/tucue", time + ".jpg")));

startActivityForResult(intent, ACTIVITY_GET_CAMERA_IMAGE);

<uses-permission android:name="android.permission.READ_CONTACTS"/>
