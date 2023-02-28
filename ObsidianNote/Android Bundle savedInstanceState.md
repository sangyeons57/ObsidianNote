#Android #Java #savedInstanceState

```Java
@Override
protected void onCreate(Bundle savedInstanceState) {  
    super.onCreate(savedInstanceState);  
    setContentView(R.layout.activity_main);
}
```
savedInstanceState는 Activity가 끝났을때 이전Activity의 값을 가지고오는 것이다.
(The "savedInstanceState" gets the previous  "Activity" value when the "Activity" was close.)
예를들어서 화면을 회전시키면 Activity가 종료되는데 이때 회전하기전 Activity의 값을 활용할때 사용합니다.
