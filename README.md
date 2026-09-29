# InspectorsAlly - OpenVINO

TensorFlow/Keras로 학습한 가죽 제품 정상·불량 분류 모델을 OpenVINO IR 형식으로 변환해 CPU 추론에 적용한 프로젝트입니다. Streamlit UI와 이미지·웹캠용 CLI 예제를 제공합니다.

## 주요 기능

- OpenVINO Runtime 기반 CPU 추론
- 이미지 업로드 또는 카메라 촬영을 지원하는 Streamlit UI
- 파일 이미지와 웹캠 입력을 지원하는 CLI 예제
- 정상·불량 확률과 최종 판정 표시
- 학습 환경과 동일한 VGG16 전처리 적용

## 기술 스택

- Python 3.10
- OpenVINO 2024.6
- Streamlit
- NumPy, Pillow
- OpenCV, Matplotlib(CLI 웹캠·결과 표시 시)

## 프로젝트 구조

```text
.
├── tf_App_openvino.py          # Streamlit 애플리케이션
├── infer_openvino.py           # 파일·웹캠 CLI 추론 예제
├── weights/
│   ├── leather_model.xml       # OpenVINO 모델 구조
│   └── leather_model.bin       # OpenVINO 모델 가중치
└── requirements.txt            # Streamlit 실행 의존성
```

## Streamlit 앱 실행

```bash
python -m venv .venv
```

가상환경을 활성화한 뒤 다음 명령을 실행합니다.

```bash
pip install -r requirements.txt
streamlit run tf_App_openvino.py
```

## CLI 추론 실행

CLI에서 웹캠을 사용하려면 추가 패키지를 설치합니다.

```bash
pip install matplotlib opencv-python
python infer_openvino.py
```

실행 후 `image` 또는 `webcam`을 선택합니다. 파일 입력은 `infer_openvino.py`의 `TEST_IMAGE_PATH`를 실제 이미지 경로로 설정해야 합니다.

## 모델 입력과 출력

| 항목 | 내용 |
|---|---|
| 입력 | RGB 이미지, 224 x 224 |
| 전처리 | RGB를 BGR로 변환 후 ImageNet 평균값 차감 |
| 실행 장치 | CPU |
| 출력 | Sigmoid 기반 불량 확률 |
| 판정 | 0.5 초과 시 불량 |

## 참고

교육 프로젝트에서 만든 추론 프로토타입입니다. 실제 생산 환경에 적용하려면 대상 CPU에서 지연시간을 측정하고, 현장 이미지로 정확도와 판정 임계값을 다시 검증해야 합니다.
