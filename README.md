# AI 기반 꿀벌 질병 관리 및 실시간 경보 시스템 "Bee-Health-Guard"

과학기술 정보통신부 산하의 IITP 기관에서 주관하고 SW대학중심사업에서 진행한 최종 프로젝트입니다. 전문 지식이 부족한 신규 양봉업자들의 어려움을 해결하고자, **AI를 통해 꿀벌의 질병을 탐지·진단하고, 실시간으로 위험을 경고하며, 지식을 공유하는 종합 플랫폼**을 기획하고 개발했습니다.

<br>

### 💡 My Role & Contribution (주요 역할 및 기여)

저는 **팀장으로서 프로젝트의 총괄을 담당**했으며, 특히 **3개의 독립 서버가 통신하는 마이크로서비스 아키텍처(MSA)를 직접 설계하고 전체 시스템을 구축**하는 핵심 역할을 수행했습니다.

*   **시스템 아키텍처 설계 및 구축:**
    *   **메인 서버, AI 프로세싱 서버, 경보 서버** 3개의 역할을 명확히 분리하여 MSA 구조를 설계했습니다. 이를 통해 각 서버의 부하를 분산시키고 안정성과 확장성을 확보했습니다.
    *   서버 간 통신을 위한 REST API 엔드포인트를 설계하고, 전체 시스템의 원활한 데이터 흐름을 책임졌습니다.

*   **실시간 AI 파이프라인 개발:**
    *   **2-Stage 모델 파이프라인 구현:** OpenCV로 입력된 영상에서 **YOLOv8**로 '꿀벌' 객체를 먼저 탐지하고, 해당 영역만 **EfficientNet** 모델에 전달하여 질병을 진단하는 효율적인 2단계 파이프라인을 구축했습니다.
    *   **임계값 기반 경보 시스템 개발:** 단발적인 오탐(False Positive)을 방지하고자, 일정 횟수 이상 질병이 누적될 경우에만 **Twilio API**로 SMS 경보를 발송하는 신뢰도 높은 자동화 로직을 구현했습니다.

*   **API 서버 개발 및 통합:**
    *   **Flask 기반 백엔드 API 개발:** 사용자 인증, 커뮤니티 기능, 영상 스트리밍 API(`multipart/x-mixed-replace` 방식) 등 3개 서버의 모든 API를 직접 개발했습니다.
    *   프론트엔드(React)와의 API 연동 및 시스템 통합을 주도했습니다.



 <br>

### ⚙️ 시스템 아키텍처 (System Architecture)
```
+------------------+      (Video Stream)      +-----------------------------+
|                  | <----------------------> |                             |
| Frontend (React) |                          | Webcam Server (Flask, Port:5001) |
|   (User)         |      (REST API)          | - YOLOv8 + EfficientNet     |
|                  | <----------------------> | - Real-time Video Streaming |
+------------------+                          +-----------------------------+
        ^                                                    |
        |                                                    | (Detection Data)
        | (REST API)                                         | POST /log_detection
        |                                                    v
+------------------+                          +-----------------------------+
| Main Server      |                          | SMS Server (Flask, Port:5002) |
| (Flask)          |                          | - Detection Count & Threshold |
| - Community, Auth|                          | - Twilio SMS Alert          |
+------------------+                          +-----------------------------+

```


<br>




### 🖼️ 프로젝트 실행 화면 (Project Demo)

[아래 이미지를 직접 캡처하신 영상으로 만든 GIF 파일로 교체하세요! 가장 확실한 결과물입니다.]

