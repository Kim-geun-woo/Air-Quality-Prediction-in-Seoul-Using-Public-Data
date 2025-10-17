# 💨 공공데이터를 활용한 서울 지역 미세먼지 농도 예측

본 프로젝트는 **AIVLE AI 개발자 과정 1차 미니 프로젝트**로, 서울 지역 미세먼지(PM10) 농도를 예측하는 **머신러닝 기반 예측 모델** 개발 사례입니다.  
공공 데이터를 활용하여 데이터 전처리, 특성 공학, 모델링, 성능 평가까지 **머신러닝 전반의 파이프라인**을 경험했습니다.

---

## 📌 프로젝트 개요

- **목표**: 서울 지역 미세먼지 데이터와 기상 데이터를 활용하여, 1시간 후 PM10 농도 예측 모델 개발  
- **진행 기간**: 2025.10.15  
- **참여 인원**: 총 7명

---

## 👥 팀 구성 및 개인 역할

| 역할 | 담당자 | 주요 업무 |
|------|--------|-----------|
| 조장 | 팀원 A | 프로젝트 총괄, 일정 관리 |
| 발표자 | 팀원 B | 발표 자료 설명 및 발표 진행 |
| PPT 제작자 | 팀원 C | 발표 자료 제작 |
| 서기 | 본인, 팀원 D | 회의록 작성, 문서 정리 |
| 검토 담당자 | 팀원 E | 코드 및 결과 검토 |
| 타임키퍼 | 팀원 F | 일정 관리, 회의 시간 체크 |

> 🔹 모든 팀원은 개인 파일 3가지를 모두 진행하며, 본인은 **서기로 회의록 작성** 담당

---

## 🛠️ 사용 기술 및 라이브러리

| 분야 | 도구 / 라이브러리 |
|------|-----------------|
| 언어 | Python |
| 데이터 처리 | Pandas, NumPy |
| 시각화 | Matplotlib, Seaborn |
| 모델링 | Scikit-learn (LinearRegression, RandomForestRegressor, GradientBoostingRegressor), XGBoost |
| 모델 평가/튜닝 | Scikit-learn (RandomizedSearchCV), Joblib |

---

## 🧩 프로젝트 프로세스

### 1. 데이터 분석 및 전처리
- **시간 데이터 처리**: 미세먼지(01~24시)와 기상 데이터(00~23시) 기준 통일, 24시 → 익일 00시로 변환  
- **결측치 처리**:  
  - 강수량 결측치는 0mm로 대체  
  - 기상 데이터는 ffill()과 bfill()로 보간  
- **변수 제거**: 위치 정보, QC 플래그, 원본 시간 변수, 결측률 높은 변수(적설, 3시간 신적설), 해석 어려운 코드성 변수(운형, 현상번호) 제거

### 2. 특성 공학 (Feature Engineering)
- **시간 변수 파생**: month, day, hour 숫자형 특성 추출  
- **파생 변수 생성**:  
  - `PM10_lag1`: 24시간 전 PM10 농도  
  - `PM10_1 (Target)`: 1시간 후 PM10 농도 (shift(-1))

### 3. 모델링 및 평가
- **모델 선정**: Linear Regression, Random Forest, Gradient Boosting, XGBoost  
- **하이퍼파라미터 튜닝**: RandomizedSearchCV 사용  
- **성능 평가**: Test 데이터 MSE 및 R² Score 기준

---

## 📊 모델 성능 비교

| 모델 | MSE | R² Score |
|------|-----|----------|
| Linear Regression | 51.56 | 0.9319 |
| Random Forest | 55.44 | 0.9268 |
| Gradient Boosting | 48.32 | 0.9362 |
| XGBoost | 49.87 | 0.9342 |

- **최종 선정 모델**: Gradient Boosting  
- **선정 이유**: 테스트 데이터에서 가장 낮은 MSE와 높은 R²로 예측 성능 최우수

---

## 🔍 핵심 결과 및 시각화

### 📌 예측 결과 시각화
- Gradient Boosting 모델의 예측값(predict)과 실제값(actual) 시각화  
- 전반적인 미세먼지 농도 변화 추세를 성공적으로 예측
  
![예측 결과 시각화](https://github.com/Kim-geun-woo/Air-Quality-Prediction-in-Seoul-Using-Public-Data/raw/main/images/GradientBoosting_visualization.png)

  
### 📌 Feature Importances (Gradient Boosting)
- PM10 (현재 시점 미세먼지)이 가장 중요한 변수  
- PM10_lag1, PM25, hour 등도 예측 영향력 확인
  
![Feature Importances](https://github.com/Kim-geun-woo/Air-Quality-Prediction-in-Seoul-Using-Public-Data/raw/main/images/GradientBoosting_feature_importances.png)

---

## 🖼️ 발표 자료 (Presentation)

[![발표자료 확인](https://github.com/Kim-geun-woo/Air-Quality-Prediction-in-Seoul-Using-Public-Data/raw/main/images/ppt_Air-Quality-Prediction-in-Seoul-Using-Public-Data.png)](https://github.com/Kim-geun-woo/Air-Quality-Prediction-in-Seoul-Using-Public-Data/blob/main/docs/Air-Quality-Prediction-in-Seoul-Using-Public-Data.pdf)

> 📌 썸네일을 클릭하면 PDF GitHub 페이지에서 바로 확인할 수 있습니다.

---

## 🤔 회고 (What I Learned)

- **모델별 특성 중요도**: 단일 모델의 feature_importances_에만 의존하지 않고, 다양한 모델 결과를 종합 분석 필요  
- **PM10 변수 영향력**: 모든 모델에서 현재 시점 PM10이 가장 중요한 변수로 나타남 → 단기 자기 상관성 의존 확인  
- **데이터 정제의 중요성**: 위치 정보, QC 플래그 등 불필요 변수 제거 과정이 성능 향상에 기여  
- **향후 개선점**: 장기 예측 모델 개발 시 기상 변수 영향력 분석 및 도메인 지식을 바탕으로 한 특성 선택 강화


