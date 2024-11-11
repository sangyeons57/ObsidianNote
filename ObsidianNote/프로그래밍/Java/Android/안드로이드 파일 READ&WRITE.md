#language/java/Android 


``` java
String fileName = "test";  
File folder = new File(Environment.getExternalStorageDirectory() + File.separator + "Download" + File.separator + fileName);  
Log.d("TEST", folder.toString());  
if(!folder.exists()){  
    if( folder.mkdirs()) {  
        Toast.makeText(v.getContext(),"폴더 생성 성공", Toast.LENGTH_SHORT).show();  
    } else{  
        Toast.makeText(v.getContext(),"폴더 생성 실패", Toast.LENGTH_SHORT).show();  
    }  
} else {  
    Toast.makeText(v.getContext(),"동일한 폴더 존제", Toast.LENGTH_SHORT).show();  
}
```
android 폴더 생성용 스크립트이다.
new File() 밑 mkdirs()함수로 폴더를 생성할수있다

나머지는 결과 출력밑 위치지정이다.

Enviroment.getExternalStorageDirectory() 함수는 기본 Root폴더와 비슷한 위치를 제공한다.
그런데 해당위치에 폴더를 생성하려고하니까 문제가 생겨서안됨
따라서 Download폴더에 생성함 -> 됨
이유는 정확히는 모르겠음


``` java
File sdCardRoot = new File(System.getenv("EXTERNAL_STORAGE"));  
if (sdCardRoot.exists() && sdCardRoot.isDirectory()) {  
    File[] folders = sdCardRoot.listFiles(File::isDirectory);  
    if (folders != null) {  
        for (File folder : folders) {
            Log.d("TEST", "폴더 이름: " + folder.getName());  
            // 또는 원하는 작업을 수행할 수 있음  
        }  
    }  
} else {  
    System.out.println("SD 카드가 존재하지 않거나 읽을 수 없습니다.");  
}
```
루트 파일을 읽는 코드
System.getenv() 는 시스탬 파일을 읽는 변수다. 
sdCardRoot.listFiles(File:: isDirectory) 쪽 문법이 이거 찾아보기>[[FunctionalInterface]]

