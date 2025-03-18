# PYDBFI - DB증권 API Python SDK

https://openapi.db-fi.com


## 설치

pip를 사용하여 설치할 수 있습니다:

```bash
pip install pydbfi
```


## 인증 및 초기화

DB증권 API를 사용하기 위해서는 먼저 appkey, appsecretkey가 필요합니다. 이 키를 사용하여 SDK를 초기화합니다:

```python
from pydbfi import DomesticAPI, OverseasAPI

# 국내 시장 API 초기화
domestic_api = DomesticAPI(app_key="your_app_key", app_secret_key="your_app_secret_key")

# 해외 시장 API 초기화
overseas_api = OverseasAPI(app_key="your_app_key", app_secret_key="your_app_secret_key")
```

로깅 레벨을 조정하려면 `log_level` 파라미터를 사용합니다:

```python
import logging
domestic_api = DomesticAPI(
    app_key="your_app_key", 
    app_secret_key="your_app_secret_key",
    log_level=logging.DEBUG
)
```


## 국내 API (DomesticAPI)

### 주문 관련 기능

#### 주식 매수

```python
result = domestic_api.buy(
    stock_code="005930",  # 종목코드
    quantity=10,          # 수량
    price=60000,          # 가격
    price_type="00",      # 지정가(00)
    credit_type="000",    # 보통
    loan_date="00000000", # 일반주문
    order_condition="0"   # 없음
)
```

#### 주식 매도

```python
result = domestic_api.sell(
    stock_code="005930",  # 종목코드
    quantity=10,          # 수량
    price=60000,          # 가격
    price_type="00",      # 지정가(00)
    credit_type="000",    # 보통
    loan_date="00000000", # 일반주문
    order_condition="0"   # 없음
)
```

#### 주문 취소

```python
result = domestic_api.cancel(
    order_no=12345,        # 원주문번호
    stock_code="005930",   # 종목코드
    quantity=10            # 취소수량
)
```

### 잔고 및 조회 기능

#### 거래 내역 조회

```python
result = domestic_api.get_transaction_history(
    execution_status="0",  # 체결여부 (0:전체, 1:체결, 2:미체결)
    order_type="0",        # 매매구분 (0:전체, 1:매도, 2:매수)
    stock_type="0",        # 종목구분 (0:전체)
    query_type="0",        # 조회구분 (0:전체, 1:ELW, 2:ELW제외)
    cont_yn="N",           # 연속조회여부
    cont_key=None          # 연속조회키
)
```

#### 주식 잔고 조회

```python
result = domestic_api.get_stock_balance(
    query_type="0",        # 조회구분코드 (0:전체, 1:비상장제외, 2:비상장,코넥스,kotc 제외)
    cont_yn="N",           # 연속조회여부
    cont_key=None          # 연속조회키
)
```

#### 예수금 조회

```python
result = domestic_api.get_deposit(
    cont_yn="N",           # 연속조회여부
    cont_key=None          # 연속조회키
)
```

#### 주문 가능 수량 조회

```python
result = domestic_api.get_able_order_quantity(
    stock_code="005930",   # 종목코드
    price=60000,           # 가격
    order_type="2",        # 매매구분 (1:매도, 2:매수)
    cont_yn="N",           # 연속조회여부
    cont_key=None          # 연속조회키
)
```

### 시세 관련 기능

#### 종목 목록 조회

```python
result = domestic_api.get_stock_tickers(
    market_code="J",       # 시장분류코드 (J:주식, E:ETF, EN:ETN)
    cont_yn="N",           # 연속조회여부
    cont_key=None          # 연속조회키
)
```

#### 종목 시세 조회

```python
result = domestic_api.get_stock_price(
    stock_code="005930",   # 종목코드
    market_code="J",       # 시장분류코드 (J:주식, E:ETF, EN:ETN)
    cont_yn="N",           # 연속조회여부
    cont_key=None          # 연속조회키
)
```

### 차트 관련 기능

#### 분봉 차트 조회

