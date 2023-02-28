#Android #Java #Intent 
이것은 다른 Intent를 실행시키고 값이 돌아올때 까지 기다리다
값이 돌아오면 실행시키는 것에 대한 내용이다

실행시키고 값을 기다리는부분
```Java
ActivityResultLauncher<Intent> launchSomeActivity = registerForActivityResult(  
        new ActivityResultContracts.StartActivityForResult(),  
        new ActivityResultCallback<ActivityResult>() {  
            @Override  
            public void onActivityResult(ActivityResult result) {  
                if (result.getResultCode() == Activity.RESULT_OK)  
                {  
	                //Intent실행에서 값이 돌아올경우 실행되는파트
	                result.getData()
	                //위 방법으로 데이터를 받을수있다 예시)
	                result.getData().getStringExtra(다른 Activity의토큰)
				
					String reply = "value";  
                    mReplyTextView.setText(reply); 
                
                }  
            }  
        }  
);


public static String token = "a";
Intent intent = new Intent(현제 Activity.this, 갈려는 Activity.class);
String message = "보낼려는값";
intent.putExtra(token, message);
launchSomeActivity.launch(intent);


```

값돌려주는 부분
```Java
String reply = "value";
Intent replyIntent = new Intent();
replyIntent.putExtra(main부분에서 받는다고 정했던 token, reply);
setResult(RESULT_OK, replyIntent); //replyIntent 설정
finsh(); //현재 Activity종료 후 main Activity쪽으로 돌아감
//Extra값을 가지고

```