![alt text](https://user-images.githubusercontent.com/86633049/143794486-938a1a32-a279-491c-b883-a1288c52d477.gif)



### 🌟 주요 기능 (Key Features)

| 담당 서버 | 기능 | 상세 설명 |
| :--- | :--- | :--- |
| **Webcam Server** | **실시간 AI 영상 분석** | 웹캠 영상을 실시간으로 받아 2-Stage AI 파이프라인으로 질병을 탐지하고, 결과가 표시된 영상을 프론트엔드로 스트리밍합니다. |
| **SMS Server** | **누적 기반 위험 경보** | Webcam 서버로부터 질병 데이터를 받아 누적 카운트하고, 임계값 초과 시 Twilio API를 통해 관리자에게 SMS를 발송합니다. |
| **Main Server** | **웹 플랫폼 기능** | 커뮤니티(게시판), 사용자 인증, 이미지 업로드 기반 진단 등 웹사이트의 핵심 기능들을 제공하는 API 서버입니다. |

<br>

### 🛠️ 기술 스택 (Tech Stack)

| 구분 | 기술 | 비고 (Remarks) |
| :--- | :--- | :--- |
| **Main Skills (Backend & AI)** | <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"> <img src="https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white"> <img src="https://img.shields.io/badge/YOLOv8-4F46E5?style=for-the-badge&logo=yolo&logoColor=white"> <img src="https://img.shields.io/badge/EfficientNet-8BC34A?style=for-the-badge"> <img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white"> <img src="https://img.shields.io/badge/Twilio-F22F46?style=for-the-badge&logo=twilio&logoColor=white"> | **제가 직접 설계하고 개발한 핵심 기술 스택입니다.** |
| **Project Components (Frontend)** | <img src="https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black"> <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black"> | 팀 프로젝트의 프론트엔드 파트로, API 연동 경험이 있습니다. |

<br>

### 🤔 문제 해결 과정 (Challenges & Solutions)

**과제: 실시간 영상에서 어떻게 작고 빠르게 움직이는 꿀벌의 질병을 정확하고 효율적으로 진단할 것인가?**

*   **문제점:** 단일 AI 모델만으로는 실시간 영상 전체에서 질병의 미세한 특징을 잡아내는 데 한계가 있었습니다. YOLO 모델은 객체 탐지는 빠르지만 분류 정확도가 낮았고, 분류 전문 모델(EfficientNet)은 영상 전체를 처리하기엔 너무 느려 비효율적이었습니다.
*   **해결 과정 (2-Stage 모델 파이프라인):**
    **팀원들과의 논의를 통해, 저는** 단일 모델의 속도와 정확도 한계를 극복하기 위한 **2-Stage 파이프라인을 직접 설계하고 도입을 제안**했습니다.
    1.  **1단계 (빠른 탐지):** 먼저, 속도가 빠른 **YOLOv8**을 사용하여 영상 프레임에서 '꿀벌' 객체의 위치만 정확하고 빠르게 찾아냅니다.
    2.  **2단계 (정밀 분류):** 그 후, YOLOv8이 찾아낸 꿀벌 영역(Bounding Box) 이미지만 잘라내어, 분류 성능이 뛰어난 **EfficientNet** 모델에 입력으로 넣어 질병 여부를 정밀하게 진단하도록 했습니다.
*   **결과:** 이 2단계 접근법을 통해, **'빠른 탐지'** 와 **'정확한 분류'** 라는 두 가지 목표를 모두 달성하여 실시간 서비스의 성능과 효율성을 극대화했습니다.

<br>

### 💡 배운 점 (What I Learned)

*   **시스템 전체를 보는 아키텍처 설계 역량:** 단순히 모델을 학습시키는 것을 넘어, 독립적인 서버들을 연동하는 **마이크로서비스 아키텍처**를 직접 설계하고 구축해보며 시스템 전체를 보는 시야를 확보했습니다.
*   **안정적인 서비스 운영에 대한 이해:** "좋은 서비스는 좋은 데이터에서 출발한다"는 원칙과 함께, **부하 분산**의 필요성을 체감하며 안정적인 시스템 운영에 대한 깊이 있는 경험을 쌓았습니다.
*   **팀 리더십 및 협업 능력:** 팀장으로서 팀원들의 의견을 조율하고 공동의 목표를 설정하는 과정에서, 기술적인 문제 해결뿐만 아니라 **원활한 협업과 커뮤니케이션 능력**의 중요성을 깊이 깨달았습니다.

<br>
