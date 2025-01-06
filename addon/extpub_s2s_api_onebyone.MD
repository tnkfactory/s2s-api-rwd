# 외부매체 연동 가이드 (특정 지면 건바이건 노출)
이 문서는 매체의 특정 지면에 TNK의 특정 광고를 노출하고 해당 광고를 통해 미션을 달성하는 시점에 매체사 서버로 실적 전송을 하는 방법을 설명하고 있습니다.

### 1. 매체사 매체의 특정 지면에 TNK 제공 특정 광고를 노출합니다.

담당자에게 광고 소재와 광고 URL을 전달받아 매체 특정 위치에 노출합니다.

광고 URL 뒤에 Callback URL에 전달받은 파라미터를 추가 합니다.
| **파라미터(필수)** | **상세 내용** | **최대길이** |
| --- | --- | --- |
| md_user_nm (*)| 사용자 식별을 하기 위하여 전달되는 값이다.(Callback URL에 전달되는 값이다.) | string(256) |
| adid| 모바일 기기의 광고ID (Google ADID, IOS IDFA) | string(36) |

전달 받은 광고 URL 뒤에 &md_user_nm={사용자식별값}&adid={광고ID} 을 추가 합니다. 
(만약 광고ID를 수집하지 않아 전달이 불가능하다면 adid 파라미터는 생략해도 됩니다.)

```
https://api3.tnkfactory.com/tnk/ad.click.main?pid=e0a0070-b0f1-2359-9532-1f0b07040e04&app_id=52158&md_user_nm={사용자식별값}&adid={광고ID}
```

예시)
```
https://api3.tnkfactory.com/tnk/ad.click.main?pid=e0a0070-b0f1-2359-9532-1f0b07040e04&app_id=52158&md_user_nm=K3209023&adid=84268c92-549a-4c1e-b3c9-11ffdb076ec8
```
  
### 2. Callback URL (Postback URL)

사용자가 광고 참여를 완료하면 획득한 포인트를 매체사 서버로 실시간 전송 합니다.

호출방식 : HTTP POST

Parameters

| **파라미터** | **상세 내용** | **최대길이** |
| --- | --- | --- |
| seq_id | 포인트 지급에 대한 고유한 ID 값이다. URL이 반복적으로 호출되더라도 이 값을 사용하여 중복지급여부를 확인할 수 있다. | string(50) |
| pay_pnt | 사용자에게 지급되어야 할 포인트 값이다. | long |
| md_user_nm | 사용자 식별을 하기 위하여 전달되는 값이다.(1의 md_user_nm과 같다.) | string(256) |
| md_chk | 전달된 값이 유효한지 여부를 판단하기 위하여 제공된다. 이 값은 app_key + md_user_nm + seq_id 의 MD5 Hash 값이다. (App Key는 담당자가 전달 확인) | string(32) |
| app_id | 사용자가 참여한 광고앱의 고유 ID 값이다. | long |
| pay_dt | 포인트 지급시각이다. (System milliseconds) 예) 1577343412017 | long |
| app_nm | 참여한 광고명 이다. | string(120) |
| pay_amt | 정산되는 금액. | long |
| actn_id | 광고 타입  - 0 : 설치형- 1 : 실행형- 2 : 액션형 -4 : 클릭형 - 5 : 구매형 | int |

리턴값 처리

TNK 서버에서는 위 URL을 호출하고 HTTP 리턴코드로 200이 리턴되면 정상적으로 처리되었다고 판단합니다.
만약 200이 아닌 값이 리턴된다면 TNK 서버는 비정상처리로 판단하고 이후에는 1시간 단위로 최대 24시간 동안 반복적으로 호출합니다.

- 중요! 동일한 Request가 반복적으로 호출될 수 있으므로 seq_id 값을 사용하시어 반드시 중복체크를 하셔야 합니다.
- Callback URL 작성 후 담당자 또는 [tech@tnkfactory.com](mailto:tech@tnkfactory.com) 으로 전달해주시면 됩니다.
- 연동관련 기술문의는 [tech@tnkfactory.com](mailto:tech@tnkfactory.com)으로 연락 바랍니다.
