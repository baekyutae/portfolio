# AI 기반 꿀벌 질병 탐지 및 분석 서비스 

과학기술 정보통신부 산하의 IITP 기관에서 주관하고 SW대학중심사업에서 진행한 최종 프로젝트입니다. 전문 지식이 부족한 신규 양봉업자들의 어려움을 해결하고자, **AI를 통해 꿀벌의 질병을 탐지 및 진단하고, 실시간으로 위험을 경고하는 종합 웹 서비스**를 기획하고 개발했습니다.

<br>

### 💡 My Role & Contribution (주요 역할 및 기여)

이 프로젝트는 커뮤니티 웹 플랫폼과 실시간 모니터링 서버가 분리된 **마이크로서비스 아키텍처(MSA)** 로 설계되었습니다. 저는 이 시스템의 **설계부터 AI 모델 개발, 백엔드 구축까지 전 과정**을 주도했습니다.

*   **실시간 모니터링 AI 파이프라인 설계 및 구축:**
    *   **2-Stage AI 모델 파이프라인 구현:** OpenCV로 입력된 실시간 영상에서 **YOLOv8**로 '꿀벌' 객체를 먼저 탐지하고, 탐지된 꿀벌 이미지만을 **EfficientNet** 분류 모델에 전달하여 질병을 진단하는 효율적인 2단계 파이프라인을 구축했습니다.
    *   **실시간 경고 시스템 개발:** AI 모델이 질병을 '위험' 단계로 판단할 경우, **Twilio API**를 통해 사용자의 휴대폰으로 즉시 경고 SMS를 발송하는 자동화 로직을 구현했습니다.
*   **백엔드 API 서버 개발:** Python과 Flask를 사용하여 사용자 인증, 커뮤니티(게시판) 기능, 이미지 업로드 기반 질병 판별 등 서비스의 핵심 API를 개발했습니다.
*   **시스템 통합:** 프론트엔드와의 API 연동 및 두 개의 백엔드 서버(main_server, webcam_sms_server)가 원활히 통신하도록 전체 시스템을 통합했습니다.

<br>

### ⚙️ 프로젝트 구조 (Project Structure)
```
📁 final-project/ # 꿀벌 질병 탐지 AI 서비스 (프로젝트 루트)
│
├── 📂 ML/ # AI 모델 개발 및 실험 과정 (Jupyter Notebook)
│ ├── 📜 cnn(efficient).ipynb # EfficientNet 기반 질병 분류 모델 실험
│ └── 📜 yolo_bee.ipynb # YOLOv8 모델을 이용한 꿀벌 객체 탐지 실험
│
├── 📂 backend/ # 백엔드 서버 (Python, Flask)
│ │
│ ├─ 📂 main_server/ # 메인 웹 플랫폼 API 서버 (커뮤니티, 이미지 진단)
│ │ ├── 📜 app.py # Flask 애플리케이션 실행 파일
│ │ └── 📜 requirements.txt # 백엔드 라이브러리 목록
│ │
│ └─ 📂 webcam_sms_server/ # 실시간 모니터링 및 SMS 경고 서버
│ ├── 📜 sms_server.py # 실시간 감지 및 SMS 발송 실행 파일
│ ├── 📜 bee_main.pt # YOLOv8 객체 탐지 모델
│ └── 📜 bee_image_model.keras # EfficientNet 질병 분류 모델
│
├── 📂 frontend/ # 프론트엔드 (React)
│ └── 📂 src/
│
└── 📜 README.md # 프로젝트 총괄 설명서

```
<br>

### 🖼️ 프로젝트 실행 화면 (Project Demo)

[여기에 프로젝트 실행 화면 GIF 또는 스크린샷을 삽입하세요.]

