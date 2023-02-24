# MaihubAPI

## Authorization

모든 api에 Headers 의 Authroization 에 "Basic <id:password base64 encode>" 를 넣어서 보내주시면 됩니다.

** ex) 데모계정 maihub:1234 의 경우 Basic <maihub:1234를 base64 encode> ==> "Basic bWFpaHViOjEyMzQ="

** 앞으로 배포 전 테스트가 필요할 때는 https://test.mediwhale.ai 를 사용하겠습니다. 

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
camera_type : 카메라 type (ex) TOPCON_NW400

hospital_id : maihub 측의 병원별 unique id
hospital_name : 병원이름
hospital_address : 병원주소

(optional) : 입력이 가능하시면 input 하시면 됩니다.
hospital_phone : 병원전화번호
hospital_email : 이메일
hospital_distributor_logo_url : 병원로그 이미지 url
```

**Option(parameter)**

```
synchronous : true or false, 업로드시 result api 결과까지 볼것인지(판독과 레포트 생성에 시간이 걸리기 때문에 false로 하는것을 추천드립니다.),
report_type : pdf or text , text , pdf 는 pdf파일만, text 는 간단한 Comment 와 요약 정보, both 는 둘다.
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
        "comment": "YOUR RESULT : HIGH RISK. According to a study, 1 in 5 people in HIGH-RISK group will experience a cardiovascular disease event (like    stroke, heart attack or other circulatory problem) within the next 10 years. A HIGH-RISK score does not indicate that you have a cardiovascular disease nor will you definitely experience a stroke or heart attack. However, this signals that there may be hidden diseases that you are not aware of.",
        "pdf": "https://drnoon.s3.ap-northeast-2.amazonaws.com/production/reports/fundus/78694123_Reti-CVD_20221105_030322.pdf"
    },
    "eye": {
        "left": {
            "abnormal": true,
            "comment": "Retinal abnormalities is detected (aged-related macular degeneration, diabetic retinopathy, etc).\nNo significant glaucoma suspect/media opacity",
            "diseases": {
                "녹내장의증": "normal",
                "망막질환": "abnormal",
                "매체혼탁": "normal"
            }
        },
        "left_heatmap_url": "https://dtw4kza58cxg1.cloudfront.net/production/processed/2022-11-17/443_123527442429_left_actmap.jpg",
        "pdf": "https://drnoon.s3.ap-northeast-2.amazonaws.com/production/reports/fundus/1234_Reti-Eye_20221117_123534.pdf",
        "right": {
            "abnormal": true,
            "comment": "Retinal abnormalities is detected (aged-related macular degeneration, diabetic retinopathy, etc).\nNo significant glaucoma suspect/media opacity",
            "diseases": {
                "녹내장의증": "normal",
                "망막질환": "abnormal",
                "매체혼탁": "normal"
            }
        },
        "right_heatmap_url": "https://dtw4kza58cxg1.cloudfront.net/production/processed/2022-11-17/443_123531980209_right_actmap.jpg"
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
