# TnkAd Integration Guide

### (외부 매체 연동 가이드)

### V 5.1

### [http://www.tnkfactory.com](http://www.tnkfactory.com/)


\- 변경사항

- 2021/09/24
  - 2.3 광고목록 조회 리턴 값(cat\_id - 구매형) 추가
- 2021/07/27
  - 2.3 광고목록 조회 리턴 값(cmpn\_type) 추가
  - 3.3 광고유형 코드 추가
- 2020/03/17
  - 2.9 광고 적립 URL 호출 파라미터(actn\_id, pay\_cnt) 추가
- 2020/02/24
  - 2.3 광고목록 조회 리턴 값(adv\_amt) 추가
- 2020/02/19
  - 2.3 리턴값(actn\_id) 내용 추가(구매형)
- 2019/12/26
  - 2.9 광고적립URL호출 시 파라미터(pay\_dt) 추가
- 2019/9/24
  - 광고목록 조회 / qeuryjoin 리턴 값(dev\_nm) 추가
  - wvid, wvlvl 파라미터 추가
  - 3.2 기기식별값 widevine id 추가
- 2019/8/8
  - 광고 적립 URL 호출시 파라미터(pay\_amt) 추가
  - 리턴코드값(P1) 추가
- 2018/3/15
  - 광고목록 조회시 리턴 값(cat\_id) 추가
- 2018/2/1
  - 광고목록 조회시 파라미터(orientation) / 리턴 값(img\_url) 추가
- 2017/4/5
  - 서버 주소 변경 : api3.tnkfactory.com
- 2016/7/15
  - UID, SUB\_ID 파라미터 추가
- 2014/12/10
  - 파라메터 추가 : 단말기 IP address (ip\_addr)
- 2014/10/10
  - 서버 주소 변경 : api2.tnkfactory.com
- 2014/08/26
  - UID, EID  파라메터 삭제
- 2014/07/30
  - 에러코드 E6 의미가 변경됨 : “매체에 지정된 캠페인 수량이 초과됨” → “참여할 수 없는 기기”
- 2014/06/12
  - 구글의 Advertising ID 적용을 위한 내용 추가
- 2014/01/13
  - 광고적립 URL(callback URL) 호출시 uid, chk\_cd 파라메터 deprecated 됨
- 2013/10/04
  - md\_user\_nm을 통한 중복적립 방지 기능 추가됨
- 2013/09/11
  - 연동 API용 서버 주소 변경
  - iOS Device ID 값을 mac-address에서 Idfa 값으로 변경함

1. **개요**

TnkAd의 플랫폼과 외부 매체사와의 연동을 위한 가이드이다. TnkAd에서 외부 매체용으로 제공하는 광고 형태는 설치형과 실행/액션형이 있으며 각각에 대한 처리 절차는 아래와 같다.



2. **상세 연동 절차**

  2.1. **연동 공통 사항**
- 연동을 위하여 Tnk 사이트에서 앱 등록이 선행되어야한다.
- 외부 매체사는 광고 참여를 위하여 최소한 requestJoin API를 호출해야하며, 설치형은 reqeustReward API를 사용하여 적립요청을 해야한다.
- Tnkad 서버로부터 적립 결과를 전달 받기 위한 URL을 제공해야한다.
- 연동 방식은 HTTP의 POST 이다.
- 연동 결과는 JSON 문자열로 반환된다.

  2.2. **연동 매체 설정**

외부 매체의 경우에도 다른 Tnk 매체와 동일하게 Tnk 사이트에 회원 가입후 게시앱 정보를 등록한다.

앱 등록 후 게시 정보를 설정하고 이때 반드시  포인트관리를“외부매체로 등록”를 선택하고 연동 URL을 입력해야한다. 이때 입력하는 URL은 Tnk 서버에서 리워드 지급 호출시 사용된다.

이후 연동할 매체사의 서버 IP주소를 등록하도록 한다.

앱 등록은 OS 별로 등록되어야 한다. (만약 기존에 하나의 앱을 등록하여 Android와 iOS 공통으로 사용하였다면 반드시 분리하여 등록해야 한다.)


  2.3. **광고목록 조회**

