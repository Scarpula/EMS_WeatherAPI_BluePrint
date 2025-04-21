FORMAT: 1A
HOST: http://api.weatherplanet.co.kr

# Weather Planet API

## 소개

Weather Planet은 SK테크엑스의 고해상도 기상관측망을 통해 수집된 정보를 바탕으로 날씨정보를 제공하는 서비스입니다.

REST/JSON 기반의 Weather Planet API는 사용자가 손쉽게 기상정보를 사용할 수 있게 해줍니다

Weather Planet API는 날씨정보, 생활환경, 이미지 정보, 세계날씨 등 다양한 종류의 날씨정보를 제공합니다.

## API Limit

API Limit

## 제휴문의


# Group BASIC

## WEATHER [/weather]

### 현재날씨(분별) [GET /weather/current/minutely?version={version}&lat={latitude}&lon={longitude}]

기상청 자동기상관측장비(AWS: Automatic Weather Station)로부터 수집한 기상관측 정보를 분석. 가공하여 1분 단위로 현재날씨 정보를 제공한다.

Request Parameter는 다음중 한 종류만 선택적으로 사용해야 한다.
(1) lat/lon  (2) city/county/village (3) stnid

version 2.5부터 주변날씨정보(nearValue)가 추가로 제공된다.

+ Parameters
    + version: 2.5 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보
    + city: 서울 (string, required) - 주소(시/도)
    + county: 강남구 (string, required) - 주소(시/군/구)
    + village: 삼성동 (string, required) - 주소(읍/면/동)
    + stnid: 108 (number) - 관측소 지점번호

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather
            + minutely
                + station - 관측소 정보
                    + name (string) - 관측소 이름
                    + id (number) - 관측소 지점번호
                    + type (string) - 관측소 유형(KMA:기상청 관측소, -BTN:SKTX 관측소)
                    + latitude (string) - 관측소 위도
                    + longitude (string) - 관측소 경도
                + sky - 하늘상태 정보
                    + code (string) - 하늘상태 코드
                    + name (string) - 하늘상태 코드명
                         SKY_A01:맑음, SKY_A02구름조금, SKY_A03:구름많음,SKY_A04:구름많고 비, SKY_A05:구름많고 눈, SKY_A06:구름많고 비 또는 눈,
                            SKY_A07:흐림, SKY_A08:흐리고 비, SKY_A09:흐리고 눈, SKY_A10:흐리고 비 또는 눈, SKY_A11 : 흐리고 낙뢰
                            SKY_A12:뇌우/비, SKY_A13:뇌우/눈, SKY_A14:뇌우/비 또는 눈
                + precipitation - 강수정보
                    + type (string) - 강수형태 코드
                                - 0:현상없음  - 1:비  -->  rain(sinceOntime) 사용
                                - 2:비/눈     - 3:눈  -->  precipication(sinceOntime) 사용
                    + sinceOntime (string) - 1시간 누적 강수량
                                - if type=0/1/2  --> 강우량(mm)
                                - if type=3      --> 적설량(cm)
                + rain - 강우정보
                    + last10min (string) - 10분 이동누적 강우량
                    + last15min (string) - 15분 이동누적 강우량
                    + last30min (string) - 30분 이동누적 강우량
                    + last1hour (string) - 1시간 이동누적 강우량
                    + last6hour (string) - 6시간 이동누적 강우량
                    + last12hour (string) - 12시간 이동누적 강우량
                    + last24hour (string) - 24시간 이동누적 강우량
                    + sinceOntime (string) - 1시간 누적 강우량
                    + sinceMidnight (string) - 일 누적 강우량
                + temperature - 기온정보
                    + tc (string) - 현재기온
                    + tmax (string) - 오늘의 최고기온
                    + tmin (string) - 오늘의 최저기온
                + wind - 바람정보
                    + wdir (string) - 풍향(degree)
                    + wspd (string) - 풍속(m/s)
                + humidity (string) - 상대습도(%)
                + pressure - 기압정보
                    + surface (string) - 현지기압(Ps)
                    + seaLevel (string) - 해면기압(SLP)
                + ligntning (string) - 낙뢰유무(관측소 5km반경 내)
                               - 0:없음,   - 1:있음
                + timeObservation (string) - 관측시간
                + nearValue - 근접지점 날씨정보(version 2.5이상)
                    + temperature - 기온정보
                        + tc (string) - 현재기온
                        + tmax (string) - 오늘의 최고기온
                        + tmin (string) - 오늘의 최저기온
                    + humidity (string) - 상태습도(%)
                    + sky - 하늘상태 정보
                        + code (string) - 하늘상태 코드
                        + name (string) - 하늘상태 코드명
    + Body
        {
          "weather": {
            "minutely": [
            {
              "station": {
                "longitude": "127.0657778000",
                "latitude": "37.1373888900",
                "name": "남촌",
                "id": "446",
                "type": "KMA"
              },
              "wind": { "wdir": "287.00", "wspd": "2.80" },
              "precipitation": { "sinceOntime": "0.00", "type": "0" },
              "sky": { "code": "SKY_A01", "name": "맑음"},
              "rain": {
                "sinceOntime": "0.00",
                "sinceMidnight": "0.00",
                "last10min": "0.00",
                "last15min": "0.00",
                "last30min": "0.00",
                "last1hour": "0.00",
                "last6hour": "0.00",
                "last12hour": "0.00",
                "last24hour": "0.00"
              },
              "temperature": { "tc": "22.10", "tmax": "24.00", "tmin": "14.00" },
              "humidity": "",
              "pressure": { "surface": "", "seaLevel": ""},
              "lightning": "0",
              "timeObservation": "2017-05-25 16:53:00",
              "nearValue": {
                "temperature":{ "tc": "21.50", "tmax": "25.00", "tmin": "13.00" },
                "humidity": "69.30",
                "sky": { "code": "SKY_A01", "name": "맑음"}
              }
            }
           ]
           },
         "common": { "alertYn": "Y", "stormYn": "N" },
         "result": {
            "code": 9200,
            "requestUrl": "/weather/current/minutely?version=2.5&lat=37.123&lon=127.123",
            "message": "성공"
            }
        }

### 현재날씨(시간별) [GET /weather/current/hourly?version={version}&lat={latitude}&lon={longitude}]

수집된 기상관측 정보를 격자단위로 분석.처리하여 1시간 단위로 현재날씨 정보를 제공한다.

Request Parameter는 다음중 한 종류만 선택적으로 사용해야 한다.
(1) lat/lon  (2) city/county/village 

version 2.5부터 주변날씨정보(nearValue)가 추가로 제공된다.

