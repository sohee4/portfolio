# :pushpin: 중고차 가격 예측하기


</br>

## 1. 목적
- 유/무의미한 정보가 섞여있는 빅데이터에서 유의미한 정보를 추출하여 정확한 중고차 가격을 예측하는 AI 알고리즘 구현
- AI 알고리즘별로 예측 모델의 정확도를 시각화하여 어떤 알고리즘 모델을 사용했을 때 가장 정확한 예측 값이 나오는지 판단
  

</br>

## 2. 기간 & 참여 인원

- 2023년 4월 23일 ~ 5월 13일
- 이소희, 신경훈, 안은정, 정은서, 윤아람, 김준구 총 6인 


</br>

## 3. 사용 기술

  - Pyhon 3.10
  - Pandas 1.5.3
  - Numpy 1.22.4
  - Matplotlib
  - Seaborn
  - Sklearn
  - Linear Regression
  - Random Forest
</br>

## 4. Introduction

- Data:  https://www.kaggle.com/datasets/avikasliwal/used-cars-price-prediction


- Kaggle에서 주최한 "인도 중고차 예측하기" 데이터셋의 train data를 바탕으로 중고차 가격 예측하기


- ***6019*** rows and ***14*** columns 


</br>

## 5. Data Processing



### 5.1. 데이터 살펴보기



<img width="883" alt="data" src="https://user-images.githubusercontent.com/120240261/236746713-df23f1b3-63f0-4158-b9e8-da0cdc5653f5.png">
</br>


### 5.2. 컬럼명 바꾸기


```python
df_data.columns = ['Brand', 'Location', 'Year', 'Driven', 'Fuel', 'Trans', 'Owner', 'Mileage', 'Engine', 'Power', 'Seats', 'Price']
```
</br>


### 5.3. 브랜드명 추출


```python
df_data['Brand'] = df_data.Brand.str.split(' ').str[0]
```
</br>


### 5.4. Mileage, Engine, Power 단위 


- 단위 제거


```python
df_data['Mileage'] = df_data.Mileage.str.split(' ').str[0]
df_data['Engine'] = df_data.Engine.str.split(' ').str[0]
df_data['Power'] = df_data.Power.str.split(' ').str[0]
```
</br> 
  

- 숫자형 변환


```python
df_data['Mileage'] = pd.to_numeric(df_data['Mileage'])
```
</br>


- Fuel_Type = 'CNG', 'LPG' -> Mileage 단위 km/kg
- Fuel_Type = 'Diesel', 'Petrol' -> Mileage 단위 kmpl
- 'CNG', 'LPG'에 각각 1.64, 1.3을 곱해서 kmpl로 단위 통합


```pyhton
df_data['Mileage'][df_data['Fuel'] == 'CNG'] = df_data[df_data['Fuel'] == 'CNG']['Mileage']*1.64
df_data['Mileage'][df_data['Fuel'] == 'LPG'] = df_data[df_data['Fuel'] == 'LPG']['Mileage']*1.3
```
</br>


### 5.5. 이상값 및 결측값 제거


- ['Driven'] 컬럼에 이상값 한 개


<img src="https://user-images.githubusercontent.com/120240261/236746743-ca32f490-93e2-4dd0-b5b4-92aeb68708d7.png" width="40%" alt="driven1" height="40%">  
</br>


- 이상값 제거 후


<img src="https://user-images.githubusercontent.com/120240261/236748667-dd681ab3-b2a7-4bec-b220-15405c8b6212.png" width="40%" alt="driven2" height="40%">
</br>


- 결측값 제거


```python
df_data = df_data.dropna()
```
</br>  
  

### 5.6. One-Hot Encoding


```python
df_data = pd.get_dummies(df_data, columns = ['Brand', 'Location', 'Fuel', 'Trans', 'Owner'])
```
</br>


### 5.7. Heatmap


- 모든 변수 포함


<img src="https://user-images.githubusercontent.com/120240261/236746753-0c98b960-9a87-443f-aa6d-c94eaf2c6ecd.png" alt="heatmap1">
</br>


- 주요 변수 포함


<img src="https://user-images.githubusercontent.com/120240261/236746756-6ceb91cc-ea50-473b-9f84-79d766c3d748.png" alt="heatmap2" width="60%" height="60%">
</br>


## 6. Machine Learning




### 다중공선성 판단


- 회귀 분석에서 하나의 feature(예측 변수)가 다른 feature와의 상관 관계가 높으면, 회귀 분석 시 부정적인 영향을 미칠 수 있기 때문에, 모델링 하기 전에 먼저 다중 공선성의 존재 여부를 확인 필요


- 보통 다중 공선성을 판단할 때 VIF(Variance Inflation Factors)값 이용


- 일반적으로, VIF > 10인 feature들은 다른 변수와의 상관관계가 높아, 다중 공선성이 존재하는 것으로 판단


- 'Engine'의 VIF Factor = 11
 
 
- 'Engine' 컬럼 포함 version: v1 / 미포함 version: v2
 
 
</br>


### 6.1. Linear Regression


<img src="https://github.com/sohee4/portfolio/assets/120240261/b3e399f4-89a6-4a81-a74b-f458fc6ecc07" alt="linear regression" width="70%" height="70%">
</br>


### 6.2. Random Forest Regressor


<img src="https://github.com/sohee4/portfolio/assets/120240261/db072474-6c42-461f-9689-eb53f18a4465" alt="random forest" width="70%" height="70%">
</br>


### 6.3. Decision Tree Regressor


<img src="https://github.com/sohee4/portfolio/assets/120240261/172d2f80-8712-4e68-a7df-86aa3ebfb5ce" alt="decision tree" width="40%" height="40%">
</br>


### 6.4. MLP Regressor


<img src="https://user-images.githubusercontent.com/120240261/236746767-1a2848dc-7a7b-4110-a9ab-3d44c38b9da7.png" alt="MLP regressor" width="40%" height="40%">
</br>


### 6.5. Statsmodels


```
import statsmodels.api as sm
X_train2 = sm.add_constant(X_train)
model2 = sm.OLS(y_train, X_train2).fit()
model2.summary()
```
</br>


## 7. Conclusion

- Results of v1


| v1  |LR|LR log|RF|RF log|DT|MLP|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|accuracy|0.7830|0.9253|0.9820|0.9912|***0.9999***|0.5556|
|r2 score|0.7790|0.8596|***0.9265***|0.8923|0.7712|0.5300|
|rmse|5.1974|17.1682|***2.9959***|3.6290|5.2883|7.5809|






- Results of v2


| v2  |LR|LR log|RF|RF log|DT|MLP|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|accuracy|0.7799|0.9249|0.9816|0.9912|***0.9999***|0.6257|
|r2 score|0.7771|0.8551|***0.9235***|0.8945|0.7385|0.6286|
|rmse|5.2202|4.2086|***3.0584***|3.5909|5.5638|6.7390|

* r2score: 예측 모델과 실제 모델이 얼마나 강한 상관관계를 갖는지 나타내주는 지표






- 'Engine'컬럼을 제거한 후 머신러닝 모델에 대입한 결과가 오히려 미세하게 안좋게 나옴


- v1과 v2에서 각각 가장 좋은 모델은 변하지 않음


- 제일 적합했던 모델: Random Forest Regressor