Tnk서버에서 현재 제공되는 광고 목록으로 요청한다. 광고 목록은 사용자의 단말기 종류, 설치된 마켓 종류에 따라서 다르게 제공되므로 관련 파라메터를 전달해야한다. 또한 사용자가 이미 적립한 광고는 목록에서 제외된다.

주의) 처음 테스트 시에는 Tnk 사이트에서 개발단말기 등록을 해야 광고목록이 조회된다. 개발 단말기 등록은 Tnk 사이트 로그인 후 오른쪽 상단의 “개발지원” 메뉴에서 등록한다.

- Requst URL
``` 
http://api3.tnkfactory.com/tnk/ad.offerlist.main
```
- Request Parameter

|파라메터 명|내용|
| :-: | :-: |
|pid|<p>TnkAd 사이트에 등록된 외부 매체사의 외부 매체 앱의 App ID</p><p>**(예시 : e0a08070-b0f1-2359-9532-1f0b07040e04)**</p>|
|adid|<p>Android의 경우 Advertising ID 값 (3.2 Advertising ID 참고)</p><p>iOS의 경우 IdFA (3.2 idFA 참고)</p>|
|uid|사용자 단말기의 IMEI (안드로이드인 경우만)|
|sub\_id|하위 매체의 ID(있는 경우만)|
|wvid|Widevine DRM용 ID값 (안드로이드인 경우만)|
|wvlvl|Widevine security level (L1/L3)|
|md\_user\_nm|매체사에서 사용하는 사용자 ID 값이 있는 경우 이를 지정하면 이 값을 기준으로 중복제거하여 광고노출이 이루어진다.|
|ip\_addr|사용자 단말기의 IP 주소|
|ext\_mkt|<p>사용자의 단말기에 설치된 통신사 마켓 정보를 전달한다.</p><p>- T : 티스토어,  K : 올레마켓, O : LG U+마켓(구 오즈스토어)</p><p>여러개의 마켓이 설치된 경우에는 | 기호로 연결하여 전달한다. 예) T|K</p>|


|Optional 파라메터 (타게팅을 위하여 사용됨)|
| :- |
|res\_cd|<p>(Optional)</p><p>단말기의 해상도에 따른 타게팅을 위하여 사용된다.</p><p>단말기의 가로 해상도를 지정한다</p><p>예) 해상도가 480x800 인경우 480으로 전달</p>|
|user\_brth|사용자의 연령|
|user\_sex|사용자의 성별 (M/F)|
|ph\_mdl|사용자 단말기의 모델 명|
|ph\_ntn|국가 코드 (KR, JP 등)|
|ph\_lang|언어 (ko, ja 등) – 다국어 처리용|
|orientation|전면이미지 URL을 요청한다. 1 – 세로, 2 – 가로, 3 – 정사각형|

- Return 값
  - ret\_cd : 처리 결과 코드 (3.1 리턴 코드 참고)
  - list : 광고 목록 데이터

