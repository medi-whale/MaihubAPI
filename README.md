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
```
