# 이용문의 API 이용가이드
API를 통해 TNK에 이용문의를 남길 때 호출한다.   
등록 및 답변 확인은 TNK 홈페이지 로그인 후 [Incentive > CS > CS문의]에서 확인 가능하다.

### 이용문의 등록

|타입|값|
|--|--|
| URL | https://api3.tnkfactory.com/tnk/support.helpv2.main|
| 메소드 | POST |
| Content-Type | application/x-www-form-urlencoded |


#### 요청

|파라미터|필수|상세내용|타입|
|--|--|--|--|
|action|O|[extSend] 고정값|String|
|appid|O|매체AppId 값   ex)30408070-4051-9322-2239-15040708030f|String|
|d|O|문의정보 (JsonObject String을 AES256으로 암호화한 값 - 암복호화 예시 참조)<br />매체앱의 앱키로 암호화.|String|

##### 문의정보 (JsonObject)

|파라미터|필수|상세내용|타입|
|--|--|--|--|
|cmpn_id|O|광고ID|long|
|md_user_nm|O|매체 사용자 식별값 (adid 또는 md_user_nm 중 하나는 필수이다.)|String|
|adid|O|adid (adid 또는 md_user_nm 중 하나는 필수이다.)|String|
|req_cnt|O|문의내용|String|
|note|X|비고|String|
|img_url|X|스크린샷URL|String|


```
https://dev.tnkfactory.com/tnk/support.helpv2.main?action=extSend&appid={매체AppId}&d={문의정보}
```

##### 암복호화 예시

```java
import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;

import org.apache.commons.codec.binary.Base64;
import org.json.JSONObject;

public class EncryteTest {

    public static void main(String[] args) {
        String key = "{매체앱키}";
        
        JSONObject jObj = new JSONObject();
        jObj.put("req_cnt", "문의내용");
        jObj.put("cmpn_id", 0); // 광고ID (cmpn_id)

        jObj.put("adid", "ADID");
        jObj.put("md_user_nm", "사용자식별값");
        jObj.put("note", "비고");

        String message = jObj.toString();

        String encrypt = encryptAESBase64(key, message);

        System.out.println(encrypt);
    }


    /**
     * AES 방식으로 문자열을 암호화한다.
     */
    public static String encryptAES(String key, String message) throws Exception {
        SecretKeySpec skeySpec = new SecretKeySpec(key.getBytes(), "AES");

        Cipher cipher = Cipher.getInstance("AES");
        cipher.init(Cipher.ENCRYPT_MODE, skeySpec);
        byte[] encrypted = cipher.doFinal(message.getBytes("UTF-8"));
        String enStr = new String(Base64.encodeBase64(encrypted), "UTF-8");

        return enStr;
    }

    /**
     * AES 방식으로 문자열을 복호화한다.
     */
    public static String decryptAES(String key, String encrypted) throws Exception {

        SecretKeySpec skeySpec = new SecretKeySpec(key.getBytes(), "AES");
        Cipher cipher = Cipher.getInstance("AES");
        cipher.init(Cipher.DECRYPT_MODE, skeySpec);
        byte[] byteStr = Base64.decodeBase64(encrypted.getBytes("UTF-8"));
        String decStr = new String(cipher.doFinal(byteStr), "UTF-8");

        return decStr;
    }

    /**
     * AES 방식으로 문자열을 암호화 한 후 base64 인코딩을 한 번 더 한다.
     */
    public static String encryptAESBase64(String key, String message) throws Exception {
        return new String(Base64.encodeBase64(encryptAES(key, message).getBytes("UTF-8")), "UTF-8");
    }

     /**
     * base64 디코딩을 한 후 AES 방식으로 문자열을 복호화 한다.
     */
    public static String decryptAESBase64(String key, String encrypted) throws Exception {
        return decryptAES(key, new String(Base64.decodeBase64(encrypted.getBytes("UTF-8")), "UTF-8"));
    }
}
```

#### 응답 (application/json)

|파라미터|상세내용|타입|
|--|--|--|
| ret_cd | 응답코드 |int|
| seq_id | 이용문의 등록 ID |int|


##### 응답 예시

```json
{
    "ret_cd" : 0,
    "seq_id" : 1234
}
```

##### 응답 코드
|코드|내용|
|--|--|
|0|등록 성공|
|-1|파라미터 오류(문의정보 없음)|
|-2|파라미터 오류(appid에 해당하는 매체정보 없음)|
|-3|문의정보 복호화 오류|
|-4|Json parsing 에러|
|-5|adid / md_user_nm 둘 다 값이 존재하지 않음|
|-99|서버 등록 오류|
