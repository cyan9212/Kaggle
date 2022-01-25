## 데이터 전처리
### delivery_time(target label)
`delivery_time` = `actual_delivery_time` - `created_at`  

단위: 초(second)

kdeplot을 그려 데이터의 분포를 시각화 했을 때 그래프가 왼쪽으로 치우처져 있으므로 로그 변환을 수행

### market_id
배달을 시킨 지역을 나타내는 속성  
`store_id`(가게 번호)를 이용하여 987개의 `NaN`값을 채워주었다.
<br>
### order_protocol
주문을 받을 수 있는 방식을 나타내는 아이디
`store_id`별로 묶어서 그룹화 한 뒤 동일한 store_id를 가지는 주문 데이터에서 가장 많은 `order_protocol`을 골라 `NaN`값을 채움
<br>

### total_items
주문에 포함된 아이템(음식) 개수  
scatter_plot을 그려 이상치를 확인 후 제거
<br><br>

### weekday
요일을 나타내는 속성  
`0 1 2 3 4 5 6` => `월 화 수 목 금 토 일`
<br><br>

### is_weekend
주말을 나타내는 속성
해당 주문 날짜가 주말이라면 `True` 아니라면 `False`
<br><br>

### is_holiday
공휴일을 나타내는 속성
해달 주문 날짜가 공휴일이라면 `True` 아니라면 `False`
<br><br>

### month & day & hour
각각 주문할 날짜의 월(month), 일(day), 주문 시간을 나타내는 속성
<br><br>

### estimated_time
다른 모델에서 예측한 배달 시간(초)  
526개의 `NaN`값을 실제 배달에 걸린 시간(delivery_time)과 `선형회귀 모델`을 만들어서 예측
<br><br>

### store_primary_category
식당의 카테고리(italian, asian 등)
`Label_Encoder`를 사용하여 인코딩
`store_id`를 이용하여 대부분의 NaN값을 채워주고 나머지는 `RandomForestClassifier`을 이용하여 NaN값을 채움
<br><br>

### total_onshift & total_busy & total_outstanding_orders
`total_onshift`: 주문이 생성되었을 때 가게로부터 10마일 이내에 있는 배달원들의 수  
`total_busy`: 위 배달원들 중 주문에 관여하고 있는 사람들의 수  
`total_outstanding_orders`: 주문한 가게로부터 10마일 이내에 있는 다른 주문들의 수  
RandomForestRegressor를 사용하여 NaN값을 채움

## 학습모델
`RandomForest`, `XGBoost`, `GradientBoosting`, `LGBM`

## 예측결과
`XGBoostRegressor`를 사용하였을 때 `RMSE = 982`, `under_prediction의 비율=0.32` 로 가장 높은 성능을 보였다.