```python
result = domestic_api.get_minute_chart(
    stock_code="005930",    # 종목코드
    start_date="20230101",  # 시작일자
    time_interval="60",     # 시간 간격 (60:1분, 300:5분, 600:10분 등)
    market_code="J",        # 시장분류코드 (J:주식)
    adjust_price_yn="0",    # 수정주가 사용 여부 (0:사용, 1:미사용)
    cont_yn="N",            # 연속조회여부
    cont_key=None           # 연속조회키
)
```

#### 일봉 차트 조회

```python
result = domestic_api.get_daily_chart(
    stock_code="005930",    # 종목코드
    start_date="20230101",  # 시작일자
    end_date="20230131",    # 종료일자
    market_code="J",        # 시장분류코드 (J:주식)
    adjust_price_yn="0",    # 수정주가 사용 여부 (0:사용, 1:미사용)
    cont_yn="N",            # 연속조회여부
    cont_key=None           # 연속조회키
)
```

#### 주봉 차트 조회

```python
result = domestic_api.get_weekly_chart(
    stock_code="005930",    # 종목코드
    start_date="20230101",  # 시작일자
    end_date="20230630",    # 종료일자
    market_code="J",        # 시장분류코드 (J:주식)
    adjust_price_yn="0",    # 수정주가 사용 여부 (0:사용, 1:미사용)
    cont_yn="N",            # 연속조회여부
    cont_key=None           # 연속조회키
)
```

#### 월봉 차트 조회

```python
result = domestic_api.get_monthly_chart(
    stock_code="005930",    # 종목코드
    start_date="20230101",  # 시작일자
    end_date="20231231",    # 종료일자
    market_code="J",        # 시장분류코드 (J:주식)
    adjust_price_yn="0",    # 수정주가 사용 여부 (0:사용, 1:미사용)
    cont_yn="N",            # 연속조회여부
    cont_key=None           # 연속조회키
)
```


## 해외 API (OverseasAPI)

### 주문 관련 기능

#### 주식 매수

```python
result = overseas_api.buy(
    stock_code="AAPL",    # 종목코드
    quantity=10,          # 수량
    price=180.0,          # 가격
    price_type="1",       # 지정가(1)
    order_condition="1",  # 일반
    trade_type="0",       # 주문
    original_order_no=0   # 신규주문
)
```

#### 주식 매도

```python
result = overseas_api.sell(
    stock_code="AAPL",    # 종목코드
    quantity=10,          # 수량
    price=180.0,          # 가격
    price_type="1",       # 지정가(1)
    order_condition="1",  # 일반
    trade_type="0",       # 주문
    original_order_no=0   # 신규주문
)
```

#### 주문 취소

```python
result = overseas_api.cancel(
    order_no=12345,       # 원주문번호
    stock_code="AAPL",    # 종목코드
    quantity=10           # 취소수량
)
```

### 잔고 및 조회 기능

#### 거래 내역 조회

```python
from datetime import datetime

today = datetime.now().strftime("%Y%m%d")
week_ago = (datetime.now() - timedelta(days=7)).strftime("%Y%m%d")

result = overseas_api.get_transaction_history(
    start_date=week_ago,      # 조회시작일자 (YYYYMMDD)
    end_date=today,           # 조회종료일자 (YYYYMMDD)
    stock_code="",            # 해외주식종목번호 (빈값은 전체 종목)
    order_type="0",           # 해외주식매매구분코드 (0:전체, 1:매도, 2:매수)
    execution_status="0",     # 주문체결구분코드 (0:전체, 1:체결, 2:미체결)
    sort_type="0",            # 정렬구분코드 (0:역순, 1:정순)
    query_type="0",           # 조회구분코드 (0:합산별, 1:건별)
    online_yn="0",            # 온라인여부 (0:전체, Y:온라인, N:오프라인)
    opposite_order_yn="0",    # 반대매매주문여부 (0:전체, Y:반대매매, N:일반주문)
    won_fcurr_type="1",       # 원화외화구분코드 (1:원화, 2:외화)
    cont_yn="N",              # 연속조회여부
    cont_key=None             # 연속조회키
)
```

#### 주식 잔고 조회

