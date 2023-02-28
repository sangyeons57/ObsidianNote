#Android #Java #getIdentifier #resColor
[관련한글자료](https://holika.tistory.com/entry/%EB%82%B4-%EB%A7%98%EB%8C%80%EB%A1%9C-%EC%A0%95%EB%A6%AC%ED%95%9C-%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-getIdentifer-%EB%B3%80%EC%88%98%EA%B0%92%EC%9D%84-%EC%A1%B0%ED%95%A9%ED%95%B4%EC%84%9C-%EB%A6%AC%EC%86%8C%EC%8A%A4-%EC%95%84%EC%9D%B4%EB%94%94-%EB%B0%9B%EC%95%84%EC%98%A4%EA%B8%B0)
res에 값들을 가지고올때는
R.string.title 처럼 이러한 문법을 사용해야한다
하지만 동적으로 이름을 가져와야하는경우가 있는데 이때 사용하는것이 이것이다
[추가공식문서](https://developer.android.com/codelabs/android-training-support-libraries?index=..%2F..%2Fandroid-training#5)
resources.getIdentifier(name,type,packagename); 이런형태로쓴다

sample code
```Java

private String[] mColorArray = {  
        "red", "pink", "purple", "deep_purple",  
        "indigo", "blue", "light_blue", "cyan", "teal", "green",  
        "light_green", "lime", "yellow", "amber", "orange", "deep_orange",  
        "brown", "grey", "blue_grey", "black" };

Random random = new Random();  
  
String colorName = mColorArray[random.nextInt(20)];  
  
int colorResourceName = getResources().getIdentifier(colorName, "color",  
        getApplicationContext().getPackageName());
//리소스 부분에서 color타입의 colorName이름을 가진 값가지고오기  
int colorRes = ContextCompat.getColor(this, colorResourceName);
//color가지고오기
mHelloTextView.setTextColor(colorRes);

```