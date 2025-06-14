# AI 기반 꿀벌 질병 관리 및 실시간 경보 시스템 "BEE CAREFUL" 

과학기술정보통신부 산하 IITP(정보통신기획평가원)가 주관하는 'SW중심대학사업'의 일환으로 수행한 최종 프로젝트입니다. 전문 지식이 부족한 신규 양봉업자들의 어려움을 해결하고자, **AI를 통해 꿀벌의 질병을 탐지·진단하고, 실시간으로 위험을 경고하며, RAG 챗봇으로 지식을 공유하는 종합 플랫폼**을 기획하고 개발했습니다.

<br>

### 🖼️ 프로젝트 핵심 기능 시연 (Live Demo)

[실시간 영상 감지 데모 GIF를 여기에 삽입하세요.]

![Live Detection Demo]([여기에_실시간_영상_감지_GIF_링크_삽입])

<br>

### 🖥️ 서비스 주요 화면 및 아키텍처 (Screenshots & Architecture)

<table>
  <tr>
    <td align="center"><b>메인 페이지</b></td>
    <td align="center"><b>주요 서비스 소개</b></td>
  </tr>
  <tr>
    <td width="50%">
      ![메인 페이지](https://github.com/user-attachments/assets/46245e9f-0eab-4e52-8845-c89a1b9dd067)
    </td>
    <td width="50%">
      <img src="[여기에_주요서비스도식화_이미지_링크_삽입]" alt="Key Services">
    </td>
  </tr>
  <tr>
    <td align="center"><b>실시간 영상 감시 및 SMS 경보</b></td>
    <td align="center"><b>커뮤니티 게시판</b></td>
  </tr>
  <tr>
    <td>
      <img src="[여기에_서비스구현도식화_이미지_링크_삽입]" alt="Service Architecture">
      <br>
      <img src="[여기에_SMS수신화면_이미지_링크_삽입]" alt="SMS Alert">
    </td>
    <td>
      <img src="[여기에_커뮤니티게시판_이미지_링크_삽입]" alt="Community Board">
    </td>
  </tr>
</table>

<br>

### 💡 My Role & Contribution (주요 역할 및 기여)

저는 **팀장으로서 프로젝트의 총괄을 담당**했으며, 특히 **3개의 독립 서버가 통신하는 마이크로서비스 아키텍처(MSA)를 직접 설계하고 전체 시스템을 구축**하는 핵심 역할을 수행했습니다.

*   **시스템 아키텍처 설계 및 구축:**
    *   **메인 서버, AI 프로세싱 서버, 경보 서버** 3개의 역할을 명확히 분리하여 MSA 구조를 설계했습니다. 이를 통해 각 서버의 부하를 분산시키고 안정성과 확장성을 확보했습니다.
    *   서버 간 통신을 위한 REST API 엔드포인트를 설계하고, 전체 시스템의 원활한 데이터 흐름을 책임졌습니다.

*   **실시간 AI 파이프라인 개발 및 최적화:**
    *   **데이터 전처리 및 데이터셋 구축:** **팀원과 협력**하여 AI 모델의 핵심 기반인 데이터셋부터 구축했습니다. **Roboflow와 OpenCV를 활용**하여 복잡한 배경 속에서 **순수 꿀벌 이미지만을 정밀하게 추출하고 바운딩 박스를 생성**하는 전처리 과정을 주도했습니다.
    *   **모델 파인튜닝 및 성능 비교 검증:** 이 정제된 데이터를 바탕으로, **VGG16과 EfficientNet 모델을 직접 파인튜닝하고 성능을 비교 분석**하여, 우리 서비스에 가장 적합한 EfficientNet을 데이터 기반으로 확정했습니다.
    *   **2-Stage 모델 파이프라인 구현:** 이 검증된 EfficientNet을, 빠른 객체 탐지가 가능한 **YOLOv8**과 결합하여 **속도와 정확도를 모두 잡는 효율적인 2단계 파이프라인**을 최종적으로 완성했습니다.

*   **백엔드 API 개발 및 시스템 통합:**
    *   **핵심 API 서버 개발:** 사용자 인증, 영상 스트리밍 API(`multipart/x-mixed-replace` 방식) 등 **제가 직접 담당한 서버들의 모든 API를 개발**했습니다.
    *   **신뢰도 높은 경보 시스템 구현:** 단발적인 오탐(False Positive)을 방지하고자, 일정 횟수 이상 질병이 누적될 경우에만 **Twilio API**로 SMS 경보를 발송하는 **임계값 기반의 API 로직**을 구현했습니다.
    *   **시스템 통합 총괄:** **팀원이 개발한 RAG 챗봇 모듈**을 포함한 3개의 독립 서버를 성공적으로 통합하고, 프론트엔드와의 연동을 총괄 지휘했습니다.

 <br>

### ⚙️ 시스템 아키텍처 (System Architecture)
```
+------------------+   (Video Stream & API)   +-----------------------------+
|                  | <----------------------> | AI Processing Server        |
| Frontend (React) |                          | (Flask, Port: 5001)         |
|   (User UI)      |                          | - YOLOv8 + EfficientNet     |
|                  |                          | - Real-time Video Streaming |
+--------+---------+                          +-----------------------------+
         ^                                                     ^
         |                                                     |
         | (Community, Auth, RAG API)                          | (Detection Data)
         |                                                     | POST /log_detection
         v                                                     v
+-------------------------+                   +-----------------------------+
| Main Server             |                   | Alert Server (Flask, Port: 5002)|
| (Flask, Port: 5000)     |                   | - Detection Count & Threshold |
| - Community, Auth, DB   |                   | - Twilio SMS Alert          |
| - RAG Chatbot Integration|                  +-----------------------------+
+----------+--------------+
           |
           | (SQLAlchemy: Read/Write)
           |
+----------+--------------+
| Database (MySQL)        |
+-------------------------+

```

<br>

### 🌟 주요 기능 (Key Features)

| 담당 서버 | 기능 | 상세 설명 |
| :--- | :--- | :--- |
| **AI Processing Server** | **실시간 AI 영상 분석** | 웹캠 영상을 실시간으로 받아 2-Stage AI 파이프라인으로 질병을 탐지하고, 결과가 표시된 영상을 프론트엔드로 스트리밍합니다. |
| **Alert Server** | **누적 기반 위험 경보** | AI 서버로부터 질병 데이터를 받아 누적 카운트하고, 임계값 초과 시 Twilio API를 통해 관리자에게 SMS를 발송합니다. |
| **Main Server** | **웹 플랫폼 및 정보 제공** | 사용자 인증, 이미지 기반 진단 API를 제공합니다. 또한, **LangChain과 GPT-4o mini를 활용한 RAG 챗봇**을 통해, 꿀벌 질병에 대한 전문적인 정보를 제공하는 역할을 수행합니다. |

<br>

### 🛠️ 기술 스택 (Tech Stack)

| 구분 | 기술 |
| :--- | :--- |
| **AI / ML** | <img src="https://img.shields.io/badge/TensorFlow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white"> <img src="https://img.shields.io/badge/YOLOv8-4F46E5?style=for-the-badge&logo=yolo&logoColor=white"> <img src="https://img.shields.io/badge/EfficientNet-8BC34A?style=for-the-badge"> <img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white"> |
| **Backend & API** | <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"> <img src="https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white"> <img src="https://img.shields.io/badge/Twilio-F22F46?style=for-the-badge&logo=twilio&logoColor=white"> |
| **Database** | <img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white"> <img src="https://img.shields.io/badge/SQLAlchemy-D71F00?style=for-the-badge&logo=sqlalchemy&logoColor=white"> |
| **Frontend** | <img src="https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black"> <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black"> |

<br>

### 🤔 문제 해결 과정 (Challenges & Solutions)

#### **초기 모델의 성능 저하 (오진율 90%) 문제 해결 과정**

*   **문제점:** 프로젝트 초기, EfficientNet 기반 모델의 오진율이 90%에 달해 실사용이 불가능했습니다. 단순히 파인튜닝 방식을 도입하는 것만으로는 문제가 해결되지 않아, 더 근본적인 원인 분석이 필요했습니다.

*   **해결 과정 (데이터 중심의 체계적 접근):**
    1.  **원인 분석:** 저는 이것이 단순 모델의 문제가 아니라 **'데이터 자체의 구조적 문제'** 임을 간파했습니다. 팀원들과 함께 데이터셋을 분석한 결과, 벌집, 유충 등 불필요한 이미지가 섞여 있고 질병 특성으로 인해 오진이 발생하는 것을 확인했습니다.
    2.  **데이터 정제 및 확장:** 이 문제를 해결하기 위해, **Roboflow와 OpenCV를 활용하여 배경이 복잡한 이미지에서 '꿀벌' 객체만 정밀하게 추출**하고, 데이터셋을 기존 300장에서 958장으로 확장하여 모델이 양질의 데이터를 학습하도록 환경을 개선했습니다.
    3.  **아키텍처 및 학습 최적화:** **YOLOv8을 결합한 2-Stage 구조를 설계**하여 '탐지'와 '분류' 역할을 분리했습니다. 또한, 클래스 불균형 문제를 완화하기 위해 **Focal Loss를 적용하고 질병 데이터에 가중치(정상:1.5, 질병:2.0)를 부여**하여 학습의 균형을 맞췄습니다.

*   **결과:** 이러한 반복적인 시도 끝에, 모델의 **mAP는 0.96, 분류 정확도는 0.86을 기록했고, 오진율은 14.6% 수준으로 크게 감소**시켰습니다. 이 경험을 통해 '좋은 서비스는 좋은 데이터에서 출발한다'는 원칙과 데이터 전처리의 중요성을 체감했으며, 최종 프로젝트 경연에서 **'대상'** 을 수상하는 성과로 이어졌습니다.

<br>

### 💡 배운 점 (What I Learned)

*   **시스템 전체를 보는 아키텍처 설계 역량:** 단순히 모델을 학습시키는 것을 넘어, 독립적인 서버들을 연동하는 **마이크로서비스 아키텍처**를 직접 설계하고 구축해보며 시스템 전체를 보는 시야를 확보했습니다.
*   **데이터 중심의 문제 해결 능력:** 모델 성능이 낮을 때, 문제의 근본 원인이 '데이터'에 있을 수 있음을 간파하고, 데이터 정제와 증강을 통해 실질적인 성능 개선을 이끌어낸 경험을 쌓았습니다.
*   **팀 리더십 및 기술적 의사결정 능력:** 팀장으로서 팀원들의 의견을 조율하고 공동의 목표를 설정하는 과정에서, 기술적인 문제 해결뿐만 아니라 **원활한 협업과 합리적인 기술적 의사결정 능력**의 중요성을 깊이 깨달았습니다.

<br>