```python
result = overseas_api.get_stock_balance(
    balance_type="2",          # 처리구분코드 (1:외화잔고, 2:주식잔고상세, 3:주식잔고(국가별), 9:당일실현손익)
    cmsn_type="2",             # 수수료구분코드 (0:전부 미포함, 1:매수제비용만 포함, 2:매수제비용+매도제비용)
    won_fcurr_type="2",        # 원화외화구분코드 (1:원화, 2:외화)
    decimal_balance_type="0",  # 소수점잔고구분코드 (0:전체, 1:일반, 2:소수점)
    cont_yn="N",               # 연속조회여부
    cont_key=None              # 연속조회키
)
```

#### 예수금 조회

```python
result = overseas_api.get_deposit(
    cont_yn="N",               # 연속조회여부
    cont_key=None              # 연속조회키
)
```

#### 주문 가능 수량 조회

```python
result = overseas_api.get_able_order_quantity(
    stock_code="AAPL",         # 종목코드
    price=180.0,               # 가격
    trx_type="2",              # 처리구분코드 (1:매도, 2:매수)
    won_fcurr_type="2",        # 원화외화구분코드 (1:원화, 2:외화)
    cont_yn="N",               # 연속조회여부
    cont_key=None              # 연속조회키
)
```

### 시세 관련 기능

#### 종목 목록 조회

```python
result = overseas_api.get_stock_tickers(
    market_code="NY",          # 시장 코드 (NY:뉴욕, NA:나스닥, AM:아멕스)
    cont_yn="N",               # 연속조회여부
    cont_key=None              # 연속조회키
)
```

#### 종목 시세 조회

```python
result = overseas_api.get_stock_price(
    stock_code="AAPL",         # 종목코드
    market_code="FN",          # 시장 코드 (FY:뉴욕, FN:나스닥, FA:아멕스)
    cont_yn="N",               # 연속조회여부
    cont_key=None              # 연속조회키
)
```

### 차트 관련 기능

#### 분봉 차트 조회

```python
result = overseas_api.get_minute_chart(
    stock_code="AAPL",         # 종목코드
    start_date="20230101",     # 시작일자
    end_date="20230102",       # 종료일자
    time_interval="60",        # 시간 간격 (60:1분, 300:5분, 600:10분 등)
    market_code="FN",          # 시장 코드 (FY:뉴욕, FN:나스닥, FA:아멕스)
    adjust_price_yn="1",       # 수정주가 사용 여부 (0:미사용, 1:사용)
    period_specified="Y",      # 기간지정여부코드 (Y:기간지정, N:기간미지정)
    hour_class_code="0",       # 입력시간구분코드 (항상 "0" 입력)
    cont_yn="N",               # 연속조회여부
    cont_key=None              # 연속조회키
)
```

#### 일봉 차트 조회

```python
result = overseas_api.get_daily_chart(
    stock_code="AAPL",         # 종목코드
    start_date="20230101",     # 시작일자
    end_date="20230131",       # 종료일자
    market_code="FN",          # 시장 코드 (FY:뉴욕, FN:나스닥, FA:아멕스)
    adjust_price_yn="1",       # 수정주가 사용 여부 (0:미사용, 1:사용)
    cont_yn="N",               # 연속조회여부
    cont_key=None              # 연속조회키
)
```

#### 주봉 차트 조회

```python
result = overseas_api.get_weekly_chart(
    stock_code="AAPL",         # 종목코드
    start_date="20230101",     # 시작일자
    end_date="20230630",       # 종료일자
    market_code="FN",          # 시장 코드 (FY:뉴욕, FN:나스닥, FA:아멕스)
    use_adjust_price="1",      # 수정주가 사용 여부 (0:미사용, 1:사용)
    cont_yn="N",               # 연속조회여부
    cont_key=None              # 연속조회키
)
```

#### 월봉 차트 조회

```python
result = overseas_api.get_monthly_chart(
    stock_code="AAPL",         # 종목코드
    start_date="20230101",     # 시작일자
    end_date="20231231",       # 종료일자
    market_code="FN",          # 시장 코드 (FY:뉴욕, FN:나스닥, FA:아멕스)
    use_adjust_price="1",      # 수정주가 사용 여부 (0:미사용, 1:사용)
    cont_yn="N",               # 연속조회여부
    cont_key=None              # 연속조회키
)
```

## 세션 종료

API 사용이 끝난 후에는 세션을 종료해야 합니다:

```python
domestic_api.close()
overseas_api.close()
```