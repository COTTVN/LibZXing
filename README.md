# LibZXing
二维码扫描库

## 效果图

文件比较大，请耐心等待

![](https://github.com/jwkj/LibZXing/blob/master/qrcode.gif)

## How to

### Step 1. Add the JitPack repository to your build file


Add it in your root build.gradle at the end of repositories:

```allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```
### Step 2. Add the dependency
```
dependencies {
        compile 'com.github.jwkj:LibZXing:v1.0.2'
}
```

## 生成二维码

生成一个300*300不带logo的二维码

```
QRCodeManager.getInstance().createQRCode("二维码内容", 300, 300);
```
![](https://github.com/jwkj/LibZXing/blob/master/nologo.png)

生成一个300*300有logo的二维码

```
QRCodeManager.getInstance().createQRCode("二维码内容", 300, 300,logoBitmap);
```

![](https://github.com/jwkj/LibZXing/blob/master/logo.png)

## 识别二维码

### 方法一：自动解析结果（推荐）

在调用处的activity/fragment注册onActivityResult方法

```java
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        //注册onActivityResult
        QRCodeManager.getInstance().with(this).onActivityResult(requestCode, resultCode, data);
    }
```

监听扫描按钮单击事件

```java
 public void onScanQR(View view) {
        QRCodeManager.getInstance()
                .with(this)
                .setReqeustType(1)//可以不设置，默认是0
                .scanningQRCode(new OnQRCodeScanCallback() {
                    @Override
                    public void onCompleted(String result) {//扫描成功之后回调，result就是扫描的结果
                        controlLog.append("\n\n(结果)" + result);
                    }

                    @Override
                    public void onError(Throwable errorMsg) {//扫描出错的时候回调
                        controlLog.append("\n\n(错误)" + errorMsg.toString());
                    }

                    @Override
                    public void onCancel() {//取消扫描的时候回调
                        controlLog.append("\n\n(取消)扫描任务取消了");
                    }
                });
    }
```

### 方法二：手动解析结果

开始扫描二维码

```java
QRCodeManager.getInstance().with(this).scanningQRCode(requestCode);
```

扫描结束之后，结果可以在调用处的activity/fragment的onActivityResult中拿到

```java
   @Override
   protected void onActivityResult(int requestCode, int resultCode, Intent data) {
       super.onActivityResult(requestCode, resultCode, data);
       //扫描之后，自己处理扫描结果
      }
```


