

# 🇯🇵 AI基盤の蜜蜂疾病管理およびリアルタイム警報システム「BEE CAREFUL」

科学技術情報通信部傘下のIITP(情報通信企画評価院)が主管する「SW中心大学事業」の一環として遂行した最終プロジェクトです。専門知識が不足している新規養蜂家の困難を解決するため、**AIを通じて蜜蜂の疾病を検知・診断し、リアルタイムで危険を警告し、RAGチャットボットで知識を共有する統合プラットフォーム**を企画・開発しました。

<br>

### 🖼️ プロジェクト核心機能のデモンストレーション (Live Demo)

![Project Demo GIF](https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EB%8D%B0%EB%AA%A8%EC%98%81%EC%83%81.gif?raw=true)

<br>

### 🖥️ サービス主要画面 (Service Screenshots)

<table>
  <tr>
    <td align="center" width="50%"><b>メインページ</b></td>
    <td align="center" width="50%"><b>主要サービス紹介</b></td>
  </tr>
  <tr>
    <td><img src="https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EB%A9%94%EC%9D%B8%ED%8E%98%EC%9D%B4%EC%A7%80.jpg?raw=true" alt="Main Page" width="100%"></td>
    <td><img src="https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EC%84%9C%EB%B9%84%EC%8A%A4%20%EB%AA%85%EC%84%B8.jpg?raw=true" alt="Key Services" width="100%"></td>
  </tr>
  <tr>
    <td align="center"><b>リアルタイム映像監視</b></td>
    <td align="center"><b>コミュニティ掲示板</b></td>
  </tr>
  <tr>
    <td><img src="https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EC%98%81%EC%83%81%EA%B0%90%EC%8B%9C%20%ED%8E%98%EC%9D%B4%EC%A7%80.png?raw=true" alt="Live Video Monitoring" width="100%"></td>
    <td><img src="https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EC%BB%A4%EB%AE%A4%EB%8B%88%ED%8B%B0.png?raw=true" alt="Community Board" width="100%"></td>
  </tr>
</table>

<br>

### ⚙️ サービス実装フロー (Service Implementation Flow)

![Service Implementation Flow](https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EC%84%9C%EB%B9%84%EC%8A%A4%EA%B5%AC%ED%98%84%20%ED%9D%90%EB%A6%84.jpg?raw=true)

<br>

### 💡 私の役割と貢献 (My Role & Contribution)

私は**チームリーダーとしてプロジェクトの総括を担当**し、特に**3つの独立したサーバーが通信するマイクロサービスアーキテクチャ(MSA)を直接設計し、システム全体を構築**する核心的な役割を遂行しました。

*   **システムアーキテクチャの設計および構築:**
    *   **メインサーバー、AIプロセッシングサーバー、警報サーバー**の3つの役割を明確に分離し、MSA構造を設計しました。これにより、各サーバーの負荷を分散させ、安定性と拡張性を確保しました。
    *  サーバー間の通信のためのREST APIエンドポイントを設計し、システム全体の円滑なデータフローに責任を持ちました。

*   **リアルタイムAIパイプラインの開発および最適化:**
    *   **データ前処理およびデータセット構築:** **チームメンバーと協力**し、AIモデルの基盤となるデータセットから構築しました。**RoboflowとOpenCVを活用**し、複雑な背景から**純粋な蜜蜂の画像のみを精密に抽出し、バウンディングボックスを生成**する前処理過程を主導しました。
    *   **モデルのファインチューニングおよび性能比較検証:** この精製されたデータをもとに、**VGG16とEfficientNetモデルを直接ファインチューニングし、性能を比較分析**することで、私たちのサービスに最も適したEfficientNetをデータに基づいて確定しました。
    *   **2-Stageモデルパイプラインの実装:** この検証済みのEfficientNetを、高速な物体検出が可能な**YOLOv8**と組み合わせ、**速度と精度の両方を確保する効率的な2段階パイプライン**を最終的に完成させました。