|컬럼명|내용|
| :-: | :-: |
|app\_id|**광고앱 고유 ID**|
|actn\_id|<p>광고 형태 값을 나타냄</p><p>- 0 : 설치형 (설치형의 경우 매체에서 설치확인을 하고 “2.6 requestReward”를 사용하여 적립요청을 해야함)</p><p>- 1 : 실행형</p><p>- 2 : 액션형</p><p>실행형과 액션형은 Tnk 서버에서 매체서버로 적립내용을 호출해준다.</p><p>- 5 : 구매형</p><p>사용자 결제완료시 매체서버로 적립내용을 호출해준다.</p>|
|app\_pkg|안드로이드의 경우 앱의 패키지명이고 iOS의 경우에는 custom URL 값이다. 광고 형태가 설치형인 경우(actn\_id = 0)에는 이 값을 이용하여 설치여부를 판단해야한다.|
|app\_rate|<p>광고앱의 나이 제한 값이다.</p><p>0 이면 전체연령가, 12면 청소년, 19면 성인앱을 의미한다.</p>|
|pnt\_amt|<p>사용자에게 지급되는 포인트 값이다. Tnk 사이트에서 설정한 포인트 전환 비율에 따라서 계산되어 전달된다.</p><p>예) 100원 당 10콩알로 설정되었다면 300원 광고의 경우 30으로 전달됨</p>|
|os\_type|<p>광고 앱의 OS 종류이다.</p><p>- W : 웹 광고, A : 안드로이드 앱 광고, I : iOS 앱 광고</p>|
|free\_yn|무료 여부이다. (Y :무료, N:유료)|
|app\_nm|앱 명칭|
|corp\_desc|앱 요약 설명|
|pnt\_unit|포인트 단위 명칭|
|img\_url|Orientation 파라미터가 있는 경우 orientation에 해당하는 이미지 URL|
|cat\_id|<p>앱에 속한 카테고리 ID이다.(ex- GM, GS, GT, FN ….)</p><p>구매형 카테고리 (01:생활,03:뷰티,05:식품,06:잡화,07:건강)</p>|
|dev\_nm|광고 개발사 명칭|
|adv\_amt|광고비|
|cmpn\_type|광고유형 : 3.3 광고유형 코드 참조|


- 호출 예시


```
https://api3.tnkfactory.com/tnk/ad.offerlist.main?pid=e0a08070-b0f1-2359-9532-1f0b07040e04&adid=38400000-8cf0-11bd-b23e-10b96e40000d&ip_addr=123.123.123.123&orientation=3
```


``` json
{"ret_cd":"S", "list":[{"app_id":26433,"actn_id":2,"app_rate":0,"pnt_amt":220,"bid_amt":0,"app_pkg":"com.pizzahut.android","app_nm":"피자헛_200214","corp_desc":"1","actn_desc":"1","dev_nm":"피자헛","from_dt":"19700101","to_dt":"29991231","cat_id":"LF","os_type":"W","free_yn":"Y","pnt_unit":"코인""adv_amt":300,”cmpn_type”:402},{"app_id":26415,"actn_id":2,"app_rate":0,"pnt_amt":5200,"bid_amt":0,"app_pkg":"com.gsretail.android.smapp","app_nm":"GS fresh / 심플리쿡_카카오커머스","corp_desc":"GS fresh / 심플리쿡_카카오커머스","actn_desc":"1","dev_nm":"GS프레시","from_dt":"20200218","to_dt":"20200229","cat_id":"SH","os_type":"A","free_yn":"Y","pnt_unit":"코인""adv_amt":7000,"cmpn_type":103},{"app_id":20429,"actn_id":2,"app_rate":0,"pnt_amt":430,"bid_amt":0,"app_pkg":"com.tmon","app_nm":"타임커머스 티몬 (회원가입, 상세설명 참조)","corp_desc":"티몬에선 매일매일이 쇼핑타이밍!","actn_desc":"티몬 신규 회원가입을 완료하세요 (최초 설치 유저만 10분내 보상이 지급됩니다.)","dev_nm":"티몬","from_dt":"20200201","to_dt":"20200229","cat_id":"ET","os_type":"A","free_yn":"Y","pnt_unit":"코인""adv_amt":574,”cmpn_type”:201},{"app_id":24078,"actn_id":1,"app_rate":0,"pnt_amt":150,"bid_amt":0,"app_pkg":"kr.co.beautynet.missha","app_nm":"미샤 (최초실행)","corp_desc":"최초 실행유저만 5분 이내보상을 받을 수 있습니다.","actn_desc":"앱을 설치 후 실행해주세요.","dev_nm":"ABLE C&C CO., LTD","from_dt":"20200201","to_dt":"20200229","cat_id":"SH","os_type":"A","free_yn":"Y","pnt_unit":"코인""adv_amt":202,”cmpn_type”:102},...]}
```


  2.4. **이미지 URL**

광고 목록의 app_id 값을 사용하여 Tnk 서버에 등록된 광고앱의 이미지 Thumb-nail 데이터를 가져올 수 있다.