![demo](https://via.placeholder.com/700x400.png?text=Project+Demo+GIF+or+Screenshot)

<br>

### 🌟 주요 기능 (Key Features)

*   **실시간 질병 감지 및 SMS 경고:** 양봉장 웹캠 영상을 24시간 분석하여 질병 감염 꿀벌을 자동으로 탐지하고, 위험 상황 시 관리자에게 즉시 문자 메시지를 발송합니다.
*   **이미지 기반 질병 진단:** 사용자가 직접 촬영한 꿀벌 사진을 업로드하여 질병 감염 여부를 즉시 확인할 수 있습니다.
*   **커뮤니티:** 신규 양봉업자들이 지식과 경험을 공유할 수 있는 게시판 기능을 제공합니다.

<br>

### 🛠️ 기술 스택 (Tech Stack)

| 구분 | 기술 | 비고 (Remarks) |
| :--- | :--- | :--- |
| **Main Skills (Backend & AI)** | <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"> <img src="https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white"> <img src="https://img.shields.io/badge/YOLOv8-4F46E5?style=for-the-badge&logo=yolo&logoColor=white"> <img src="https://img.shields.io/badge/EfficientNet-8BC34A?style=for-the-badge"> <img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white"> <img src="https://img.shields.io/badge/Twilio-F22F46?style=for-the-badge&logo=twilio&logoColor=white"> | **제가 직접 설계하고 개발한 핵심 기술 스택입니다.** |
| **Project Components (Frontend)** | <img src="https.img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black"> <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black"> | 팀 프로젝트의 프론트엔드 파트로, API 연동 경험이 있습니다. |

<br>

### 🤔 문제 해결 과정 (Challenges & Solutions)

**과제: 실시간 영상에서 어떻게 작고 빠르게 움직이는 꿀벌의 질병을 정확하고 효율적으로 진단할 것인가?**

*   **문제점:** 단일 AI 모델만으로는 실시간 영상 전체에서 질병의 미세한 특징을 잡아내는 데 한계가 있었습니다. YOLO 모델은 객체 탐지는 빠르지만 분류 정확도가 낮았고, 분류 전문 모델(EfficientNet)은 영상 전체를 처리하기엔 너무 느려 비효율적이었습니다.
*   **해결 과정 (2-Stage 모델 파이프라인):**
    1.  **1단계 (빠른 탐지):** 먼저, 속도가 빠른 **YOLOv8**을 사용하여 영상 프레임에서 '꿀벌' 객체의 위치만 정확하고 빠르게 찾아냅니다.
    2.  **2단계 (정밀 분류):** 그 후, YOLOv8이 찾아낸 꿀벌 영역(Bounding Box) 이미지만 잘라내어, 분류 성능이 뛰어난 **EfficientNet** 모델에 입력으로 넣어 질병 여부를 정밀하게 진단하도록 했습니다.
*   **결과:** 이 2단계 접근법을 통해, **'빠른 탐지'** 와 **'정확한 분류'** 라는 두 가지 목표를 모두 달성하여 실시간 서비스의 성능과 효율성을 극대화했습니다.

<br>

### 💡 배운 점 (What I Learned)

단순히 AI 모델을 학습시키는 것을 넘어, **데이터 수집 및 정제의 중요성**을 체감했습니다. "좋은 서비스는 좋은 데이터에서 출발한다"는 사실을 깨달았으며, 독립적인 서버들을 연동하는 **마이크로서비스 아키텍처**를 경험하며 **실제 사용 가능한 End-to-End 서비스를 구축**하는 값진 경험을 했습니다. 이 경험은 앞으로 실제 산업 현장의 문제를 기술로 해결하는 개발자로 성장하는 데 큰 원동력이 될 것입니다.

<br>

### 🚀 설치 및 실행 방법 (Installation)

```bash
# 1. 저장소 클론
git clone https://github.com/baekyutae/portfolio.git
cd portfolio

# 2. 필요한 라이브러리 설치 (가상환경 구성을 권장합니다)
# 각 서버 폴더(main_server, webcam_sms_server)에 있는
# requirements.txt 파일을 사용하여 라이브러리를 설치해주세요.
pip install -r ./backend/main_server/requirements.txt
pip install -r ./backend/webcam_sms_server/requirements.txt

# 3. 백엔드 서버 실행
# (main_server 실행)
python ./backend/main_server/app.py

# (webcam_sms_server 실행 - 별도 터미널에서)
python ./backend/webcam_sms_server/sms_server.py

# 4. 프론트엔드 서버 실행
cd frontend
npm install
npm start
