#Android #Intent 
[[Android URI]]
### 왭사이트 열기
```Java
//주소 받기
String url = mWebesiteEditText.getText().toString(); 
//URI 만들기
Uri webpage = Uri.parse(url);  
//인텐트만들기
Intent intent = new Intent(Intent.ACTION_VIEW, webpage);  

//Activity가 존제할경우 실행하는 코드인데 잘 작동하지가 않는다.
if (intent.resolveActivity(getPackageManager()) != null) {  
    startActivity(intent);  
} else {  
    Log.d("ImplicitIntents", "Can't handle this intent!");  
}
```


### 구글맵 검색
```Java
//검색어
String loc = mLocationEditText.getText().toString();  
//여기가 검색부분같은데 왜 맵이 실행 되는지는 모르겠다
//아마 "geo:0,0?q=" 이부분이 그런거 관련인거 같은데
Uri addressUri = Uri.parse("geo:0,0?q=" + loc); 

//나머지 부분은 같다
Intent intent = new Intent(Intent.ACTION_VIEW, addressUri);  
  
if (intent.resolveActivity(getPackageManager()) != null) {  
    startActivity(intent);  
} else {  
    Log.d("ImplictIntents", "Can't handle this intent!");  
}
```

### 공유라고하는데 정확히 뭔지는 모르겠다.
```Java
String txt = mShareTextEditText.getText().toString();  
String mimeType = "text/plain";  

//이건 어떻게 작동시키는지 잘모르겠다.
ShareCompat.IntentBuilder  
        .from(this)  
        .setType(mimeType)  
        .setChooserTitle("share this text with:")  
        .setText(txt)  
        .startChooser();
```

#### intent.resolveActivity(getPackageManager()) != null
가 어떻게 쓰이는지 알았다
여러가지 앱들중에 선어떤 앱으로 실행할지 선택하는 과정에서 쓰이는것  같다.