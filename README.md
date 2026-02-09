🚩 Connect Bridge (장애인 맞춤형 길 안내 서비스)
Barrier-Free Navigator는 이동이 불편한 장애인들에게 최적의 경로를 제공하여 학교 시설 및 공공 장소에서의 이동 편의성을 향상시키기 위한 프로젝트입니다. 음성 인식(STT/TTS)을 통한 인터페이스와 장애물 데이터를 반영한 실시간 경로 최적화 알고리즘을 제공합니다.

🌟 주요 기능 (Key Features)
장애 맞춤형 경로 안내: 사용자의 장애 상태(휠체어, 시각 장애 등)를 고려하여 계단, 턱 등 장애물이 없는 최적의 경로를 산출합니다.

음성 기반 인터페이스 (Voice-Driven UI):

STT (Speech-to-Text): 구글 Speech API를 활용하여 음성으로 목적지를 검색합니다.

TTS (Text-to-Speech): 경로 및 장애물 정보를 음성으로 안내하여 시각 장애인의 사용성을 높였습니다.

실시간 장애물 회피 알고리즘:

자체 DB에 저장된 장애물 위치(위도/경도) 정보를 분석합니다.

카카오 Navi API의 경유지(Waypoints) 기능을 활용하여 장애물을 우회하는 최적 경로를 제공합니다.

실시간 장애물 등록 및 공유: 사용자가 직접 경로상의 장애물을 등록하고, 이를 기반으로 다른 사용자에게 더욱 정확한 경로를 안내합니다.

🛠 기술 스택 (Tech Stack)
Backend
Framework: Flask (Python)

Database: MySQL (PyMySQL)

External APIs:

Google Cloud STT / TTS API

Kakao Local API (주소-좌표 변환)

Kakao Navi API (경로 최적화 및 안내)

Frontend (App)
Android Studio (Java/Kotlin)

Libraries
PyAudio: 실시간 음성 입력 처리

NumPy: 좌표 데이터 배열 처리

🏗 시스템 아키텍처 (System Architecture)
Audio Processing: 사용자의 음성 입력을 받아 서버에서 STT를 거쳐 텍스트로 변환합니다.

Location Mapping: 변환된 목적지 텍스트를 카카오 API를 통해 좌표값으로 변환합니다.

Route Calculation:

DB에서 출발지-목적지 구간 사이의 장애물 데이터를 추출합니다.

장애물 위치를 피해가는 경로를 카카오 Navi 알고리즘을 통해 계산합니다.

Feedback: 계산된 경로와 장애물 주의 사항을 TTS로 변환하여 음성으로 안내하고, 화면에 카카오맵 경로를 표시합니다.

🚀 시작하기 (Getting Started)
Prerequisites
Python 3.8+

MySQL Server

Google Cloud Platform Service Account Key (JSON)

Kakao Developers API Key

Installation
Repository 클론

Bash
git clone https://github.com/your-repo/barrier-free-navigator.git
cd barrier-free-navigator
의존성 라이브러리 설치

Bash
pip install flask google-cloud-speech google-cloud-texttospeech requests pymysql numpy pyaudio
환경 설정

Server.py 및 Navigation.py 내의 API 키와 DB 접속 정보를 본인의 환경에 맞게 수정합니다.

Google 인증 키 파일(eminent-tape-*.json)을 프로젝트 루트에 배치합니다.

서버 실행

Bash
python Server.py
python Navigation.py
👥 팀원 (Team)
강범석 (Kang Bumseok) - Backend Developer (Server Logic, API Integration)

유동균 (Yoo Dong-gyun) - Frontend Developer (Android App Development)

💡 기대 효과
장애인의 이동권 보장 및 사고율 감소

실시간 장애물 데이터 축적을 통한 스마트 시티 인프라 기여

음성 제어를 통한 스마트 보조 기기와의 연동 가능성 확장
