# Part 2-1 데이터셋 설명

## 개요
설비 진동 분석 및 이상 탐지를 위한 진동 센서 데이터와 FFT 스펙트럼 참조 이미지.

## 포함 샘플 파일

| 파일 | 크기 | 설명 |
|------|------|------|
| `sample_kamp_vibration.csv` | ~1.5MB | 진동 센서 데이터 (10,000행, 합성) |
| `fft_normal.png` | ~100KB | 정상 상태 FFT 스펙트럼 참조 이미지 |
| `fft_unbalance.png` | ~100KB | 불균형 결함 FFT 스펙트럼 |
| `fft_misalignment.png` | ~100KB | 정렬 불량 FFT 스펙트럼 |
| `fft_bearing.png` | ~100KB | 베어링 결함 FFT 스펙트럼 |
| `health_index_trend.png` | ~100KB | 설비 건강 지수 추이 참조 이미지 |

## sample_kamp_vibration.csv 컬럼

| 컬럼 | 타입 | 설명 |
|------|------|------|
| 측정시간 | datetime | 센서 측정 시각 |
| 설비ID | str | MOTOR-001, PUMP-001, FAN-001 등 |
| 센서위치 | str | Housing, DE_Bearing, NDE_Bearing |
| RMS_velocity_mm_s | float | 진동 속도 RMS (mm/s) |
| Peak_acceleration_g | float | 최대 가속도 (g) |
| Kurtosis | float | 첨도 — 충격성 결함 지표 |
| Crest_factor | float | 파고율 |
| 온도_C | float | 측정 온도 (°C) |
| RPM | float | 회전수 |
| 상태 | str | 정상 / 주의 / 경고 / 위험 |

## FFT 참조 이미지

노트북 03 (이상 탐지)에서 결함 유형별 주파수 스펙트럼 패턴 비교 시 사용하는 교육용 참조 이미지.

| 파일 | 특징 주파수 | 결함 유형 |
|------|------------|----------|
| `fft_normal.png` | 1×RPM 소량 | 정상 |
| `fft_unbalance.png` | 1×RPM 지배적 | 불균형 |
| `fft_misalignment.png` | 2×RPM 증가 | 축 정렬 불량 |
| `fft_bearing.png` | 고주파 대역 증가 | 베어링 손상 |

## KAMP 원본 데이터셋 (전체)

**출처**: [KAMP (Korea AI Manufacturing Platform)](https://www.kamp-ai.kr/)

### 설비 진동 데이터
- **데이터셋명**: 회전기기 진동 진단 데이터
- **전체 규모**: ~500만 행, ~2GB
- **수집 주기**: 10ms~1s (고속 진동), 1분 (트렌드)
- **설비 유형**: 모터, 펌프, 팬, 컴프레서 등
- **센서**: 3축 가속도계 (IEPE), 온도 센서, 속도 센서
- **포함 항목**: 시계열 진동, FFT 스펙트럼, 포락선 분석, RUL 레이블

### 다운로드 방법
1. https://www.kamp-ai.kr/ 접속
2. 회전기기 진동 데이터셋 신청
3. `../../dataset/part2-1/` 경로에 배치

> 노트북은 KAMP 실데이터가 없으면 자동으로 샘플 CSV + 참조 이미지를 사용합니다.
