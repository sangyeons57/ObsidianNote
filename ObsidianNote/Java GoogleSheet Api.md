#Java #GoogleSheet #GoogleSheetAPI 

여러가지 방법을 시도했은데 최종적으로 성공한 방법을 적겠다
[[Java Google Sheet API & OAuth]] 에 관련 내용있다.

gradle.build
```gradle

apply plugin: 'java'  
apply plugin: 'application'  
  
mainClassName = 'SheetsQuickstart'  
sourceCompatibility = 1.8  
targetCompatibility = 11  
version = '1.0'  
  
repositories {  
    mavenCentral()  
}  
  
dependencies {  
    implementation 'com.google.api-client:google-api-client:2.0.0'  
    implementation 'com.google.oauth-client:google-oauth-client-jetty:1.34.1'  
    implementation 'com.google.apis:google-api-services-sheets:v4-rev20221216-2.0.0'  
  
    implementation 'com.google.auth:google-auth-library-oauth2-http:1.11.0'  
    implementation 'com.google.apis:google-api-services-drive:v3-rev20220815-2.0.0'  
    implementation 'com.google.apis:google-api-services-sheets:v4-rev20220927-2.0.0'  
  
}

```

Java file
```Java
package org.example;  
  
import com.google.api.client.googleapis.json.GoogleJsonError;  
import com.google.api.client.googleapis.json.GoogleJsonResponseException;  
  
import com.google.api.client.auth.oauth2.Credential;  
import com.google.api.client.extensions.java6.auth.oauth2.AuthorizationCodeInstalledApp;  
import com.google.api.client.extensions.jetty.auth.oauth2.LocalServerReceiver;  
import com.google.api.client.googleapis.auth.oauth2.GoogleAuthorizationCodeFlow;  
import com.google.api.client.googleapis.auth.oauth2.GoogleClientSecrets;  
import com.google.api.client.googleapis.auth.oauth2.GoogleCredential;  
import com.google.api.client.googleapis.javanet.GoogleNetHttpTransport;  
import com.google.api.client.http.HttpRequestInitializer;  
import com.google.api.client.http.HttpTransport;  
import com.google.api.client.http.LowLevelHttpRequest;  
import com.google.api.client.http.javanet.NetHttpTransport;  
import com.google.api.client.json.JsonFactory;  
import com.google.api.client.json.gson.GsonFactory;  
import com.google.api.client.util.store.FileDataStoreFactory;  
import com.google.api.services.sheets.v4.Sheets;  
import com.google.api.services.sheets.v4.SheetsScopes;  
import com.google.api.services.sheets.v4.model.AppendValuesResponse;  
import com.google.api.services.sheets.v4.model.UpdateValuesResponse;  
import com.google.api.services.sheets.v4.model.ValueRange;  
import com.google.api.services.sheets.v4.model.BatchUpdateSpreadsheetRequest;  
import com.google.api.services.sheets.v4.model.BatchUpdateSpreadsheetResponse;  
import com.google.api.services.sheets.v4.model.FindReplaceRequest;  
import com.google.api.services.sheets.v4.model.FindReplaceResponse;  
import com.google.api.services.sheets.v4.model.Request;  
import com.google.api.services.sheets.v4.model.SpreadsheetProperties;  
import com.google.api.services.sheets.v4.model.UpdateSpreadsheetPropertiesRequest;  
import com.google.auth.http.HttpCredentialsAdapter;  
import com.google.auth.oauth2.GoogleCredentials;  
import java.io.FileNotFoundException;  
import java.io.IOException;  
import java.io.InputStream;  
import java.io.InputStreamReader;  
import java.security.GeneralSecurityException;  
import java.util.Arrays;  
import java.util.Collections;  
import java.util.List;  
import  java.util.ArrayList;  
  
public class SheetsQuickstart {  
  
    private static Sheets sheetsService;  
    private static String APPLICATION_NAME = "Google sheets Example";  
    private static String SPREADSHEET_ID = "1qyYxTzImmdsWad_2zM1rvXwbZ96ihnJjHTkx-K1efow";  
  
  
    private static Credential authorize() throws IOException, GeneralSecurityException {  

		/**
		credential.json 파일을 google cloud에서 다운받아서
		이름을 credential.json으로 바꾸고
		resources 폴더에 넣는다
		*/
        InputStream in = SheetsQuickstart.class.getResourceAsStream("/credentials.json");  


        GoogleClientSecrets clientSecrets = GoogleClientSecrets.load(  
                GsonFactory.getDefaultInstance(), new InputStreamReader(in)  
        );  
  
        List<String> scopes = Arrays.asList(SheetsScopes.SPREADSHEETS);  
  
        GoogleAuthorizationCodeFlow flow = new GoogleAuthorizationCodeFlow.Builder(  
                GoogleNetHttpTransport.newTrustedTransport(), GsonFactory.getDefaultInstance(),  
                clientSecrets, scopes)  
                .setDataStoreFactory(new FileDataStoreFactory(new java.io.File("tokens")))  
                .setAccessType("offline")  
                .build();  
        
        //계정 인증 아이디 입력
        Credential credential = new AuthorizationCodeInstalledApp(  
                flow, new LocalServerReceiver())  
                .authorize("sangyeons57");  
  
        return credential;  
    }  
  
  
    /**  
     * Prints the names and majors of students in a sample spreadsheet:     * https://docs.google.com/spreadsheets/d/1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgvE2upms/edit     */    
public static Sheets getSheetsService() throws IOException, GeneralSecurityException {  

	Credential credential = authorize();  

    return new Sheets.Builder(GoogleNetHttpTransport.newTrustedTransport(),  
                GsonFactory.getDefaultInstance(), credential)  
                .setApplicationName(APPLICATION_NAME)  
                .build();  
    }  
  
public static void  main(String[] args) throws  IOException, GeneralSecurityException {  
		//서비스 가져오기
	sheetsService = getSheetsService();  


//업데이틑 할때쓴느 코드
ValueRange body = new ValueRange()  
	.setValues(Arrays.asList(  
		Arrays.asList("updated")  
	));  

UpdateValuesResponse result = sheetsService.spreadsheets().values()  
	.update(SPREADSHEET_ID, "A1", body)  
	.setValueInputOption("RAW")  
	.execute();  
//append create할때 쓰는 코드
ValueRange appendBody = new ValueRange()  
	.setValues( Arrays.asList(
		Arrays.asList("바보")  
	));  

AppendValuesResponse appendValuesResponse = sheetsService.spreadsheets().values()  
	.append(SPREADSHEET_ID, "A", appendBody)
	.setValueInputOptioe("USER_ENTERED")
	.setInsertDataOption("INSERT_ROWS")
	.setIncludeValuesInResponse(true)
	.execute();

//read할때 쓰는 코드
String range = "A!D2:E13";  
  
ValueRange response = sheetsService.spreadsheets().values()  
	.get(SPREADSHEET_ID, range)        
	.execute();  
List<List<Object>> values = response.getValues();  
  
if (values == null || values.isEmpty()) {  
    System.out.println("No data found.");} else {  
    for (List row: values) {
        System.out.println(row.get(0));    }}
    }  
  
}
```