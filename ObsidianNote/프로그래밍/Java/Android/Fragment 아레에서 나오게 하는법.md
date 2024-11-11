```Java
@Override  
public void onStart() {  
    super.onStart();  
    if (getDialog() != null && getDialog().getWindow() != null) {  
        getDialog().getWindow().setLayout(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT);  
        getDialog().getWindow().setBackgroundDrawable(new ColorDrawable(Color.TRANSPARENT));  
        getDialog().getWindow().setGravity(Gravity.BOTTOM);  
    }
```
주요 코드는
- getDialog().getWindow().setLayout(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT);  
- getDialog().getWindow().setBackgroundDrawable(new ColorDrawable(Color.TRANSPARENT));  
- getDialog().getWindow().setGravity(Gravity.BOTTOM);
-

이거 3개 인데
각각
- layout 크기 바꾸기
- 백그라운드 반투명 기능이다.
- 아레쪽에 표시하는 기능