- Requst URL
``` 
http://api3.tnkfactory.com/tnk/ad.icon.main
```  
- Request Parameter

|파라메터 명|내용|
| :-: | :-: |
|app\_id|광고앱 ID|

- 호출 예시
```
http://api3.tnkfactory.com/tnk/ad.icon.main?app_id=105
```

  2.5. **queryJoin**

해당 광고 상품이 사용자에게 이미 지급되었는지 확인 요청한다. 이 API는 사용자에게 동일 광고에 대한 보상이 중복되어 지급지 않도록하기 위하여 사전 점검하는 API이며 다음에 설명할 requestJoin API를 통해서도 중복 여부에대한 확인이 가능하므로 queryJoin API를 꼭 사용해야하는 것은 아니다.

- Requst URL
  - https://api3.tnkfactory.com/tnk/ad.queryjoin.main
- Request Parameter

|파라메터 명|내용|
| :-: | :-: |
|pid|<p>TnkAd 사이트에 등록된 외부 매체사의 외부 매체 앱의 App ID</p><p>**(예시 : e0a08070-b0f1-2359-9532-1f0b07040e04)**</p>|
|app\_id|캠페인 중인 광고 앱 ID (향후 별도로 전달)|
|adid|<p>Android의 경우 Advertising ID 값 (3.2 Advertising ID 참고)</p><p>iOS의 경우 IdFA (3.2 idFA 참고)</p>|
|uid|사용자 단말기의 IMEI (안드로이드인 경우만)|
|sub\_id|하위 매체의 ID(있는 경우만)|
|wvid|Widevine DRM용 ID값 (안드로이드인 경우만)|
|wvlvl|Widevine security level (L1/L3)|
|ip\_addr|사용자 단말기의 IP 주소|
|ph\_lang|언어코드 (ko, en, ja 등 다국어 처리용) (기본값 : ko)|


- Return 값
  - ret\_cd : 처리 경과 값 (3.1 리턴 코드 참고)
  - actn\_id : 광고 형태 (0:설치형, 1:실행형, 2: 액션형)
  - actn\_desc : 액션형 광고인 경우(actn\_id = 2) 사용자가 포인트 적립을 위하여 수행해야할 내용이다.
  - app\_desc : 광고에 대한 상세 설명 문구이다.
  - dev\_nm : 광고 개발사 명칭

- 호출 예시
``` 
https://api3.tnkfactory.com/tnk/ad.queryjoin.main?pid=e0a08070-b0f1-2359-9532-1f0b07040e04&app_id=105&adid=38400000-8cf0-11bd-b23e-10b96e40000d&ip_addr=123.123.123.123
```
``` json
{"ret_cd":"S", "actn_id":2, "actn_desc":"롯데체크카드를 신청합니다.(포인트는 자동 지급됩니다.)", "app_desc":"- 본 이벤트는 만 18세 이상 롯데카드 신규 회원님만 1일 1회 참여 가능합니다.\r\n- 롯데닷컴 최고 5% 할인\r\n- 롯데포인트 최고 10% 적립\r\n- 연회비 면체 및 SMS 무료\r\n- 롯데리아 할인","dev_nm":"롯데"}
```

  2.6. **requestJoin**

해당 광고 상품에 대하여 사용자가 참여한다는 것을 TnkAd 서버에게 알리는 API 이다. 이 API는 설치형이나 실행/액션형 광고 모두 반드시 호출해야하는 API이다. 이 API를 호출하지 않고 외부 매체가 임의로 광고 참여하게 되면 이후 리워드 지급을 받을 수 없다.

- Requst URL
```
https://api3.tnkfactory.com/tnk/ad.requestjoin.main
```  
- Request Parameter

