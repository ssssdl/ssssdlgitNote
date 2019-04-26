# 通过xml实现去除标题栏（理论上可以通过Java代码实现）
/app/manifests/manifests/AndroidManifest.xml
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.lenovo.ssssjisuan">
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.DayNight.NoActionBar"><!--设置没有标题栏-->
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```
# 通过写style文件全屏
文件路径.\app\src\main\res\values\styles.xml
添加如下代码，在 AndroidManifest.xml中应用这个style就行了
```
<style name="FullScreenTheme" parent="Theme.AppCompat.DayNight.NoActionBar">
    <item name="android:windowFullscreen">true</item>
</style>
```