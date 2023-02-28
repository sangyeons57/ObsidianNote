#Java #OAuth #GoogleSheetAPI #gradle

[Java Quick Start 공식 구글시트 퀵스타트 가이드](https://developers.google.com/sheets/api/quickstart/java)
[[Java GoogleSheet Api]] 성공내용

[Google Cloud](https://console.cloud.google.com/) 에서 프로젝트를 생성 해야한다.
API & Services 에서 Credentials부분에 들어가서
CREATE CREDENTIALS -> OAuth 부분을 선택한다
적합한 플렛폼을 선택해서 Json 파일을 다운 받는다

/resources부분에 해당파일을 넣은다

(!주위) 자바에서는 Gradle방식을 사용해야한다
->Maven 방식도 존제하기는 한는것 같다.

gradle.build파일 설정은 이렇게
[Github](https://github.com/googleworkspace/java-samples/blob/main/drive/quickstart/build.gradle)
```gradle
apply plugin: 'java'  
apply plugin: 'application'

mainClassName = 'DriveQuickstart'
sourceCompatibility = 11
targetCompatibility = 11
version = '1.0'

repositories {
	mavenCentral()  
}

dependencies {
	implementation 'com.google.api-client:google-api-client:2.0.0'
	implementation 'com.google.oauth-client:google-oauth-client-jetty:1.34.1' 
	implementation 'com.google.apis:google-api-services-drive:v3-rev20220815-2.0.0'  
}
```
mainClassName 은 말그대로 mainClassName이다 해당 이름으로 파일이름을 하고 메인함수를 만들어야 한다.

---
---

[Github](https://github.com/googleworkspace/java-samples/blob/main/drive/quickstart/src/main/java/DriveQuickstart.java)
```Java

import com.google.api.client.auth.oauth2.Credential;
import com.google.api.client.extensions.java6.auth.oauth2.AuthorizationCodeInstalledApp;
import com.google.api.client.extensions.jetty.auth.oauth2.LocalServerReceiver;
import com.google.api.client.googleapis.auth.oauth2.GoogleAuthorizationCodeFlow;
import com.google.api.client.googleapis.auth.oauth2.GoogleClientSecrets;
import com.google.api.client.googleapis.javanet.GoogleNetHttpTransport;
import com.google.api.client.http.javanet.NetHttpTransport;
import com.google.api.client.json.JsonFactory;
import com.google.api.client.json.gson.GsonFactory;
import com.google.api.client.util.store.FileDataStoreFactory;
import com.google.api.services.drive.Drive;
import com.google.api.services.drive.DriveScopes;
import com.google.api.services.drive.model.File;
import com.google.api.services.drive.model.FileList;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.security.GeneralSecurityException;
import java.util.Collections;
import java.util.List;

/* class to demonstarte use of Drive files list API */
public class DriveQuickstart {
  /**
   * Application name.
   */
  private static final String APPLICATION_NAME = "Google Drive API Java Quickstart";
  /**
   * Global instance of the JSON factory.
   */
  private static final JsonFactory JSON_FACTORY = GsonFactory.getDefaultInstance();
  /**
   * Directory to store authorization tokens for this application.
   */
  private static final String TOKENS_DIRECTORY_PATH = "tokens";

  /**
   * Global instance of the scopes required by this quickstart.
   * If modifying these scopes, delete your previously saved tokens/ folder.
   */
  private static final List<String> SCOPES =
      Collections.singletonList(DriveScopes.DRIVE_METADATA_READONLY);
  private static final String CREDENTIALS_FILE_PATH = "/credentials.json";

  /**
   * Creates an authorized Credential object.
   *
   * @param HTTP_TRANSPORT The network HTTP Transport.
   * @return An authorized Credential object.
   * @throws IOException If the credentials.json file cannot be found.
   */
  private static Credential getCredentials(final NetHttpTransport HTTP_TRANSPORT)
      throws IOException {
    // Load client secrets.
    InputStream in = DriveQuickstart.class.getResourceAsStream(CREDENTIALS_FILE_PATH);
    if (in == null) {
      throw new FileNotFoundException("Resource not found: " + CREDENTIALS_FILE_PATH);
    }
    GoogleClientSecrets clientSecrets =
        GoogleClientSecrets.load(JSON_FACTORY, new InputStreamReader(in));

    // Build flow and trigger user authorization request.
    GoogleAuthorizationCodeFlow flow = new GoogleAuthorizationCodeFlow.Builder(
        HTTP_TRANSPORT, JSON_FACTORY, clientSecrets, SCOPES)
        .setDataStoreFactory(new FileDataStoreFactory(new java.io.File(TOKENS_DIRECTORY_PATH)))
        .setAccessType("offline")
        .build();
    LocalServerReceiver receiver = new LocalServerReceiver.Builder().setPort(8888).build();
    Credential credential = new AuthorizationCodeInstalledApp(flow, receiver).authorize("user");
    //returns an authorized Credential object.
    return credential;
  }

  public static void main(String... args) throws IOException, GeneralSecurityException {
    // Build a new authorized API client service.
    final NetHttpTransport HTTP_TRANSPORT = GoogleNetHttpTransport.newTrustedTransport();
    Drive service = new Drive.Builder(HTTP_TRANSPORT, JSON_FACTORY, getCredentials(HTTP_TRANSPORT))
        .setApplicationName(APPLICATION_NAME)
        .build();

    // Print the names and IDs for up to 10 files.
    FileList result = service.files().list()
        .setPageSize(10)
        .setFields("nextPageToken, files(id, name)")
        .execute();
    List<File> files = result.getFiles();
    if (files == null || files.isEmpty()) {
      System.out.println("No files found.");
    } else {
      System.out.println("Files:");
      for (File file : files) {
        System.out.printf("%s (%s)\n", file.getName(), file.getId());
      }
    }
  }
}
```


jdk버전이랑 안 맞는 경우가있다 그때는 자바버전을 해당 버전에 맞게 해줘야한다

다운로드 받은 Json 파일을 
위코드에   CREDENTIALS_FILE_PATH 에 나와있는것처럼 바꿔주어야 한다

[GoogleSheet API READ&WRITE](https://developers.google.com/sheets/api/guides/values)
[Github](https://github.com/googleworkspace/java-samples/tree/main/sheets/snippets/src/main/java)