|파라메터 명|내용|
| :-: | :-: |
|pid|<p>TnkAd 사이트에 등록된 외부 매체사의 외부 매체 앱의 App ID</p><p>**(예시 : e0a08070-b0f1-2359-9532-1f0b07040e04)**</p>|
|app\_id|캠페인 중인 광고 앱 ID (향후 별도로 전달)|
|adid|<p>Android의 경우 Advertising ID 값 (3.2 Advertising ID 참고)</p><p>iOS의 경우 IdFA (3.2 idFA 참고)</p>|
|uid|사용자 단말기의 IMEI (안드로이드인 경우만)|
|md\_user\_nm|매체사에서 사용하는 사용자 ID 값이 있는 경우 이를 지정하면 이후  적립 URL 호출시 다시 전달해준다.|
|sub\_id|하위 매체의 ID(있는 경우만)|
|wvid|Widevine DRM용 ID값 (안드로이드인 경우만)|
|wvlvl|Widevine security level (L1/L3)|
|ph\_mdl|사용자 단말기의 모델 명|
|ip\_addr|사용자 단말기의 IP 주소|
|ext\_data|추가 데이터가 있는 경우 지정한다. 이후 적립 URL 호출 시 다시 전달해 준다.|
|ext\_mkt|<p>사용자의 단말기에 설치된 통신사 마켓 정보를 전달한다. (구글 또는 앱스토어 정보는 전달할 필요없음)</p><p>- T : 티스토어,  K : 올레마켓, O : LG U+마켓(구 오즈스토어)</p><p>여러개의 마켓이 설치된경우에는 | 기호로 연결하여 전달한다.</p>|
- Return 값
  - ret\_cd : 처리 결과 값 (3.1 리턴 코드 참고)
  - mkt\_id : 이동해야할 마켓의 종류이다 (W: 웹페이지, G: 구글플레이, T:티스토어, K:올레마켓, O: LG U+마켓, I : 앱스토어)
  - mkt\_url : 이동해야할 웹 또는 마켓 URL 이다.
- 호출 예시
```
http://api3.tnkfactory.com/tnk/ad.requestjoin.main?pid=e0a08070-b0f1-2359-9532-1f0b07040e04&app_id=105& md_user_nm=00001&ext_mkt=T&adid=38400000-8cf0-11bd-b23e-10b96e40000d&ip_addr=123.123.123.123
```
```
{"ret_cd":"S", "mkt_id":"W", "mkt_url":"http://www.tnkfactory.com/tnk/customers.app.main?action=lottecardpage&adkey=972854a8b006c155c47b771229ec26a6afcd160b3742cf8f1a0e844d2eef90323749df69b6926cc96c14a148a1"}
```
  2.7. **Click URL**

사용자가 광고에 참여하는 것을 서버연동 API인 requestJoin을 사용하지 않고 사용자의 단말기에서 단순하게 아래의 Link를 open하는 것으로 대치할 수 있다.

- Requst URL : 
```
https://api3.tnkfactory.com/tnk/ad.click.main
```
|파라메터 명|내용|
| :-: | :-: |
|pid|<p>TnkAd 사이트에 등록된 외부 매체사의 외부 매체 앱의 App ID</p><p>**(예시 : e0a08070-b0f1-2359-9532-1f0b07040e04)**</p>|
|app\_id|캠페인 중인 광고 앱 ID|
|adid|<p>Android의 경우 Advertising ID 값 (3.4 Advertising ID 참고)</p><p>iOS의 경우 IdFA (3.2 idFA 참고)</p>|
|uid|사용자 단말기의 IMEI (안드로이드인 경우만)|
|md\_user\_nm|매체사에서 사용하는 사용자 ID 값이 있는 경우 이를 지정하면 이후  적립 URL 호출시 다시 전달해준다.|
|sub\_id|하위 매체의 ID(있는 경우만)|
|wvid|Widevine DRM용 ID값 (안드로이드인 경우만)|
|wvlvl|Widevine security level (L1/L3)|
|ph\_mdl|사용자 단말기의 모델 명|
|ip\_addr|사용자 단말기의 IP 주소|
|ext\_data|추가 데이터가 있는 경우 지정한다. 이후 적립 URL 호출 시 다시 전달해 준다.|
|ext\_mkt|<p>사용자의 단말기에 설치된 통신사 마켓 정보를 전달한다. (구글 또는 앱스토어 정보는 전달할 필요없음)</p><p>- T : 티스토어,  K : 올레마켓, O : LG U+마켓(구 오즈스토어)</p><p>여러개의 마켓이 설치된경우에는 | 기호로 연결하여 전달한다.</p><p>하나의 광고에 여러개의 마켓이 연결될 수 있는데, 이 값을 사용하여 사용자의 단말기에 설치된 마켓에 맞는 마켓 URL을 전달해준다.</p>|

