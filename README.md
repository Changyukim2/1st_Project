# 1st_Project
IM Digital banker Academy 1st project ( Statistical Analysis )

통계학 기반 금융데이터 분석 프로젝트 (4조 - 알짜발굴단)

# iM뱅크 중소기업 세분화 프로젝트

![Python](https://img.shields.io/badge/Python-3.10-blue)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458)
![NumPy](https://img.shields.io/badge/NumPy-Numerical%20Computing-013243)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-Machine%20Learning-F7931E)
![SciPy](https://img.shields.io/badge/SciPy-Statistics-8CAAE6)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557C)
![Seaborn](https://img.shields.io/badge/Seaborn-EDA-4C72B0)
![Status](https://img.shields.io/badge/Project-Completed-brightgreen)

기존 **안정성 중심 기업등급**을 보완하기 위해,  
**활동성(FAI)** 과 **수익성(FPI)** 을 결합한 중소기업 세분화 모델을 설계하고 검증한 프로젝트입니다.

이 프로젝트의 목표는 기존 등급체계만으로는 드러나지 않는  
**숨은 우량 중소기업**과 **잠재 우량고객**을 데이터 기반으로 발굴하는 것이었습니다.

---

## 프로젝트 개요

iM뱅크는 사업보고서 전반에서 **중소기업 지원 확대**와 **지역경제 활성화**를 주요 전략 방향으로 제시하고 있습니다.  
하지만 기존 기업등급체계는 안정성과 신뢰도 중심으로 설계되어 있어, 리스크 관리에는 강점이 있지만  
중소기업 내부의 실제 **영업활동성**, **수익창출력**, **성장 잠재력** 차이를 충분히 드러내기에는 한계가 있었습니다.

이에 따라 본 프로젝트에서는 기존 등급을 유지하되, 이를 보완하는 새로운 관점으로  
**활동성 지수(FAI)** 와 **수익성 지수(FPI)** 를 설계하고,  
기업을 보다 정교하게 세분화하는 모델을 제안했습니다.

---

## 문제 정의

기존 등급체계만으로는 다음과 같은 한계가 존재한다고 보았습니다.

- 동일한 기존 등급 내에서도 실제 영업활동성과 수익성 차이가 클 수 있음
- 자산 규모는 작지만 수익 효율이 높은 기업이 기존 등급에서 충분히 드러나지 않을 수 있음
- 중소기업 지원 전략을 정교화하려면 안정성 외의 추가 지표가 필요함

따라서 본 프로젝트는 다음 질문에서 출발했습니다.

> **기존 안정성 중심 등급체계를 유지하면서, 활동성과 수익성을 결합해 중소기업 집단 내부를 더 정밀하게 구분할 수 있는가?**

---

## 사용 데이터

- 분석 대상: 이상치를 제외한 **0원 ~ 5,000억 원 규모 기업 집단**
- 대상 기준: 법적 기준상 중소기업 범위에 해당하는 기업
- 처리 방식: 전처리 및 이상치 제거를 통해 비교 가능성 확보

---

## 핵심 아이디어

기업을 단순히 **안정적인가** 로만 평가하지 않고, 아래 두 축으로 재해석했습니다.

- **FAI (Firm Activity Index)**  
  기업의 외형적 활동 수준
- **FPI (Firm Profitability Index)**  
  기업의 은행 수익 기여 및 효율 수준

이 두 지수를 결합해 기업을 4개 세그먼트로 분류했습니다.

| Segment | 의미 |
|---|---|
| **Star** | 고활동 · 고수익 |
| **Cash Cow** | 저활동 · 고수익 |
| **Potential** | 고활동 · 저수익 |
| **Standard** | 저활동 · 저수익 |

---

## 지표 설계

### 1. 활동성 지수 (FAI)

활동성 지수는 세 가지 하위 요소를 동일 가중치로 반영했습니다.

#### 규모점수
- 총예금잔액
- 총대출잔액
- 총채널거래금액
- 총외환금액
- 총카드사용금액

#### 빈도점수
- 총채널거래건수
- 총외환거래건수

#### 다양성점수
- 예금, 대출, 카드, 외환 이용 여부를 기반으로 산출한 상품다양성개수

최종 식은 다음과 같습니다.


FAI = (규모점수 + 빈도점수 + 다양성점수) / 3

# 📈 2. 수익성 지수 (FPI: Financial Profitability Index)수익성 지수는 고객의 거래 규모뿐만 아니라, 자산 대비 얼마나 효율적으로 수익을 창출하는지를 반영합니다.수익기여점수: 예대마진, 추정 비이자수익수익효율점수: 자산대비수익률(%)$$FPI = \frac{수익기여점수 + 수익효율점수}{2}$$🔢 3. 점수화 및 세그먼트 분류 방식점수화 절차금융 데이터 특유의 **분포 왜곡(0값 다수, 우편향)**을 해결하기 위해 다음 절차를 적용했습니다.log1p 변환: 왜도(Skewness) 완화MinMaxScaler: 0~1 사이 정규화Score 생성: 변수별 가중치 적용 및 하위 점수 평균 산출세그먼트 분류 (4사분면 모델)활동성 지수(FAI)와 수익성 지수(FPI)의 **중앙값(Median)**을 기준으로 고객군을 4개로 정의했습니다.중앙값 사용 이유: 평균보다 이상치에 강건하며, 0 값이 많은 금융 데이터 분포에서 더 안정적인 기준점이 됨분류 (Segment)정의 (Criteria)특징StarFAI $\ge$ Med, FPI $\ge$ Med활동성과 수익성 모두 높은 우량 고객Cash CowFAI < Med, FPI $\ge$ Med활동량은 적으나 자산 대비 수익 효율이 높은 알짜 고객PotentialFAI $\ge$ Med, FPI < Med활동은 활발하나 실제 수익 연결이 필요한 잠재 고객StandardFAI < Med, FPI < Med관리 및 육성 대상인 일반 고객✅ 검증 및 가설 검정 (Hypothesis Testing)데이터의 등분산성 미충족 및 비정규성을 고려하여 비모수 검정을 수행했습니다.사용한 검정: Kruskal-Wallis H, Dunn's Post-hoc, Mann-Whitney U, Chi-square주요 가설 검정 결과기존 등급과의 관계: 기존 등급별 지수 차이는 유의하나, 등급만으로는 고객의 활동/수익 특성을 완전히 설명하기 어려움 (신규 분류의 필요성 입증).축의 유효성: 고활동/저활동 집단 간 FAI 약 1.8배, 고수익/저수익 집단 간 FPI 약 8.6% 차이로 유의미한 구분 확인.알짜(Cash Cow)의 발견: 스타(Star) 그룹보다 자산 규모는 작지만, 자산 대비 수익 효율은 통계적으로 유의하게 높음을 확인.💡 주요 분석 결과 및 인사이트기존 등급체계의 한계 보완: 기존 등급은 '안정성' 판단에는 유효하나, 기업 내부의 실제 '활동성'과 '효율성' 차이를 세밀하게 포착하지 못함.숨은 우량고객(Hidden Champions) 발굴: 기존 '일반' 등급이면서 Cash Cow로 분류된 고객군 확인. 이들은 규모는 작으나 효율이 높은 핵심 관리 대상임.지역/업종별 특성: Cash Cow 그룹 중 일반 등급 고객은 주로 제조/부동산업 중심이며, 대구·경북 비중이 높고 오프라인 채널 선호도가 높음.🎯 실무적 시사점 (Strategy)Segment전략 방향Star유지 및 우대 전략 중심 (VIP 관리)Cash Cow관계 심화 및 선별적 지원 확대 (우선 검토 대상)Potential활동성을 수익으로 전환하기 위한 교차판매(Cross-selling) 전략Standard비용 효율적 관리 및 성장 가능성 기반 육성 대상 선별⚠️ 한계 및 향후 과제하위 지표 동일 가중치 적용에 따른 실제 중요도 반영 미흡.산업/지역별 절대 기준이 아닌 상대적 기준(중앙값) 사용.시계열 변화(Time-series) 및 미래 성과와의 상관관계 검증 필요.🛠 사용 기술 및 파일Tech StackLanguage: PythonLibrary: Pandas, NumPy, Scikit-learn, SciPy, Seaborn, MatplotlibMethodology: 비모수 통계검정, 고객 세분화(Segmentation), EDAProject Files최종_전처리코드.ipynb: 데이터 클렌징 및 지수 산출최종_지표활용EDA.ipynb: 세그먼트별 특성 분석 및 인사이트 도출가설검정_총정리.ipynb: 통계적 유의성 검증 수행

