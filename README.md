# ♿ 장애인 길안내 서비스 (Accessibility Navigation Service)

학술제 프로젝트로, **장애인의 이동 편의성을 높이기 위해 장애 상태·출발지·도착지 정보를 기반으로 최적 경로를 안내**하는 서비스입니다.  
앱은 Android로 구현하고, 서버는 Flask로 구성하여 **장애물 DB + 카카오 지도/내비 API**를 이용해 경로를 계산/표시합니다. :contentReference[oaicite:2]{index=2}

---

## 🧩 목차
- [프로젝트 목적](#-프로젝트-목적)
- [작품 설명](#-작품-설명)
- [개발 결과](#-개발-결과)
- [기대효과 및 목표](#-기대효과-및-목표)
- [기술 스택](#-기술-스택)
- [시스템 구조](#-시스템-구조)
- [API 명세](#-api-명세)
- [DB 스키마(장애물)](#-db-스키마장애물)
- [설치 및 실행 방법](#-설치-및-실행-방법)
- [보안 주의사항](#-보안-주의사항)
- [개선 사항](#-개선-사항)
- [팀원](#-팀원)

---

## 🎯 프로젝트 목적
- **장애인에게 최적의 경로 제공**
- 학교 시설 및 길찾기 편의성 향상 :contentReference[oaicite:3]{index=3}

---

## 🧠 작품 설명
### App (Android)
- Android Studio로 개발
- 사용자의 **장애 상태 / 출발지 / 도착지**를 서버로 전송
- 서버가 계산한 **최적 경로를 안내** :contentReference[oaicite:4]{index=4}

### Server (Flask)
- Flask 기반 서버
- 전달받은 데이터에 맞게 **장애물 위치 데이터(DB)**를 활용해 경로 설정
- 경로 설정 및 지도 출력은 **카카오 API** 사용 :contentReference[oaicite:5]{index=5}

---

## ✅ 개발 결과
- 사용자 위치 표시
- 장애물 거리 표시
- 경로 표시 :contentReference[oaicite:6]{index=6}

---

## 🌱 기대효과 및 목표
- 추천 경로 기반으로 안전하게 목적지 도달 가능
- 실시간 장애물 등록을 통해 더 최적화된 길 제공
- 서비스 적용 범위 확대 → 장애인 사고율 감소 기대 :contentReference[oaicite:7]{index=7}

---

## 🛠 기술 스택
- **Android**: Android Studio (클라이언트 앱)
- **Backend**: Python, Flask
- **STT/TTS**: Google Cloud Speech-to-Text / Text-to-Speech (`Server.py`)
- **지도/경로**: Kakao Local Address API, Kakao Mobility Directions API (`Navigation.py`)
- **DB**: MySQL(MariaDB) + `pymysql` (장애물 데이터 저장)
- **기타**: `numpy`, `requests`, (옵션) `pyaudio` 실시간 음성 입력

---

## 🧱 시스템 구조
1) 앱에서 사용자 정보 전송  
- `Id`, `StartingLocation`, `DepartureLocation`, `Timestamp`, `Disability`

2) 서버에서 주소 → 좌표 변환  
- 카카오 주소 검색 API로 출발/도착 좌표 획득

3) DB에서 장애물 조회  
- 출발~도착 범위 사각형 내 장애물(종류/좌표) 조회

4) 카카오 내비 Directions API로 경로 요청  
- 장애물 위치를 경유지/회피 로직에 활용(현재 코드는 waypoints 구성)

5) 앱에 결과 반환  
- 좌표 배열, 장애물 목록, 최적 경로 JSON, 카카오 지도 URL 등

---

## 📡 API 명세

### 1) 최적 경로 요청 (Navigation 서버)
**POST** `/get_optimal_route`

**Request Body (JSON)**
```json
{
  "Id": "20204068",
  "StartingLocation": "출발지 주소",
  "DepartureLocation": "도착지 주소",
  "Timestamp": "2026-02-09T12:00:00",
  "Disability": "wheelchair"
}
Response 예시

{
  "StartingLocation": "출발지 주소",
  "DepartureLocation": "도착지 주소",
  "CoordinatesArray": [[위도, 경도], [위도, 경도]],
  "Obstacles": [{"type": "stairs", "location": [lat, lon]}],
  "OptimizedRoute": { "...kakao directions response..." },
  "KakaoMapUrl": "https://map.kakao.com/link/to/도착지명,경도,위도"
}
2) 실시간 음성 인식 시작 (STT/TTS 서버)
GET /start_live_speech

서버에서 마이크 입력을 받아 STT 수행 후 client.txt에 저장

인식된 텍스트를 TTS로 변환하여 response.mp3로 저장

⚠️ 이 엔드포인트는 서버 프로세스가 실시간 입력 루프에 들어가므로,
운영 환경에서는 비동기 처리(스레드/큐) 또는 별도 워커로 분리하는 것을 권장합니다.

3) (옵션) 오디오 파일 업로드 기반 STT/TTS
Server.py 주석에 포함된 방식

POST /process_audio

multipart/form-data로 audio 파일 업로드 → STT → TTS(mp3) 반환

🗄 DB 스키마(장애물)
obstacle_db.obstacles

컬럼	타입(예시)	설명
obstacle_type	VARCHAR	장애물 종류(예: stairs, slope, etc)
latitude	DOUBLE	위도
longitude	DOUBLE	경도
Navigation.py는 출발~도착 좌표 범위(사각형) 내 장애물을 조회합니다.

🚀 설치 및 실행 방법
1) Backend 공통 준비
python -m venv venv
# windows: venv\Scripts\activate
# linux/mac: source venv/bin/activate
pip install -U pip
pip install flask requests numpy pymysql google-cloud-speech google-cloud-texttospeech
실시간 음성 입력을 쓰는 경우:

pip install pyaudio
2) 환경변수 설정 (권장)
✅ .env 예시(직접 생성)
KAKAO_API_KEY=YOUR_KAKAO_KEY
DB_HOST=127.0.0.1
DB_USER=root
DB_PASSWORD=YOUR_PASSWORD
DB_NAME=obstacle_db

GOOGLE_STT_CREDENTIALS=path/to/stt_key.json
GOOGLE_TTS_CREDENTIALS=path/to/tts_key.json
현재 코드처럼 키/비밀번호를 파일에 하드코딩하지 말고,
실행 시 환경변수로 읽도록 수정하는 것을 권장합니다.

3) Navigation 서버 실행
python Navigation.py
# 기본: http://127.0.0.1:5000
4) STT/TTS 서버 실행
python Server.py
현재 Server.py는 실행 시 live_speech_to_text()를 바로 호출합니다.
운영용이라면 Flask app.run() 형태로 바꾸고, 엔드포인트 호출 시 스레드로 실행하도록 분리하는 편이 안정적입니다.

🔐 보안 주의사항
절대 커밋 금지

Google Cloud 인증키(*.json)

Kakao API Key

DB 비밀번호

.gitignore에 아래를 추가하세요.

*.json
.env
client.txt
response.mp3
__pycache__/
venv/
🧪 트러블슈팅 (자주 발생)
Kakao API 401: KAKAO_API_KEY 누락/오타/권한 확인

DB 연결 실패: host/user/password/database 값 및 포트(3306) 확인

PyAudio 설치 오류(Windows): 빌드 도구/호환 wheel 필요 → 파일 업로드 방식(/process_audio)을 우선 사용 권장

Google STT/TTS 오류: 인증키 경로/권한/프로젝트 설정 확인

🔧 개선 사항
장애 분류 세분화: 사용자 UI에서 불편 부위를 선택 → 장애 유형에 맞춘 맞춤 경로 설계 
소프트웨어공학 발표자료


실시간 장애물 감지: 센서 기반 장애물 실시간 탐지/분석 → 위험 상황 대응 
소프트웨어공학 발표자료


사용자 맞춤형 네비게이션: 현재 위치 + 장애물 데이터 기반 안전·효율 경로 제공 
소프트웨어공학 발표자료


👥 팀원
유동균 (20204077) 
소프트웨어공학 발표자료


강범석 (20204068) 
소프트웨어공학 발표자료
