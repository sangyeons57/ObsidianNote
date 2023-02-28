#Android 
**Q**
android 처음 실행할때나오는 아이콘의 종류가 달라졌어 고치는법 알아?

**A**
Yes, you can change the default launch icon for your Android app by following these steps:

1.  Create an icon in the desired size and format, typically PNG with transparency.
2.  Place the icon file in the appropriate folder for your app's build variant, such as `res/mipmap-mdpi` for a standard density icon.
3.  Update the `android:icon` attribute in the `<application>` element of your app's `AndroidManifest.xml` file to reference the new icon.
4.  Rebuild and run your app to see the updated launch icon.

Note that you may need to create multiple versions of your icon for different screen densities, and ensure that they are placed in the corresponding folders.

## 정리
manifest 파일에서 icon을 설정하는데
해당 값은 @minap을 받는다
project > android 로 프로젝트를 표현할때 
mipmap폴더의 값을 조정해서 실행아이콘을 수정할수있다.
