# MaihubAPI

## Authorization

모든 api에 Headers 의 Authroization 에 "Basic <id:password base64 encode>" 를 넣어서 보내주시면 됩니다.

** ex) 데모계정 maihub:1234 의 경우 Basic <maihub:1234를 base64 encode> ==> "Basic bWFpaHViOjEyMzQ="

## Upload

**URL** : `https://api.mediwhale.ai/v1/external/maihub/analyse?synchronous=false&report_type=pdf&pdf_type=url&diagnose_type=both`

**Method** : `POST`

**Input Data(form-data)**

```
right_image : 이미지 파일(.jpg,.png,.jpeg,.tif 등) 또는 다이콤파일(.dcm),
left_image : 이미지 파일(.jpg,.png,.jpeg,.tif 등) 또는 다이콤파일(.dcm),
patient_id : 환자번호,
age : 나이(int),
sex : 성별(M or F),
patient_name : 이름
```

**Option(parameter)**

```
synchronous : true or false, 업로드시 result api 결과까지 볼것인지(판독과 레포트 생성에 시간이 걸리기 때문에 false로 하는것을 추천드립니다.),
report_type : pdf or text , text 는 간단한 Positive, Negative 나 저중고 정도의 정보를 전달하기 때문에 maihub 연동시에는 pdf를 넣어주시면 될거 같습니다.
pdf_type : url or base64, pdf 레포트를 url 로 받을 것인지, base64 로 encode된 text를 받을 것인지.
diagnose_type : eye or cvd or both , 안과, 심혈관 레포트 중 하나 또는 둘다 받을 것인지에 대한 option
```

**Success Response**

**Code** : `200 OK`

**Content example(json)**

```json
{
    "key": "20938606",
    // option synchronous=true 시에는 result api response 와 같음.
}
```
**Fail Response**
**Content example(json)**

```json
{
    "success": "Fail",
    "reason" : 이미지가 없는 경우, patient id 가 없는 경우, Credit 이 없는 경우 등
}
```

## Result

**URL** : `https://api.mediwhale.ai/v1/external/maihub/result?key=20938606&report_type=pdf&pdf_type=url&diagnose_type=both`

**Method** : `GET`

**Option(parameter)**

```
synchronous : true or false, 업로드시 result api 결과까지 볼것인지(판독과 레포트 생성에 시간이 걸리기 때문에 false로 하는것을 추천드립니다.),
report_type : pdf or text , text 는 간단한 Positive, Negative 나 저중고 정도의 정보를 전달하기 때문에 maihub 연동시에는 pdf를 넣어주시면 될거 같습니다.
pdf_type : url or base64, pdf 레포트를 url 로 받을 것인지, base64 로 encode된 text를 받을 것인지.
diagnose_type : eye or cvd or both , 안과, 심혈관 레포트 중 하나 또는 둘다 받을 것인지에 대한 option
```
**Success Response**

**Code** : `200 OK`

**Content example(json)**

```json
{
    "cvd": {
        "pdf": "https://drnoon.s3.ap-northeast-2.amazonaws.com/production/reports/fundus/78694123_Reti-CVD_20221105_030322.pdf"
    },
    "eye": {
        "pdf": "https://drnoon.s3.ap-northeast-2.amazonaws.com/production/reports/fundus/78694123_Reti-Eye_20221105_030317.pdf",
        "left_heatmap_url": "https://dtw4kza58cxg1.cloudfront.net/production/processed/2022-11-5/443_030236867779_left_actmap.jpg",
        "right_heatmap_url": "https://dtw4kza58cxg1.cloudfront.net/production/processed/2022-11-5/443_030252125282_right_actmap.jpg"
    },
    "status": "READY"
}
```
**Fail Response**
**Content example(json)**

```json
{
    "success": "Fail",
    "reason" : key를 못찾는 경우, Credit 이 없는 경우 등
}
```

## Update

**URL** : `https://api.mediwhale.ai/v1/external/maihub/update?pdf_type=url&report_type=pdf&diagnose_type=both`

**Method** : `POST`

**Input Data(form-data)**

```
key : 업데이트할 환자 key,
age : 나이(int),
sex : 성별(M or F),
patient_name : 이름
```
업데이트할 항목만 넣어도 됩니다.

**Option(parameter)**

```
synchronous : true or false, 업로드시 result api 결과까지 볼것인지(판독과 레포트 생성에 시간이 걸리기 때문에 false로 하는것을 추천드립니다.),
report_type : pdf or text , text 는 간단한 Positive, Negative 나 저중고 정도의 정보를 전달하기 때문에 maihub 연동시에는 pdf를 넣어주시면 될거 같습니다.
pdf_type : url or base64, pdf 레포트를 url 로 받을 것인지, base64 로 encode된 text를 받을 것인지.
diagnose_type : eye or cvd or both , 안과, 심혈관 레포트 중 하나 또는 둘다 받을 것인지에 대한 option
```

**Success Response**

**Code** : `200 OK`

**Content example(json)**

```json
{
    // result api response 와 같음.
}
```

## Credit

**URL** : `https://api.mediwhale.ai/v1/external/maihub/credit`

**Method** : `GET`

**Content example(json)**

```json
{   
    "eye_limit": 100 (안과질환 credit),
    "cvd_limit": 100 (심혈관질환 credit),
    "expired_at": "20230101"(credit만료),
    "remain_eye": 85 (남아있는 안과질환 credit),
    "remain_cvd": 86 (남아있는 심혈관질환 credit),
   
}
```
