# PIASPACE VSS Engine – UI 변경 및 테스트 가이드 ( by baerri )

Gradio 기반 UI를 변경하고, 테스트하기 위한 로컬 실행 환경 설정, 종속성 설치, 실행 방법 등을 정리했다.


## Conda 환경 구성

```bash
conda create -n vss_ui python=3.10 -y
conda activate vss_ui
````

<br>

## 필수 패키지 설치

```bash
# PyGObject 설치 (GStreamer 연동용)
sudo apt update
sudo apt install -y python3-gi python3-gi-cairo gir1.2-gtk-3.0 gir1.2-gst-plugins-base-1.0 gir1.2-gstreamer-1.0
```

```bash
pip install gradio aiohttp uvicorn watchdog
```

<br>

## 환경 변수 설정

UI에서 사용할 설정 파일 및 GStreamer 연동을 위해 아래 환경 변수를 반드시 설정해야  한다.

```bash
# GStreamer Python bindings 활성화
export GI_TYPELIB_PATH=/usr/lib/x86_64-linux-gnu/girepository-1.0/

# 설정 파일 경로 지정 (경로 수정 필요)
export CA_RAG_CONFIG=/home/hyeri/study/video-search-and-summarization/src/vss-engine/config/config.yaml
```

<br>

## UI 실행

개발 중 UI 관련 `.py` 파일이 변경될 경우 자동으로 Gradio 서버가 재시작되도록 `watchdog` 기반 스크립트를 사용했다.

```bash
watchmedo auto-restart --patterns="*.py" --recursive -- \
    python via_demo_client.py --host 127.0.0.1 --port 7860
```

* `watchmedo`: 파일 변경 감지 및 자동 재시작
* `--patterns`: 변경 감지할 파일 확장자 지정 (`*.py`)
* `--recursive`: 하위 폴더 포함 감지
* `--`: 이후 명령어를 감시 실행

<br>

## UI 확인

정상 실행 시, 아래 주소에서 Gradio UI를 확인할 수 있다.

```
http://127.0.0.1:7860/
```