+ Parameters
    + version: 2.5 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보
    + city: 서울 (string, required) - 주소(시/도)
    + county: 강남구 (string, required) - 주소(시/군/구)
    + village: 삼성동 (string, required) - 주소(읍/면/동)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather
            + hourly
                + grid - 격자 정보
                    + city (string) - 시(특별,광역), 도
                    + county (string) - 시, 군, 구
                    + village (string) - 읍, 면, 동
                    + latitude (string) - 격자중심 위도
                    + longitude (string) - 격자중심 경도
                + sky - 하늘상태 정보
                    + code (string) - 하늘상태 코드
                    + name (string) - 하늘상태 코드명
                         SKY_O01:맑음, SKY_O02구름조금, SKY_O03:구름많음,SKY_O04:구름많고 비, SKY_O05:구름많고 눈, SKY_O06:구름많고 비 또는 눈,
                            SKY_O07:흐림, SKY_O08:흐리고 비, SKY_O09:흐리고 눈, SKY_O10:흐리고 비 또는 눈, SKY_O11 : 흐리고 낙뢰
                            SKY_O12:뇌우/비, SKY_O13:뇌우/눈, SKY_O14:뇌우/비 또는 눈
                + precipitation - 강수정보
                    + type (string) - 강수형태 코드
                                - 0:현상없음,  - 1:비,   - 2:비/눈,   - 3:눈
                    + sinceOntime (string) - 1시간 누적 강수량
                                - if type=0/1/2  --> 강우량(mm)
                                - if type=3      --> 적설량(cm)
                + temperature - 기온정보
                    + tc (string) - 1시간 현재기온
                    + tmax (string) - 오늘의 최고기온
                    + tmin (string) - 오늘의 최저기온
                + wind - 바람정보
                    + wdir (string) - 풍향(degree)
                    + wspd (string) - 풍속(m/s)
                + humidity (string) - 상대습도(%)
                + ligntning (string) - 낙뢰유무(해당격자내)
                               - 0:없음,   - 1:있음
                + sunRiseTime (string) - 일출시간
                + sunSetTime (string) - 일몰시간
                + timeRelease (string) - 발표시간
                + nearValue - 근접지점 날씨정보(version 2.5이상)
                    + temperature - 기온정보
                        + tc (string) - 현재기온
                        + tmax (string) - 오늘의 최고기온
                        + tmin (string) - 오늘의 최저기온
                    + humidity (string) - 상태습도(%)
                    + sky - 하늘상태 정보
                        + code (string) - 하늘상태 코드
                        + name (string) - 하늘상태 코드명
    + Body
        {
              "weather": {
                "hourly": [
                  {
                    "grid": {
                      "longitude": "127.0977600000",
                      "latitude": "37.1177600000",
                      "county": "오산시",
                      "village": "청호동",
                      "city": "경기"
                    },
                    "wind": { "wdir": "288.00", "wspd": "3.50" },
                    "precipitation": { "sinceOntime": "0.00", "type": "0" },
                    "sky": { "code": "SKY_O01", "name": "맑음" },
                    "temperature": { "tc": "19.20", "tmax": "24.00", "tmin": "13.00" },
                    "humidity": "39.00",
                    "lightning": "0",
                    "timeRelease": "2017-05-25 18:00:00",
                    "sunRiseTime": "2017-05-25 05:17:00",
                    "sunSetTime": "2017-05-25 19:40:00"
                    "nearValue": {
                        "temperature":{ "tc": "21.50", "tmax": "25.00", "tmin": "13.00" },
                        "humidity": "69.30",
                        "sky": { "code": "SKY_O01", "name": "맑음"}
                  }
                ]
              },
              "common": { "alertYn": "Y", "stormYn": "N" },
              "result": {
                "code": 9200,
                "requestUrl": "/weather/current/hourly?version=2.5&lat=37.123&lon=127.123",
                "message": "성공"
              }
        }

### 초단기예보 [GET /weather/forecast/3hours?version={version}&lat={latitude}&lon={longitude}]

1시간 간격으로 매일 24회, 5Km 격자 단위로 4시간까지의 초단기예보 정보를 제공한다

Request Parameter는 다음중 한 종류만 선택적으로 사용해야 한다.

(1) lat/lon  (2) city/county/village 

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보
    + city: 서울 (string, required) - 주소(시/도)
    + county: 강남구 (string, required) - 주소(시/군/구)
    + village: 삼성동 (string, required) - 주소(읍/면/동)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather
            + forecast3hours
                + grid - 격자 정보
                    + city (string) - 시(특별,광역), 도
                    + county (string) - 시, 군, 구
                    + village (string) - 읍, 면, 동
                    + latitude (string) - 격자중심 위도
                    + longitude (string) - 격자중심 경도
                + sky - 하늘상태 정보
                    + codeNhour (string) - 하늘상태 코드(발표시간+N시간)
                    + nameNhour (string) - 하늘상태 코드명(발표시간+N시간)
                         SKY_V01:맑음, SKY_V02구름조금, SKY_V03:구름많음,SKY_V04:구름많고 비, SKY_V05:구름많고 눈, SKY_V06:구름많고 비 또는 눈,
                            SKY_V07:흐림, SKY_V08:흐리고 비, SKY_V09:흐리고 눈, SKY_V10:흐리고 비 또는 눈, SKY_V11 : 흐리고 낙뢰
                            SKY_V12:뇌우/비, SKY_V13:뇌우/눈, SKY_V14:뇌우/비 또는 눈
                + precipitation - 강수정보
                    + typeNhour (string) - 강수형태 코드(발표시간+N시간)
                                - 0:현상없음,  - 1:비,   - 2:비/눈,   - 3:눈
                    + sinceOntimeNhour (string) - 1시간 누적 강수량(발표시간+N시간)
                                - if type=0/1/2  --> 강우량(mm)
                                - if type=3      --> 적설량(cm)
                + temperature - 기온정보
                    + tempNhour (string) - 기온(발표시간+N시간)
                + wind - 바람정보
                    + wdirNhour (string) - 풍향(발표시간+N시간)
                    + wspdNhour (string) - 풍속(발표시간+N시간)
                + humidity  - 습도정보
                    + rhNhour (string) - 상대습도(발표시간+N시간)
                + ligntningNhour (string) - 낙뢰확률(발표시간+N시간, 관측소 5km반경 내)
                               - 0:현상없음,   - 1:낮음(30~50%),  - 2:보통(50~70%),  - 3:높음(70%이상)
                + timeRelease (string) - 발표시간

    + Body
        {
  "weather": {
    "forecast3hours": [
      {
        "grid": {  "city": "경기", "county": "오산시",  "longitude": "127.0977600000", "latitude": "37.1177600000", "village": "청호동"  },
        "wind": { "wdir3hour": "314.00",  "wdir1hour": "293.00", "wspd1hour": "3.90",  "wdir2hour": "303.00", "wspd2hour": "3.70",  "wspd3hour": "3.70",  "wdir4hour": "", "wspd4hour": "" },
        "precipitation": {  "sinceOntime3hour": "0.00", "type3hour": "0", "sinceOntime4hour": "",  "type4hour": "", "sinceOntime1hour": "0.00", "type1hour": "0", "sinceOntime2hour": "0.00", "type2hour": "0"  },
        "sky": { "code1hour": "SKY_V01", "name1hour": "맑음", "code2hour": "SKY_V01",  "name2hour": "맑음", "code3hour": "SKY_V01", "name3hour": "맑음",  "code4hour": "",  "name4hour": "" },
        "temperature": { "temp2hour": "16.60", "temp1hour": "18.20", "temp3hour": "15.00", "temp4hour": "" },
        "humidity": { "rh1hour": "39.00", "rh2hour": "39.00", "rh3hour": "40.00", "rh4hour": "" },
        "timeRelease": "2017-05-25 18:30:00",
        "lightning1hour": "0",
        "lightning2hour": "0",
        "lightning3hour": "0",
        "lightning4hour": ""
      }
    ]
  },
  "common": { "alertYn": "Y",    "stormYn": "N" },
  "result": {
    "code": 9200,
    "requestUrl": "/weather/forecast/3hours?version=2&lat=37.123&lon=127.123",
    "message": "성공"
  }
}

### 단기예보 [GET /weather/forecast/3days?version={version}&lat={latitude}&lon={longitude}&foretxt={foretxt}]

3시간 간격으로 매일 8회(2, 5, 8, 11, 14, 17, 20, 23시), 5Km 격자 단위로 단기예보 정보를 제공하며, 예보시간은 발표시간+4시간부터 최대 67시간(3일)까지 제공한다

Request Parameter는 다음중 한 종류만 선택적으로 사용해야 한다.
(1) lat/lon  (2) city/county/village 

Version 2.5부터 일출일몰 시간이 추가로 제공된다.

+ Parameters
    + version: 2.5 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보
    + city: 서울 (string, required) - 주소(시/도)
    + county: 강남구 (string, required) - 주소(시/군/구)
    + village: 삼성동 (string, required) - 주소(읍/면/동)
    + foretxt: Y (string, optional) - 단기예보 기상개황 수신여부(Y:수신, N:미수신)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + forecast3days (object)
                + grid (object) - 격자 정보
                    + city (string) - 시(특별,광역), 도
                    + county (string) - 시, 군, 구
                    + village (string) - 읍, 면, 동
                    + latitude (string) - 격자중심 위도
                    + longitude (string) - 격자중심 경도
                + fsct3hour (object) - 3시간 예보(예보간격:3시간) - 적용요소:하늘상태,강수,기온,풍향/풍속,습도 - 자료개수:15판~22판(발표시간에 따라 판수가 다름)
                  + sky (object) - 하늘상태 정보(N=4,7,10,13,...,64,67)
                        + codeNhour (string) - 하늘상태 코드(발표시간+N시간)
                        + nameNhour (string) - 하늘상태 코드명(발표시간+N시간)
                            SKY_S01:맑음, SKY_S02구름조금, SKY_S03:구름많음,SKY_S04:구름많고 비, SKY_S05:구름많고 눈, SKY_S06:구름많고 비 또는 눈,
                            SKY_S07:흐림, SKY_S08:흐리고 비, SKY_S09:흐리고 눈, SKY_S10:흐리고 비 또는 눈, SKY_S11 : 흐리고 낙뢰
                            SKY_S12:뇌우/비, SKY_S13:뇌우/눈, SKY_S14:뇌우/비 또는 눈
                  + precipitation (object) - 강수정보(N=4,7,10,13,...,64,67)
                        + typeNhour (string) - 강수형태 코드(발표시간+N시간)
                                - 0:현상없음,  - 1:비,   - 2:비/눈,   - 3:눈    
                        + probNhour (string) - 강수확률(%)(발표시간+N시간)
                  + temperature (object) - 기온정보(N=4,7,10,13,...,64,67)
                        + tempNhour (string) - 기온(발표시간+N시간)
                  + wind (object) - 바람정보(N=4,7,10,13,...,64,67)
                        + wdirNhour (string) - 풍향(발표시간+N시간)
                        + wspdNhour (string) - 풍속(발표시간+N시간)
                  + humidity (object) - 습도정보(N=4,7,10,13,...,64,67)
                        + rhNhour (string) - 상대습도(발표시간+N시간)
                + fsct6hour (object) - 6시간 예보(예보간격:6시간)  - 적용요소 강우량/적설량(발표시간+1시간부터 다음판까지 강수량)   - 자료개수:8판~11판(발표시간에 따라 판수가 다름)
                    + rainMhour (string) - 6시간 누적 강우량(발표시간+M시간 / M=6,12,18,24,30,36,42,48,54,60,66)
                            - Value는 아래 문자열로 내려옴
                            - 없음, 1mm미만, 1~4mm, 5~9mm, 10~19mm, 20~39mm, 40~69mm, 70mm이상
                    + snowMhour (string) - 6시간 신 적설량(발표시간+M시간 / M=6,12,18,24,30,36,42,48,54,60,66)
                            - Value는 아래 문자열로 내려옴
                            - 없음, 1cm미만, 1~4cm, 5~9cm, 10~19cm, 20cm이상
                + fcstdaily (object) - 24시간 예보(예보간견:24시간)  - 적용요소:최고/최저기온  - 자료개수:2판(최저기온), 2~3판(최고기온)(발표시간에 따라 판수가 다름)
                    + temperature (object) - 기온정보(X=1,2,3)
                        + tmaxXday (string) - 일 최고기온(오늘,내일,모레)
                        + tminXday (string) - 일 최저기온(오늘,내일,모레)
                + fcstext (object) - 단기예보 개황(전국)
                    + locationName (string) - 기준지역명(전국)
                    + text1 (string) - 기상개황(오늘)
                    + text2 (string) - 기상개황(내일)
                    + text3 (string) - 기상개항(모레)
                    + wn (string) - 특보발표현황
                    + wr (string) - 예비특보현황
                    + timeRelease (string) -  발표시간
                + fcstextRegion (object) - 단기예보 개황(요청지역)
                    + locationName (string) - 기준지역명(요청지역)
                    + text1 (string) - 기상개황(오늘)
                    + text2 (string) - 기상개황(내일)
                    + text3 (string) - 기상개항(모레)
                    + wn (string) - 특보발표현황
                    + wr (string) - 예비특보현황
                    + timeRelease (string) -  발표시간
                + sun (object) - 일출일몰정보(X=1,2,3)
                    + sunriseXday (string) - 일출시간(오늘,내일,모레)
                    + sunsetXday (string) - 일몰시간(오늘,내일,모레)
                + timeRelease (string) - 발표시간

    + Body
        {
      "weather": {
        "forecast3days": [
          {
            "grid": { "city": "경기", "county": "오산시", "longitude": "127.0977600000", "latitude": "37.1177600000", "village": "청호동" },
            "timeRelease": "2017-05-29 08:00:00",
            "fcst3hour": {
              "wind": {
                "wdir4hour": "248.00", "wspd4hour": "1.60", "wdir7hour": "252.00", "wspd7hour": "2.50", "wdir10hour": "243.00", "wspd10hour": "3.30",
                "wdir13hour": "254.00", "wspd13hour": "2.20", "wdir16hour": "162.00", "wspd16hour": "0.90", "wdir19hour": "196.00", "wspd19hour": "0.70",
                "wdir22hour": "135.00", "wspd22hour": "1.10", "wdir25hour": "184.00", "wspd25hour": "1.40", "wdir28hour": "218.00", "wspd28hour": "1.80",
                "wdir31hour": "238.00", "wspd31hour": "2.20", "wdir34hour": "231.00", "wspd34hour": "2.60", "wdir37hour": "202.00", "wspd37hour": "2.40",
                "wdir40hour": "337.00", "wspd40hour": "0.80", "wdir43hour": "318.00", "wspd43hour": "1.20", "wdir46hour": "34.00", "wspd46hour": "0.70",
                "wdir49hour": "205.00", "wspd49hour": "1.40", "wdir52hour": "211.00", "wspd52hour": "2.30", "wdir55hour": "255.00", "wspd55hour": "2.80",
                "wdir58hour": "293.00", "wspd58hour": "2.30", "wdir61hour": "301.00", "wspd61hour": "1.70", "wdir64hour": "270.00", "wspd64hour": "0.70",
                "wdir67hour": "", "wspd67hour": "" },
              "precipitation": {
                "type4hour": "0", "prob4hour": "0.00", "type7hour": "0", "prob7hour": "0.00", "type10hour": "0", "prob10hour": "0.00", "type13hour": "0", "prob13hour": "0.00",
                "type16hour": "0", "prob16hour": "0.00", "type19hour": "0", "prob19hour": "10.00", "type22hour": "0", "prob22hour": "20.00", "type25hour": "0", "prob25hour": "20.00",
                "type28hour": "0", "prob28hour": "20.00", "type31hour": "0", "prob31hour": "20.00", "type34hour": "0", "prob34hour": "20.00", "type37hour": "0", "prob37hour": "20.00",
                "type40hour": "0", "prob40hour": "20.00", "type43hour": "0", "prob43hour": "20.00", "type46hour": "0", "prob46hour": "20.00", "type49hour": "0", "prob49hour": "20.00",
                "type52hour": "0", "prob52hour": "30.00", "type55hour": "0", "prob55hour": "30.00", "type58hour": "1", "prob58hour": "60.00", "type61hour": "0", "prob61hour": "30.00",
                "type64hour": "0", "prob64hour": "30.00", "type67hour": "", "prob67hour": "" },
              "sky": {
                "code4hour": "SKY_S01", "name4hour": "맑음", "code7hour": "SKY_S01", "name7hour": "맑음", "code10hour": "SKY_S01", "name10hour": "맑음", "code13hour": "SKY_S01", "name13hour": "맑음",
                "code16hour": "SKY_S01", "name16hour": "맑음", "code19hour": "SKY_S02", "name19hour": "구름조금", "code22hour": "SKY_S03", "name22hour": "구름많음", "code25hour": "SKY_S03", "name25hour": "구름많음",
                "code28hour": "SKY_S03", "name28hour": "구름많음", "code31hour": "SKY_S03", "name31hour": "구름많음", "code34hour": "SKY_S03", "name34hour": "구름많음", "code37hour": "SKY_S03", "name37hour": "구름많음",
                "code40hour": "SKY_S03", "name40hour": "구름많음", "code43hour": "SKY_S03", "name43hour": "구름많음", "code46hour": "SKY_S03", "name46hour": "구름많음", "code49hour": "SKY_S03", "name49hour": "구름많음",
                "code52hour": "SKY_S07", "name52hour": "흐림", "code55hour": "SKY_S07", "name55hour": "흐림", "code58hour": "SKY_S08", "name58hour": "흐리고 비", "code61hour": "SKY_S07", "name61hour": "흐림",
                "code64hour": "SKY_S07", "name64hour": "흐림", "code67hour": "", "name67hour": "" },
              "temperature": {
                "temp4hour": "27.00", "temp7hour": "29.00", "temp10hour": "27.00", "temp13hour": "22.00", "temp16hour": "19.00", "temp19hour": "17.00", "temp22hour": "17.00",
                "temp25hour": "22.00", "temp28hour": "26.00", "temp31hour": "28.00", "temp34hour": "26.00", "temp37hour": "22.00", "temp40hour": "19.00", "temp43hour": "17.00",
                "temp46hour": "16.00", "temp49hour": "21.00", "temp52hour": "25.00", "temp55hour": "24.00", "temp58hour": "22.00", "temp61hour": "20.00", "temp64hour": "18.00", "temp67hour": "" },
              "humidity": {
                "rh4hour": "35.00", "rh7hour": "35.00", "rh10hour": "35.00", "rh13hour": "45.00", "rh16hour": "60.00", "rh19hour": "65.00", "rh22hour": "60.00", "rh25hour": "40.00", "rh28hour": "35.00",
                "rh31hour": "25.00", "rh34hour": "25.00", "rh37hour": "35.00", "rh40hour": "45.00", "rh43hour": "55.00", "rh46hour": "65.00", "rh49hour": "55.00", "rh52hour": "50.00", "rh55hour": "70.00",
                "rh58hour": "80.00", "rh61hour": "75.00", "rh64hour": "75.00", "rh67hour": "" }
            },
            "fcstdaily": {
              "temperature": { "tmax1day": "30.00", "tmax2day": "29.00", "tmax3day": "25.00", "tmin1day": "", "tmin2day": "16.00", "tmin3day": "16.00" }
            },
            "fcst6hour": {
              "rain6hour": "없음", "rain12hour": "없음", "rain18hour": "없음", "rain24hour": "없음", "rain30hour": "없음", "rain36hour": "없음", "rain42hour": "없음", "rain48hour": "없음", "rain54hour": "없음", 
              "snow6hour": "없음", "snow12hour": "없음", "snow18hour": "없음", "snow24hour": "없음", "snow30hour": "없음", "snow36hour": "없음", "snow42hour": "없음", "snow48hour": "없음", "snow54hour": "없음",
              "rain60hour": "1~4mm","rain66hour": "없음", "snow60hour": "없음", "snow66hour": "없음" },
            "sun": {
              "sunrise1day": "2017-05-29 06:28:00", "sunrise2day": "2017-05-30 06:29:00", "sunrise3day": "2017-05-31 06:30:00",
              "sunset1day": "2017-05-29 18:14:00", "sunset2day": "2017-05-30 18:12:00", "sunset3day": "2017-05-31 18:11:00"
            }
          }
        ]
      },
      "common": { "alertYn": "Y", "stormYn": "N" },
      "result": { "code": 9200, "requestUrl": "/weather/forecast/3days?version=2.5&lat=37.123&lon=127.123", "message": "성공" }
    }

### 중기예보 [GET /weather/forecast/6days?version={version}&lat={latitude}&lon={longitude}&foretxt={foretxt}]

12시간 간격으로 매일 2회(6시, 18시), 주요 시도 단위로 중기예보 정보를 제공하며, 예보시간은 일단위로 +3일부터 +10일까지 제공한다.

Request Parameter는 다음중 한 종류만 선택적으로 사용해야 한다.
(1) lat/lon  (2) city/county/village 

Version 2.5부터 일출일몰 시간이 추가로 제공된다.

+ Parameters
    + version: 2.5 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보
    + city: 서울 (string, required) - 주소(시/도)
    + county: 강남구 (string, required) - 주소(시/군/구)
    + village: 삼성동 (string, required) - 주소(읍/면/동)
    + foretxt: Y (string, optional) - 중기예보 기상개황 수신여부(Y:수신, N:미수신)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + forecast6days (object)
                + grid (object) - 격자 정보
                    + city (string) - 시(특별,광역), 도
                    + county (string) - 시, 군, 구
                    + village (string) - 읍, 면, 동
                + location (object) = 중기예보 기준지역
                    + name (string) - 기준지역명
                + sky (object) - 하늘상태 정보(N=3,4,5,6,7,8,9,10)
                    + amCodeNday (string) - 하늘상태코드(오전)(발표시간+N일)
                    + amNameNday (string) - 하늘상태코드명(오전)(발표시간+N일)
                            SKY_W01:맑음, SKY_W02구름조금, SKY_W03:구름많음,SKY_W04:흐림, SKY_W07: 흐리고 비,
                            SKY_W09:구름많고 비, SKY_W10:소나기, SKY_W11:비 또는 눈, SKY_W12:구름많고 눈, SKY_W13:흐리고 눈
                    + pmCodeNday (string) - 하늘상태코드(오후)(발표시간+N일)
                    + pmNameNday (string) - 하늘상태코드명(오흐)(발표시간+N일)
                + temperature (object) - 기온정보(N=3,4,5,6,7,8,9,10)
                    + tmaxNday (string) - 일 최고기온(발표시간+N일)
                    + tminNday (string) - 일 최저기온(발표시간+N일)
                + fcstext (object) - 중기예보 개황(전국)
                    + locationName (string) - 기준지역명(전국)
                    + text (string) - 기상개황(주간)
                    + timeRelease (string) -  발표시간
                + fcstextRegion (object) - 중기예보 개황(요청지역)
                    + locationName (string) - 기준지역명(요청지역)
                    + text (string) - 기상개황(주간)
                    + timeRelease (string) -  발표시간
                + sun (object) - 일출일몰 시간(N=3,4,5,6,7,8,9,10)
                    + sunriseNday (string) - 일출시간(발표시간+N일)
                    + sunsetNday (string) -  일몰시간(발표시간+N일)
                + timeRelease (string) - 발표시간

    + Body
        {
      "weather": {
        "forecast6days": [
          {
            "grid": { "county": "용인시 처인구", "village": "남사면", "city": "경기" },
            "sky": {
              "amCode2day": "SKY_W03", "amName2day": "구름많음", "pmCode2day": "SKY_W03", "pmName2day": "구름많음", 
              "amCode3day": "SKY_W03", "amName3day": "구름많음", "pmCode3day": "SKY_W03", "pmName3day": "구름많음",
              "amCode4day": "SKY_W02", "amName4day": "구름조금", "pmCode4day": "SKY_W02", "pmName4day": "구름조금",
              "amCode5day": "SKY_W02", "amName5day": "구름조금", "pmCode5day": "SKY_W02", "pmName5day": "구름조금",
              "amCode6day": "SKY_W01", "amName6day": "맑음", "pmCode6day": "SKY_W01", "pmName6day": "맑음",
              "amCode7day": "SKY_W02", "amName7day": "구름조금", "pmCode7day": "SKY_W02", "pmName7day": "구름조금",
              "amCode8day": "SKY_W03", "amName8day": "구름많음", "pmCode8day": "SKY_W03", "pmName8day": "구름많음",
              "amCode9day": "SKY_W03", "amName9day": "구름많음", "pmCode9day": "SKY_W03", "pmName9day": "구름많음",
              "amCode10day": "SKY_W02", "amName10day": "구름조금", "pmCode10day": "SKY_W02", "pmName10day": "구름조금"
            },
            "temperature": {
              "tmax2day": "24", "tmax3day": "24", "tmin2day": "17", "tmin3day": "17", "tmax4day": "25", "tmax5day": "24",
              "tmax6day": "26", "tmax7day": "26", "tmin4day": "12", "tmin5day": "14", "tmin6day": "13", "tmin7day": "14",
              "tmax8day": "24", "tmax9day": "26", "tmax10day": "29", "tmin8day": "15", "tmin9day": "13", "tmin10day": "14"
            },
            "timeRelease": "2017-05-29 18:00:00",
            "fcstextRegion": {
              "text": "고기압의 가장자리에 들어 가끔 구름이 많겠습니다.\n기온은 평년(최저기온 : 13~17도, 최고기온 : 24~27도)과 비슷하겠습니다. \n강수량은 평년(2~4mm)보다 적겠습니다.\n서해중부해상의 물결은 0.5~2.0m로 일겠습니다.",
              "timeRelease": "2017-05-29 18:00:00",
              "locationName": "서울.경기도"
            },
            "fcstext": {
              "text": "기압골의 영향으로 6월 1일은 경상도에, 6일은 전남과 경남, 제주도에 비가 오겠습니다. 또한, 동풍의 영향으로 6월 1일은 강원영동에도 비가 오겠습니다. \n그 밖의 날은 고기압 가장자리에 들어 가끔 구름이 많겠습니다.\n기온은 평년(최저기온: 11~18도, 최고기온: 22~29도)과 비슷하겠습니다.\n강수량은 평년(2~8mm)보다 적겠으나, 남부지방과 제주도, 강원영동은 비슷하겠습니다.",
              "timeRelease": "2017-05-29 18:00:00",
              "locationName": "전국"
            },
            "sun": {
                "sunrise2day":"2017-05-31 06:29:00","sunrise3day":"2017-06-01 06:30:00","sunset2day":"2017-05-31 18:12:00","sunset3day":"2017-06-01 18:11:00",
                "sunrise4day":"2017-06-02 06:31:00","sunrise5day":"2017-06-03 06:32:00","sunrise6day":"2017-06-04 06:33:00","sunrise7day":"2017-06-05 06:33:00",
                "sunrise8day":"2017-06-06 06:34:00","sunrise9day":"2017-06-07 06:35:00","sunrise10day":"2017-06-08 06:36:00","sunset4day":"2017-06-02 18:09:00",
                "sunset5day":"2017-06-03 18:08:00","sunset6day":"2017-06-04 18:07:00","sunset7day":"2017-06-05 18:05:00","sunset8day":"2017-06-06 18:04:00",
                "sunset9day":"2017-06-07 18:02:00","sunset10day":"2017-06-08 18:01:00"            
            },
            "location": { "name": "용인" }
          }
        ]
      },
      "common": { "alertYn": "Y", "stormYn": "N" },
      "result": { "code": 9200, "requestUrl": "/weather/forecast/6days?version=2.5&lat=37.123&lon=127.123&foretxt=Y", "message": "성공" }
    }

### 기상특보 [GET /weather/severe/alert?version={version}&lat={latitude}&lon={longitude}]

강풍, 호우, 한파, 건조, 해일, 풍랑, 태풍, 대설, 황사, 폭염 등의 특보 발생시에 200여개의 특보구역에 대해 기상특보를 제공한다. 또한, 동일 특보구역에 복수개의 기상특보가 존재할 경우에는 발생한 특보의 개수만큼을 응답한다.

Request Parameter는 다음중 한 종류만 선택적으로 사용해야 한다.

(1) lat/lon  (2) city/county/village 

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보
    + city: 서울 (string, required) - 주소(시/도)
    + county: 강남구 (string, required) - 주소(시/군/구)
    + village: 삼성동 (string, required) - 주소(읍/면/동)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + alert (object)
                + grid (object) - 격자 정보
                    + city (string) - 시(특별,광역), 도
                    + county (string) - 시, 군, 구
                    + village (string) - 읍, 면, 동
                + stationId (string) - 발표관서 지점번호
                + timeRelease (string) - 발표시간
                + number (string) - 발표번호(월별)
                + areaCode (string) - 특보구역코드
                + areaName (string) - 특보구역코드명
                + alert51 (object) = 기상특보(코드)
                    + cmdCode (string) - 특보발표코드
                    + cmdName (string) - 특보발표코드명
                            - 1:발표 / 2:해제 / 3:연장 / 4:대치에 의한 해제 / 5:대치에 의한 발표 / 6:정정
                    + varCode (string) - 특보종류코드
                    + varName (string) - 특보종류코드명
                            - 1:강품 / 2:호우 / 3:한파 / 4:건조 / 5:해일 / 6:풍랑 / 7:태풍 / 8:대설 / 9:황사 / 12:폭염
                    + stressCode (string) - 특보강도코드(0,1)
                    + stressName (string) - 특보강도코드명(0:주의보, 1:경보)
                + alert60 (object) - 기상특보(서술)
                    + t1 (string) - 제목
                    + t2 (string) - 해당구역
                    + t3 (string) - 발효시간
                    + t4 (string) - 내용
                    + t5 (string) - 특보발효현황 시간
                    + t6 (string) - 특보발효현황 내용
                    + t7 (string) - 예비특보발효 현황
                    + other (string) - 참고사항
                + timeStart (string) - 발효시간
                + timeEnd (string) - 발효해제시간
                + timeAllEnd (string) - 전체특보해제시간

    + Body
        {
            "result": { "message":"성공", "code":9200, "requestUrl":"/weather/severe/alert?version=2&lat=33.514&lon=126.5297" },
            "common": { "alertYn":"Y", "stormYn":"N" },
            "weather":
            {
                "alert": [
                {
                    "grid": { "city":"제주", "county":"제주시", "village":"건입동" },
                    "number":"1",
                    "timeRelease":"2013-10-01 19:30:00",
                    "stationId":"108",
                    "areaCode":"L1090500",
                    "areaName":"제주도산간",
                    "timeStart":"2013-10-01 19:30:00",
                    "timeEnd":"",
                    "timeAllEnd":"",
                    "alert51": {
                        "cmdCode":"1",
                        "cmdName":"발표",
                        "varCode":"2",
                        "varName":"호우",
                        "stressCode":"0",
                        "stressName":"주의보"
                    },
                    "alert60": {
                        "t1":"호우주의보 발표",
                        "t2":"(1) 호우주의보 발표 : 제주도(제주도산간)",
                        "t3":"(1) 호우주의보 발표 : 2013년 10월 01일 19시 30분",
                        "t4":"(1) 호우주의보 발표\r\n  o 현재 강수량(1일 9시 ~ 현재): 10~110mm\n  o 예상 강수량(현재~2일 오전까지): 10~30mm\n  o 총 예상 강수량: 20~140mm",
                        "t5":"2013-10-01 19:30:00",
                        "t6":"o 호우주의보 : 제주도(제주도산간)",
                        "t7":"(1) 강풍 예비특보\r\no 10월 02일 낮 : 서해5도\r\no 10월 02일 밤 : 흑산도.홍도\r\n(2) 풍랑 예비특보\r\no 10월 02일 낮 : 서해중부먼바다\r\no 10월 02일 밤 : 서해남부먼바다, 제주도남쪽먼바다",
                        "other":"o 없음"
                    }
                }
                ]
           }
        }

### 태풍정보 [GET /weather/severe/storm?version={version}&isThatAll={isThatAll}]

태풍의 현 위치 및 예상위치를 지도상에 표시할 수 있도록 태풍예보에 대한 상세 정보를 제공한다. 복수개의 태풍이 존재할 경우에는 발생한 태풍의 개수만큼을 응답한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + isThatAll: N (string, optional) - 태풍검색 범위 ( N:경계구역내 태풍, Y:모든구역의 태풍)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + storm (object) - 태풍정보 (태풍 N개 응답)
                + number (string) - 태풍번호(기본정보)
                + timeRelease (string) - 발표시간(기본정보)
                + nameKor (string) - 태풍이름 한글(기본정보)
                + nameEng (string) - 태풍이름 영어(기본정보)
                + influenceYn (string) - 경계구역내 존재여부 ( Y:경계구역내, N:경계구역외 )
                + status (object) - 현재상태
                    + time (string) - 현재위치 시간
                    + lat (string) - 위도(deg)(현재위치)
                    + lon (string) - 경도(deg)(현재위치)
                    + loc (string) - 현재위치 내용
                    + dir (string) - 방향(16방위)
                    + spd (string) - 이동속도(km/h)
                    + ps (string) - 중심기압(hPa)
                    + ws (string) - 최대풍속(m/s)
                    + level (string) - 태풍등급
                    + r25 (string) - 25m/s 반경(km)
                    + ed25 (string) - 25m/s 예외방향(16방위)
                    + er25 (string) - 25m/s 예외반경(km)
                    + r15 (string) - 15m/s 반경(km)
                    + ed15 (string) - 15m/s 예외방향(16방위)
                    + er15 (string) - 15m/s 예외반경(km)
                + course (object) - 예상경로(과거경로포함, N개)
                    + time (string) - 예상위치 시간
                    + lat (string) - 위도(deg)(예상위치)
                    + lon (string) - 경도(deg)(예상위치)
                    + loc (string) - 에상위치 내용
                    + dir (string) - 진행방향(16방위)
                    + spd (string) - 이동속도(km/h)
                    + ps (string) - 중심기압(hPa)
                    + ws (string) - 최대풍속(m/s)
                    + r70p (string) - 70% 확률반경(km)
                    + level (string) - 태풍등급
                    + r15 (string) - 15m/s 반경(km)
                    + ed15 (string) - 15m/s 예외방향(16방위)
                    + er15 (string) - 15m/s 예외반경(km)

    + Body
        {
            "result": { "message":"성공", "code":9200, "requestUrl":"/weather/severe/storm?isThatAll=Y&version=2" },
            "common": { "alertYn":"Y", "stormYn":"N" },
            "weather": {
                "storm": [
                    {   "number":"30",
                        "timeRelease":"2013-11-07 16:00:00",
                        "nameKor":"하이옌",
                        "nameEng":"HAIYAN",
                        "influenceYn":"N",
                        "status":
                        {   "time":"2013-11-07 15:00:00",   "level":"TY",   "lat":"9.30", "lon":"131.10",
                            "loc":"필리핀 마닐라 동남동쪽 약 1250 km 부근 해상",
                            "dir":"WNW", "spd":"33.00", "ps":"900.00", "ws":"59.00", "r25":"150.00", "ed25":"", "er25":"",
                            "r15":"320.00", "ed15":"", "er15":""  },
                        "course": [
                        {   "time":"2013-11-04 09:00:00", "lat":"6.00", "lon":"152.10",
                            "loc":"괌 남동쪽 약 1150 km 부근 해상",
                            "dir":"W", "spd":"22.00", "ps":"1000.00", "ws":"18.00", "r15":"150.00", "ed15":"",  "er15":"",  "r70p":"" },
                        {   "time":"2013-11-07 15:00:00", "lat":"9.30", "lon":"131.10",  
                            "loc":"필리핀 마닐라 동남동쪽 약 1250 km 부근 해상",
                            "dir":"WNW", "spd":"33.00", "ps":"900.00", "ws":"59.00", "r15":"320.00", "ed15":"", "er15":"", "r70p":"" },
                        {   "time":"2013-11-07 09:00:00", "lat":"8.70", "lon":"132.80",
                            "loc":"필리핀 마닐라 동남동쪽 약 1440 km 부근 해상",
                            "dir":"WNW", "spd":"31.00", "ps":"900.00", "ws":"59.00", "r15":"330.00", "ed15":"", "er15":"", "r70p":"" },
                        {   "time":"2013-11-07 03:00:00", "lat":"8.20", "lon":"134.40",
                            "loc":"필리핀 마닐라 동남동쪽 약 1630 km 부근 해상",
                            "dir":"W", "spd":"33.00", "ps":"905.00", "ws":"57.00", "r15":"300.00", "ed15":"", "er15":"", "r70p":"" },
                        {   "time":"2013-11-06 21:00:00", "lat":"8.00", "lon":"136.20",
                            "loc":"괌 서남서쪽 약 1120 km 부근 해상", 
                            "dir":"WNW", "spd":"32.00", "ps":"915.00", "ws":"54.00", "r15":"300.00", "ed15":"", "er15":"", "r70p":"" },
                        { "time":"2013-11-06 15:00:00", "lat":"7.60", "lon":"137.90",
                            "loc":"괌 남서쪽 약 1000 km 부근 해상",
                            "dir":"W", "spd":"33.00", "ps":"930.00", "ws":"50.00", "r15":"320.00", "ed15":"", "er15":"", "r70p":"" },
                        {   "time":"2013-11-06 09:00:00", "lat":"7.40", "lon":"139.70",
                            "loc":"괌 남서쪽 약 870 km 부근 해상",
                            "dir":"W", "spd":"30.00", "ps":"945.00", "ws":"45.00", "r15":"300.00", "ed15":"", "er15":"", "r70p":"" },
                        {   "time":"2013-11-06 03:00:00", "lat":"7.10", "lon":"141.30",
                            "loc":"괌 남남서쪽 약 800 km 부근 해상",
                            "dir":"W", "spd":"34.00", "ps":"955.00", "ws":"41.00", "r15":"300.00", "ed15":"", "er15":"", "r70p":"" },
                        {   "time":"2013-11-05 21:00:00", "lat":"6.80", "lon":"143.10",
                            "loc":"괌 남남서쪽 약 760 km 부근 해상",
                            "dir":"W", "spd":"30.00", "ps":"975.00", "ws":"34.00", "r15":"240.00",  "ed15":"", "er15":"", "r70p":"" },
                        {   "time":"2013-11-04 15:00:00", "lat":"6.30", "lon":"150.40",
                            "loc":"괌 남동쪽 약 1010 km 부근 해상",
                            "dir":"W", "spd":"32.00", "ps":"998.00", "ws":"18.00", "r15":"160.00", "ed15":"", "er15":"", "r70p":"" },
                        {   "time":"2013-11-04 21:00:00", "lat":"6.30", "lon":"148.70",
                            "loc":"괌 남남동쪽 약 910 km 부근 해상",
                            "dir":"W", "spd":"31.00", "ps":"996.00", "ws":"19.00", "r15":"170.00", "ed15":"", "er15":"", "r70p":"" },
                        {   "time":"2013-11-05 03:00:00", "lat":"6.50", "lon":"147.70",
                            "loc":"괌 남남동쪽 약 840 km 부근 해상",
                            "dir":"WNW", "spd":"19.00", "ps":"990.00", "ws":"24.00", "r15":"200.00", "ed15":"", "er15":"", "r70p":"" },
                        {   "time":"2013-11-05 09:00:00", "lat":"6.50", "lon":"146.30",
                            "loc":"괌 남남동쪽 약 790 km 부근 해상",
                            "dir":"W", "spd":"26.00", "ps":"985.00", "ws":"27.00", "r15":"200.00", "ed15":"", "er15":"", "r70p":"" },
                        {   "time":"2013-11-05 15:00:00", "lat":"6.50", "lon":"144.70",
                            "loc":"괌 남쪽 약 780 km 부근 해상",
                            "dir":"W", "spd":"29.00", "ps":"975.00", "ws":"34.00", "r15":"240.00", "ed15":"", "er15":"", "r70p":"" },
                        {   "time":"2013-11-08 15:00:00", "lat":"11.60", "lon":"123.90",
                            "loc":"필리핀 마닐라 남동쪽 약 460 km 부근 해상",
                            "dir":"WNW", "spd":"34.00", "ps":"910.00", "ws":"56.00", "r15":"310.00", "ed15":"S", "er15":"280.00", "r70p":"140.00" },
                        {   "time":"2013-11-09 15:00:00", "lat":"13.80", "lon":"115.80",
                            "loc":"필리핀 마닐라 서쪽 약 570 km 부근 해상",
                            "dir":"WNW", "spd":"38.00", "ps":"945.00", "ws":"45.00", "r15":"300.00", "ed15":"", "er15":"", "r70p":"230.00" },
                        {   "time":"2013-11-10 15:00:00", "lat":"16.10", "lon":"109.50",
                            "loc":"베트남 하노이 남동쪽 약 670 km 부근 해상",
                            "dir":"WNW", "spd":"30.00", "ps":"960.00", "ws":"40.00", "r15":"270.00", "ed15":"SW", "er15":"240.00", "r70p":"320.00" },
                        {   "time":"2013-11-11 15:00:00", "lat":"17.80", "lon":"105.60", "loc":"", "dir":"WNW",
                            "spd":"19.00", "ps":"990.00", "ws":"24.00", "r15":"200.00", "ed15":"SW", "er15":"150.00", "r70p":"460.00" },
                        {   "time":"2013-11-12 15:00:00", "lat":"21.00", "lon":"101.90", "loc":"", "dir":"NW",
                            "spd":"22.00", "ps":"1000.00", "ws":"", "r15":"", "ed15":"", "er15":"", "r70p":"" }
                        ]
                    }
                ]
            }
        }

### 낙뢰정보 [GET /weather/severe/lightning?version={version}&lat={latitude}&lon={longitude}&radius={radius}]

낙뢰 지점을 지도상에 표시할 수 있도록 10분 주기로 수집되는 낙뢰 상세 정보를 제공한다. 복수개의 낙뢰가 존재할 경우에는 발생한 낙뢰의 개수만큼을 응답한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 37.1234 (string, optional) - 위도
    + longitude: 126.1234 (string, optional) - 경도
    + radius: 5 (int, optional) - 반경(km)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + lightning (object) - 낙뢰정보 (낙뢰 N개 응답)
                + location (object) - 낙뢰위치
                    + lat (string) - 위도
                    + lon (string) - 경도
                + type (string) - 낙뢰유형 ( C:Cloud-to-Cloud, G:Cloud-to-Ground )
                + stroke (string) - 낙뢰강도(kA)
                + mmp (string) - 다중성(Maximum multiplicity)
                + ce (object) - 확률타원
                    + ce1 (string) - 확률타원의 장축(km)
                    + ce2 (string) - 확률타원의 단축(km)
                    + cee (string) - 확률타원의 이심율(Eccentricity)
                    + cea (string) - 확률타원의 기울기(deg)
                + chi (string) - 낙뢰강도의 카이제곱(chi-square)
                + timeObjervation (string) - 관측시간

    + Body
        {
            "result": { "message":"성공", "code":9200, "requestUrl":"/weather/severe/lightning?version=2" },
            "common": { "alertYn":"Y", "stormYn":"N" },
            "weather": {
                "lightning": [
                    {
                        "location": { "lat":"36.1126", "lon":"129.6439" },
                        "type":"C",
                        "timeObservation":"2013-11-07 18:57:39",
                        "ce": { "ce1":"28.60", "ce2":"1.60", "cee":"17.90", "cea":"82.00" },
                        "stroke":"9.20",
                        "mmp":"0.00",
                        "chi":"0.60"
                    }
                ]
            }
        }

### 간편날씨 [GET /weather/summary?version={version}&lat={latitude}&lon={longitude}&stnid={stnid}]

어제날씨와 더불어 오늘/내일/모레날씨 정보를 간략하게 제공한다.
어제날씨는 하늘상태/최고,최저기온/일누적 강수량 정보를 제공하고 오늘/내일/모레날씨는 하늘상태/최고,최저기온 정보를 제공한다.

Request Parameter는 다음중 한 종류만 선택적으로 사용해야 한다.

(1) lat/lon  (2) stnid 

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 37.1234 (string, required) - 위도
    + longitude: 126.1234 (string, required) - 경도
    + stnid: 108 (int, required) - 관측소 지점번호(stnid)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + summary
                + grid - 격자 정보
                    + city (string) - 시(특별,광역), 도
                    + county (string) - 시, 군, 구
                    + village (string) - 읍, 면, 동
                    + latitude (string) - 격자중심 위도
                    + longitude (string) - 격자중심 경도
                + yesterday - 어제날씨
                    + sky - 하늘상태 정보
                        + code (string) - 하늘상태 코드
                        + name (string) - 하늘상태 코드명
                                SKY_Y01:맑음, SKY_Y02구름조금, SKY_Y03:구름많음, SKY_Y04:흐림, SKY_Y05:비, SKY_Y06:눈, SKY_Y07:비 또는 눈
                    + precipitation - 강수정보
                        + rain (string) - 일 누적 강우량(mm)
                        + snow (string) - 일 누적 적설량(cm)
                    + temperature - 기온정보
                        + tmax (string) - 어제의 최고기온
                        + tmin (string) - 어제의 최저기온
                + today - 오늘날씨
                    + sky - 하늘상태 정보
                        + code (string) - 하늘상태 코드
                        + name (string) - 하늘상태 코드명
                                SKY_D01:맑음, SKY_D02구름조금, SKY_D03:구름많음, SKY_D04:흐림, SKY_D05:비, SKY_D06:눈, SKY_D07:비 또는 눈
                    + temperature - 기온정보
                        + tmax (string) - 오늘의 최고기온
                        + tmin (string) - 오늘의 최저기온
                + tomorrow - 내일날씨
                    + sky - 하늘상태 정보
                        + code (string) - 하늘상태 코드
                        + name (string) - 하늘상태 코드명
                                SKY_M01:맑음, SKY_M02구름조금, SKY_M03:구름많음, SKY_M04:흐림, SKY_M05:비, SKY_M06:눈, SKY_M07:비 또는 눈
                    + temperature - 기온정보
                        + tmax (string) - 내일의 최고기온
                        + tmin (string) - 내일의 최저기온
                + dayAfterTomorrow - 모레날씨
                    + sky - 하늘상태 정보
                        + code (string) - 하늘상태 코드
                        + name (string) - 하늘상태 코드명
                                SKY_M01:맑음, SKY_M02구름조금, SKY_M03:구름많음, SKY_M04:흐림, SKY_M05:비, SKY_M06:눈, SKY_M07:비 또는 눈
                    + temperature - 기온정보
                        + tmax (string) - 모레의 최고기온
                        + tmin (string) - 모레의 최저기온
                + timeRelease (string) - 발표시간
                
    + Body
        {
            "result": { "code": 9200, "requestUrl": "/weather/summary?version=2&lat=37.1234&lon=127.1234", "message": "성공" },
            "common": { "alertYn": "Y", "stormYn": "N" },
            "weather": {
                "summary": [
                {
                    "grid": { "longitude": "127.0977600000", "latitude": "37.1177600000", "city": "경기", "county": "오산시", "village": "청호동" },
                    "timeRelease": "2017-06-02 17:00:00",
                    "yesterday": {
                        "precipitation": { "rain": "0.10", "snow": "0.00" },
                        "sky": { "code": "SKY_Y05", "name": "비" },
                        "temperature": { "tmax": "25.80", "tmin": "16.70" }
                    },
                    "today": {
                        "sky": { "code": "SKY_D03", "name": "구름많음" },
                        "temperature": { "tmax": "24.00", "tmin": "12.00" }
                    },
                    "tomorrow": {
                        "sky": { "code": "SKY_M03", "name": "구름많음" },
                        "temperature": { "tmax": "25.00", "tmin": "13.00" }
                    },
                    "dayAfterTomorrow": {
                        "sky": { "code": "SKY_M01", "name": "맑음" },
                      "temperature": { "tmax": "27.00", "tmin": "12.00" }
                    }
                }
                ]
            }
        }

### 어제날씨 [GET /weather/yesterday?version={version}&lat={latitude}&lon={longitude}&isTimeRange={isTimeRange}]

수집된 기상관측 정보를 격자단위로 분석.처리한 1시간 단위의 어제날씨 정보를 제공한다.

Request Parameter는 다음중 한 종류만 선택적으로 사용해야 한다.

(1) lat/lon  (2) city/county/village 

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보
    + city: 서울 (string, required) - 주소(시/도)
    + county: 강남구 (string, required) - 주소(시/군/구)
    + village: 삼성동 (string, required) - 주소(읍/면/동)
    + isTimeRange: Y (string, optional) - 어제날씨 구간설정(N:일통계+요청시간대, Y:일통계+24시간)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather
            + yesterday
                + grid - 격자 정보
                    + city (string) - 시(특별,광역), 도
                    + county (string) - 시, 군, 구
                    + village (string) - 읍, 면, 동
                    + latitude (string) - 격자중심 위도
                    + longitude (string) - 격자중심 경도
                + day (object)
                    + sky - 하늘상태 정보
                        + code (string) - 하늘상태 코드
                        + name (string) - 하늘상태 코드명
                                SKY_Y01:맑음, SKY_Y02구름조금, SKY_Y03:구름많음, SKY_Y04:흐림, SKY_Y05:비, SKY_Y06:눈, SKY_Y07:비 또는 눈
                    + precipitation - 강수정보
                        + rain (string) - 일 누적 강우량(mm)
                        + snow (string) - 일 누적 적설량(cm)
                    + temperature - 기온정보
                        + tmax (string) - 일 최고기온
                        + tmin (string) - 일 최저기온
                    + hourly - 시간대별 날씨(1개 또는 24개)
                        + sky - 하늘상태 정보
                            + code (string) - 하늘상태 코드
                            + name (string) - 하늘상태 코드명
                                    SKY_O01:맑음, SKY_O02구름조금, SKY_O03:구름많음,SKY_O04:구름많고 비, SKY_O05:구름많고 눈, SKY_O06:구름많고 비 또는 눈,
                                    SKY_O07:흐림, SKY_O08:흐리고 비, SKY_O09:흐리고 눈, SKY_O10:흐리고 비 또는 눈, SKY_O11 : 흐리고 낙뢰
                                    SKY_O12:뇌우/비, SKY_O13:뇌우/눈, SKY_O14:뇌우/비 또는 눈
                        + precipitation - 강수정보
                            + type (string) - 강수형태 코드
                                        - 0:현상없음,  - 1:비,   - 2:비/눈,   - 3:눈
                            + sinceOntime (string) - 1시간 누적 강수량
                                        - if type=0/1/2  --> 강우량(mm)
                                        - if type=3      --> 적설량(cm)
                        + temperature (string) -  1시간 기온(어제)
                        + wind - 바람정보
                            + wdir (string) - 풍향(degree)
                            + wspd (string) - 풍속(m/s)
                        + humidity (string) - 상대습도(%)
                        + ligntning (string) - 낙뢰유무( 해당격자내, 0:없음 / 1:있음 )
                        + timeRelease (string) - 발표시간
                + timeRelease (string) - 발표시간(어제)

    + Body
        {
          "result": { "code": 9200, "requestUrl": "/weather/yesterday?version=2&lat=37.1234&lon=127.1234&isTimeRange=Y", "message": "성공" },
          "common": { "alertYn": "Y", "stormYn": "N" },
          "weather": {
            "yesterday": [
              {
                "grid": { "latitude": "37.1177600000", "longitude": "127.0977600000", "city": "경기", "county": "오산시", "village": "청호동" },
                "day": {
                  "sky": { "code": "SKY_Y05", "name": "비" },
                  "temperature": { "tmax": "25.80", "tmin": "16.70" },
                  "precipitation": { "rain": "0.10", "snow": "0.00" },
                  "timeRelease": "2017-06-01",
                  "hourly": [
                    { "sky": { "code": "SKY_O07", "name": "흐림" },
                      "temperature": "20.70", "humidity": "76.00", "lightning": "0",
                      "wind": { "wdir": "342.00", "wspd": "0.30" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 01:00:00" },
                    { "sky": { "code": "SKY_O07", "name": "흐림" },
                      "temperature": "20.70", "humidity": "75.00", "lightning": "0",
                      "wind": { "wdir": "315.00", "wspd": "0.40" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 02:00:00" },
                    { "sky": { "code": "SKY_O07", "name": "흐림" },
                      "temperature": "20.70", "humidity": "75.00", "lightning": "0",
                      "wind": { "wdir": "300.00", "wspd": "0.80" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 03:00:00" },
                    { "sky": { "code": "SKY_O07", "name": "흐림" },
                      "temperature": "20.20", "humidity": "76.00", "lightning": "0",
                      "wind": { "wdir": "307.00", "wspd": "0.50" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 04:00:00" },
                    { "sky": { "code": "SKY_O07", "name": "흐림" },
                      "temperature": "19.80", "humidity": "77.00", "lightning": "0", 
                      "wind": { "wdir": "270.00", "wspd": "0.50" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 05:00:00" },
                    { "sky": { "code": "SKY_O07", "name": "흐림" },
                      "temperature": "19.50", "humidity": "77.00", "lightning": "0",
                      "wind": { "wdir": "288.00", "wspd": "0.60" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0"},
                      "timeRelease": "2017-06-01 06:00:00"  },
                    { "sky": { "code": "SKY_O07", "name": "흐림" },
                      "temperature": "20.20", "humidity": "77.00", "lightning": "0",
                      "wind": { "wdir": "288.00", "wspd": "0.90" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 07:00:00" },
                    { "sky": { "code": "SKY_O07", "name": "흐림" },
                      "temperature": "21.40", "humidity": "76.00", "lightning": "0",
                      "wind": { "wdir": "266.00", "wspd": "1.40" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 08:00:00" },
                    { "sky": { "code": "SKY_O07", "name": "흐림" },
                      "temperature": "22.60", "humidity": "75.00", "lightning": "0",
                      "wind": { "wdir": "261.00", "wspd": "1.80" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 09:00:00" },
                    { "sky": { "code": "SKY_O07", "name": "흐림" },
                      "temperature": "23.50","humidity": "65.00", "lightning": "0", 
                      "wind": { "wdir": "268.00", "wspd": "2.40"}, 
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 10:00:00" },
                    { "sky": { "code": "SKY_O08", "name": "흐리고 비" },
                      "temperature": "24.10", "humidity": "56.00", "lightning": "0",
                      "wind": { "wdir": "270.00", "wspd": "3.30" },
                      "precipitation": { "sinceOntime": "0.10", "type": "1" },
                      "timeRelease": "2017-06-01 11:00:00" },
                    {   "sky": { "code": "SKY_O07", "name": "흐림" },
                      "temperature": "24.80", "humidity": "62.00", "lightning": "0",
                      "wind": { "wdir": "275.00", "wspd": "3.50" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 12:00:00" },
                    { "sky": { "code": "SKY_O03", "name": "구름많음" },
                      "temperature": "25.00", "humidity": "57.00", "lightning": "0",
                      "wind": { "wdir": "287.00", "wspd": "3.90" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 13:00:00" },
                    { "sky": { "code": "SKY_O02", "name": "구름조금" },
                      "temperature": "25.40", "humidity": "52.00", "lightning": "0",
                      "wind": { "wdir": "284.00", "wspd": "3.70" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 14:00:00" },
                    { "sky": { "code": "SKY_O01", "name": "맑음" },
                      "temperature": "25.80", "humidity": "47.00", "lightning": "0",
                      "wind": { "wdir": "281.00", "wspd": "3.70" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 15:00:00" },
                    { "sky": { "code": "SKY_O01", "name": "맑음" },
                      "temperature": "25.10", "humidity": "50.00", "lightning": "0",
                      "wind": { "wdir": "271.00", "wspd": "3.90" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 16:00:00" },
                    { "sky": { "code": "SKY_O01", "name": "맑음" },
                      "temperature": "24.50", "humidity": "52.00", "lightning": "0",
                      "wind": { "wdir": "273.00", "wspd": "3.60" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 17:00:00" },
                    { "sky": { "code": "SKY_O01", "name": "맑음" },
                      "temperature": "22.30", "humidity": "44.00", "lightning": "0",
                      "wind": { "wdir": "276.00", "wspd": "3.10" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 18:00:00" },
                    { "sky": { "code": "SKY_O01", "name": "맑음" },
                      "temperature": "20.10", "humidity": "42.00", "lightning": "0",
                      "wind": { "wdir": "263.00", "wspd": "2.30" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 19:00:00" },
                    { "sky": { "code": "SKY_O01", "name": "맑음" },
                      "temperature": "18.60", "humidity": "40.00", "lightning": "0",
                      "wind": { "wdir": "266.00", "wspd": "1.50" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 20:00:00" },
                    { "sky": { "code": "SKY_O01", "name": "맑음" },
                      "temperature": "17.70", "humidity": "36.00", "lightning": "0",
                      "wind": { "wdir": "277.00", "wspd": "0.80" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 21:00:00" },
                    { "sky": { "code": "SKY_O01", "name": "맑음" },
                      "temperature": "17.60", "humidity": "36.00", "lightning": "0",
                      "wind": { "wdir": "295.00", "wspd": "1.40" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 22:00:00" },
                    { "sky": { "code": "SKY_O01", "name": "맑음" },
                      "temperature": "17.70", "humidity": "36.00", "lightning": "0",
                      "wind": { "wdir": "313.00", "wspd": "1.80" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-01 23:00:00" },
                    { "sky": { "code": "SKY_O01", "name": "맑음" },
                      "temperature": "16.70", "humidity": "39.00", "lightning": "0",
                      "wind": { "wdir": "293.00", "wspd": "0.80" },
                      "precipitation": { "sinceOntime": "0.00", "type": "0" },
                      "timeRelease": "2017-06-02 00:00:00"}
                  ]
                }
              }
            ]
          }
        }

## INDEX [/weather/index]
   
### 체감온도 [GET /weather/index/wct?version={version}&lat={latitude}&lon={longitude}]

생활기상지수 중 체감온도(Wind Chill Temperature Index)의 데이터를 서비스한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 37.1234 (string, required) - 위도
    + longitude: 126.1234 (string, required) - 경도

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + wIndex (object) - 날씨지수
                + wctIndex (object) - 체감온도지수
                    + grid (object) - 생활지수 지점정보
                        + city (string) - 시(특별시,광역시), 도
                        + county (string) - 시,군,구
                        + village (string) - 읍,면,동
                    + current (object) - 현재 체감온도
                        + timeRelease (string) - 1시간 현재 체감온도 발표시간
                        + index (string) - 1시간 현재 체감온도 지수
                    + forecast (object) - 예보 체감온도
                        + timeRelease (string) - 에보 체감온도 발표시간(3시간 간격)
                        + indexNhour (string) - 예보 체감온도 지수(발표시간+N시간)
                                - N=4,7,10,13,16,19,22,25,28,31,34,37,40,43,46,49,52,55,58,61,64,67
                        
    + Body
        {
            "result": { "code": 9200, "requestUrl": "/weather/index/wct?version=2&lat=37.1234&lon=127.1234", "message": "성공" },
            "common": { "alertYn": "Y", "stormYn": "N" },
            "weather": {
                "wIndex": {
                    "wctIndex": [
                    {
                        "grid": { "city": "경기", "county": "오산시", "village": "청호동" },
                        "current": { "timeRelease": "2017-06-02 18:00:00", "index": "21.80" },
                        "forecast": {
                            "timeRelease": "2017-06-02 17:00:00",
                            "index4hour": "16.00",
                            "index7hour": "16.00",
                            "index10hour": "14.00",
                            "index13hour": "13.00",
                            "index16hour": "18.00",
                            "index19hour": "22.00",
                            "index22hour": "24.00",
                            "index25hour": "22.00",
                            "index28hour": "17.00",
                            "index31hour": "15.00",
                            "index34hour": "13.00",
                            "index37hour": "12.00",
                            "index40hour": "19.00",
                            "index43hour": "24.00",
                            "index46hour": "27.00",
                            "index49hour": "23.00",
                            "index52hour": "18.00",
                            "index55hour": "16.00",
                            "index58hour": "",
                            "index61hour": "",
                            "index64hour": "",
                            "index67hour": ""
                        }
                    }
                    ]
                }
            }
        }

### 열지수 [GET /weather/index/heat?version={version}&lat={latitude}&lon={longitude}]

생활기상지수 중 열지수(Heat Index)의 데이터를 서비스한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 37.1234 (string, required) - 위도
    + longitude: 126.1234 (string, required) - 경도

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + wIndex (object) - 날씨지수
                + heatIndex (object) - 열지수
                    + grid (object) - 생활지수 지점정보
                        + city (string) - 시(특별시,광역시), 도
                        + county (string) - 시,군,구
                        + village (string) - 읍,면,동
                    + current (object) - 현재 열지수
                        + timeRelease (string) - 1시간 현재 열지수 발표시간
                        + index (string) - 1시간 현재 열지수
                    + forecast (object) - 예보 열지수
                        + timeRelease (string) - 에보 열지수 발표시간(3시간 간격)
                        + indexNhour (string) - 예보 얄지수(발표시간+N시간)
                                - N=4,7,10,13,16,19,22,25,28,31,34,37,40,43,46,49,52,55,58,61,64,67
                        
    + Body
        {
            "result": { "code": 9200, "requestUrl": "/weather/index/heat?version=2&lat=37.1234&lon=127.1234", "message": "성공" },
            "common": { "alertYn": "Y", "stormYn": "N" },
            "weather": {
                "wIndex": {
                    "heatIndex": [
                    {
                        "grid": { "city": "경기", "county": "오산시", "village": "청호동" },
                        "current": { "timeRelease": "2017-06-02 18:00:00", "index": "24.85" },
                        "forecast": {
                            "timeRelease": "2017-06-02 17:00:00",
                            "index4hour": "27.86",
                            "index7hour": "27.22",
                            "index10hour": "28.06",
                            "index13hour": "30.81",
                            "index16hour": "26.19",
                            "index19hour": "24.20",
                            "index22hour": "24.77",
                            "index25hour": "24.56",
                            "index28hour": "26.89",
                            "index31hour": "28.76",
                            "index34hour": "31.50",
                            "index37hour": "32.72",
                            "index40hour": "25.53",
                            "index43hour": "24.77",
                            "index46hour": "26.09",
                            "index49hour": "24.73",
                            "index52hour": "26.19",
                            "index55hour": "27.86",
                            "index58hour": "",
                            "index61hour": "",
                            "index64hour": "",
                            "index67hour": ""
                        }
                    }
                    ]
                }
            }
        }
        
### 불쾌지수 [GET /weather/index/th?version={version}&lat={latitude}&lon={longitude}]

생활기상지수 중 불쾌지수(Temperature-Humidity Index)의 데이터를 서비스한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 37.1234 (string, required) - 위도
    + longitude: 126.1234 (string, required) - 경도

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + wIndex (object) - 날씨지수
                + thIndex (object) - 불쾌지수
                    + grid (object) - 생활지수 지점정보
                        + city (string) - 시(특별시,광역시), 도
                        + county (string) - 시,군,구
                        + village (string) - 읍,면,동
                    + current (object) - 현재 불쾌지수
                        + timeRelease (string) - 1시간 현재 불쾌지수 발표시간
                        + index (string) - 1시간 현재 불쾌지수
                    + forecast (object) - 예보 불쾌지수
                        + timeRelease (string) - 에보 불쾌지수 발표시간(3시간 간격)
                        + indexNhour (string) - 예보 불쾌지수(발표시간+N시간)
                                - N=4,7,10,13,16,19,22,25,28,31,34,37,40,43,46,49,52,55,58,61,64,67
                        
    + Body
        {
            "result": { "code": 9200, "requestUrl": "/weather/index/th?version=2&lat=37.1234&lon=127.1234", "message": "성공" },
            "common": { "alertYn": "Y", "stormYn": "N" },
            "weather": {
                "wIndex": {
                    "thIndex": [
                    {
                        "grid": { "city": "경기", "county": "오산시", "village": "청호동" },
                        "current": { "timeRelease": "2017-06-02 11:00:00", "index": "70.75" },
                        "forecast": {
                            "timeRelease": "2017-06-02 17:00:00",
                            "index4hour": "72.67",
                            "index7hour": "70.66",
                            "index10hour": "65.26",
                            "index13hour": "62.11",
                            "index16hour": "59.88",
                            "index19hour": "61.21",
                            "index22hour": "64.43",
                            "index25hour": "66.74",
                            "index28hour": "65.26",
                            "index31hour": "63.72",
                            "index34hour": "61.97",
                            "index37hour": "61.84",
                            "index40hour": "63.17",
                            "index43hour": "63.34",
                            "index46hour": "65.07",
                            "index49hour": "65.80",
                            "index52hour": "66.88",
                            "index55hour": "63.95",
                            "index58hour": "61.71",
                            "index61hour": "58.84",
                            "index64hour": "",
                            "index67hour": ""
                        }
                    }
                    ]
                }
            }
        }

### 세차지수 [GET /weather/index/carwash?version={version}]

전국을 12개 권역으로 나누어 세차지수 예보 정보를 제공한다.
12개 권역은 서울/경기/인천, 강원영서, 강원영동, 충남, 충북, 전남, 전북, 경남, 경북, 제주, 서해5도, 울릉도/독도 이다.

+ Parameters
    + version: 2 (number, required) - API 버전정보

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + wIndex (object) - 날씨지수
                + carWash (object) - 세차지수(기준지역 개수만큼 반복)
                    + locationName (string) - 기준지역명
                    + index (string) - 세차지수
                    + comment (string) - 멘트
                + timeRelease (string) - 발표시간
                        
    + Body
        {
            "result": { "code": 9200, "requestUrl": "/weather/index/carwash?version=2", "message": "성공" },
            "common": { "alertYn": "Y", "stormYn": "N" },
            "weather": {
                "wIndex": {
                    "carWash": [
                        {   "locationName": "서울.경기.인천",
                            "index": "30",
                            "comment": "내일 비(눈)예보가 있습니다. 세차 하신다면 실내 주차는 필수!" },
                        {   "locationName": "강원영서",
                            "index": "30",
                            "comment": "내일 비(눈)예보가 있습니다. 세차 하신다면 실내 주차는 필수!" },
                        {   "locationName": "강원영동",
                            "index": "40",
                            "comment": "3일 내 비(눈)예보가 있습니다. 가급적 세차를 피하세요" },
                        {   "locationName": "충남",
                            "index": "30",
                            "comment": "내일 비(눈)예보가 있습니다. 세차 하신다면 실내 주차는 필수!" },
                        {   "locationName": "충북",
                            "index": "30",
                            "comment": "내일 비(눈)예보가 있습니다. 세차 하신다면 실내 주차는 필수!" },
                        {   "locationName": "전남",
                            "index": "30",
                            "comment": "내일 비(눈)예보가 있습니다. 세차 하신다면 실내 주차는 필수!" },
                        {   "locationName": "전북",
                            "index": "30",
                            "comment": "내일 비(눈)예보가 있습니다. 세차 하신다면 실내 주차는 필수!" },
                        {   "locationName": "경남",
                            "index": "30",
                            "comment": "내일 비(눈)예보가 있습니다. 세차 하신다면 실내 주차는 필수!" },
                        {   "locationName": "경북",
                            "index": "30",
                            "comment": "내일 비(눈)예보가 있습니다. 세차 하신다면 실내 주차는 필수!" },
                        {   "locationName": "제주",
                            "index": "30",
                            "comment": "내일 비(눈)예보가 있습니다. 세차 하신다면 실내 주차는 필수!" },
                        {   "locationName": "서해5도",
                            "index": "30", 
                            "comment": "내일 비(눈)예보가 있습니다. 세차 하신다면 실내 주차는 필수!" },
                        {   "locationName": "울릉도.독도",
                            "index": "40",
                            "comment": "3일 내 비(눈)예보가 있습니다. 가급적 세차를 피하세요" }
                    ]
                }
            }
        }

### 자외선지수 [GET /weather/index/uv?version={version}&lat={latitude}&lon={longitude]

생활기상기수 중 자외선지수의 예보정보(오늘/내일/모레)를 서비스한다.
06시 및 18시에 발표한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + wIndex (object) - 날씨지수
                + uvIndex (object) - 자외선지수
                    + grid (object) - 생활지수 지점정보
                        + city (string) - 시(특별시,광역시), 도
                        + county (string) - 시,군,구
                        + village (string) - 읍,면,동
                    + day00 (object) - 오늘(발표일)의 자외선지수(18시 발표자료 : 오늘자료 NULL)
                        + index (string) - 지수
                        + comment (string) - 멘트
                        + imageUrl (string) - 이미지 경로
                    + day01 (object) - 내일(발표일+1일)의 자외선지수
                        + index (string) - 지수
                        + comment (string) - 멘트
                        + imageUrl (string) - 이미지 경로
                    + day02 (object) - 모레(발표일+2일)의 자외선지수
                        + index (string) - 지수
                        + comment (string) - 멘트
                        + imageUrl (string) - 이미지 경로
                + timeRelease (string) - 발표시간
                        
    + Body
        {
            "result": { "code": 9200, "requestUrl": "/weather/index/uv?version=2&lat=37.1234&lon=127.1234", "message": "성공" },
            "common": { "alertYn": "Y", "stormYn": "N" },
            "weather": {
                "wIndex": {
                    "uvIndex": [
                        {
                          "grid": {
                            "city": "경기도",
                            "county": "오산시",
                            "village": ""
                          },
                          "day00": {
                            "imageUrl": "http://files.skplanet.com/weather/01_FCT/INDEX1/20170605/FCT_IDX_A07_2017060506_00D.gif",
                            "index": "70.00",
                            "comment": "자외선 차단제를 꼭 바르세요"
                          },
                          "day01": {
                            "imageUrl": "http://files.skplanet.com/weather/01_FCT/INDEX1/20170605/FCT_IDX_A07_2017060506_01D.gif",
                            "index": "50.00",
                            "comment": "자외선 노출 대비하세요"
                          },
                          "day02": {
                            "imageUrl": "http://files.skplanet.com/weather/01_FCT/INDEX1/20170605/FCT_IDX_A07_2017060506_02D.gif",
                            "index": "50.00",
                            "comment": "자외선 노출 대비하세요"
                          }
                        }
                    ],
                    "timeRelease": "2017-06-05 06:00"
                }
            }
        }
        
### 빨래지수 [GET /weather/index/laundry?version={version}&lat={latitude}&lon={longitude]

생활기상기수 중 빨래지수의 예보정보(오늘/내일/모레)를 서비스한다.
06시 및 18시에 발표한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + wIndex (object) - 날씨지수
                + laundry (object) - 빨래지수
                    + grid (object) - 생활지수 지점정보
                        + city (string) - 시(특별시,광역시), 도
                        + county (string) - 시,군,구
                        + village (string) - 읍,면,동
                    + day00 (object) - 오늘(발표일)의 빨래지수(18시 발표자료 : 오늘자료 NULL)
                        + index (string) - 지수
                        + comment (string) - 멘트
                        + imageUrl (string) - 이미지 경로
                    + day01 (object) - 내일(발표일+1일)의 빨래지수
                        + index (string) - 지수
                        + comment (string) - 멘트
                        + imageUrl (string) - 이미지 경로
                    + day02 (object) - 모레(발표일+2일)의 빨래지수
                        + index (string) - 지수
                        + comment (string) - 멘트
                        + imageUrl (string) - 이미지 경로
                + timeRelease (string) - 발표시간
                        
    + Body
        {
            "result": { "code": 9200, "requestUrl": "/weather/index/laundry?version=2&lat=37.1234&lon=127.1234", "message": "성공" },
            "common": { "alertYn": "Y", "stormYn": "N" },
            "weather": {
                "wIndex": {
                    "uvIndex": [
                        {
                          "grid": {
                            "city": "경기도",
                            "county": "오산시",
                            "village": ""
                          },
                          "day00": {
                            "imageUrl": "",
                            "index": "95",
                            "comment": "빨래할 절호의 기회!"
                          },
                          "day01": {
                            "imageUrl": "",
                            "index": "63",
                            "comment": "대체로 잘 말라요"
                          },
                          "day02": {
                            "imageUrl": "",
                            "index": "45",
                            "comment": "통풍이 잘 되는 곳에서 말리세요"
                          }
                        }
                    ],
                    "timeRelease": "2017-06-05 06:00"
                }
            }
        }
        
### 미세먼지 [GET /weather/dust?version={version}&lat={latitude}&lon={longitude]

기상청 미세먼지(pm10)측정소의 실시간 정보를 제공한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + dust (object) - 미세먼지 관측정보
                + station (object) - 측정보 지점정보
                    + name (string) - 측정소명
                    + id (string) - 측정소 ID(stnid)
                    + latitude (string) - 측정소 위도
                    + longitude (string) - 측정소 경도
                + pm10 (object) 미세먼지
                    + value (string) - 농도(㎍/㎥)
                            - 0~30:좋음, 31~80:보통, 81~120:약간나쁨, 121~200:나쁨, 200~:매우나쁨
                    + grade (string) - 등급(좋음, 보통, 약간나쁨, 나쁨, 매우나쁨)
                    + timeObservation (string) - 관측시간
                        
    + Body
        {
          "common": { "alertYn": "Y", "stormYn": "N" },
          "result": { "code": 9200, "requestUrl": "/weather/dust?version=2&lat=37.1234&lon=127.1234", "message": "성공" },
          "weather": {
            "dust": [
              {
                "timeObservation": "2017-06-05 14:00:00",
                "station": {
                  "latitude": "37.2723000000",
                  "longitude": "126.9853000000",
                  "name": "수원",
                  "id": "119"
                },
                "pm10": {
                  "grade": "좋음",
                  "value": "20.83"
                }
              }
            ]
          }
        }
        
## CODE [/weather/code]

### 관측소 정보 [GET /weather/code/station?version={version}&lat={latitude}&lon={longitude}]

위경도 기준으로 가장 가까운 관측소 정보를 제공한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보

+ Request (application/json)
    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + station (object) - 관측소 정보
                + name (string) - 관측소명
                + id (string) - 관측소 지점번호(stnid)
                + type (string) - 관측소 유형( KMA:기상청 관측소, BTN:SKTX 관측소 )
                + latitude (string) - 관측소 위도
                + longitude (string) - 관측소 경도
                        
    + Body
        {
          "common": { "alertYn": "Y", "stormYn": "N" },
          "result": { "code": 9200, "requestUrl": "/weather/code/station?version=2&lat=37.1234&lon=127.1234", "message": "성공" },
          "weather": {
            "station": [
              {
                "latitude": 37.1092333333,
                "name": "봉명리",
                "id": 10663,
                "type": "BTN",
                "longitude": 127.13475
              }
            ]
          }
        }

### 격자 정보 [GET /weather/code/grid?version={version}&lat={latitude}&lon={longitude}]

위경도 기준으로 가장 가까운 격자(grid) 정보를 제공한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + grid (object) - 격자 정보
                + city (string) - 시(특별, 광역), 도
                + county (string) - 시,군,구
                + village (string) - 읍,면,동
                + latitude (string) - 격자중심 위도
                + longitude (string) - 격자중심 경도
                        
    + Body
        {
          "common": { "alertYn": "Y", "stormYn": "N" },
          "result": { "code": 9200, "requestUrl": "/weather/code/grid?version=2&lat=37.1234&lon=127.1234", "message": "성공" },
          "weather": {
            "grid": [
              {
                "city": "경기",
                "latitude": 37.11776,
                "county": "오산시",
                "village": "청호동",
                "longitude": 127.09776
              }
            ]
          }
        }

# Group IMAGE

## 날씨영상 [/weather/wmap]

### 레이더영상 [GET /weather/wmap/radar?version={version}&proj={proj}&isTimeRange={isTimeRange}&start={startTime}&end={endTime}]

한반도의 레이더영상을 지도에 중첩할 수 있도록 투영법에 따라 KML과 XML 파일 형태로 제공하며, time animation이 가능하도록 시간대 설정을 통해 복수개의 KML/XML을 응답할 수 있다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + proj: 4326 (string, optional) - 투영법( 4326:EPSG 4326, 3857:EPSG 3857 )
    + isTimeRange: Y (string, optional) - time animation 사용유무( Y:최신 레이더영상, N:특정시간대 레이더영상 )
    + startTime: 201706011800 (string, optional) - 시간시작(년월일시분, 현재~과거24시간)
    + endTime: 201706012030 (string, optional) - 종료시간(년원일시분)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + radar (object) - 레이더영상(과겨영상 포함시 N개 응답)
                + fileName (string) - 원본 파일명
                + filePath (string) - KML or XML 파일의 경로(URL)
                + timeObservation (string) - 관측시간
                
    + Body
        {
            "result": { "code": 9200, "requestUrl": "/weather/wmap/radar?version=2&isTimeRange=Y&start=201706041800&end=201706041900", "message": "성공" },
            "common": { "alertYn": "Y", "stormYn": "N" },
            "weather": {
                "radar": [
                    {
                        "fileName": "RDR_CMI_201706041900.bin.gz",
                        "timeObservation": "2017-06-04 19:00:00",
                        "filePath": "http://files.skplanet.com/weather/07_RDR/CAPPI/20170604/RDR_CMI_201706041900_web.kml"
                    },
                    {
                        "fileName": "RDR_CMI_201706041850.bin.gz",
                        "timeObservation": "2017-06-04 18:50:00",
                        "filePath": "http://files.skplanet.com/weather/07_RDR/CAPPI/20170604/RDR_CMI_201706041850_web.kml"
                    },
                    {
                        "fileName": "RDR_CMI_201706041840.bin.gz",
                        "timeObservation": "2017-06-04 18:40:00",
                        "filePath": "http://files.skplanet.com/weather/07_RDR/CAPPI/20170604/RDR_CMI_201706041840_web.kml"
                    },
                    {
                        "fileName": "RDR_CMI_201706041830.bin.gz",
                        "timeObservation": "2017-06-04 18:30:00",
                        "filePath": "http://files.skplanet.com/weather/07_RDR/CAPPI/20170604/RDR_CMI_201706041830_web.kml"
                    },
                    {
                        "fileName": "RDR_CMI_201706041820.bin.gz",
                        "timeObservation": "2017-06-04 18:20:00",
                        "filePath": "http://files.skplanet.com/weather/07_RDR/CAPPI/20170604/RDR_CMI_201706041820_web.kml"
                    },
                    {
                        "fileName": "RDR_CMI_201706041810.bin.gz",
                        "timeObservation": "2017-06-04 18:10:00",
                        "filePath": "http://files.skplanet.com/weather/07_RDR/CAPPI/20170604/RDR_CMI_201706041810_web.kml"
                    },
                    {
                        "fileName": "RDR_CMI_201706041800.bin.gz",
                        "timeObservation": "2017-06-04 18:00:00",
                        "filePath": "http://files.skplanet.com/weather/07_RDR/CAPPI/20170604/RDR_CMI_201706041800_web.kml"
                    }
                ]
            }
        }

### 위성영상 [GET /weather/wmap/satellite?version={version}&proj={proj}&channel={channel}&isTimeRange={isTimeRange}&start={startTime}&end={endTime}]

한반도의 위성영상을 지도에 중첩할 수 있도록 투영법에 따라 KML과 XML 파일 형태로 제공하며, time animation이 가능하도록 시간대 설정을 통해 복수개의 KML/XML을 응답할 수 있다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + proj: 4326 (string, optional) - 투영법( 4326:EPSG 4326, 3857:EPSG 3857 )
    + channel: 1 (string, optional) - 위성영상 채널( 1:적외영상1(10.8㎛), 2:적외영상2(12.0㎛) )
    + isTimeRange: Y (string, optional) - time animation 사용유무( Y:최신 위성영상, N:특정시간대 위성영상 )
    + startTime: 201706011800 (string, optional) - 시간시작(년월일시분, 현재~과거24시간)
    + endTime: 201706012030 (string, optional) - 종료시간(년원일시분)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + satellite (object) - 위성영상(과겨영상 포함시 N개 응답)
                + fileName (string) - 원본 파일명
                + filePath (string) - KML or XML 파일의 경로(URL)
                + timeObservation (string) - 관측시간
                
    + Body
        {
            "result": { "code": 9200, "requestUrl": "/weather/wmap/satellite?version=2&isTimeRange=Y&start=201706041800&end=201706041900", "message": "성공" },
            "common": { "alertYn": "Y", "stormYn": "N" },
            "weather": {
               "satellite": [
                  {
                    "fileName": "coms_le1b_ir1_ch1_k_201706041000.bin.gz",
                    "timeObservation": "2017-06-04 19:00:00",
                    "filePath": "http://files.skplanet.com/weather/06_SAT/COMS/20170604/coms_le1b_ir1_ch1_k_201706041000.kml"
                  },
                  {
                    "fileName": "coms_le1b_ir1_ch1_k_201706040945.bin.gz",
                    "timeObservation": "2017-06-04 18:45:00",
                    "filePath": "http://files.skplanet.com/weather/06_SAT/COMS/20170604/coms_le1b_ir1_ch1_k_201706040945.kml"
                  },
                  {
                    "fileName": "coms_le1b_ir1_ch1_k_201706040930.bin.gz",
                    "timeObservation": "2017-06-04 18:30:00",
                    "filePath": "http://files.skplanet.com/weather/06_SAT/COMS/20170604/coms_le1b_ir1_ch1_k_201706040930.kml"
                  },
                  {
                    "fileName": "coms_le1b_ir1_ch1_k_201706040915.bin.gz",
                    "timeObservation": "2017-06-04 18:15:00",
                    "filePath": "http://files.skplanet.com/weather/06_SAT/COMS/20170604/coms_le1b_ir1_ch1_k_201706040915.kml"
                  },
                  {
                    "fileName": "coms_le1b_ir1_ch1_k_201706040900.bin.gz",
                    "timeObservation": "2017-06-04 18:00:00",
                    "filePath": "http://files.skplanet.com/weather/06_SAT/COMS/20170604/coms_le1b_ir1_ch1_k_201706040900.kml"
                  }
                ]
            }
        }

### 태풍영상 [GET /weather/wmap/storm?version={version}&proj={proj}&isThatAll={isThatAll}]

태풍의 이동경로를 지도상에 표시할 수 있도록 KML 파일 형태로 제공하며, 복수개의 태풍이 존재할 경우에는 발생한 태풍의 개수만큼을 응답한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + proj: 4326 (string, optional) - 투영법( 4326:EPSG 4326, 3857:EPSG 3857 )
    + isThatAll: N (string, optional) - 태풍 검색범위 ( Y:모든 구역의 태풍, N:경계 구역내 태풍 )

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + storm (object) - 태풍영상(태풍 N개 응답)
                + number (number) - 태풍번호
                + timeRelease (string) - 발표시간
                + nameKor (string) - 태풍이름(한글)
                + nameEng (string) - 태풍이름(영문)
                + InfluenceYn (string) - 경계구역내 존재여부 ( Y:경계구역내, N:경계구역외 )
                + filePath (string) - KML 파일의 경로(URL)
                
    + Body
        {
            "result": { "code": 9200, "requestUrl": "/weather/wmap/storm?version=2&isThatAll=N", "message": "성공" },
            "common": { "alertYn": "Y", "stormYn": "N" },
            "weather": {
               "storm": [
                  {
                    "number": 30
                    "timeRelease": "2013-11-07 16:00:00",
                    "nameKor": "하이옌",
                    "nameEng": "HYIYAN",
                    "InfluenceYn": "Y",
                    "filePath": "http://files.skplanet.com/weather/01_FCT/WRN/20131107/RTKO63_201311071600]30.kml""
                  }
                ]
            }
        }

### 낙뢰영상 [GET /weather/wmap/lightning?version={version}&proj={proj}&isTimeRange={isTimeRange}&start={startTime}&end={endTime}]

낙뢰 지점을 지도상에 표시할 수 있도록 10분 주기로 수집되는 낙뢰 상세 정보를 KML 파일 형태로 제공하며, time animation이 가능하도록 시간대 설정을 통해 복수개의 KML을 응답할 수 있다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + proj: 4326 (string, optional) - 투영법( 4326:EPSG 4326 )
    + isTimeRange: Y (string, optional) - time animation 사용유무( Y:최신 낙뢰영상, N:특정시간대 낙뢰영상 )
    + startTime: 201706011800 (string, optional) - 시간시작(년월일시분, 현재~과거24시간)
    + endTime: 201706012030 (string, optional) - 종료시간(년원일시분)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + lightning (object) - 낙뢰영상(과거영상 포함시 N개 응답)
                + timeRelease (string) - 발표시간
                + filePath (string) - KML 파일의 경로(URL)
                
    + Body
        {
          "common": { "alertYn": "Y", "stormYn": "N" },
          "result": { "code": 9200, "requestUrl": "/weather/wmap/lightning?version=2&isTimeRange=Y&start=201706041900&end=201706041930", "message": "성공" },
          "weather": {
            "lightning": [
              {
                "timeRelease": "2017-06-04 19:30:00",
                "filePath": "http://files.skplanet.com/weather/08_LGT/20170604/LGT_KMA_201706041930.kml"
              },
              {
                "timeRelease": "2017-06-04 19:25:00",
                "filePath": "http://files.skplanet.com/weather/08_LGT/20170604/LGT_KMA_201706041925.kml"
              },
              {
                "timeRelease": "2017-06-04 19:20:00",
                "filePath": "http://files.skplanet.com/weather/08_LGT/20170604/LGT_KMA_201706041920.kml"
              },
              {
                "timeRelease": "2017-06-04 19:15:00",
                "filePath": "http://files.skplanet.com/weather/08_LGT/20170604/LGT_KMA_201706041915.kml"
              },
              {
                "timeRelease": "2017-06-04 19:10:00",
                "filePath": "http://files.skplanet.com/weather/08_LGT/20170604/LGT_KMA_201706041910.kml"
              },
              {
                "timeRelease": "2017-06-04 19:05:00",
                "filePath": "http://files.skplanet.com/weather/08_LGT/20170604/LGT_KMA_201706041905.kml"
              },
              {
                "timeRelease": "2017-06-04 19:00:00",
                "filePath": "http://files.skplanet.com/weather/08_LGT/20170604/LGT_KMA_201706041900.kml"
              }
            ]
          }
        }

### 예보영상 [GET /weather/wmap/forecast?version={version}&proj={proj}&type={type}&factor={factor}]

강수, 온도 등의 예보정보를 지도에 중첩할 수 있도록 투영법(projection)에 따라 KML과 XML 파일 형태로 제공하며, time animation이 가능하도록 복수개의 KML 또는 XML을 응답할 수 있다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + proj: 4326 (string, optional) - 투영법( 4326:EPSG 4326, 3857: EPSG 3857 )
    + type: 1 (string, optional) - 예보영상 종류 : [1] UM(1.5km), 1시간간격, 36시간(일 2회)  [2] KLAPS(5km), 1시간 간격, 12시간(일 24회)  [3]~ 예비(RFU)
    + factor: precipitation (string, optional) - 기상요소( preicipitation:강수, temperature:기온, windSpeed:풍속, pressure:기압, humidity:습도 )

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + forecast (object) - 예보영상
                + timeModelrun (string) - 모델 수행시간
                + fcstList (object) - 예보영상 목록( N개응답, type=1:37개, type=2: 13개 )
                    + timeForecast (string) - 예보시간
                    + filePath (string) - KML or XML 파일의 경로(URL)
                
    + Body
        {
          "common": { "alertYn": "Y", "stormYn": "N" },
          "result": { "code": 9200, "requestUrl": "/weather/wmap/forecast?version=2", "message": "성공" },
          "weather": {
            "forecast": {
              "fcstList": [
                { "timeForecast": "2017-06-05 09:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h000.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-05 10:00:00", 
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h001.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-05 11:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h002.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-05 12:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h003.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-05 13:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h004.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-05 14:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h005.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-05 15:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h006.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-05 16:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h007.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-05 17:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h008.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-05 18:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h009.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-05 19:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h010.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-05 20:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h011.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-05 21:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h012.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-05 22:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h013.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-05 23:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h014.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 00:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h015.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 01:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h016.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 02:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h017.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 03:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h018.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 04:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h019.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 05:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h020.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 06:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h021.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 07:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h022.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 08:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h023.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 09:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h024.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 10:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h025.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 11:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h026.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 12:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h027.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 13:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h028.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 14:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h029.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 15:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h030.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 16:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h031.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 17:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h032.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 18:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h033.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 19:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h034.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 20:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h035.2017060500_NCPCP.kml" },
                { "timeForecast": "2017-06-06 21:00:00",
                  "filePath": "http://files.skplanet.com/weather/05_MODEL/20170605/l015_v070_erlo_unis_h036.2017060500_NCPCP.kml"}
              ],
              "timeModelrun": "2017-06-05 09:00:00"
            }
          }
        }
    

# Group BUNDLE

## 전지점 기상정보 [/weather/bundle]

전지점 기상정보 API는 제휴등급의 개발자에게만 제공합니다.(문의 및 신청: weather@sk.com)

### 현재날씨:시간별 [GET /weather/bundle/hourly?version={version}]

수집된 기상관측 정보를 격자단위로 분석.처리하여 1시간 단위로 현재날씨 정보를 제공한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather
            + hourly - 현재날씨(시간단위, 격자 N개 응답)
                + grid - 격자 정보
                    + city (string) - 시(특별,광역), 도
                    + county (string) - 시, 군, 구
                    + village (string) - 읍, 면, 동
                    + latitude (string) - 격자중심 위도
                    + longitude (string) - 격자중심 경도
                + sky - 하늘상태 정보
                    + code (string) - 하늘상태 코드
                    + name (string) - 하늘상태 코드명
                         SKY_O01:맑음, SKY_O02구름조금, SKY_O03:구름많음,SKY_O04:구름많고 비, SKY_O05:구름많고 눈, SKY_O06:구름많고 비 또는 눈,
                            SKY_O07:흐림, SKY_O08:흐리고 비, SKY_O09:흐리고 눈, SKY_O10:흐리고 비 또는 눈, SKY_O11 : 흐리고 낙뢰
                            SKY_O12:뇌우/비, SKY_O13:뇌우/눈, SKY_O14:뇌우/비 또는 눈
                + precipitation - 강수정보
                    + type (string) - 강수형태 코드
                                - 0:현상없음,  - 1:비,   - 2:비/눈,   - 3:눈
                    + sinceOntime (string) - 1시간 누적 강수량
                                - if type=0/1/2  --> 강우량(mm)
                                - if type=3      --> 적설량(cm)
                + temperature - 기온정보
                    + tc (string) - 1시간 현재기온
                    + tmax (string) - 오늘의 최고기온
                    + tmin (string) - 오늘의 최저기온
                + wind - 바람정보
                    + wdir (string) - 풍향(degree)
                    + wspd (string) - 풍속(m/s)
                + humidity (string) - 상대습도(%)
                + ligntning (string) - 낙뢰유무(해당격자내)
                               - 0:없음,   - 1:있음
                + sunRiseTime (string) - 일출시간
                + sunSetTime (string) - 일몰시간
                + timeRelease (string) - 발표시간

### 초단기예보 [GET /weather/bundle/forecast3hours?version={version}]

1시간 간격으로 매일 24회, 5Km 격자 단위로 4시간까지의 초단기예보 정보를 제공한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather
            + forecast3hours - 초단기예보(격자 N개 응답)
                + grid - 격자 정보
                    + city (string) - 시(특별,광역), 도
                    + county (string) - 시, 군, 구
                    + village (string) - 읍, 면, 동
                    + latitude (string) - 격자중심 위도
                    + longitude (string) - 격자중심 경도
                + sky - 하늘상태 정보
                    + codeNhour (string) - 하늘상태 코드(발표시간+N시간)
                    + nameNhour (string) - 하늘상태 코드명(발표시간+N시간)
                         SKY_V01:맑음, SKY_V02구름조금, SKY_V03:구름많음,SKY_V04:구름많고 비, SKY_V05:구름많고 눈, SKY_V06:구름많고 비 또는 눈,
                            SKY_V07:흐림, SKY_V08:흐리고 비, SKY_V09:흐리고 눈, SKY_V10:흐리고 비 또는 눈, SKY_V11 : 흐리고 낙뢰
                            SKY_V12:뇌우/비, SKY_V13:뇌우/눈, SKY_V14:뇌우/비 또는 눈
                + precipitation - 강수정보
                    + typeNhour (string) - 강수형태 코드(발표시간+N시간)
                                - 0:현상없음,  - 1:비,   - 2:비/눈,   - 3:눈
                    + sinceOntimeNhour (string) - 1시간 누적 강수량(발표시간+N시간)
                                - if type=0/1/2  --> 강우량(mm)
                                - if type=3      --> 적설량(cm)
                + temperature - 기온정보
                    + tempNhour (string) - 기온(발표시간+N시간)
                + wind - 바람정보
                    + wdirNhour (string) - 풍향(발표시간+N시간)
                    + wspdNhour (string) - 풍속(발표시간+N시간)
                + humidity  - 습도정보
                    + rhNhour (string) - 상대습도(발표시간+N시간)
                + ligntningNhour (string) - 낙뢰확률(발표시간+N시간, 관측소 5km반경 내)
                               - 0:현상없음,   - 1:낮음(30~50%),  - 2:보통(50~70%),  - 3:높음(70%이상)
                + timeRelease (string) - 발표시간

### 단기예보 [GET /weather/bundle/forecast3days?version={version}&foretxt={foretxt}]

3시간 간격으로 매일 8회(2, 5, 8, 11, 14, 17, 20, 23시), 5Km 격자 단위로 단기예보 정보를 제공하며, 예보시간은 발표시간+4시간부터 최대 67시간(3일)까지 제공한다

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + foretxt: Y (string, optional) - 단기예보 기상개황 수신여부(Y:수신, N:미수신)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + forecast3days (object) - 단기예보(격자 N개 응답)
                + grid (object) - 격자 정보
                    + city (string) - 시(특별,광역), 도
                    + county (string) - 시, 군, 구
                    + village (string) - 읍, 면, 동
                    + latitude (string) - 격자중심 위도
                    + longitude (string) - 격자중심 경도
                + fsct3hour (object) - 3시간 예보(예보간격:3시간) - 적용요소:하늘상태,강수,기온,풍향/풍속,습도 - 자료개수:15판~22판(발표시간에 따라 판수가 다름)
                  + sky (object) - 하늘상태 정보(N=4,7,10,13,...,64,67)
                        + codeNhour (string) - 하늘상태 코드(발표시간+N시간)
                        + nameNhour (string) - 하늘상태 코드명(발표시간+N시간)
                            SKY_V01:맑음, SKY_V02구름조금, SKY_V03:구름많음,SKY_V04:구름많고 비, SKY_V05:구름많고 눈, SKY_V06:구름많고 비 또는 눈,
                            SKY_V07:흐림, SKY_V08:흐리고 비, SKY_V09:흐리고 눈, SKY_V10:흐리고 비 또는 눈, SKY_V11 : 흐리고 낙뢰
                            SKY_V12:뇌우/비, SKY_V13:뇌우/눈, SKY_V14:뇌우/비 또는 눈
                  + precipitation (object) - 강수정보(N=4,7,10,13,...,64,67)
                        + typeNhour (string) - 강수형태 코드(발표시간+N시간)
                                - 0:현상없음,  - 1:비,   - 2:비/눈,   - 3:눈    
                        + probNhour (string) - 강수확률(%)(발표시간+N시간)
                  + temperature (object) - 기온정보(N=4,7,10,13,...,64,67)
                        + tempNhour (string) - 기온(발표시간+N시간)
                  + wind (object) - 바람정보(N=4,7,10,13,...,64,67)
                        + wdirNhour (string) - 풍향(발표시간+N시간)
                        + wspdNhour (string) - 풍속(발표시간+N시간)
                  + humidity (object) - 습도정보(N=4,7,10,13,...,64,67)
                        + rhNhour (string) - 상대습도(발표시간+N시간)
                + fsct6hour (object) - 6시간 예보(예보간격:6시간)  - 적용요소 강우량/적설량(발표시간+1시간부터 다음판까지 강수량)   - 자료개수:8판~11판(발표시간에 따라 판수가 다름)
                    + rainMhour (string) - 6시간 누적 강우량(발표시간+M시간 / M=6,12,18,24,30,36,42,48,54,60,66)
                            - Value는 아래 문자열로 내려옴
                            - 없음, 1mm미만, 1~4mm, 5~9mm, 10~19mm, 20~39mm, 40~69mm, 70mm이상
                    + snowMhour (string) - 6시간 신 적설량(발표시간+M시간 / M=6,12,18,24,30,36,42,48,54,60,66)
                            - Value는 아래 문자열로 내려옴
                            - 없음, 1cm미만, 1~4cm, 5~9cm, 10~19cm, 20cm이상
                + fcstdaily (object) - 24시간 예보(예보간견:24시간)  - 적용요소:최고/최저기온  - 자료개수:2판(최저기온), 2~3판(최고기온)(발표시간에 따라 판수가 다름)
                    + temperature (object) - 기온정보(X=1,2,3)
                        + tmaxXday (string) - 일 최고기온(오늘,내일,모레)
                        + tminXday (string) - 일 최저기온(오늘,내일,모레)
                + fcstext (object) - 단기예보 개황(전국)
                    + locationName (string) - 기준지역명(전국)
                    + text1 (string) - 기상개황(오늘)
                    + text2 (string) - 기상개황(내일)
                    + text3 (string) - 기상개항(모레)
                    + wn (string) - 특보발표현황
                    + wr (string) - 예비특보현황
                    + timeRelease (string) -  발표시간
                + fcstextRegion (object) - 단기예보 개황(요청지역)
                    + locationName (string) - 기준지역명(요청지역)
                    + text1 (string) - 기상개황(오늘)
                    + text2 (string) - 기상개황(내일)
                    + text3 (string) - 기상개항(모레)
                    + wn (string) - 특보발표현황
                    + wr (string) - 예비특보현황
                    + timeRelease (string) -  발표시간
                + timeRelease (string) - 발표시간

### 중기예보 [GET /weather/bundle/forecast6days?version={version}&foretxt={foretxt}]

12시간 간격으로 매일 2회(6시, 18시), 주요 시도 단위로 중기예보 정보를 제공하며, 예보시간은 일단위로 +3일부터 +10일까지 제공한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + foretxt: Y (string, optional) - 중기예보 기상개황 수신여부(Y:수신, N:미수신)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + forecast6days (object) - 중기예보(격자 N개 응답)
                + grid (object) - 격자 정보
                    + city (string) - 시(특별,광역), 도
                    + county (string) - 시, 군, 구
                    + village (string) - 읍, 면, 동
                + location (object) = 중기예보 기준지역
                    + name (string) - 기준지역명
                + sky (object) - 하늘상태 정보(N=3,4,5,6,7,8,9,10)
                    + amCodeNday (string) - 하늘상태코드(오전)(발표시간+N일)
                    + amNameNday (string) - 하늘상태코드명(오전)(발표시간+N일)
                            SKY_W01:맑음, SKY_W02구름조금, SKY_W03:구름많음,SKY_W04:흐림, SKY_W09:구름많고 비, 
                            SKY_V10:소나기, SKY_W11:비 또는 눈, SKY_W12:구름많고 눈, SKY_W13:흐리고 눈
                    + pmCodeNday (string) - 하늘상태코드(오후)(발표시간+N일)
                    + pmNameNday (string) - 하늘상태코드명(오흐)(발표시간+N일)
                + temperature (object) - 기온정보(N=3,4,5,6,7,8,9,10)
                    + tmaxNday (string) - 일 최고기온(발표시간+N일)
                    + tminNday (string) - 일 최저기온(발표시간+N일)
                + fcstext (object) - 중기예보 개황(전국)
                    + locationName (string) - 기준지역명(전국)
                    + text (string) - 기상개황(주간)
                    + timeRelease (string) -  발표시간
                + fcstextRegion (object) - 중기예보 개황(요청지역)
                    + locationName (string) - 기준지역명(요청지역)
                    + text (string) - 기상개황(주간)
                    + timeRelease (string) -  발표시간
                + timeRelease (string) - 발표시간

### 기상특보 [GET /weather/bundle/alert?version={version}]

강풍, 호우, 한파, 건조, 해일, 풍랑, 태풍, 대설, 황사, 폭염 등의 특보 발생시에 200여개의 특보구역에 대해 기상특보를 제공한다. 또한, 동일 특보구역에 복수개의 기상특보가 존재할 경우에는 발생한 특보의 개수만큼을 응답한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + alert (object) - 기상특보(특보구역 N개 응답)
                + grid (object) - 격자정보
                    + city (string) - 시(특별,광역), 도
                    + county (string) - 시, 군, 구
                    + village (string) - 읍, 면, 동
                + stationId (string) - 발표관서 지점번호
                + timeRelease (string) - 발표시간
                + number (string) - 발표번호(월별)
                + areaCode (string) - 특보구역코드
                + areaName (string) - 특보구역코드명
                + alert51 (object) = 기상특보(코드)
                    + cmdCode (string) - 특보발표코드
                    + cmdName (string) - 특보발표코드명
                            - 1:발표 / 2:해제 / 3:연장 / 4:대치에 의한 해제 / 5:대치에 의한 발표 / 6:정정
                    + varCode (string) - 특보종류코드
                    + varName (string) - 특보종류코드명
                            - 1:강품 / 2:호우 / 3:한파 / 4:건조 / 5:해일 / 6:풍랑 / 7:태풍 / 8:대설 / 9:황사 / 12:폭염
                    + stressCode (string) - 특보강도코드(0,1)
                    + stressName (string) - 특보강도코드명(0:주의보, 1:경보)
                + alert60 (object) - 기상특보(서술)
                    + t1 (string) - 제목
                    + t2 (string) - 해당구역
                    + t3 (string) - 발효시간
                    + t4 (string) - 내용
                    + t5 (string) - 특보발효현황 시간
                    + t6 (string) - 특보발효현황 내용
                    + t7 (string) - 예비특보발효 현황
                    + other (string) - 참고사항
                + timeStart (string) - 발효시간
                + timeEnd (string) - 발효해제시간
                + timeAllEnd (string) - 전체특보해제시간

# Group AIRQUALITY

## 대기질정보 [/weather/airquality]

대기질정보 API는 제휴등급의 개발자에게만 제공합니다.(문의 및 신청: weather@sk.com)

### 대기관측 [GET /weather/airquality/current?version={version}&lat={latitude}&lon={longitude}]

한국환경공단 전국 대기환경 측정소의 실시간 정보를 제공한다.

version 2.5부터 요청한 위경도가 속한 시/도의 대기환경 측정정보를 같이 제공한다.

+ Parameters
    + version: 2.5 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + airQuality (object) - 대기환경정보
                + current (object) - 관측정보
                    + station (object) - 측정소 정보
                        + name (string) - 측정소명
                        + owner (string) - 관리기관명
                        + network (string) - 측정망( 도시대기, 도로변대기, 국가배경농도, 교외대기 )
                        + latitude (string) - 측정소 위도
                        + longitude (string) - 측정소 경도
                    +  so2 (object) - 아황산가스
                        + value (string) - 농도(ppm)
                                - 0~0.02:좋음, 0.021~0.05:보통, 0.051~0.15:나쁨, 0.151~:매우나쁨
                        + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                    +  co (object) - 일산화탄소
                        + value (string) - 농도(ppm)
                                - 0~2:좋음, 2.01~9:보통, 9.01~15:나쁨, 15.01~:매우나쁨
                        + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                    +  o3 (object) - 오존
                        + value (string) - 농도(ppm)
                                - 0~0.03:좋음, 0.031~0.09:보통, 0.091~0.15:나쁨, 0.151~:매우나쁨
                        + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                    +  no2 (object) - 이산화질소
                        + value (string) - 농도(ppm)
                                - 0~0.03:좋음, 0.031~0.06:보통, 0.061~0.2:나쁨, 0.201~:매우나쁨
                        + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                    +  pm10 (object) - 미세먼지(pm10)
                        + value (string) - 농도(㎍/㎥)
                                - 0~30:좋음, 31~80:보통, 81~150:나쁨, 151~:매우나쁨
                        + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                    +  pm25 (object) - 미세먼지(pm25)
                        + value (string) - 농도(㎍/㎥)
                                - 0~15:좋음, 16~35:보통, 36~75:나쁨, 76~:매우나쁨
                        + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                    +  khai (object) - 통합대기환경(CAI : Comprehensive airquailty index)
                        + value (string) - 수치
                                - 0~50:좋음, 51~100:보통, 101~250:나쁨, 251~:매우나쁨
                        + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                    + timeObservation (string) - 관측시간
                    + cityValue (object) - 시도별 대기질 정보
                        +  so2 (object) - 아황산가스
                            + value (string) - 농도(ppm)
                                    - 0~0.02:좋음, 0.021~0.05:보통, 0.051~0.15:나쁨, 0.151~:매우나쁨
                            + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                        +  co (object) - 일산화탄소
                            + value (string) - 농도(ppm)
                                    - 0~2:좋음, 2.01~9:보통, 9.01~15:나쁨, 15.01~:매우나쁨
                            + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                        +  o3 (object) - 오존
                            + value (string) - 농도(ppm)
                                    - 0~0.03:좋음, 0.031~0.09:보통, 0.091~0.15:나쁨, 0.151~:매우나쁨
                            + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                        +  no2 (object) - 이산화질소
                            + value (string) - 농도(ppm)
                                    - 0~0.03:좋음, 0.031~0.06:보통, 0.061~0.2:나쁨, 0.201~:매우나쁨
                            + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                        +  pm10 (object) - 미세먼지(pm10)
                            + value (string) - 농도(㎍/㎥)
                                    - 0~30:좋음, 31~80:보통, 81~150:나쁨, 151~:매우나쁨
                            + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                        +  pm25 (object) - 미세먼지(pm25)
                            + value (string) - 농도(㎍/㎥)
                                    - 0~15:좋음, 16~50:보통, 51~100:나쁨, 101~:매우나쁨
                            + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                        +  khai (object) - 통합대기환경(CAI : Comprehensive airquailty index)
                            + value (string) - 수치
                                    - 0~50:좋음, 51~100:보통, 101~250:나쁨, 251~:매우나쁨
                            + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )

                    
    + Body
        {
          "common": { "alertYn": "Y", "stormYn": "N" },
          "result": { "code": 9200, "requestUrl": "/weather/airquality/current?version=2&lat=36.1234&lon=127.1234", "message": "성공" },
          "weather": {
            "airQuality": {
              "current": [
                {
                  "timeObservation": "2017-06-09 17:00:00",
                  "station": {
                    "latitude": "35.9616330000",
                    "longitude": "127.0042330000",
                    "network": "도시대기",
                    "name": "팔봉동",
                    "owner": "전라북도보건환경연구원"
                  },
                  "so2": { "grade": "좋음", "value": "0.003" },
                  "co": { "grade": "좋음", "value": "0.4" },
                  "o3": { "grade": "보통", "value": "0.068" },
                  "no2": { "grade": "좋음", "value": "0.012" },
                  "pm10": { "grade": "보통", "value": "73" },
                  "khai": { "grade": "보통", "value": "90" },
                  "pm25": { "grade": "보통", "value": "37" },
                  "cityValue": {
                      "so2": { "grade": "좋음", "value": "0.001" },
                      "co": { "grade": "좋음", "value": "0.2" },
                      "o3": { "grade": "보통", "value": "0.058" },
                      "no2": { "grade": "좋음", "value": "0.011" },
                      "pm10": { "grade": "보통", "value": "63" },
                      "khai": { "grade": "보통", "value": "89" },
                      "pm25": { "grade": "보통", "value": "35" },
                  }
                }
              ]
            }
          }
        }

### 대기예보 [GET /weather/airquality/forecast?version={version}]

한국환경공단의 전국 대기환경 예보 정보를 제공한다.
매일 4회(05시, 11시, 17시, 23시) 발표되며, 내일예보의 경우 05시, 11시 발표자료에는 예보개황만 발표되고, 모레예보의 경우에는 17시발표부터 예보개황만 발표된다

+ Parameters
    + version: 2 (number, required) - API 버전정보

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + airQuality (object) - 대기환경정보
                + forecast (object) - 대기환경 예보정보(N:오늘+내일+모레)
                    + date (string) - 예보일
                    + typeCode (string) - 통보코드
                    + fcstext (string) - 에보개황
                    + cause (string) - 발생원인
                    + region (object) - 권역별 예보등급(권역개수만큼 반복)
                        + name (string) - 권역명
                                - 서울, 인천, 경기남부, 경기북부, 충남, 충북, 대전, 세동, 영동권, 영서권
                                - 전남, 전북, 광주, 경남, 경북, 울산, 대구, 부산, 제주
                        + grade (string) - 예보등급 ( 좋음, 보통, 나쁨, 매우나쁨 )
                    + actionGuide (string) - 행동요령
                    + timeRelease (string) - 발표시간( 오전05시, 오전11시, 오후17시, 오후23시 )
                    
    + Body
        {
          "weather": {
            "airQuality": {
              "forecast": [
                {
                  "timeRelease": "2017-06-09 11:00:00",
                  "fcstext": "○[오존] 경기남부·충남·전북·전남·부산·울산·경북·경남은 ‘나쁨’, 그 밖의 권역은 ‘보통’으로 예상됨.",
                  "actionGuide": "",
                  "date": "2017-06-09",
                  "cause": "대기오염물질의 광화학 반응에 의한 오존 생성이 활발하고 국외 유입이 더해져 남부지역을 중심으로 농도가 높을 것으로 예상됨.",
                  "typeCode": "O3",
                  "region": [
                    { "name": "서울 ", "value": " 보통" },
                    { "name": "제주 ", "value": " 보통" },
                    { "name": "전남 ", "value": " 나쁨" },
                    { "name": "전북 ", "value": " 나쁨" },
                    { "name": "광주 ", "value": " 보통" },
                    { "name": "경남 ", "value": " 나쁨" },
                    { "name": "경북 ", "value": " 나쁨" },
                    { "name": "울산 ", "value": " 나쁨" },
                    { "name": "대구 ", "value": " 보통" },
                    { "name": "부산 ", "value": " 나쁨" },
                    { "name": "충남 ", "value": " 나쁨" },
                    { "name": "충북 ", "value": " 보통" },
                    { "name": "세종 ", "value": " 보통" },
                    { "name": "대전 ", "value": " 보통" },
                    { "name": "영동 ", "value": " 보통" },
                    { "name": "영서 ", "value": " 보통" },
                    { "name": "경기남부 ", "value": " 나쁨" },
                    { "name": "경기북부 ", "value": " 보통" },
                    { "name": "인천 ", "value": " 보통" }
                  ]
                },
                {
                  "timeRelease": "2017-06-09 11:00:00",
                  "fcstext": "○[미세먼지] 전 권역이 ‘보통’으로 예상됨.",
                  "actionGuide": "",
                  "date": "2017-06-09",
                  "cause": "○[미세먼지] 원활한 대기확산으로 대부분 ‘보통’ 수준을 나타내겠으나, 일부 중서부지역은 대기정체와 국외 미세먼지 유입으로 밤에 농도가 다소 높을 것으로 예상됨.",
                  "typeCode": "PM10",
                  "region": [
                    { "name": "서울 ", "value": " 보통" },
                    { "name": "제주 ", "value": " 보통" },
                    { "name": "전남 ", "value": " 보통" },
                    { "name": "전북 ", "value": " 보통" },
                    { "name": "광주 ", "value": " 보통" },
                    { "name": "경남 ", "value": " 보통" },
                    { "name": "경북 ", "value": " 보통" },
                    { "name": "울산 ", "value": " 보통" },
                    { "name": "대구 ", "value": " 보통" },
                    { "name": "부산 ", "value": " 보통" },
                    { "name": "충남 ", "value": " 보통" },
                    { "name": "충북 ", "value": " 보통" },
                    { "name": "세종 ", "value": " 보통" },
                    { "name": "대전 ", "value": " 보통" },
                    { "name": "영동 ", "value": " 보통" },
                    { "name": "영서 ", "value": " 보통" },
                    { "name": "경기남부 ", "value": " 보통" },
                    { "name": "경기북부 ", "value": " 보통" },
                    { "name": "인천 ", "value": " 보통" }
                  ]
                },
                {
                  "timeRelease": "2017-06-09 11:00:00",
                  "fcstext": "○[미세먼지] 전 권역이 ‘보통’으로 예상됨.",
                  "actionGuide": "",
                  "date": "2017-06-09",
                  "cause": "○[미세먼지] 원활한 대기확산으로 대부분 ‘보통’ 수준을 나타내겠으나, 일부 중서부지역은 대기정체와 국외 미세먼지 유입으로 밤에 농도가 다소 높을 것으로 예상됨.",
                  "typeCode": "PM25",
                  "region": [
                    { "name": "서울 ", "value": " 보통" },
                    { "name": "제주 ", "value": " 보통" },
                    { "name": "전남 ", "value": " 보통" },
                    { "name": "전북 ", "value": " 보통" },
                    { "name": "광주 ", "value": " 보통" },
                    { "name": "경남 ", "value": " 보통" },
                    { "name": "경북 ", "value": " 보통" },
                    { "name": "울산 ", "value": " 보통" },
                    { "name": "대구 ", "value": " 보통" },
                    { "name": "부산 ", "value": " 보통" },
                    { "name": "충남 ", "value": " 보통" },
                    { "name": "충북 ", "value": " 보통" },
                    { "name": "세종 ", "value": " 보통" },
                    { "name": "대전 ", "value": " 보통" },
                    { "name": "영동 ", "value": " 보통" },
                    { "name": "영서 ", "value": " 보통" },
                    { "name": "경기남부 ", "value": " 보통" },
                    { "name": "경기북부 ", "value": " 보통" },
                    { "name": "인천 ", "value": " 보통" }
                  ]
                },
                {
                  "timeRelease": "2017-06-09 11:00:00",
                  "fcstext": "○[오존] 전 권역의 농도가 전일과 비슷하거나 다소 낮을 것으로 예상됨.",
                  "actionGuide": "",
                  "date": "2017-06-10",
                  "cause": "",
                  "typeCode": "O3",
                  "region": [
                    { "name": "서울 ", "value": " 예보없음" },
                    { "name": "제주 ", "value": " 예보없음" },
                    { "name": "전남 ", "value": " 예보없음" },
                    { "name": "전북 ", "value": " 예보없음" },
                    { "name": "광주 ", "value": " 예보없음" },
                    { "name": "경남 ", "value": " 예보없음" },
                    { "name": "경북 ", "value": " 예보없음" },
                    { "name": "울산 ", "value": " 예보없음" },
                    { "name": "대구 ", "value": " 예보없음" },
                    { "name": "부산 ", "value": " 예보없음" },
                    { "name": "충남 ", "value": " 예보없음" },
                    { "name": "충북 ", "value": " 예보없음" },
                    { "name": "세종 ", "value": " 예보없음" },
                    { "name": "대전 ", "value": " 예보없음" },
                    { "name": "영동 ", "value": " 예보없음" },
                    { "name": "영서 ", "value": " 예보없음" },
                    { "name": "경기남부 ", "value": " 예보없음" },
                    { "name": "경기북부 ", "value": " 예보없음" },
                    { "name": "인천 ", "value": " 예보없음" }
                  ]
                },
                {
                  "timeRelease": "2017-06-09 11:00:00",
                  "fcstext": "○[미세먼지] 전 권역의 농도가 전일과 비슷하거나 다소 낮을 것으로 예상됨.",
                  "actionGuide": "",
                  "date": "2017-06-10",
                  "cause": "",
                  "typeCode": "PM10",
                  "region": [
                    { "name": "서울 ", "value": " 예보없음" },
                    { "name": "제주 ", "value": " 예보없음" },
                    { "name": "전남 ", "value": " 예보없음" },
                    { "name": "전북 ", "value": " 예보없음" },
                    { "name": "광주 ", "value": " 예보없음" },
                    { "name": "경남 ", "value": " 예보없음" },
                    { "name": "경북 ", "value": " 예보없음" },
                    { "name": "울산 ", "value": " 예보없음" },
                    { "name": "대구 ", "value": " 예보없음" },
                    { "name": "부산 ", "value": " 예보없음" },
                    { "name": "충남 ", "value": " 예보없음" },
                    { "name": "충북 ", "value": " 예보없음" },
                    { "name": "세종 ", "value": " 예보없음" },
                    { "name": "대전 ", "value": " 예보없음" },
                    { "name": "영동 ", "value": " 예보없음" },
                    { "name": "영서 ", "value": " 예보없음" },
                    { "name": "경기남부 ", "value": " 예보없음" },
                    { "name": "경기북부 ", "value": " 예보없음" },
                    { "name": "인천 ", "value": " 예보없음" }
                  ]
                },
                {
                  "timeRelease": "2017-06-09 11:00:00",
                  "fcstext": "○[미세먼지] 전 권역의 농도가 전일과 비슷하거나 다소 낮을 것으로 예상됨.",
                  "actionGuide": "",
                  "date": "2017-06-10",
                  "cause": "",
                  "typeCode": "PM25",
                  "region": [
                    { "name": "서울 ", "value": " 예보없음" },
                    { "name": "제주 ", "value": " 예보없음" },
                    { "name": "전남 ", "value": " 예보없음" },
                    { "name": "전북 ", "value": " 예보없음" },
                    { "name": "광주 ", "value": " 예보없음" },
                    { "name": "경남 ", "value": " 예보없음" },
                    { "name": "경북 ", "value": " 예보없음" },
                    { "name": "울산 ", "value": " 예보없음" },
                    { "name": "대구 ", "value": " 예보없음" },
                    { "name": "부산 ", "value": " 예보없음" },
                    { "name": "충남 ", "value": " 예보없음" },
                    { "name": "충북 ", "value": " 예보없음" },
                    { "name": "세종 ", "value": " 예보없음" },
                    { "name": "대전 ", "value": " 예보없음" },
                    { "name": "영동 ", "value": " 예보없음" },
                    { "name": "영서 ", "value": " 예보없음" },
                    { "name": "경기남부 ", "value": " 예보없음" },
                    { "name": "경기북부 ", "value": " 예보없음" },
                    { "name": "인천 ", "value": " 예보없음" }
                  ]
                },
                {
                  "timeRelease": "2017-06-09 11:00:00",
                  "fcstext": "○[오존] 전 권역의 농도가 전일과 비슷하거나 다소 낮을 것으로 예상됨.",
                  "actionGuide": "",
                  "date": "2017-06-11",
                  "cause": "",
                  "typeCode": "O3",
                  "region": [
                    { "name": "서울 ", "value": " 예보없음" },
                    { "name": "제주 ", "value": " 예보없음" },
                    { "name": "전남 ", "value": " 예보없음" },
                    { "name": "전북 ", "value": " 예보없음" },
                    { "name": "광주 ", "value": " 예보없음" },
                    { "name": "경남 ", "value": " 예보없음" },
                    { "name": "경북 ", "value": " 예보없음" },
                    { "name": "울산 ", "value": " 예보없음" },
                    { "name": "대구 ", "value": " 예보없음" },
                    { "name": "부산 ", "value": " 예보없음" },
                    { "name": "충남 ", "value": " 예보없음" },
                    { "name": "충북 ", "value": " 예보없음" },
                    { "name": "세종 ", "value": " 예보없음" },
                    { "name": "대전 ", "value": " 예보없음" },
                    { "name": "영동 ", "value": " 예보없음" },
                    { "name": "영서 ", "value": " 예보없음" },
                    { "name": "경기남부 ", "value": " 예보없음" },
                    { "name": "경기북부 ", "value": " 예보없음" },
                    { "name": "인천 ", "value": " 예보없음" }
                  ]
                },
                {
                  "timeRelease": "2017-06-09 11:00:00",
                  "fcstext": "○[미세먼지] 모레 예보는 17시에 발표됩니다.",
                  "actionGuide": "",
                  "date": "2017-06-11",
                  "cause": "",
                  "typeCode": "PM10",
                  "region": [
                    { "name": "서울 ", "value": " 예보없음" },
                    { "name": "제주 ", "value": " 예보없음" },
                    { "name": "전남 ", "value": " 예보없음" },
                    { "name": "전북 ", "value": " 예보없음" },
                    { "name": "광주 ", "value": " 예보없음" },
                    { "name": "경남 ", "value": " 예보없음" },
                    { "name": "경북 ", "value": " 예보없음" },
                    { "name": "울산 ", "value": " 예보없음" },
                    { "name": "대구 ", "value": " 예보없음" },
                    { "name": "부산 ", "value": " 예보없음" },
                    { "name": "충남 ", "value": " 예보없음" },
                    { "name": "충북 ", "value": " 예보없음" },
                    { "name": "세종 ", "value": " 예보없음" },
                    { "name": "대전 ", "value": " 예보없음" },
                    { "name": "영동 ", "value": " 예보없음" },
                    { "name": "영서 ", "value": " 예보없음" },
                    { "name": "경기남부 ", "value": " 예보없음" },
                    { "name": "경기북부 ", "value": " 예보없음" },
                    { "name": "인천 ", "value": " 예보없음" }
                  ]
                },
                {
                  "timeRelease": "2017-06-09 11:00:00",
                  "fcstext": "○[미세먼지] 모레 예보는 17시에 발표됩니다.",
                  "actionGuide": "",
                  "date": "2017-06-11",
                  "cause": "",
                  "typeCode": "PM25",
                  "region": [
                    { "name": "서울 ", "value": " 예보없음" },
                    { "name": "제주 ", "value": " 예보없음" },
                    { "name": "전남 ", "value": " 예보없음" },
                    { "name": "전북 ", "value": " 예보없음" },
                    { "name": "광주 ", "value": " 예보없음" },
                    { "name": "경남 ", "value": " 예보없음" },
                    { "name": "경북 ", "value": " 예보없음" },
                    { "name": "울산 ", "value": " 예보없음" },
                    { "name": "대구 ", "value": " 예보없음" },
                    { "name": "부산 ", "value": " 예보없음" },
                    { "name": "충남 ", "value": " 예보없음" },
                    { "name": "충북 ", "value": " 예보없음" },
                    { "name": "세종 ", "value": " 예보없음" },
                    { "name": "대전 ", "value": " 예보없음" },
                    { "name": "영동 ", "value": " 예보없음" },
                    { "name": "영서 ", "value": " 예보없음" },
                    { "name": "경기남부 ", "value": " 예보없음" },
                    { "name": "경기북부 ", "value": " 예보없음" },
                    { "name": "인천 ", "value": " 예보없음" }
                  ]
                }
              ]
            }
          },
          "common": { "alertYn": "Y", "stormYn": "N" },
          "result": { "code": 9200, "requestUrl": "/weather/airquality/forecast?version=2", "message": "성공" } 
        }
        
### 시도별 대기질정보 [GET /weather/airquality/location/hourly?version={version}&lat={latitude}&lon={longitude}&location={location}]

한국환경공단 시도별 대기환경 관측정보의 시간평균 정보를 제공한다.

Request Parameter는 다음중 한 종류만 선택적으로 사용해야 한다.

(1) lat/lon  (2) location

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보
    + location: 부산 (string, required) = 시도정보(서울,부산,대구,인천,광주,대전,울산,경기,강원,충북,충남,전북,전남,경북,경남,제주,세종)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather (object)
            + airQuality (object) - 대기환경정보
                + location (object) - 시도별 대기질정보
                    + hourly (object) - 시간별 평균정보
                        + name (string) - 시도명
                                - 서울,부산,대구,인천,광주,대전,울산,경기,강원,충북,충남,전북,전남,경북,경남,제주,세종
                        +  so2 (object) - 아황산가스
                            + value (string) - 농도(ppm)
                                    - 0~0.02:좋음, 0.021~0.05:보통, 0.051~0.15:나쁨, 0.151~:매우나쁨
                            + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                        +  co (object) - 일산화탄소
                            + value (string) - 농도(ppm)
                                    - 0~2:좋음, 2.01~9:보통, 9.01~15:나쁨, 15.01~:매우나쁨
                            + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                        +  o3 (object) - 오존
                            + value (string) - 농도(ppm)
                                    - 0~0.03:좋음, 0.031~0.09:보통, 0.091~0.15:나쁨, 0.151~:매우나쁨
                            + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                        +  no2 (object) - 이산화질소
                            + value (string) - 농도(ppm)
                                    - 0~0.03:좋음, 0.031~0.06:보통, 0.061~0.2:나쁨, 0.201~:매우나쁨
                            + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                        +  pm10 (object) - 미세먼지(pm10)
                            + value (string) - 농도(㎍/㎥)
                                    - 0~30:좋음, 31~80:보통, 81~150:나쁨, 151~:매우나쁨
                            + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                        +  pm25 (object) - 미세먼지(pm25)
                            + value (string) - 농도(㎍/㎥)
                                    - 0~15:좋음, 16~50:보통, 51~100:나쁨, 101~:매우나쁨
                            + grade (string) - 등급( 좋음, 보통, 나쁨, 매우나쁨 )
                        + timeObservation (string) - 관측시간
                    
    + Body
        {
          "common": { "alertYn": "Y", "stormYn": "N" },
          "result": { "code": 9200, "requestUrl": "/weather/airquality/location/hourly?version=2&lat=36.1234&lon=127.1234", "message": "성공" },
          "weather": {
            "airQuality": {
              "location": [
                {
                  "hourly": {
                    "name": "충남",
                    "so2": { "grade": "좋음", "value": "0.003" },
                    "co": { "grade": "좋음", "value": "0.4" },
                    "o3": { "grade": "보통", "value": "0.078" },
                    "no2": { "grade": "좋음", "value": "0.009" },
                    "pm10": { "grade": "보통", "value": "59" },
                    "pm25": { "grade": "보통", "value": "30" },
                    "timeObservation": "2017-06-09 17:00:00"
                  }
                }
              ]
            }
          }
        }

# Group ENTERPRISE

## 국지기상 [/weather/enterprise]

국지기상 API는 제휴등급의 개발자에게만 제공합니다.(문의 및 신청: weather@sk.com)

### 실시간 국지기상 [GET /weather/enterprise/observation?version={version}&cellAWS={celLAWS}&lat={latitude}&lon={longitude}]

SKTX의 자동기상관측장비(AWS: Automatic Weather Station)로부터 수집한 기상관측 정보를 분석.가공하여 1분 단위로 현재날씨 정보를 제공한다. 기지국 이외에도 기상청의 관측자료도 함께 제공한다.

Request Parameter는 다음중 한 종류만 선택적으로 사용해야 한다.

(1) lat/lon, radius  (2) stnid  (3) city/county/village

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + cellAWS: 2 (number, optional) - 관측소 유형 ( 0:기상청관측소, 1:SKTX관측소, 2:ALL )
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보
    + radius: 5 (string, optional) - 위경도+반경으로 검색
    + city: 서울 (string, required) - 주소(시/도)
    + county: 강남구 (string, required) - 주소(시/군/구)
    + village: 삼성동 (string, required) - 주소(읍/면/동)
    + stnid: 108 (number) - 관측소 지점번호

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather
            + observation - 관측자료
                + station - 관측소 정보
                    + name (string) - 관측소 이름
                    + id (number) - 관측소 지점번호
                    + type (string) - 관측소 유형(KMA:기상청 관측소, -BTN:SKTX 관측소)
                    + latitude (string) - 관측소 위도
                    + longitude (string) - 관측소 경도
                + sky - 하늘상태 정보
                    + code (string) - 하늘상태 코드
                    + name (string) - 하늘상태 코드명
                         SKY_A01:맑음, SKY_A02구름조금, SKY_A03:구름많음,SKY_A04:구름많고 비, SKY_A05:구름많고 눈, SKY_A06:구름많고 비 또는 눈,
                            SKY_A07:흐림, SKY_A08:흐리고 비, SKY_A09:흐리고 눈, SKY_A10:흐리고 비 또는 눈, SKY_A11 : 흐리고 낙뢰
                            SKY_A12:뇌우/비, SKY_A13:뇌우/눈, SKY_A14:뇌우/비 또는 눈
                + precipitation - 강수정보
                    + type (string) - 강수형태 코드
                                - 0:현상없음  - 1:비  -->  rain(sinceOntime) 사용
                                - 2:비/눈     - 3:눈  -->  precipication(sinceOntime) 사용
                    + sinceOntime (string) - 1시간 누적 강수량
                                - if type=0/1/2  --> 강우량(mm)
                                - if type=3      --> 적설량(cm)
                + rain - 강우정보
                    + last10min (string) - 10분 이동누적 강우량
                    + last15min (string) - 15분 이동누적 강우량
                    + last30min (string) - 30분 이동누적 강우량
                    + last1hour (string) - 1시간 이동누적 강우량
                    + last6hour (string) - 6시간 이동누적 강우량
                    + last12hour (string) - 12시간 이동누적 강우량
                    + last24hour (string) - 24시간 이동누적 강우량
                    + sinceOntime (string) - 1시간 누적 강우량
                    + sinceMidnight (string) - 일 누적 강우량
                + temperature - 기온정보
                    + tc (string) - 현재기온
                    + tmax (string) - 오늘의 최고기온
                    + tmin (string) - 오늘의 최저기온
                + wind - 바람정보
                    + wdir (string) - 풍향(degree)
                    + wspd (string) - 풍속(m/s)
                + humidity (string) - 상대습도(%)
                + pressure - 기압정보
                    + surface (string) - 현지기압(Ps)
                    + seaLevel (string) - 해면기압(SLP)
                + ligntning (string) - 낙뢰유무(관측소 5km반경 내)
                               - 0:없음,   - 1:있음
                + timeObservation (string) - 관측시간
                    
    + Body
        {
          "common": { "alertYn": "Y", "stormYn": "N" },
          "result": { "code": 9200, "requestUrl": "/weather/enterprise/observation?version=2&lat=36.1234&lon=127.1234", "message": "성공" },
          "weather": {
            "minutely": [
              {
                "lightning": "0",
                "timeObservation": "2017-06-09 18:28:00",
                "station": { "longitude": "127.0901000000", "latitude": "36.1366000000", "name": "연무", "id": "644", "type": "KMA" },
                "wind": { "wdir": "235.60", "wspd": "2.00"
                },
                "precipitation": { "sinceOntime": "0.00", "type": "0" },
                "sky": { "code": "SKY_A01", "name": "맑음" },
                "rain": {
                  "sinceOntime": "0.00",
                  "sinceMidnight": "0.00",
                  "last10min": "0.00",
                  "last15min": "0.00",
                  "last30min": "0.00",
                  "last1hour": "0.00",
                  "last6hour": "0.00",
                  "last12hour": "0.00",
                  "last24hour": "0.00"
                },
                "temperature": { "tc": "23.90", "tmax": "27.00", "tmin": "15.00" },
                "humidity": "",
                "pressure": { "surface": "", "seaLevel": "" }
              }
            ]
          }
        }

### 과거 관측자료 [GET /weather/enterprise/history?version={version}&lat=latitude}&lon={longitude}&isTimeRange={isTimeRange}&start={startTime}&end={endTime}]

자동기상관측장비(AWS: Automatic Weather Station)로부터 수집한 기상관측 정보를 분석.가공하여 1분 단위의 과거날씨 정보를 제공합니다.(과거 24시간까지)

Request Parameter는 다음중 한 종류만 선택적으로 사용해야 한다.

(1) lat/lon  (2) stnid

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + stnid: 108 (number) - 관측소 지점번호
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보
    + isTimeRange: Y (string, optional) - 관측자료 구간설정 ( N:과거24시간~현재, Y:과거 특정구간 자료 )
    + startTime: 201606091400 (string, optional) - 시작시간(년월일시분, 현재부터 과거24시간내)
    + endTime: 201606091500 (string, optional) - 종료시간(년월일시분)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather
            + minutely
                + station - 관측소 정보
                    + name (string) - 관측소 이름
                    + id (number) - 관측소 지점번호
                    + type (string) - 관측소 유형(KMA:기상청 관측소, -BTN:SKTX 관측소)
                    + latitude (string) - 관측소 위도
                    + longitude (string) - 관측소 경도
                + aws - 관측자료(요청구간:~1440개)
                    + sky - 하늘상태 정보
                        + code (string) - 하늘상태 코드
                        + name (string) - 하늘상태 코드명
                             SKY_A01:맑음, SKY_A02구름조금, SKY_A03:구름많음,SKY_A04:구름많고 비, SKY_A05:구름많고 눈, SKY_A06:구름많고 비 또는 눈,
                                SKY_A07:흐림, SKY_A08:흐리고 비, SKY_A09:흐리고 눈, SKY_A10:흐리고 비 또는 눈, SKY_A11 : 흐리고 낙뢰
                                SKY_A12:뇌우/비, SKY_A13:뇌우/눈, SKY_A14:뇌우/비 또는 눈
                    + precipitation - 강수정보
                        + type (string) - 강수형태 코드
                                    - 0:현상없음  - 1:비  -->  rain(sinceOntime) 사용
                                    - 2:비/눈     - 3:눈  -->  precipication(sinceOntime) 사용
                        + sinceOntime (string) - 1시간 누적 강수량
                                    - if type=0/1/2  --> 강우량(mm)
                                    - if type=3      --> 적설량(cm)
                    + rain - 강우정보
                        + last10min (string) - 10분 이동누적 강우량
                        + last15min (string) - 15분 이동누적 강우량
                        + last30min (string) - 30분 이동누적 강우량
                        + last1hour (string) - 1시간 이동누적 강우량
                        + last6hour (string) - 6시간 이동누적 강우량
                        + last12hour (string) - 12시간 이동누적 강우량
                        + last24hour (string) - 24시간 이동누적 강우량
                        + sinceOntime (string) - 1시간 누적 강우량
                        + sinceMidnight (string) - 일 누적 강우량
                    + temperature - 기온정보
                        + tc (string) - 현재기온
                        + tmax (string) - 오늘의 최고기온
                        + tmin (string) - 오늘의 최저기온
                    + wind - 바람정보
                        + wdir (string) - 풍향(degree)
                        + wspd (string) - 풍속(m/s)
                    + humidity (string) - 상대습도(%)
                    + pressure - 기압정보
                        + surface (string) - 현지기압(Ps)
                        + seaLevel (string) - 해면기압(SLP)
                    + ligntning (string) - 낙뢰유무(관측소 5km반경 내)
                                   - 0:없음,   - 1:있음
                    + timeObservation (string) - 관측시간
                    
    + Body
        {
          "common": { "alertYn": "Y", "stormYn": "N" },
          "result": { "code": 9200, "requestUrl": "/weather/enterprise/history?version=2&lat=37.507486&lon=127.051496&isTimeRange=Y&start=201706112100&end=201706112104", "message": "성공" },
          "weather": {
            "observation": [
              {
                "station": {
                  "longitude": "127.0530691667",
                  "latitude": "37.5122347222",
                  "name": "봉은사",
                  "id": "10019",
                  "type": "BTN"
                },
                "aws": [
                  {
                    "wind": { "wdir": "313.20", "wspd": "0.40" },
                    "precipitation": { "sinceOntime": "0.00", "type": "0" },
                    "sky": { "code": "SKY_A01", "name": "맑음" },
                    "rain": { "sinceOntime": "0.00", "sinceMidnight": "0.00", "last10min": "0.00", "last15min": "0.00", "last30min": "0.00", 
                              "last1hour": "0.00", "last6hour": "0.00", "last12hour": "0.00", "last24hour": "0.00" },
                    "temperature": "24.50",
                    "humidity": "42.50",
                    "pressure": { "surface": "1002.50", "seaLevel": "1009.42" },
                    "lightning": "0",
                    "timeObservation": "2017-06-11 21:00:00",
                    "stationId": "10019"
                  },
                  {
                    "wind": { "wdir": "16.60", "wspd": "1.50" },
                    "precipitation": { "sinceOntime": "0.00", "type": "0" },
                    "sky": { "code": "SKY_A01", "name": "맑음" },
                    "rain": { "sinceOntime": "0.00", "sinceMidnight": "0.00", "last10min": "0.00", "last15min": "0.00", "last30min": "0.00",
                              "last1hour": "0.00", "last6hour": "0.00", "last12hour": "0.00", "last24hour": "0.00" },
                    "temperature": "24.50",
                    "humidity": "42.50",
                    "pressure": { "surface": "1002.50", "seaLevel": "1009.42" },
                    "lightning": "0",
                    "timeObservation": "2017-06-11 21:01:00",
                    "stationId": "10019"
                  },
                  {
                    "wind": { "wdir": "32.30", "wspd": "1.30" },
                    "precipitation": { "sinceOntime": "0.00", "type": "0" },
                    "sky": { "code": "SKY_A01", "name": "맑음" },
                    "rain": { "sinceOntime": "0.00", "sinceMidnight": "0.00", "last10min": "0.00", "last15min": "0.00", "last30min": "0.00",
                      "last1hour": "0.00", "last6hour": "0.00", "last12hour": "0.00", "last24hour": "0.00" },
                    "temperature": "24.50",
                    "humidity": "42.30",
                    "pressure": { "surface": "1002.50", "seaLevel": "1009.42" },
                    "lightning": "0",
                    "timeObservation": "2017-06-11 21:02:00",
                    "stationId": "10019"
                  },
                  {
                    "wind": { "wdir": "27.70", "wspd": "1.10" },
                    "precipitation": { "sinceOntime": "0.00", "type": "0" },
                    "sky": { "code": "SKY_A01", "name": "맑음" },
                    "rain": { "sinceOntime": "0.00", "sinceMidnight": "0.00", "last10min": "0.00", "last15min": "0.00", "last30min": "0.00",
                              "last1hour": "0.00", "last6hour": "0.00", "last12hour": "0.00", "last24hour": "0.00" },
                    "temperature": "24.50",
                    "humidity": "42.30",
                    "pressure": { "surface": "1002.50", "seaLevel": "1009.42" },
                    "lightning": "0",
                    "timeObservation": "2017-06-11 21:03:00",
                    "stationId": "10019"
                  },
                  {
                    "wind": { "wdir": "17.50", "wspd": "1.40" },
                    "precipitation": { "sinceOntime": "0.00", "type": "0" },
                    "sky": { "code": "SKY_A01", "name": "맑음" },
                    "rain": { "sinceOntime": "0.00", "sinceMidnight": "0.00", "last10min": "0.00", "last15min": "0.00", "last30min": "0.00",
                              "last1hour": "0.00", "last6hour": "0.00", "last12hour": "0.00", "last24hour": "0.00" },
                    "temperature": "24.50",
                    "humidity": "42.30",
                    "pressure": { "surface": "1002.50", "seaLevel": "1009.42" },
                    "lightning": "0",
                    "timeObservation": "2017-06-11 21:04:00",
                    "stationId": "10019"
                  }
                ]
              }
            ]
          }
        }

## HYPER-LOCAL 예보 [/weather/enterprise/hl-forecast/]

HYPER-LOCAL 예보 API는 제휴등급의 개발자에게만 제공합니다.(문의 및 신청: weather@sk.com)

### 단기예보 [GET /weather/enterprise/hl-forecast/short-range?version={version}&lat={latitude}&lon={longitude}]

SK테크엑스의 고해상도 기상정보 플랫폼을 사용하여 1시간 단위의 자체예보 정보를 제공한다. 

1일 2회(06:30, 18:30) 수도권 SKTX AWS 및 기상청 AWS 기준단위로 단기예보 정보를 제공하며, 발표시간 기준 +3H~27H까지이 예보정보를 제공한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather
            + hl_forecast (object) - Weather Planet 자체예보
                + station - 관측소 정보
                    + name (string) - 관측소 이름
                    + id (number) - 관측소 지점번호
                    + type (string) - 관측소 유형(KMA:기상청 관측소, -BTN:SKTX 관측소)
                    + latitude (string) - 관측소 위도
                    + longitude (string) - 관측소 경도
                + fcst1hour - 1시간에보(에보간격:1시간)
                    + sky - 하늘상태 정보(N개, N=3,4,5,6,...,27)
                        + codeNhour (string) - 하늘상태 코드(발표시간+N시간)
                        + nameNhour (string) - 하늘상태 코드명(발표시간+N시간)
                             SKY_S01:맑음, SKY_S02구름조금, SKY_S03:구름많음,SKY_S04:구름많고 비, SKY_S05:구름많고 눈, SKY_S06:구름많고 비 또는 눈,
                                SKY_S07:흐림, SKY_S08:흐리고 비, SKY_S09:흐리고 눈, SKY_S10:흐리고 비 또는 눈, SKY_S11 : 흐리고 낙뢰
                                SKY_S12:뇌우/비, SKY_S13:뇌우/눈, SKY_S14:뇌우/비 또는 눈
                    + precipitation - 강수정보(N개, N=3,4,5,6,...,27)
                        + typeNhour (string) - 강수형태 코드(발표시간+N시간)
                                    - 0:현상없음,  - 1:비,   - 2:비/눈,   - 3:눈
                        + probNhour (string) - 강수확률(%)(발표시간+N시간)
                    + temperature - 기온정보(N개, N=3,4,5,6,...,27)
                        + tempNhour (string) - 기온(발표시간+N시간)
                    + wind - 바람정보(N개, N=3,4,5,6,...,27)
                        + wdirNhour (string) - 풍향(발표시간+N시간)
                        + wspdNhour (string) - 풍속(발표시간+N시간)
                    + humidity  - 습도정보(N개, N=3,4,5,6,...,27)
                        + rhNhour (string) - 상대습도(발표시간+N시간)
                    + pressure - 기압정보(N개, N=3,4,5,6,...,27)
                        +  seaLevelNhour (string) - 해면기압(발표시간+N시간)
                + fcst6hour - 6시간예보(예보간격:6시간, 적용요소:강우량)
                    + rainMhour (string) - 6시간 누적 강우량(발표시간+M시간, M=6,12,18,24)
                            - value: 없음, 1mm미만, 1~4mm, 5~9mm, 10~19mm, 20~39m, 40~69mm, 70mm이상
                + fcstdaily - 24시간 예보(예보간견:24시간, 적용요소:최고/최저기온)
                    + temperature - 기온정보(X=1,2)
                        + tmaxXday (string) - 일 최고기온(오늘, 내일)
                        + tminXday (string) - 일 최저기온(오늘, 내일)
                + timeRelease (string) - 발표시간
                
    + Body
        {
            "common":{ "alertYn":"Y", "stormYn":"N"
            },
            "result":{ "code":9200, "requestUrl":"/weather/hl-forecast/short-range?lon=127.051496&lat=37.507486&version=2", "message":"성공"
            },
            "weather":{
                "hl-forecast":[
                    {
                        "station":{ "latitude":"37.5122347222", "longitude":"127.0530691667", "name":"봉은사", "id":"10019", "type":"BTN" },
                        "timeRelease":"2017-06-12 18:00:00",
                        "fcst6hour":{ "rain6hour":"없음", "rain12hour":"없음", "rain18hour":"없음", "rain24hour":"1~4mm" },
                        "fcstdaily":{
                            "temperature":{ "tmax1day":"26.00", "tmax2day":"25.00", "tmin1day":"18.00", "tmin2day":"18.00" }
                        },
                        "fcst1hour":{
                            "wind":{ "wdir3hour":"282.00", "wspd3hour":"1.70", "wdir4hour":"173.00", "wspd4hour":"1.47", "wdir7hour":"196.00", "wspd7hour":"1.03",
                                     "wdir10hour":"114.00", "wspd10hour":"1.10", "wdir13hour":"180.00", "wspd13hour":"1.17", "wdir16hour":"270.00", "wspd16hour":"1.50",
                                     "wdir19hour":"283.00", "wspd19hour":"2.03", "wdir22hour":"278.00", "wspd22hour":"2.57", "wdir25hour":"247.00", "wspd25hour":"2.90",
                                     "wdir5hour":"173.00", "wdir6hour":"173.00", "wdir8hour":"196.00", "wdir9hour":"196.00", "wdir11hour":"114.00", "wdir12hour":"114.00",
                                     "wdir14hour":"180.00", "wdir15hour":"180.00", "wdir17hour":"270.00", "wdir18hour":"270.00", "wdir20hour":"283.00", "wdir21hour":"283.00",
                                     "wdir23hour":"278.00", "wdir24hour":"278.00", "wdir26hour":"247.00", "wdir27hour":"247.00", "wspd5hour":"1.23", "wspd6hour":"1.00",
                                     "wspd8hour":"1.07", "wspd9hour":"1.10", "wspd11hour":"1.10", "wspd12hour":"1.10", "wspd14hour":"1.23", "wspd15hour":"1.30", "wspd17hour":"1.70",
                                     "wspd18hour":"1.90", "wspd20hour":"2.17", "wspd21hour":"2.30", "wspd23hour":"2.83", "wspd24hour":"3.10", "wspd26hour":"2.70", "wspd27hour":"2.50"
                            },
                            "precipitation":{ "type3hour":"0", "type4hour":"0", "prob4hour":"20.00", "type7hour":"0", "prob7hour":"20.00", "type10hour":"0", "prob10hour":"20.00",
                                              "type13hour":"0", "prob13hour":"20.00", "type16hour":"0", "prob16hour":"20.00", "type19hour":"1", "prob19hour":"60.00", "type22hour":"0",
                                              "prob22hour":"20.00", "type25hour":"0", "prob25hour":"10.00", "type5hour":"0", "type6hour":"0", "type8hour":"0", "type9hour":"0",
                                              "type11hour":"0", "type12hour":"0", "type14hour":"0", "type15hour":"0", "type17hour":"0", "type18hour":"0", "type20hour":"1",
                                              "type21hour":"1", "type23hour":"0", "type24hour":"0", "type26hour":"0", "type27hour":"0", "prob3hour":"20.00", "prob5hour":"20.00",
                                              "prob6hour":"20.00", "prob8hour":"20.00", "prob9hour":"20.00", "prob11hour":"20.00", "prob12hour":"20.00", "prob14hour":"20.00",
                                              "prob15hour":"20.00", "prob17hour":"20.00", "prob18hour":"20.00", "prob20hour":"60.00", "prob21hour":"60.00", "prob23hour":"20.00",
                                              "prob24hour":"20.00", "prob26hour":"10.00", "prob27hour":"10.00"
                            },
                            "sky":{ "code3hour":"SKY_S03", "name3hour":"구름많음", "code4hour":"SKY_S03", "name4hour":"구름많음", "code7hour":"SKY_S03", "name7hour":"구름많음",
                                    "code10hour":"SKY_S03", "name10hour":"구름많음", "code13hour":"SKY_S03", "name13hour":"구름많음", "code16hour":"SKY_S03", "name16hour":"구름많음",
                                    "code19hour":"SKY_S08", "name19hour":"흐리고 비", "code22hour":"SKY_S03", "name22hour":"구름많음", "code25hour":"SKY_S02", "name25hour":"구름조금",
                                    "code5hour":"SKY_S03", "code6hour":"SKY_S03", "code8hour":"SKY_S03", "code9hour":"SKY_S03", "code11hour":"SKY_S03", "code12hour":"SKY_S03",
                                    "code14hour":"SKY_S03", "code15hour":"SKY_S03", "code17hour":"SKY_S03", "code18hour":"SKY_S03", "code20hour":"SKY_S08", "code21hour":"SKY_S08",
                                    "code23hour":"SKY_S03", "code24hour":"SKY_S03", "code26hour":"SKY_S02", "code27hour":"SKY_S02", "name5hour":"구름많음", "name6hour":"구름많음",
                                    "name8hour":"구름많음", "name9hour":"구름많음", "name11hour":"구름많음", "name12hour":"구름많음", "name14hour":"구름많음", "name15hour":"구름많음",
                                    "name17hour":"구름많음","name18hour":"구름많음", "name20hour":"흐리고 비", "name21hour":"흐리고 비", "name23hour":"구름많음", "name24hour":"구름많음",
                                    "name26hour":"구름조금", "name27hour":"구름조금"
                            },
                            "temperature":{ "temp3hour":"22.00", "temp4hour":"21.67", "temp7hour":"20.67", "temp10hour":"19.67", "temp13hour":"20.00", "temp16hour":"23.00",
                                            "temp19hour":"25.00", "temp22hour":"24.33", "temp25hour":"22.33", "temp5hour":"21.33", "temp6hour":"21.00", "temp8hour":"20.33",
                                            "temp9hour":"20.00", "temp11hour":"19.33", "temp12hour":"19.00", "temp14hour":"21.00", "temp15hour":"22.00", "temp17hour":"24.00",
                                            "temp18hour":"25.00", "temp20hour":"25.00", "temp21hour":"25.00", "temp23hour":"23.67", "temp24hour":"23.00","temp26hour":"21.67", "temp27hour":"21.00"
                            },
                            "humidity":{ "rh3hour":"50.00", "rh4hour":"55.00", "rh7hour":"65.00", "rh10hour":"66.67", "rh13hour":"66.67", "rh16hour":"58.33", "rh19hour":"50.00",
                                         "rh22hour":"43.33", "rh25hour":"53.33", "rh5hour":"60.00", "rh6hour":"65.00", "rh8hour":"65.00", "rh9hour":"65.00", "rh11hour":"68.33",
                                         "rh12hour":"70.00", "rh14hour":"63.33", "rh15hour":"60.00", "rh17hour":"56.67", "rh18hour":"55.00", "rh20hour":"45.00", "rh21hour":"40.00",
                                         "rh23hour":"46.67", "rh24hour":"50.00", "rh26hour":"56.67", "rh27hour":"60.00"
                            },
                            "pressure":{ "surface3hour":"", "surface4hour":"", "surface5hour":"", "surface6hour":"", "surface7hour":"", "surface8hour":"", "surface9hour":"",
                                         "surface10hour":"", "surface11hour":"", "surface12hour":"", "surface13hour":"", "surface14hour":"", "surface15hour":"", "surface16hour":"",
                                         "surface17hour":"", "surface18hour":"", "surface19hour":"", "surface20hour":"", "surface21hour":"", "surface22hour":"", "surface23hour":"",
                                         "surface24hour":"", "surface25hour":"", "surface26hour":"", "surface27hour":""
                            }
                        }
                    }
                ]
            }
        }
    
### 중기예보 [GET /weather/enterprise/hl-forecast/mid-range/]

TBD

# Group PREMIUM

## Premium 날씨정보 [/weather/premium]

Pemium 날씨정보 API는 제휴등급의 개발자에게만 제공합니다.(문의 및 신청: weather@sk.com)

### 현재날씨(30분별) [GET /weather/premium/30minutely?version={version}&lat={latitude}&lon={longitude}]

기상청의 기상실황과 SK테크엑스의 고해상도 기상관측망 정보를 사용하여 사용하여 30분 단위의 현재날씨 정보를 제공한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather
            + 30minutely - 현재날씨(30분별)
                + grid - 격자 정보
                    + city (string) - 시(특별,광역), 도
                    + county (string) - 시, 군, 구
                    + village (string) - 읍, 면, 동
                    + latitude (string) - 격자중심 위도
                    + longitude (string) - 격자중심 경도
                + sky - 하늘상태 정보
                    + code (string) - 하늘상태 코드
                    + name (string) - 하늘상태 코드명
                         SKY_P01:맑음, SKY_P02구름조금, SKY_P03:구름많음,SKY_P04:구름많고 비, SKY_P05:구름많고 눈, SKY_P06:구름많고 비 또는 눈,
                            SKY_P07:흐림, SKY_P08:흐리고 비, SKY_P09:흐리고 눈, SKY_P10:흐리고 비 또는 눈, SKY_P11 : 흐리고 낙뢰
                            SKY_P12:뇌우/비, SKY_P13:뇌우/눈, SKY_P14:뇌우/비 또는 눈
                + precipitation - 강수정보
                    + type (string) - 강수형태 코드
                                - 0:현상없음,  - 1:비,   - 2:비/눈,   - 3:눈
                    + sinceOntime (string) - 1시간 누적 강수량
                                - if type=0/1/2  --> 강우량(mm)
                                - if type=3      --> 적설량(cm)
                + temperature - 기온정보
                    + tc (string) - 현재기온
                    + tmax (string) - 오늘의 최고기온
                    + tmin (string) - 오늘의 최저기온
                + wind - 바람정보
                    + wdir (string) - 풍향(degree)
                    + wspd (string) - 풍속(m/s)
                + humidity (string) - 상대습도(%)
                + ligntning (string) - 낙뢰유무(해당격자내)
                               - 0:없음,   - 1:있음
                + timeRelease (string) - 발표시간
    + Body
        {
          "common": { "alertYn": "Y", "stormYn": "N" },
          "result": { "code": 9200, "requestUrl": "/weather/premium/30minutely?version=2&lat=37.507486&lon=127.051496", "message": "성공" },
          "weather": {
            "30minutely": [
              {
                "grid": { "longitude": "127.0530691667", "latitude": "37.5122347222", "city": "서울", "county": "강남구", "village": "삼성2동" },
                "wind": { "wdir": "286.10", "wspd": "2.30" },
                "precipitation": { "sinceOntime": "0.00", "type": "0" },
                "sky": { "code": "SKY_P07", "name": "흐림" },
                "temperature": { "tc": "24.10", "tmax": "26.00", "tmin": "18.00" },
                "humidity": "52.50",
                "lightning": "0",
                "timeRelease": "2017-06-13 11:00:00"
              }
            ]
          }
        }
        
### 현재날씨(시간별) [GET /weather/premium/hourly?version={version}&lat={latitude}&lon={longitude}]

기상청의 기상실황과 SK테크엑스의 고해상도 기상관측망 정보를 사용하여 사용하여 1시간 단위의 현재날씨 정보를 제공한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + common
            + alertYn (string) - 특보 존재유무(한반도 전역 기준)
                                - Y:특보있음, N: 특보없음
            + stormYn (string) - 태풍 존재유무(경계구역 기준)
                                - Y:태풍있음, N:태풍없음
        + weather
            + hourly - 현재날씨(시간별)
                + grid - 격자 정보
                    + city (string) - 시(특별,광역), 도
                    + county (string) - 시, 군, 구
                    + village (string) - 읍, 면, 동
                    + latitude (string) - 격자중심 위도
                    + longitude (string) - 격자중심 경도
                + sky - 하늘상태 정보
                    + code (string) - 하늘상태 코드
                    + name (string) - 하늘상태 코드명
                         SKY_P01:맑음, SKY_P02구름조금, SKY_P03:구름많음,SKY_P04:구름많고 비, SKY_P05:구름많고 눈, SKY_P06:구름많고 비 또는 눈,
                            SKY_P07:흐림, SKY_P08:흐리고 비, SKY_P09:흐리고 눈, SKY_P10:흐리고 비 또는 눈, SKY_P11 : 흐리고 낙뢰
                            SKY_P12:뇌우/비, SKY_P13:뇌우/눈, SKY_P14:뇌우/비 또는 눈
                + precipitation - 강수정보
                    + type (string) - 강수형태 코드
                                - 0:현상없음,  - 1:비,   - 2:비/눈,   - 3:눈
                    + sinceOntime (string) - 1시간 누적 강수량
                                - if type=0/1/2  --> 강우량(mm)
                                - if type=3      --> 적설량(cm)
                + temperature - 기온정보
                    + tc (string) - 현재기온
                    + tmax (string) - 오늘의 최고기온
                    + tmin (string) - 오늘의 최저기온
                + wind - 바람정보
                    + wdir (string) - 풍향(degree)
                    + wspd (string) - 풍속(m/s)
                + humidity (string) - 상대습도(%)
                + ligntning (string) - 낙뢰유무(해당격자내)
                               - 0:없음,   - 1:있음
                + timeRelease (string) - 발표시간
    + Body
        {
          "common": { "alertYn": "Y", "stormYn": "N" },
          "result": { "code": 9200, "requestUrl": "/weather/premium/hourly?version=2&lat=37.507486&lon=127.051496", "message": "성공" },
          "weather": {
            "hourly": [
              {
                "grid": { "longitude": "127.0530691667", "latitude": "37.5122347222", "city": "서울", "county": "강남구", "village": "삼성2동" },
                "lightning": "0",
                "wind": { "wdir": "286.10", "wspd": "2.30" },
                "precipitation": { "sinceOntime": "0.00", "type": "0" },
                "sky": { "code": "SKY_P07", "name": "흐림" },
                "temperature": { "tc": "24.10", "tmax": "26.00", "tmin": "18.00"},
                "humidity": "52.50",
                "timeRelease": "2017-06-13 11:00:00"
              }
            ]
          }
        }
        
# Group GLOBAL

## 세계날씨정보 [/gweather]

전 세계 주요 도시의 현재날씨 및 단기,중기예보를 제공합니다.

세계날씨정보 API는 제휴등급의 개발자에게만 제공합니다.(문의 및 신청: weather@sk.com)

### 현재날씨 [GET /gweather/current?version={version}&lat={latitude}&lon={longitude}&units={units}&timezone={timezone}]

전 세계 주요도시의 관측소로부터 수집한 기상관측 정보를 분석, 가공하여 현재날씨 정보로 제공한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보
    + units: celsius (string, optional) - 기온단위(fehrenheit, celsius)
    + timezone: local (string, optional) - timezone(utc, local)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + gweather
            + current - 현재날씨
                + location - 관측소정보
                    + country (string) - 국가명 
                    + city (string) - 도시명
                    + latitude (string) - 관측소 위도
                    + longitude (string) - 관측소 경도
                + sun - 일출일몰 정보
                    + rise (string) - 일출시간
                    + set (string) - 일몰시간
                + sky - 하늘상태 정보
                    + code (string) - 하늘상태 코드
                    + name (string) - 하늘상태 코드명
                    + icon (string) - 아이콘번호
                + precipitation - 강수정보
                    + value (string) - 강수량(강우량 또는 적설량)
                    + type (string) - 강수형태 코드
                                - 0:현상없음,  - 1:비,   - 2:비/눈,   - 3:눈
                    + unit (string) - 강수량 기준(3h: 3시간강수량)
                + temperature - 기온정보
                    + tc (string) - 현재기온
                    + tmax (string) - 오늘의 최고기온
                    + tmin (string) - 오늘의 최저기온
                + wind - 바람정보
                    + speed - 풍속정보
                        + name (string) - 풍속명
                        + value (string) - 풍속(m/s)
                    + direction - 풍향정보
                        + name (string) - 풍향명
                        + value (string) - 풍향
                        + code (string) - 풍향코드
                + humidity (string) - 상대습도(%)
                + pressure (string) - 해면기압(hPa)
                + timeObservation (string) - 관측시간
    + Body
        {
            "result":{
                "message":"성공",
                "code":9200,
                "requestUrl":"/gweather/current?timezone=&lon=126.9658000000&lat=37.5714000000&units=&version=1"
            },
            "gweather":{
                "current":[
                    {
                        "location":{
                            "country":"KR",
                            "city":"Seoul",
                            "latitude":"37.5700000000",
                            "longitude":"126.9800000000"
                        },
                        "sun":{
                            "rise":"2014-05-18T20:19Z",
                            "set":"2014-05-19T10:37Z"
                        },
                        "sky":{
                            "name":"sky is clear",
                            "icon":"1",
                            "code":"800"
                        },
                        "precipitation":{
                            "value":"no",
                            "type":"",
                            "unit":""
                        },
                        "temperature":{
                            "tc":"20.81",
                            "tmax":"20.81",
                            "tmin":"20.81"
                        },
                        "wind":{
                            "speed":{
                                "name":"Gentle Breeze",
                                "value":"4.14"
                            },
                            "direction":{
                                "name":"West-southwest",
                                "value":"240.00",
                                "code":"WSW"
                            }
                        },
                        "humidity":"66.00",
                        "pressure":"1018.66",
                        "timeObservation":"2014-05-19T07:08Z"
                    }
                ]
            }
        }

### 단기예보 [GET /gweather/forecast/short-range?version={version}&lat={latitude}&lon={longitude}&units={units}&timezone={timezone}]

전 세계 주요도시의 단기예보정보를 제공하며, 시간해상도는 3시간, 최대 5일의 예보를 제공한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보
    + units: celsius (string, optional) - 기온단위(fehrenheit, celsius)
    + timezone: local (string, optional) - timezone(utc, local)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + gweather
            + forecastDays - 단기예보
                + location - 예보지점정보
                    + country (string) - 국가명 
                    + city (string) - 도시명
                    + latitude (string) - 지점 위도
                    + longitude (string) - 지점 경도
                + sun - 일출일몰 정보
                    + rise (string) - 일출시간
                    + set (string) - 일몰시간
                + forecast -  예보정보(time1 ~ timeN 반복)
                    + time - 예보시간
                        + from (string) - 예보시작시간
                        + to (string) - 예보종료 시간
                    + sky - 하늘상태 정보
                        + code (string) - 하늘상태 코드
                        + name (string) - 하늘상태 코드명
                        + icon (string) - 아이콘번호
                    + precipitation - 강수정보
                        + value (string) - 강수량(강우량 또는 적설량)
                        + type (string) - 강수형태 코드
                                    - 0:현상없음,  - 1:비,   - 2:비/눈,   - 3:눈
                        + unit (string) - 강수량 기준(3h: 3시간강수량)
                    + temperature - 기온정보
                        + tc (string) - 현재기온
                        + tmax (string) - 오늘의 최고기온
                        + tmin (string) - 오늘의 최저기온
                    + windSpeed - 풍속정보
                        + name (string) - 풍속명
                        + value (string) - 풍속(m/s)
                    + windDirection - 풍향정보
                        + name (string) - 풍향명
                        + value (string) - 풍향
                        + code (string) - 풍향코드
                    + humidity (string) - 상대습도(%)
                    + pressure (string) - 해면기압(hPa)
                + timeRelease (string) - 발표시간
    + Body
        {
            "result":{
                "message":"성공",
                "code":9200,
                "requestUrl":"/gweather/forecast/short-range?timezone=&lon=126.9658000000&lat=37.5714000000&units=&version=1"
            },
            "gweather":{
                "forecastDays":[
                    {
                        "location":{
                            "country":"KR",
                            "city":"Seoul",
                            "latitude":"37.5700000000",
                            "longitude":"126.9800000000"
                        },
                        "sun":{
                            "rise":"2014-05-18T20:19Z",
                            "set":"2014-05-19T10:37Z"
                        },
                        "forecast":[
                            {
                                "time":{
                                    "from":"2014-05-19T06:00Z",
                                    "to":"2014-05-19T09:00Z"
                                },
                                "sky":{
                                    "name":"sky is clear",
                                    "icon":"1",
                                    "code":"800"
                                },
                                "precipitation":{
                                    "value":"",
                                    "type":"",
                                    "unit":""
                                },
                                "temperature":{
                                    "tc":"20.81",
                                    "tmax":"20.81",
                                    "tmin":"20.81"
                                },
                                "humidity":"66.00",
                                "pressure":"1018.66",
                                "windSpeed":{
                                    "name":"Gentle Breeze",
                                    "value":"4.14"
                                },
                                "windDirection":{
                                    "name":"West-southwest",
                                    "value":"240.00",
                                    "code":"WSW"
                                }
                            },
                            {
                                "time":{
                                    "from":"2014-05-24T00:00Z",
                                    "to":"2014-05-24T03:00Z"
                                },
                                "sky":{
                                    "name":"sky is clear",
                                    "icon":"1",
                                    "code":"800"
                                },
                                "precipitation":{
                                    "value":"",
                                    "type":"",
                                    "unit":""
                                },
                                "temperature":{
                                    "tc":"18.51",
                                    "tmax":"18.51",
                                    "tmin":"18.51"
                                },
                                "humidity":"86.00",
                                "pressure":"1024.97",
                                "windSpeed":{
                                    "name":"Calm",
                                    "value":"1.31"
                                },
                                "windDirection":{
                                    "name":"SouthEast",
                                    "value":"138.50",
                                    "code":"SE"
                                }
                            }
                        ],
                        "timeRelease":""
                    }
                ]
            }
        }

### 중기예보 [GET /gweather/forecast/mid-range?version={version}&lat={latitude}&lon={longitude}&units={units}&timezone={timezone}]

전 세계 주요도시의 중기예보정보를 제공하며, 일단위로 최대 14일까지 제공한다.

+ Parameters
    + version: 2 (number, required) - API 버전정보
    + latitude: 36.1234 (string, required) - 위도정보
    + longitude: 127.1234 (string, required) - 경도정보
    + units: celsius (string, optional) - 기온단위(fehrenheit, celsius)
    + timezone: local (string, optional) - timezone(utc, local)

+ Request (application/json)

    + Headers
        Accept-Encoidng: gzip,deflate,sdch
        Accept: application/json
        appKey: xxxxxxxxxxxxxx

+ Response 200 (application/json)

    + Attributes
        + result
            + code (number) - 요청결과
            + message (string) - 메시지
            + requestUrl (string) - Request URL
        + gweather
            + forecastDays - 중기예보
                + location - 예보지점정보
                    + country (string) - 국가명 
                    + city (string) - 도시명
                    + latitude (string) - 지점 위도
                    + longitude (string) - 지점 경도
                + sun - 일출일몰 정보
                    + rise (string) - 일출시간
                    + set (string) - 일몰시간
                + forecast -  예보정보(time1 ~ timeN 반복)
                    + time - 예보일
                    + sky - 하늘상태 정보
                        + code (string) - 하늘상태 코드
                        + name (string) - 하늘상태 코드명
                        + icon (string) - 아이콘번호
                    + precipitation - 강수정보
                        + value (string) - 강수량(강우량 또는 적설량)
                        + type (string) - 강수형태 코드
                                    - 0:현상없음,  - 1:비,   - 2:비/눈,   - 3:눈
                    + temperature - 기온정보
                        + tc (string) - 현재기온
                        + tmax (string) - 오늘의 최고기온
                        + tmin (string) - 오늘의 최저기온
                    + windSpeed - 풍속정보
                        + name (string) - 풍속명
                        + value (string) - 풍속(m/s)
                    + windDirection - 풍향정보
                        + name (string) - 풍향명
                        + value (string) - 풍향
                        + code (string) - 풍향코드
                    + humidity (string) - 상대습도(%)
                    + pressure (string) - 해면기압(hPa)
                + timeRelease (string) - 발표시간
    + Body
        {
            "result":{
                "message":"성공",
                "code":9200,
                "requestUrl":"/gweather/forecast/mid-range?timezone=&lon=126.9658000000&lat=37.5714000000&units=&version=1"
            },
            "gweather":{
                "forecastDays":[
                    {
                        "location":{
                            "country":"KR",
                            "city":"Seoul",
                            "latitude":"37.5700000000",
                            "longitude":"126.9800000000"
                        },
                        "sun":{
                            "rise":"2014-05-18T20:19Z",
                            "set":"2014-05-19T10:37Z"
                        },
                        "forecast":[
                            {
                                "time":"2014-05-19T00:00Z",
                                "sky":{
                                    "name":"sky is clear",
                                    "icon":"1",
                                    "code":"800"
                                },
                                "precipitation":{
                                    "value":"",
                                    "type":""
                                },
                                "temperature":{
                                    "tc":"23.16",
                                    "tmax":"24.06",
                                    "tmin":"12.92"
                                },
                                "humidity":"63.00",
                                "pressure":"1003.30",
                                "windSpeed":{
                                    "name":"Light breeze",
                                    "value":"1.65"
                                },
                                "windDirection":{
                                    "name":"West-southwest",
                                    "value":"243.00",
                                    "code":"WSW"
                                }
                            },
                            {
                                "time":"2014-06-01T00:00Z",
                                "sky":{
                                    "name":"heavy intensity rain",
                                    "icon":"23",
                                    "code":"502"
                                },
                                "precipitation":{
                                    "value":"rain",
                                    "type":"25.51"
                                },
                                "temperature":{
                                    "tc":"20.12",
                                    "tmax":"20.12",
                                    "tmin":"16.39"
                                },
                                "humidity":"0.00",
                                "pressure":"1006.05",
                                "windSpeed":{
                                    "name":"Calm",
                                    "value":"1.44"
                                },
                                "windDirection":{
                                    "name":"East",
                                    "value":"100.00",
                                    "code":"E"
                                }
                            }
                        ],
                        "timeRelease":""
                    }
                ]
            }
        }

# Group 세계날씨 하늘상태 정보

개발자의 편의성을 위해 세계날씨 API의 응답정보 중에 하늘상태 정보를 제공합니다.

<table style="width:100%">
  <tr>
    <th>하늘상태코드</th><th>코드명</th>
    <th>하늘상태코드</th><th>코드명</th>
    <th>하늘상태코드</th><th>코드명</th>
    <th>하늘상태코드</th><th>코드명</th>
  </tr>
  <tr>
    <td>200</td> <td>thunderstorm with light rain </td>
   <td>500</td><td>light rain</td>
   <td>622</td><td>heavy shower snow</td>
   <td>903</td><td>Cold</td>
  </tr>
  <tr>
   <td>201</td><td>thunderstorm with rain</td>
   <td>501</td><td>moderate rain</td>
   <td>701</td><td>Mist</td>
   <td>904</td><td>Hot</td>
  </tr>
  <tr>
   <td>202</td><td>thunderstorm with heavy rain</td>
   <td>502</td><td>heavy intensity rain</td>
   <td>711</td><td>Smoke</td>
   <td>905</td><td>Windy</td>
  </tr>
  <tr>
   <td>210</td><td>light thunderstorm</td>
   <td>503</td><td>very heavy rain</td>
   <td>721</td><td>Haze</td>
   <td>906</td><td>Hail</td>
  </tr>
  <tr>
   <td>211</td><td>thunderstorm</td>
   <td>504</td><td>extreme rain</td>
   <td>731</td><td>Sand/Dust Whirls</td>
   <td>950</td><td>Setting</td>
  </tr>
  <tr>
    <td>212</td><td>heavy thunderstorm</td>
    <td>511</td><td>freezing rain</td>
    <td>741</td><td>Fog</td>
    <td>951</td><td>Calm</td>
  </tr>
  <tr>
    <td>221</td><td>ragged thunderstorm</td>
    <td>520</td><td>light intensity shower rain</td>
    <td>751</td><td>Sand</td>
    <td>952</td><td>Light breeze</td>
  </tr>
  <tr>
    <td>230</td><td>thunderstorm with light drizzle</td>
    <td>521</td><td>shower rain</td>
    <td>761</td><td>Dust</td>
    <td>953</td><td>Gentle Breeze</td>
  </tr>
  <tr>
    <td>231</td><td>thunderstorm with drizzle</td>
    <td>522</td><td>heavy intensity shower rain</td>
    <td>762</td><td>VOLCANIC ASH</td>
    <td>954</td><td>Moderate breeze</td>
  </tr>
  <tr>
    <td>232</td><td>thunderstorm with heavy drizzle</td>
    <td>531</td><td>ragged shower rain</td>
    <td>771</td><td>SQUALLS</td>
    <td>955</td><td>Fresh Breeze</td>
  </tr>
  <tr>
    <td>300</td><td>light intensity drizzle</td>
    <td>600</td><td>light snow</td>
    <td>781</td><td>TORNADO</td>
    <td>956</td><td>Strong breeze</td>
  </tr>
  <tr>
    <td>301</td><td>Drizzle</td>
    <td>601</td><td>Snow</td>
    <td>800</td><td>sky is clear</td>
    <td>957</td><td>High wind, near gale</td>
  </tr>
  <tr>
    <td>302</td><td>heavy intensity drizzle</td>
    <td>602</td><td>heavy snow</td>
    <td>801</td><td>few clouds</td>
    <td>958</td><td>Gale</td>
  </tr>
  <tr>
    <td>310</td><td>light intensity drizzle rain</td>
    <td>611</td><td>Sleet</td>
    <td>802</td><td>scattered clouds</td>
    <td>959</td><td>Severe Gale</td>
  </tr>
  <tr>
    <td>311</td><td>drizzle rain</td>
    <td>612</td><td>shower sleet</td>
    <td>803</td><td>broken clouds</td>
    <td>960</td><td>Storm</td>
  </tr>
  <tr>
    <td>312</td><td>heavy intensity drizzle rain</td>
    <td>615</td><td>light rain and snow</td>
    <td>804</td><td>overcast clouds</td>
    <td>961</td><td>Violent Storm</td>
  </tr>
  <tr>
    <td>313</td><td>shower rain and drizzle</td>
    <td>616</td><td>rain and snow</td>
    <td>900</td><td>Tornado</td>
    <td>962</td><td>Hurricane</td>
  </tr>
  <tr>
    <td>314</td><td>heavy shower rain and drizzle</td>
    <td>620</td><td>light shower snow</td>
    <td>901</td><td>tropical storm</td>
    <td>&nbsp;</td><td>&nbsp;</td>
  </tr>
  <tr>
    <td>321</td><td>shower drizzle</td>
    <td>621</td><td>shower snow</td>
    <td>902</td><td>Hurricane</td>
    <td>&nbsp;</td><td>&nbsp;</td>
  </tr>
</table>
  
# Group Error Codes

<table style="width:100%">
  <tr>
    <th>ID</th>
    <th>Error Code</th> 
    <th>Message</th>
    <th>HTTP Status Code</th>
  </tr>
  <tr>
    <td rowspan="4">Syntax Errors</td>
    <td>9400</td> 
    <td>URI가 변경되었습니다.</td>
    <td>301 Moved Permanently</td>
  </tr>
  <tr>
    <td>9401</td> 
    <td>필수 파라미터가 없습니다.</td>
    <td>400 Bad request</td>
  </tr> 
  <tr>
    <td>9402</td> 
    <td>요청한 URI를 찾을 수 없습니다.</td>
    <td>404 Not found</td>
  </tr>  
  <tr>
    <td>9410</td> 
    <td>파라미터가 유효범위를 초과하였습니다.</td>
    <td>400 Bad request</td>
  </tr> 
  <tr>
    <td rowspan="6">Service Errors</td>
    <td>9403</td> 
    <td>클라이언트 인증에 실패하였습니다.</td>
    <td>401 Not authorized</td>
  </tr>
  <tr>
    <td>9404</td> 
    <td>접근 권한이 없습니다.</td>
    <td>403 Forbidden</td>
  </tr> 
  <tr>
    <td>9405</td> 
    <td>필수 헤더 정보가 존재하지 않습니다.</td>
    <td>412 Precondition failed</td>
  </tr>  
  <tr>
    <td>9420</td> 
    <td>선택한 지점에 대해서는 서비스를 제공하지 않습니다.</td>
    <td></td>
  </tr> 
  <tr>
    <td>9421</td> 
    <td>기상청 기상자료 수신이 지연되고 있습니다. 잠시후 다시 이용해 주세요.</td>
    <td></td>
  </tr> 
   <tr>
    <td>9422</td> 
    <td>SK테크엑스 자료수신이 지연되고 있습니다. 잠시후 다시 이용해 주세요</td>
    <td></td>
  </tr> 
   <tr>
    <td rowspan="3">Server Errors</td>
    <td>9500</td> 
    <td>내부 시스템에서 예기치 않은 오류가 발생하였습니다.</td>
    <td>500 Internal Server Error</td>
  </tr>
  <tr>
    <td>9501</td> 
    <td>서비스 연결에 실패하였습니다.</td>
    <td>502 Bad gateway</td>
  </tr> 
  <tr>
    <td>9502</td> 
    <td>시스템 점검 중입니다.</td>
    <td>503 Service Unavailable</td>
  </tr>
</table>