- Return 값
  - 해당 광고의 페이지 또는 마켓으로 이동한다.
  - 내부적으로 중복 참여 방지 등을 체크한 후 해당 광고의 페이지 또는 마켓으로 이동한다.
  - 중복 참여 또는 기타 오류 발생시에는 그 내용을 화면에 표시해준다.
- 호출 예시
```
https://api3.tnkfactory.com/tnk/ad.click.main?pid=e0a08070-b0f1-2359-9532-1f0b07040e04&app_id=105&md_user_nm=00001&adid=38400000-8cf0-11bd-b23e-10b96e40000d&ip_addr=123.123.123.123
```

  2.8. **requestReward (설치형)**

설치형 광고 상품에 대하여 외부 매체가 해당 앱에 대한 설치확인을 수행한 후 설치가 확인되면 이 API를 호출하여 TnkAd 서버에 리워드 지급 요청을 해야한다.

이 API의 호출하면 Tnk 서버는 매체사가 등록한 “외부매체로 등록” URL로 리워드 적립 호출을 하고나서 그 결과를 반환한다.

- Requst URL : 
```
http://api3.tnkfactory.com/tnk/ad.requestreward.main
```
|파라메터 명|내용|
| :-: | :-: |
|pid|<p>TnkAd 사이트에 등록된 외부 매체사의 외부 매체 앱의 App ID</p><p>**(예시 : e0a08070-b0f1-2359-9532-1f0b07040e04)**</p>|
|app\_id|캠페인 중인 광고 앱 ID (향후 별도로 전달)|
|adid|<p>Android의 경우 Advertising ID 값 (3.2 Advertising ID 참고)</p><p>iOS의 경우 IdFA (3.2 idFA 참고)</p>|
|uid|사용자 단말기의 IMEI (안드로이드인 경우만)|
|sub\_id|하위 매체의 ID(있는 경우만)|
|wvid|Widevine DRM용 ID값 (안드로이드인 경우만)|
|wvlvl|Widevine security level (L1/L3)|
|ip\_addr|사용자 단말기의 IP 주소|

- Return 값
  - ret\_cd : 처리 결과 값 (3.1 리턴 코드 참고)

  2.9. **광고 적립 URL 호출**

설치형이나 실행형/액션형 광고 상품 종류와 상관없이 적립이 성공적으로 수행되면 그 결과를 사전에 설정한 “포인트 자체관리 URL” 로 호출해 준다.

- Request Parameter

|파라메터 명|내용|
| :-: | :-: |
|app\_id|캠페인 중인 광고 앱 ID|
|md\_user\_nm|매체사에서 사용하는 사용자 ID (Request Join 시 전달한 경우 이를 다시 반환해 줌)|
|md\_chk|전달된 값이 유효한지 여부를 판단하기 위하여 제공된다. 이 값은 app\_key + md\_user\_nm + seq\_id 의 MD5 Hash 값이다. app\_key 값은 앱 등록시 부여된 값으로 Tnk 사이트에서 확인할 수 있다.|
|ext\_data|RequestJoin 호출시 전달했던 값|
|pay\_pnt|지급 되는 포인트 값이다.|
|pay\_amt|정산되는 금액.|
|seq\_id|포인트 지급에 대한 고유한 ID 값이다. 동일한 리워드 적립이 반복적으로 호출되어도 이 값을 이용하여 중복 지급을 확인할 수 있다.|
|app\_nm|광고앱의 명칭|
|pay\_dt|포인트 지급일시.(포인트 지급 조건이 충족되어 처음으로 포인트 지급을 시도한 시간) – milliseconds (ex - 1580358657348)|
|pay\_cnt|구매형 – 구매수량|
|actn\_id|<p>- 0 : 설치형</p><p>- 1 : 실행형</p><p>- 2 : 액션형</p><p>- 5 : 구매형</p>|

