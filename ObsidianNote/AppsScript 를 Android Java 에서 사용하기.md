#ApsScript #Android #Java #REST #RestAPI
Android Java 에서 google sheet api를 사용하는것이 힘들어서
Android Java로 AppsScript를 실행시키고 AppsScript에서 google sheet를 사용하는 방식을 사용했다.

우선 전체 코드다
Android Java
```
이거 필요하다
implementation 'com.android.volley:volley:1.2.1'
```
```Java
package com.example.googlesheets;  
  
import androidx.annotation.Nullable;  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.app.ProgressDialog;  
import android.content.Intent;  
import android.os.Bundle;  
import android.view.View;  
import android.widget.Button;  
import android.widget.EditText;  
  
import com.android.volley.AuthFailureError;  
import com.android.volley.DefaultRetryPolicy;  
import com.android.volley.Request;  
import com.android.volley.RequestQueue;  
import com.android.volley.Response;  
import com.android.volley.RetryPolicy;  
import com.android.volley.VolleyError;  
import com.android.volley.toolbox.StringRequest;  
import com.android.volley.toolbox.Volley;  
  
import java.io.IOException;  
import java.security.GeneralSecurityException;  
import java.util.HashMap;  
import java.util.Map;  
  
public class MainActivity extends AppCompatActivity {  
  
    EditText etName, etPhone, etAddres;  
    Button btnInsert;  
    ProgressDialog progressDialog;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
  
  
        etName = findViewById(R.id.editTextTextPersonName);  
        etName = findViewById(R.id.editTextTextPersonName2);  
        etName = findViewById(R.id.editTextTextPersonName3);  
        btnInsert = findViewById(R.id.button);  
  
        progressDialog = new ProgressDialog(MainActivity.this);  
        progressDialog.setMessage("LOADING....");  
  
        btnInsert.setOnClickListener(new View.OnClickListener(){  
            @Override  
            public void onClick(View v) {  
                addStudentData();  
                progressDialog.show();  
            }  
        });  
    }  
  
    public void addStudentData() {  
        String sName = "a";  
        String sPhone = "b";  
        String sAddress = "c";  
  
        StringRequest stringRequest = new StringRequest(Request.Method.POST, "https://script.google.com/macros/s/AKfycbzzMCtQXr0lXTuNW6cPmkKULispaTditibdNLNFW8r9YMHxcbBgZQZMKHkXULIV2Dz1/exec", new Response.Listener<String>() {  
            @Override  
            public void onResponse(String response) {  
                System.out.println("SUCCESS");  
                System.out.println(response);  
            }  
        }, new Response.ErrorListener() {  
            @Override  
            public void onErrorResponse(VolleyError error) {  
                System.out.println("NOT");  
            }  
        }){  
            @Nullable  
            @Override            
            protected Map<String, String> getParams() throws AuthFailureError {  
                Map<String,String> params = new HashMap<>();  
                params.put("action","test");  
                params.put("vName", sName);  
                params.put("vPhone", sPhone);  
                params.put("vAddress",sAddress);  
  
                return params;  
            }  
        };  
  
        int socketTimeout = 50000;  
        RetryPolicy retryPolicy = new DefaultRetryPolicy(socketTimeout,0,DefaultRetryPolicy.DEFAULT_BACKOFF_MULT);  
        stringRequest.setRetryPolicy(retryPolicy);  
  
        RequestQueue requestQueue = Volley.newRequestQueue(this);  
        requestQueue.add(stringRequest);  
    }  
}

```


중요부분만 정리

```Java

        StringRequest stringRequest = new StringRequest(Request.Method.POST, "https://script.google.com/macros/s/AKfycbzzMCtQXr0lXTuNW6cPmkKULispaTditibdNLNFW8r9YMHxcbBgZQZMKHkXULIV2Dz1/exec", new Response.Listener<String>() {  
            @Override  
            public void onResponse(String response) {  
                System.out.println("SUCCESS");  
                System.out.println(response);  
            }  
        }, new Response.ErrorListener() {  
            @Override  
            public void onErrorResponse(VolleyError error) {  
                System.out.println("NOT");  
            }  
        }){  
            @Nullable  
            @Override            
            protected Map<String, String> getParams() throws AuthFailureError {  
                Map<String,String> params = new HashMap<>();  
                params.put("action","test");  
                params.put("vName", sName);  
                params.put("vPhone", sPhone);  
                params.put("vAddress",sAddress);  
  
                return params;  
            }  
        };

```

우선 이건 StringRequest가 주축으로 작동된다.
Appscript REST API 중 POST 와 GET 만 작동한다

각각 AppsScrip에서 doGet() 함수와 doPost()함수를 실행 시킨다
일반적으로 쓸때는 POST 읽을때는GET을 사용한다
```Java
Request.Method.POST
Request.Method.GET
```
---
그다음 공간에응 배포한 AppsScript의 url 을 적는데
코드를 바꿀때 마다 제배포해서 새로운 url 을 사용해야 한다.
배포관리에서 다시 하면 된다

---
그다음 으로는
new Response.Listener\<String\>() {} 부분과
new Response.ErrorListener() {}이다
```Java
new Response.Listener<String>() {  
	@Override  
	public void onResponse(String response) {  
		System.out.println("SUCCESS");  
		System.out.println(response);  
	}  
}, new Response.ErrorListener() {  
	@Override  
	public void onErrorResponse(VolleyError error) {  
		System.out.println("NOT");  
	}  
```
그냥 위에거는 성공했을때고
아레부분은 실패했을때다
성공할때 값을 가져올수있다

아레는 가져오는 부분 AppsScript다 doGet이게때무에GET 타입으로 해더를 설정해야한다
```Javascript

function doGet(e) {
	return test();
}

function test() {
	return ContentService.createTextOutPut("abc");
}


```

---
다음은
```Java
{  
	@Nullable  
	@Override            
	protected Map<String, String> getParams() throws AuthFailureError {  
		Map<String,String> params = new HashMap<>();  
		params.put("action","test");  
		params.put("vName", sName);  
		params.put("vPhone", sPhone);  
		params.put("vAddress",sAddress);  

		return params;  
	}  
};
```
이부분인데 파라미터를 넣는 부분이다.

AppsScript부분에서는
```Javascript
function doPost(e) {
	var name = e.parameter.vName;
	var phone = e.parameter.vPhone;
	var address = e.parameter.vAddress;
	var action = e.parameter.action;
	//이렇게 받는다

}
```

---

마지막 부분이다.
```Java

int socketTimeout = 50000;  
RetryPolicy retryPolicy = new DefaultRetryPolicy(socketTimeout,0,DefaultRetryPolicy.DEFAULT_BACKOFF_MULT);  
stringRequest.setRetryPolicy(retryPolicy);  

RequestQueue requestQueue = Volley.newRequestQueue(this);  
requestQueue.add(stringRequest);  

```

이쪽부분은 StringRequest부분 변수를 
\<여기\>.setRetryPolicy(retryPolicy); 랑
requestQueue.add(\<여기\>); 에넣어 주면 된다

자세한 역하릉 잘 모르겠지만
실해시켜주는 부분이랑 설정하는 부분인것같다.
 