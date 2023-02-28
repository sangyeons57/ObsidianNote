#Android #Java #Intent 

Android 에서 한Activity에서 다른 Activity 로 이동할때 사용한다

```Java
Intent intent = getIntent();
intent.putExtra(key값,value값);
startActivity(intent);
```
각각 intent생성
intent에 파리미터 생성
intent 실행


받는부분
```Java
Intent secondIntent = getIntent();
secondIntent.getIntExtra(key값);
```
getExtra는 들어오는 값에 따라 달라진다.