*   **バックエンドAPI開発およびシステム統合:**
    *   **核心APIサーバー開発:** ユーザー認証、映像ストリーミングAPI(`multipart/x-mixed-replace`方式)など、**私が直接担当したサーバーの全てのAPIを開発**しました。
    *   **信頼性の高い警報システムの実装:** 単発的な誤検出(False Positive)を防ぐため、一定回数以上疾病が累積した場合にのみ**Twilio API**でSMS警報を送信する**閾値ベースのAPIロジック**を実装しました。
    *   **システム統合担当:** **チームメンバーが開発したRAGチャットボットモジュール**を含む3つの独立サーバーを成功裏に統合し、フロントエンドとの連携を担当しました。

 <br>

### ⚙️ システムアーキテクチャ (System Architecture)

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

### 🌟 主要機能 (Key Features)

| 担当サーバー | 機能 | 詳細説明 |
| :--- | :--- | :--- |
| **AI Processing Server** | **リアルタイムAI映像分析** | ウェブカメラの映像をリアルタイムで受け取り、2-Stage AIパイプラインで疾病を検知し、結果が表示された映像をフロントエンドにストリーミングします。 |
| **Alert Server** | **累積ベースの危険警報** | AIサーバーから疾病データを受け取って累積カウントし、閾値を超えた場合にTwilio APIを通じて管理者にSMSを送信します。 |
| **Main Server** | **ウェブプラットフォーム及び情報提供** | ユーザー認証、画像ベースの診断APIを提供します。また、**LangChainとGPT-4o miniを活用したRAGチャットボット**を通じて、蜜蜂の疾病に関する専門的な情報を提供する役割を遂行します。 |

<br>

### 🛠️ 技術スタック (Tech Stack)

| 区分 | 技術 |
| :--- | :--- |
| **AI / ML** | <img src="https://img.shields.io/badge/TensorFlow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white"> <img src="https://img.shields.io/badge/YOLOv8-4F46E5?style=for-the-badge&logo=yolo&logoColor=white"> <img src="https://img.shields.io/badge/EfficientNet-8BC34A?style=for-the-badge"> <img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white"> |
| **Backend & API** | <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"> <img src="https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white"> <img src="https://img.shields.io/badge/Twilio-F22F46?style=for-the-badge&logo=twilio&logoColor=white"> |
| **Database** | <img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white"> <img src="https://img.shields.io/badge/SQLAlchemy-D71F00?style=for-the-badge&logo=sqlalchemy&logoColor=white"> |
| **Frontend** | <img src="https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black"> <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black"> |

<br>

### 🤔 課題解決の過程 (Challenges & Solutions)

#### **初期モデルの性能低下（誤検出率90%）問題の解決過程**

*   **問題点:** プロジェクト初期、EfficientNetベースのモデルの誤検出率が90%に達し、実用不可能でした。単にファインチューニング方式を導入するだけでは問題は解決されず、より根本的な原因分析が必要でした。
*   **解決過程（データ中心の体系的アプローチ）:**
    1.  **原因分析:** 私はこれが単純なモデルの問題ではなく、**「データ自体の構造的問題」** であると見抜きました。チームメンバーと共にデータセットを分析した結果、蜂の巣や幼虫などの不要な画像が混在し、疾病の特性により誤検出が発生していることを確認しました。
    2.  **データ精製および拡張:** この問題を解決するため、**RoboflowとOpenCVを活用して背景が複雑な画像から「蜜蜂」のオブジェクトのみを精密に抽出し**、データセットを既存の300枚から958枚に拡張して、モデルが良質なデータを学習できる環境を改善しました。
    3.  **アーキテクチャおよび学習の最適化:** **YOLOv8を組み合わせた2-Stage構造を設計**し、「検出」と「分類」の役割を分離しました。また、クラスの不均衡問題を緩和するため、**Focal Lossを適用し、疾病データに重み（正常:1.5、疾病:2.0）を付与**して学習のバランスを調整しました。
*   **結果:** このような反復的な試みの末、モデルの**mAPは0.96、分類精度は0.86を記録し、誤検出率も14.6%水準に大幅に減少**させました。この経験を通じて、「良いサービスは良いデータから始まる」という原則とデータ前処理の重要性を体感し、最終プロジェクトコンテストで**「大賞」** を受賞する成果につながりました。

<br>

### 💡 学んだこと (What I Learned)

