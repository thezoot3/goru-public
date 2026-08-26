# 제3자 소프트웨어·모델 고지

이 고지는 고루 앱 바이너리 배포와 이 저장소 배포 양쪽에 적용된다.

고루가 번들하는 제3자 자산은 전부 Google MediaPipe 계열이며, 확인된
라이선스는 전부 Apache License 2.0이다. 라이선스 전문은
[LICENSES/Apache-2.0.txt](LICENSES/Apache-2.0.txt)(정본)와
[LICENSES/MediaPipeTasks-LICENSE.txt](LICENSES/MediaPipeTasks-LICENSE.txt)
(MediaPipe Tasks 배포본 동봉 결합 라이선스 원문)에 있다. 예외는 3번
`gesture_recognizer.task`의 gesture classification 구성 모델 하나이며,
해당 항목에 미확정 사유를 그대로 적었다.

## 1. MediaPipeTasksVision / MediaPipeTasksCommon xcframework 0.10.14

- 용도: 온디바이스 얼굴 특징점 추적(Face Landmarker)과 손 특징점·제스처
  인식(Gesture Recognizer)의 실행 프레임워크
- 배포 형태: 앱 바이너리에 링크 (Google 공식 CocoaPods 배포본)
- 저작권 귀속: Google LLC, MediaPipe
- 라이선스: Apache License 2.0. 배포본 동봉 LICENSE는 Apache-2.0 본문에
  Lucent Technologies의 UTF 코드 고지가 결합된 문서이며, 해당 고지가 전체
  사본 포함을 요구하므로 원문 그대로
  [LICENSES/MediaPipeTasks-LICENSE.txt](LICENSES/MediaPipeTasks-LICENSE.txt)에
  포함했다 (MediaPipeTasksVision과 MediaPipeTasksCommon의 동봉 LICENSE는
  동일 문서다).
- 근거: CocoaPods 배포본 동봉 LICENSE 원문

## 2. face_landmarker.task (float16, 버전 1)

- 용도: 얼굴 특징점 468점 온디바이스 추론
- 배포 형태: 앱 번들 리소스
- 저작권 귀속: Google LLC, MediaPipe
- 라이선스: Apache License 2.0, 구성 3개 모델(FaceMesh V2, Blendshape V2,
  BlazeFace Short-Range)의 모델 카드가 각각 Apache-2.0을 명시한다.
- 근거:
  - [Model Card MediaPipe Face Mesh V2](https://storage.googleapis.com/mediapipe-assets/Model%20Card%20MediaPipe%20Face%20Mesh%20V2.pdf)
  - [Model Card Blendshape V2](https://storage.googleapis.com/mediapipe-assets/Model%20Card%20Blendshape%20V2.pdf)
  - [MediaPipe BlazeFace Model Card (Short Range)](https://storage.googleapis.com/mediapipe-assets/MediaPipe%20BlazeFace%20Model%20Card%20(Short%20Range).pdf)

## 3. gesture_recognizer.task (float16, 버전 1)

- 용도: 손 특징점 21점 추출과 제스처 분류 온디바이스 추론 (세션 시작/종료
  제스처 인식)
- 배포 형태: 앱 번들 리소스
- 저작권 귀속: Google LLC, MediaPipe
- 라이선스:
  - hand landmark 구성 모델: Apache License 2.0
    [Model Card Hand Tracking (Lite/Full) with Fairness](https://storage.googleapis.com/mediapipe-assets/Model%20Card%20Hand%20Tracking%20(Lite_Full)%20with%20Fairness%20Oct%202021.pdf)
  - gesture classification 구성 모델(gesture embedder / canned gesture
    classifier): Google가 공개한 모델 카드에 라이선스 조항이 명시되어 있지
    않아 **미확정**이다. 파일은 Google 공식 배포본과 바이트 동일하며 배포
    페이지 전반은 Apache-2.0을 따르지만, 이 구성 모델 단위의 명시 근거는
    확인되지 않았으므로 사실대로 표기한다. 고루는 이 파일에서 hand
    landmark 출력만 사용한다.

## 4. hand_landmarker.task (float16, 버전 1)

- 용도: 손 특징점 21점 온디바이스 추론
- 배포 형태: 앱 번들 리소스
- 저작권 귀속: Google LLC, MediaPipe
- 라이선스: Apache License 2.0
- 근거: [Model Card Hand Tracking (Lite/Full) with Fairness](https://storage.googleapis.com/mediapipe-assets/Model%20Card%20Hand%20Tracking%20(Lite_Full)%20with%20Fairness%20Oct%202021.pdf)
  (2번과 같은 Hands 모델 카드)

## 5. canonical_face_model.obj (표준 얼굴 3D 모델·UV 좌표)

- 용도: 얼굴 표면 UV 맵 좌표계 구성의 원천 topology와 UV 좌표
- 배포 형태: 저장소 포함 자산 (google-ai-edge/mediapipe 저장소 파일과
  바이트 동일 확인)
- 저작권 귀속: Google LLC, MediaPipe
- 라이선스: Apache License 2.0
- 근거: [google-ai-edge/mediapipe 저장소 LICENSE](https://github.com/google-ai-edge/mediapipe/blob/master/LICENSE)

## 6. face-atlas-v1.json / region-mask-v1.json (UV 맵 파생 데이터)

- 용도: 얼굴 표면 UV 맵의 부위 구획·마스크 데이터
- 배포 형태: 앱 번들 리소스
- 유래·변경 표시 (Apache-2.0 §4(b)): 이 두 파일은 위 5번
  **MediaPipe canonical_face_model.obj에서 자체 도구로 결정적으로
  생성 및 가공한 파생물**이다. 원본의 mesh topology와 UV 좌표를 기반으로
  부위 구획과 마스크 형태로 변환했다.
- 원본 저작권 귀속: Copyright Google LLC, MediaPipe
- 원본 라이선스: Apache License 2.0 (5번과 동일 근거)
