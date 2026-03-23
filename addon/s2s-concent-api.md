# 개인정보 수집동의 철회 API 이용가이드
API를 통해 TNK에 개인정보 수집동의 철회를 요청한다.
개인정보  수집동의 철회를 해도 기존에 참여한 이력등은 회사 내부 방침 및 관련법령에 의한 정보보유 사유로 일정기간동안 보존됩니다.

### 동의 철회

|타입|값|
|--|--|
| URL | https://api3.tnkfactory.com/tnk/ppi.consent.main |
| 메소드 | POST |
| Content-Type | application/x-www-form-urlencoded |


#### 요청

|파라미터|필수|상세내용|타입|
|--|--|--|--|
|action|O|withdraw|String|
|pid|O|매체AppId 값   ex)30408070-4051-9322-2239-15040708030f|String|
|md_user_nm|O|매체 사용자 식별값|String|
|chk_cd|O|확인 코드 값 (JsonObject String을 SHA256 암호화한 값 - 암호화 예시 참조)<br />매체앱의 앱키로 암호화.|String|


```
https://api3.tnkfactory.com/tnk/ppi.consent.main?action=withdraw&pid={매체APP ID}&md_user_nm={매체 사용자 식별값}&chk_cd={확인 코드값}
```

##### 암복호화 예시

```java
import org.apache.commons.codec.digest.DigestUtils;

public class EncryptTest {

    public static void main(String[] args) {
        String appId = "{매체앱ID}";
        String mdUserName = "{매체 사용자 식별값}";
        String key = "{매체앱키}";
        
        String checkCode = DigestUtils.sha256Hex((appId + mdUserName + key).getBytes());

        System.out.println(checkCode);
    }

}
```

#### 응답 (application/json)

|파라미터|상세내용|타입|
|--|--|--|
| ret_cd | 응답코드 |String|
| ret_msg | 응답메세지 |String|


##### 응답 예시

```json
{
    "ret_cd" : "S",
    "seq_id" : "SUCCESS"
}
```

##### 응답 코드
|코드|메세지|내용|
|--|--|
|S|SUCCESS|성공|
|F|INVALID PARAMETER|파라미터 오류|
|F|NOT REGIST PUBLISHER|파라미터 오류(appid에 해당하는 매체정보 없음)|
|F|INVALID chk_cd|확인 코드 불일치|
|F|SERVER ERROR|서버 오류|
|F|INTERNAL SERVER ERROR|서버 오류|
