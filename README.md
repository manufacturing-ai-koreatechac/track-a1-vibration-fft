# Part 2-1: 예지보전 (Predictive Maintenance)

> **v12 Enhanced**: KAMP 진동 데이터 + FFT 분석 + 이상 탐지

---

## 🎯 학습 목표

- ✅ 시계열 진동 데이터 분석
- ✅ FFT를 통한 주파수 분석
- ✅ Health Index 계산
- ✅ 이상 탐지 알고리즘
- ✅ 고장 예측 모델

---

## 📚 실습 구성

| 순서 | 실습 | 파일 | 소요 시간 | 난이도 |
|:----:|------|------|:---------:|:------:|
| 1 | 진동 데이터 분석 | `01_vibration_analysis.ipynb` | 45분 | ⭐⭐ |
| 2 | FFT 주파수 분석 | `02_fft_frequency_analysis.ipynb` | 45분 | ⭐⭐⭐ |
| 3 | 이상 탐지 모델 | `03_anomaly_detection.ipynb` | 60분 | ⭐⭐⭐ |

**총 소요 시간**: 약 2.5시간

---

## 🚀 시작하기

### 1️⃣ 환경 설정

```bash
# Part 2-1 폴더로 이동
cd practice-v12-enhanced/part2-1

# 추가 패키지 설치 (신호 처리)
pip install scipy
```

---

## 📊 사용 데이터

### KAMP 진동 센서 데이터

| 항목 | 내용 |
|------|------|
| **파일** | `sample_kamp_vibration.csv` |
| **크기** | 1.5MB (약 10,000행) |
| **출처** | [KAMP](https://mdata.kamp-ai.kr/) |
| **변수** | 시간, X축 진동, Y축 진동, Z축 진동, 온도, RPM |

**고장 유형**:
- 정상 (Normal)
- 베어링 불량 (Bearing)
- 언밸런스 (Unbalance)
- 미스얼라인먼트 (Misalignment)

---

## 🔧 실습 상세 내용

### 실습 1: 진동 데이터 분석 (45분)

**학습 내용**:
- 시계열 진동 데이터 시각화
- 통계적 특징 추출 (RMS, Peak, Kurtosis)
- 축별 진동 비교
- 온도-진동 상관관계

**주요 코드**:
```python
import pandas as pd
import numpy as np

# RMS 계산
def calculate_rms(signal):
    return np.sqrt(np.mean(signal**2))

# Peak-to-Peak
def calculate_peak_to_peak(signal):
    return np.max(signal) - np.min(signal)

# Kurtosis (첨도)
def calculate_kurtosis(signal):
    return ((signal - signal.mean()) ** 4).mean() / (signal.std() ** 4)
```

### 실습 2: FFT 주파수 분석 (45분)

**학습 내용**:
- FFT 기본 개념
- 주파수 스펙트럼 분석
- 고장별 주파수 패턴 식별
- Spectrogram 생성

**주요 코드**:
```python
from scipy.fft import fft, fftfreq
import matplotlib.pyplot as plt

def analyze_fft(signal, sampling_rate=1000):
    # FFT 계산
    n = len(signal)
    fft_values = fft(signal)
    frequencies = fftfreq(n, 1/sampling_rate)

    # 양의 주파수만
    pos_mask = frequencies > 0
    frequencies = frequencies[pos_mask]
    magnitude = np.abs(fft_values[pos_mask])

    # 시각화
    plt.plot(frequencies, magnitude)
    plt.xlabel('주파수 (Hz)')
    plt.ylabel('진폭')
    plt.title('FFT 스펙트럼')
    plt.show()

    return frequencies, magnitude
```

### 실습 3: 이상 탐지 모델 (60분)

**학습 내용**:
- Isolation Forest
- Autoencoder 기반 이상 탐지
- Health Index 계산
- 고장 예측 타임라인

**주요 코드**:
```python
from sklearn.ensemble import IsolationForest

# 피처 엔지니어링
features = df[['rms_x', 'rms_y', 'rms_z',
               'peak_x', 'peak_y', 'peak_z',
               'kurtosis_x', 'kurtosis_y', 'kurtosis_z']]

# Isolation Forest
iso_forest = IsolationForest(contamination=0.1, random_state=42)
predictions = iso_forest.fit_predict(features)

# Health Index 계산
anomaly_scores = iso_forest.score_samples(features)
health_index = 100 * (1 - (anomaly_scores - anomaly_scores.min()) /
                      (anomaly_scores.max() - anomaly_scores.min()))

# 시각화
plt.plot(df['timestamp'], health_index)
plt.axhline(y=70, color='r', linestyle='--', label='경고 임계값')
plt.xlabel('시간')
plt.ylabel('Health Index')
plt.title('설비 건강도 추이')
plt.legend()
plt.show()
```

---

## 💡 학습 팁

### FFT 해석 가이드

| 주파수 범위 | 의미 | 고장 유형 |
|------------|------|----------|
| 1x RPM | 회전 주파수 | 언밸런스 |
| 2x RPM | 2배 주파수 | 미스얼라인먼트 |
| BPFO/BPFI | 베어링 주파수 | 베어링 불량 |

### Health Index 해석

- **90-100**: 정상
- **70-90**: 주의
- **50-70**: 경고
- **<50**: 위험

---

## 📚 참고 자료

### 이론
- [FFT 기초](https://en.wikipedia.org/wiki/Fast_Fourier_transform)
- [진동 분석 가이드](https://www.machinedesign.com/basics-design/article/21832030/a-guide-to-vibration-analysis)

### 라이브러리
- [SciPy Signal Processing](https://docs.scipy.org/doc/scipy/reference/signal.html)
- [sklearn Anomaly Detection](https://scikit-learn.org/stable/modules/outlier_detection.html)

---

## 🎓 학습 체크리스트

- [ ] 진동 데이터의 통계적 특징을 추출했다
- [ ] FFT를 사용하여 주파수 스펙트럼을 분석했다
- [ ] 이상 탐지 모델을 구현했다
- [ ] Health Index를 계산하고 시각화했다

---

*제조AI 교육 v12 Enhanced | Part 2-1 | 2025.02*