- Return 값
  - 별도 리턴값을 받지 않는다. 다만 HTTP 리턴 코드가 200 인경우에는 정상처리로 간주하고, 그 외에는 비정상처리로 간주한다. 비정상 처리 호출은 Tnk 서버에서 5분 및 1시간 간격으로 최대 24시간 동안 반복적으로 호출된다.
  - 중요! 동일한 request 가 반복적으로 호출될 수 있으므로 seq\_id 값을 사용하시어 반드시 중복체크를 하셔야합니다.
- 서버단 chk\_cd 검증 로직 예시 (Java)

```java
// 해당 사용자에게 지급되는 포인트
int payPoint = Integer.parseInt(request.getParameter("pay_pnt"));
// tnk 내부에서 생성한 고유 번호로 이 거래에 대한 Id이다.
String seqId = request.getParameter("seq_id");
// 전달된 파라메터가 유효한지 여부를 판단하기 위하여 사용한다. (아래 코딩 참고)
String checkCode = request.getParameter("md_chk");

String mdUserName = request.getParameter("md_user_nm");

// 앱 등록시 부여된 app_key (tnk 사이트에서 확인가능)
String appKey = "d2bbd...........19c86c8b021";

// 유효성을 검증하기 위하여 아래와 같이 verifyCode를 생성한다. DigestUtils는 Apache의
// commons-codec.jar 이 필요하다. 다른 md5 해쉬함수가 있다면 그것을 사용해도 무방하다.		
String verifyCode = DigestUtils.md5Hex(appKey + mdUserName + seqId);

// 생성한 verifyCode와 chk_cd 파라메터 값이 일치하지 않으면 잘못된 요청이다.
if (checkCode == null || !checkCode.equals(verifyCode)) {
    // 오류
    log.error("tnkad() check error : " + verifyCode + " != " + checkCode);
} else {
    log.debug("tnkad() : " + mdUserName + ", " + seqId);	
    // 포인트 부여하는 로직수행 (예시)
    purchaseManager.getPointByAd(mdUserName, payPoint, seqId);
}

```


3. **기타**
  3.1. **리턴 코드**

|구분|코드값|내용|
| :-: | :-: | :-: |
|정상|S|정상처리(해당 광고 캠페인이 Live 상태인 경우)|
||T|정상처리(해당 광고 캠페인이 Test 상태인 경우)|
||P1|정상처리(실제 포인트 적립시까지 약간의 시간 소요)|
|오류|E1|등록되지않은 매체 또는 해당 캠페인 진행 권한이 없는 경우|
||E2|중지되었거나 등록되지않은 광고 캠페인|
||E3|잘못된 광고형식 요청(실행형 광고에 requestReward 호출한 경우 등)|
||E4|실패 – 이미 참여하였음|
||E5|실패 – 참여 이력이 없음|
||E6|참여할 수 없는 기기|
||E7|등록되지않은 매체 서버 IP 주소|
||E8|파라메터 오류|
||E9|시스템 오류|

  3.2. **기기 식별값**
- iOS – IdFA (Id for Advertiser)
  - 예시) **D155CEB1-382B-437D-BEC0-B80A4C7AFCF3**
- Android – Advertising ID