*   **システム全体を見渡すアーキテクチャ設計能力:** 単純にモデルを学習させるだけでなく、独立したサーバーを連携させる**マイクロサービスアーキテクチャ**を直接設計・構築することで、システム全体を見る視野を確保しました。
*   **データ中心の問題解決能力:** モデルの性能が低い時、問題の根本原因が「データ」にある可能性を見抜き、データの精製と拡張を通じて実質的な性能改善を導いた経験を積みました。
*   **チームリーダーシップおよび技術的意思決定能力:** チームリーダーとしてメンバーの意見を調整し、共同の目標を設定する過程で、技術的な問題解決だけでなく、**円滑な協業と合理的な技術的意思決定能力**の重要性を深く学びました。

<br>

---

<details>
<summary><strong>🇬🇧 English Version (Click to expand)</strong></summary>

<br>

# AI-based Honeybee Disease Management and Real-time Alert System "BEE CAREFUL"

This is the final project conducted as part of the 'SW-Oriented University Project,' supervised by the IITP under the Ministry of Science and ICT. To address the challenges faced by new beekeepers lacking expert knowledge, we planned and developed a **comprehensive platform that detects and diagnoses honeybee diseases via AI, provides real-time risk alerts, and shares knowledge through a RAG chatbot.**

<br>

### 🖼️ Live Demo

![Project Demo GIF](https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EB%8D%B0%EB%AA%A8%EC%98%81%EC%83%81.gif?raw=true)

<br>

### 🖥️ Service Screenshots

<table>
  <tr>
    <td align="center" width="50%"><b>Main Page</b></td>
    <td align="center" width="50%"><b>Key Services Overview</b></td>
  </tr>
  <tr>
    <td><img src="https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EB%A9%94%EC%9D%B8%ED%8E%98%EC%9D%B4%EC%A7%80.jpg?raw=true" alt="Main Page" width="100%"></td>
    <td><img src="https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EC%84%9C%EB%B9%84%EC%8A%A4%20%EB%AA%85%EC%84%B8.jpg?raw=true" alt="Key Services" width="100%"></td>
  </tr>
  <tr>
    <td align="center"><b>Live Video Monitoring</b></td>
    <td align="center"><b>Community Board</b></td>
  </tr>
  <tr>
    <td><img src="https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EC%98%81%EC%83%81%EA%B0%90%EC%8B%9C%20%ED%8E%98%EC%9D%B4%EC%A7%80.png?raw=true" alt="Live Video Monitoring" width="100%"></td>
    <td><img src="https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EC%BB%A4%EB%AE%A4%EB%8B%88%ED%8B%B0.png?raw=true" alt="Community Board" width="100%"></td>
  </tr>
</table>

<br>

### ⚙️ Service Implementation Flow

![Service Implementation Flow](https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EC%84%9C%EB%B9%84%EC%8A%A4%EA%B5%AC%ED%98%84%20%ED%9D%90%EB%A6%84.jpg?raw=true)

<br>

### 💡 My Role & Contribution

As the **team leader, I was responsible for overseeing the project**, and played a key role in **personally designing a microservices architecture (MSA) with three independent servers and building the entire system.**

*   **System Architecture Design & Implementation:**
    *   Designed an MSA structure by clearly separating the roles of the **main server, AI processing server, and alert server**, thereby distributing the load and securing stability and scalability.
    *   Designed REST API endpoints for inter-server communication and was responsible for the smooth data flow of the entire system.

*   **Real-time AI Pipeline Development & Optimization:**
    *   **Data-driven Model Selection:** **Collaborated with team members** to build a honeybee image dataset, and **personally fine-tuned and compared the performance of VGG16 and EfficientNet models**. Through this systematic verification process, we selected EfficientNet as the most suitable model for our service.
    *   **2-Stage Model Pipeline Implementation:** Completed an efficient two-stage pipeline that achieves both **speed and accuracy** by combining the validated EfficientNet with the fast object detection capabilities of **YOLOv8**.

*   **Backend API Development & System Integration:**
    *   **Core API Server Development:** Personally developed all APIs for the servers I was in charge of, including user authentication and video streaming API (`multipart/x-mixed-replace`).
    *   **High-Reliability Alert System Implementation:** Implemented a **threshold-based API logic** that sends SMS alerts via the **Twilio API** only when the number of disease detections exceeds a certain threshold, to prevent false positives.
    *   **System Integration Lead:** Led the successful integration of three independent servers, including a RAG chatbot module developed by a team member, and handled the integration with the frontend.

 <br>

