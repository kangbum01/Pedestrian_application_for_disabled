🚩 Connect Bridge
장애인 맞춤형 길 안내 및 음성 인터페이스 서비스

기술을 통해 이동 장벽을 허물고, 모두를 위한 보편적 이동권을 실현합니다.

<p align="center"> <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"> <img src="https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white"> <img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white"> <img src="https://img.shields.io/badge/Android%20Studio-3DDC84?style=for-the-badge&logo=android-studio&logoColor=white">


<img src="https://img.shields.io/badge/Google%20Cloud%20STT/TTS-4285F4?style=for-the-badge&logo=google-cloud&logoColor=white"> <img src="https://img.shields.io/badge/Kakao%20API-FFCD00?style=for-the-badge&logo=kakao&logoColor=black"> </p>

📌 Project Overview
Barrier-Free Navigator는 휠체어 이용자나 시각 장애인 등 이동 약자들이 겪는 '길 위의 장애물' 문제를 해결하기 위해 기획되었습니다. 단순한 최단 거리 안내가 아닌, 장애물 데이터베이스 기반의 안전한 경로를 제공하며, 모든 조작을 음성으로 제어할 수 있는 배리어 프리 인터페이스를 지향합니다.

✨ Key Features
기능,상세 설명
♿ 맞춤형 경로 산출,"계단, 턱, 공사 구역 등 사용자 설정에 따른 장애물 회피 경로 안내"
🎙️ Voice-Driven UI,Google STT/TTS를 통한 음성 목적지 검색 및 실시간 음성 가이드
📍 실시간 장애물 회피,카카오 Navi API의 Waypoints 기능을 활용해 DB 내 장애물을 우회하는 경로 계산
🤝 커뮤니티형 데이터,사용자가 직접 현장의 장애물을 등록하고 실시간으로 공유하는 시스템

🏗 System Architecture
서비스의 흐름은 아래와 같은 단계로 이루어집니다.

Voice Input: PyAudio로 수집된 음성을 Google STT가 텍스트로 변환.

Logic Processing: Flask 서버가 텍스트를 분석하고 Kakao Local API로 좌표값 획득.

Data Filtering: MySQL DB에서 출발지-목적지 사이의 장애물(Obstacles) 리스트 추출.

Route Optimization: 장애물 좌표를 경유지로 설정하여 Kakao Navi API를 통해 최적 우회 경로 생성.

Output: 안내 메시지를 Google TTS로 변환하여 음성 출력 및 앱 화면에 경로 표시.

🛠 Tech Stack
Language & Framework
Backend: Python (Flask)

Frontend: Android Studio (Java / Kotlin)

Infrastructure & API
Database: MySQL (PyMySQL)

Speech: Google Cloud Speech-to-Text / Text-to-Speech

Map/Navi: Kakao Maps API, Kakao Navi API

Libraries
NumPy: 좌표 및 경로 데이터 처리를 위한 수치 계산

Requests: 외부 API 통신 및 데이터 송수신

🚀 Getting Started
1. Prerequisites
Python 3.8 이상

Google Cloud Service Account Key (JSON 파일)

Kakao Developers API Key

2. Installation & Setup
Bash
# 레포지토리 클론
git clone https://github.com/your-username/barrier-free-navigator.git
cd barrier-free-navigator

# 필요 라이브러리 설치
pip install flask google-cloud-speech google-cloud-texttospeech requests pymysql numpy pyaudio
3. Execution
서버 측 코드를 실행하여 API 엔드포인트를 활성화합니다.

Bash
python Server.py       # 음성 처리 서버 실행
python Navigation.py   # 경로 분석 서버 실행

📸 Demo Screenshots

👥 Contributors
강범석 (Kang Bumseok) - Backend Developer (Server Architecture, API & DB Integration)

유동균 (Yoo Dong-gyun) - Frontend Developer (Android App Development & UI/UX)