`	`2014년 부터 구글에서 제공하는 광고용 식별자이며, 구글에서는 2014년 8월1일부터는 광고 추적을 위해서는 반드시 Advertising ID를 사용해야한다고 약관을 개정하였습니다. (약관보기 : <https://play.google.com/about/developer-content-policy.html#ADID>)

이에 따라 Tnk의 서버 API 호출시 반드시 Advertising ID를 전달해야하며 그렇지 않은 경우 이후 Tnk API의 이용이 안됩니다.

기기의 Advertising ID 값은 설정 > 계정 > Google >  광고 > 내 광고 ID 값으로 확인하실 수 있습니다. (Advertising ID 값은 사용자가 언제든지 변경이 가능하므로 사용자의 abusing 방지를 위하여 사용자 관리를 보다 강화해주시기 바랍니다.)

프로그램 상에서 Advertising ID 값을 획득하기 위해서는 GooglePlay Service API를 사용해야하며 상세 내용은 구글 개발자 사이트 페이지 (http://developer.android.com/google/play-services/id.html)를 참고해주세요.

Tnk의 SDK를 적용하실 경우에는 SDK내에서 Advertising ID값을 획득해주기때문에 위 GooglePlay Service API를 적용하실 필요는 없습니다.

획득하신 Advertising ID는 Tnk의 서버 API 호출시 adid 파라메터로 전달해주시면 됩니다.

호출 예시) [http://api3.tnkfactory.com/tnk/ad.offerlist.main?pid=e0a08070-b0f1-2359-9532-1f0b07040e04](http://api3.tnkfactory.com/tnk/ad.offerlist.main?pid=e0a08070-b0f1-2359-9532-1f0b07040e04&adid=38400000-8cf0-11bd-b23e-10b96e40000d)[&adid=38400000-8cf0-11bd-b23e-10b96e40000d](http://api3.tnkfactory.com/tnk/ad.offerlist.main?pid=e0a08070-b0f1-2359-9532-1f0b07040e04&adid=38400000-8cf0-11bd-b23e-10b96e40000d)

- Widevine ID

`	`Android 10 이후부터 IMEI 식별자를 사용할 수 없게되어 그 대안으로 아래와 같이 Widevine DRM용 ID값을 사용하고자 합니다.

`	`Widevine ID는 별도의 특별한 권한없이 현재 대부분의 기기에서 획득이 가능한 식별값입니다.

`	`기기단위로 동일한 ID 값을 제공하지는 않지만 앱의 패키지 단위로는 항상 같은 값을 제공하기때문에 매체별로는 중복적립을 확인하는데 도움이 될 것으로 판단하고 있습니다.

`	`아래의 코드를 참고하시어 widevine ID 값과 보안레벨을 파라미터로 전달해주시기 바랍니다.

```java
private static fianl UUID WIDEVINE_UUID = new UUID(0xEDEF8BA979D64ACEL, 0xA3C827DCD51D21EDL);
private static fianl String WIDEVINE_SECURITY_LEVEL_1 = "L1";
private static fianl String WIDEVINE_SECURITY_LEVEL_3 = "L3";
private static fianl String SECURITY_LEVEL_PROPERTY = "securityLevel";

// widevine 정보확인
private String[] getWidevineInfo() {

    String widevineId = null;
    String levelInfo = null;

    try {
        MediaDrm mediaDrm = new MediaDrm(WIDEVINE_UUID);
        byte[] uniqueIdArray = mediaDrm.getPropertyByteArray(MediaDrm.PROPERTY_DEVICE_UNIQUE_ID);
        widevineId = Base64.encodeToString(uniqueIdArray, Base64.NO_WRAP).trim();
        levelInfo = mediaDrm.getPropertyString(SECURITY_LEVEL_PROPERTY);

        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.P) {
            mediaDrm.close();
        }
        else {
            mediaDrm.release();
        }
    }  catch (Throwable e) {
    }
    return new String[] {widevineId, levelInfo};
}


```




  3.3. **광고유형 코드**

|코드값|내용|
| :-: | :-: |
|100|설치형|
|101|실행형|
|102|카톡 로그인|
|103|로그인(카톡제외)|
|104|앱 회원가입|
|105|레벨달성|
|106|튜토리얼|
|107|카톡게임 사전예약|
|108|출석형|
|109|클릭시|
|199|액션(기타)|
|200|페북 좋아요|
|201|트위터 팔로우|
|202|인스타 팔로우|
|203|카스 소식받기|
|204|사이트 회원가입|
|205|DB 수집(상담신청)|
|206|유튜브 구독하기|
|207|네이버 카페가입|
|299|SNS(기타)|
|300|동영상(자체)|
|301|유튜브 동영상|
|302|네이버 동영상|
|399|동영상(기타)|
|400|구매형|
|401|예매형|
|402|유료결제|