### ⚙️ System Architecture
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

### 🌟 Key Features

| Server | Function | Description |
| :--- | :--- | :--- |
| **AI Processing Server** | **Real-time AI Video Analysis** | Receives real-time video from a webcam, detects diseases through a 2-Stage AI pipeline, and streams the video with results to the frontend. |
| **Alert Server** | **Cumulative-based Risk Alert** | Receives disease data from the AI server, counts it cumulatively, and sends an SMS to the administrator via the Twilio API when a threshold is exceeded. |
| **Main Server** | **Web Platform & Information Service** | Provides APIs for user authentication and image-based diagnosis. Also serves as a RAG chatbot using **LangChain and GPT-4o mini** to provide expert information on honeybee diseases. |

<br>

### 🛠️ Tech Stack
(Same as the Korean version)

<br>

### 🤔 Challenges & Solutions
(English translation of the Challenges & Solutions section)

<br>

### 💡 What I Learned
(English translation of the What I Learned section)

</details>

<br>

<details>
<summary><strong>🇰🇷 한국어 원본 (클릭하여 펼치기)</strong></summary>

<br>

<!--
  여기에 우리가 최종적으로 완성한
  한국어 버전 README 내용을 그대로 복사해서 넣습니다.
-->
# AI 기반 꿀벌 질병 관리 및 실시간 경보 시스템 "BEE CAREFUL" 

과학기술정보통신부 산하 IITP(정보통신기획평가원)가 주관하는 'SW중심대학사업'의 일환으로 수행한 최종 프로젝트입니다. 전문 지식이 부족한 신규 양봉업자들의 어려움을 해결하고자, **AI를 통해 꿀벌의 질병을 탐지·진단하고, 실시간으로 위험을 경고하며, RAG 챗봇으로 지식을 공유하는 종합 플랫폼**을 기획하고 개발했습니다.

<br>

### 🖼️ 프로젝트 핵심 기능 시연 (Live Demo)

![Project Demo GIF](https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EB%8D%B0%EB%AA%A8%EC%98%81%EC%83%81.gif?raw=true)

<br>

### 🖥️ 서비스 주요 화면 (Service Screenshots)

<table>
  <tr>
    <td align="center" width="50%"><b>메인 페이지</b></td>
    <td align="center" width="50%"><b>주요 서비스 소개</b></td>
  </tr>
  <tr>
    <td><img src="https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EB%A9%94%EC%9D%B8%ED%8E%98%EC%9D%B4%EC%A7%80.jpg?raw=true" alt="메인 페이지" width="100%"></td>
    <td><img src="https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EC%84%9C%EB%B9%84%EC%8A%A4%20%EB%AA%85%EC%84%B8.jpg?raw=true" alt="주요 서비스 소개" width="100%"></td>
  </tr>
  <tr>
    <td align="center"><b>실시간 영상 감시</b></td>
    <td align="center"><b>커뮤니티 게시판</b></td>
  </tr>
  <tr>
    <td><img src="https://github.com/baekyutae/portfolio/blob/main/port_image/%EC%98%81%EC%83%81%EA%B0%90%EC%8B%9C%20%ED%8E%98%EC%9D%B4%EC%A7%80.png?raw=true" alt="실시간 영상 감시" width="100%"></td>
    <td><img src="https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EC%BB%A4%EB%AE%A4%EB%8B%88%ED%8B%B0.png?raw=true" alt="커뮤니티 게시판" width="100%"></td>
  </tr>
</table>

<br>

### ⚙️ 서비스 구현 흐름 (Service Implementation Flow)

![서비스 구현 흐름](https://raw.githubusercontent.com/baekyutae/portfolio/main/port_image/%EC%84%9C%EB%B9%84%EC%8A%A4%EA%B5%AC%ED%98%84%20%ED%9D%90%EB%A6%84.jpg?raw=true)

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
    *   **시스템 통합 담당:** **팀원이 개발한 RAG 챗봇 모듈**을 포함한 3개의 독립 서버를 성공적으로 통합하고, 프론트엔드와의 연동을 담당했습니다.

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



