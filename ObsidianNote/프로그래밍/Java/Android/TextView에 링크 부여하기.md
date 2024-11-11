
```Java

public static final String URL_PATTERN = "(https?|ftp):\\/\\/([^\\s\\/?\\.#-]+\\.?)+(\\/[^\\s]*)?";
Pattern urlPattern = Pattern.compile(URL_PATTERN);

private void setClickableUrl(TextView textview, String message){  
    SpannableString spannable = new SpannableString(message);  
    Matcher matcher = DataManager.Instance().urlPattern.matcher(message);  
  
    // url이 없을경우 실행 안함  
    if (!matcher.find()) {  
        return;  
    }  
    matcher.reset();  
    while (matcher.find()) {  
        final String url = matcher.group();  
        setClickableSpan(spannable, matcher.start(), matcher.end(), new ClickableSpan() {  
            @Override  
            public void onClick(@NonNull View widget) {  
                Intent intent = new Intent(Intent.ACTION_VIEW, android.net.Uri.parse(url));  
                widget.getContext().startActivity(intent);  
            }  
        });  
    }  
  
    textview.setText(spannable);  
    textview.setMovementMethod(LinkMovementMethod.getInstance());  
}

private void setClickableSpan(SpannableString spannableString, int start, int end, ClickableSpan clickableSpan) {  
    spannableString.setSpan(clickableSpan, start, end, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);  
}


```

Spannable
은 andoird에서 String에 특정 동작이나 스타일을 추가할수있는 클래스이다.
위에서는
Spannable String을 만든뒤에

url 의 범위를 식별하여
onclick  이벤트를 할당했다.
