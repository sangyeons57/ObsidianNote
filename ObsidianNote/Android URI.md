#Android #Java #URI #url 
### URI : URI는 특정 리소스를 식별하는 통합 식별자(Uniform Resource Identifier)를 의미한다. 웹기술에서 사용하는 논리적 물리적 리소스를 식별하는 고유한 문자열 시퀀스
### URL: 웹주소, 컴퓨터 네트워크상에서 리서스가 어디있는지 알려주기 위한 규약. URI의 서브셋이다
URI가 더 큰 범위
URI: 식별자
URL: 주소

```Java

String url = mWebsiteEditText.getText().toString();
Uri webpage = Uri.parse(url);
Intent intent = new Intent(Intent.ACTION_VIEW, webpage);
if (intent.resolveActivity(getPackageManager()) != null) {
   startActivity(intent);
} else {
   Log.d("ImplicitIntents", "Can't handle this intent!");
}

Intent intent = new Intent
String loc = mLocationEditText.getText().toString();
Uri addressUri = Uri.parse("geo:0,0?q=" + loc);
Intent intent = new Intent(Intent.ACTION_VIEW, addressUri);
if (intent.resolveActivity(getPackageManager()) != null) {
	startActivity(intent);
} else {
	Log.d("ImplicitIntents", "Can't handle this intent!");
}
```
Activity 생성
Uri.parse(특정한검색용값);
Intent intent = new(Intent.ACTION_VIEW, uri 값)
startIntent(intent);
이렇게 실행한다

if (intent.resolveActivity(getPackageManager()) != null) {
이건은 작동이 안되는데 어떻게 해야할지 모르겠다.
