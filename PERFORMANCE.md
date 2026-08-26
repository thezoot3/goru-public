# 실기기 성능

GoruShowcase를 iPad에서 계측한 처리 시간과 실행 조건을 정리한다. 접촉 판정의 정확도는 [모델카드](MODEL_CARD.md)에서 별도로 다룬다. 아래 수치는 특정 기기와 측정 구간의 결과이며 모든 실행 조건을 보장하지 않는다.

## 1. 측정 환경

모든 실기기 계측은 iPad Pro 11형 4세대(`iPad14,3`, Apple M2) 한 대에서 수행했다. 다른 기기에서의 성능은 검증하지 않았다.

## 2. CNN cycle (n=763)

접촉 판정의 목표 주기는 15Hz이며 한 사이클의 시간 예산은 66.7ms다.

| Metric | Diagnostics field | 측정 범위 | 평균 요약값 |
|---|---|---|---:|
| Core ML inference | `cnn.predict_ms` | `MLModel.prediction` 호출 시간 | 0.9ms |
| Preprocessing | `cnn.preprocess_ms` | 사이클의 손가락 패치 구성에 걸린 시간 | 8.9ms |
| CNN cycle | `cnn.cycle_ms` | 전처리와 추론 등을 포함한 사이클 처리 시간 | **9.5ms** |
| Capture-to-score latency | `cnn.capture_to_score_ms` | 입력 프레임 캡처부터 접촉 점수 산출까지의 시간 | **39.6ms** |

평균 CNN cycle 시간 9.5ms는 목표 예산의 약 14%다.

이 측정에서는 Core ML inference보다 preprocessing에 더 많은 시간이 들었다. Core ML의 계산 장치 설정은 `.all`이며, 실제 연산이 ANE, GPU, CPU 중 어디에서 수행됐는지는 이 로그로 확인하지 않았다.

Capture-to-score latency에는 CNN 처리 외에 landmark 결과 대기와 스케줄링 등이 포함된다.

## 3. MediaPipe와 결합한 상태

같은 측정 구간에서 MediaPipe face/hand landmark 추론과 접촉 CNN을 함께 실행했다.

| Metric | Diagnostics field | 평균 요약값 또는 관측값 |
|---|---|---:|
| Camera FPS | `camera.fps` | 60.01fps |
| Camera drops | `camera.drops` | 0 |
| Face landmark inference | `vision.face_ms` | 8.28ms |
| Hand landmark inference | `vision.hand_ms` | 12.08ms |
| Face landmark completion rate | `vision.face_hz` | 58.73Hz |
| Hand landmark completion rate | `vision.hand_hz` | 52.11Hz |
| Render capture-to-GPU-completion latency | `render.gpu_e2e_ms` | 15.41ms |

렌더링은 비동기 결과를 사용한다. 위 15.41ms는 렌더링이 시작된 시각부터 렌더링 GPU 작업 완료까지의 시간이다.

## 4. 장시간 연속 실행

2026년 8월 4일의 19분 30초 연속 실행 기록이다. 코칭 화면을 유지하며 간헐적으로 손을 얼굴에 올렸다. 현재 접촉 CNN을 계속 실행한 부하 시험은 아니지만, 접촉 CNN 모델의 추론 부담과 시간이 크게 다르지 않다는 점은 고려해야한다.

| 구간 | Face landmark inference | Hand landmark inference |
|---|---:|---:|
| 0–2분 | 9.2ms | 10.2ms |
| 8–10분 | 9.3ms | 10.1ms |
| 17–19분 | 9.1ms | 10.5ms |

이 조건에서는 시간이 지남에 따라 MediaPipe 추론 시간이 크게 증가하지 않았다.
