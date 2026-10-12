<div align="center">

<img src="frontend/src/assets/brand/logo.png" width="120" alt="한끼뚝딱 로고" />

# 한끼뚝딱

**냉장고 속 재료로 오늘 한 끼를 뚝딱 — 식재료 관리 · 레시피 추천 · 최저가 장보기 서비스**

🏆 **SSAFY 15기 1학기 관통 프로젝트 구미 1반 최우수상**

**한국어** · [English](README_en.md)

![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-5.2-092E20?logo=django&logoColor=white)
![DRF](https://img.shields.io/badge/DRF-3.17-A30000)
![Vue](https://img.shields.io/badge/Vue-3.5-4FC08D?logo=vuedotjs&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-8-646CFF?logo=vite&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT%20·%20gpt--image-412991?logo=openai&logoColor=white)

![랜딩](docs/screenshots/02-landing.gif)

</div>

---

## 목차
1. [프로젝트 소개](#1-프로젝트-소개)
2. [팀원 & 역할](#2-팀원--역할)
3. [기술 스택 & 아키텍처](#3-기술-스택--아키텍처)
4. [주요 기능](#4-주요-기능)
5. [추천 알고리즘](#5-추천-알고리즘)
6. [데이터 & ERD](#6-데이터--erd)
7. [실행 방법](#7-실행-방법)

---

## 1. 프로젝트 소개

| 항목 | 내용                                               |
| :--- | :------------------------------------------------- |
| 기간 | 2026.05 ~ 2026.06                                  |
| 인원 | 2명 (신지환, 현승준)                |
| 구분 | SSAFY 15기 1학기 최종 관통 프로젝트 |
| 수상 | 🏆 **구미 1반 최우수상**                            |

### 문제 정의
- 냉장고에 재료가 있어도 **무엇을 만들지 몰라** 재료가 버려진다.
- 레시피를 찾아도 **부족한 재료를 어디서 얼마에** 사야 할지 알기 어렵다.
- 장 본 재료를 일일이 입력하는 것은 번거롭다.

### 해결
| 문제            | 한끼뚝딱의 해결 방식                                                            |
| :-------------- | :------------------------------------------------------------------------------ |
| 뭘 만들지 모름  | 보유 재료 기반 **100점 만점 추천 엔진** + 식단/알레르기 맞춤 **AI 레시피 생성** |
| 재료 낭비       | 소비기한 임박 재료 **알림** 및 우선 노출                                        |
| 입력이 번거로움 | **영수증 사진 한 장**으로 냉장고 자동 등록 (OCR + LLM)                          |
| 장보기 비용     | 내 지역 마트별 **최저가 비교 + 지도** 표시                                      |

---

## 2. 팀원 & 역할

<table>
  <tr>
    <td align="center" width="50%">
      <img src="https://github.com/jihwan4155.png?size=200" width="100" alt="신지환" /><br />
      <b>신지환</b><br />
      기획 · Full-stack · UI/UX<br />
      <a href="https://github.com/jihwan4155">GitHub</a>
    </td>
    <td align="center" width="50%">
      <img src="https://github.com/sejhyun13.png?size=200" width="100" alt="현승준" /><br />
      <b>현승준</b><br />
      Full-stack · AI / 외부 API<br />
      <a href="https://github.com/sejhyun13">GitHub</a>
    </td>
  </tr>
</table>

### 신지환
- **레시피 추천 엔진 설계**: 레시피/유저 커버리지 결합, 인기도·요리 종류 가중치, 초간단 레시피 패널티 → 100점 정규화
- **커뮤니티**: 게시글·댓글 CRUD, 임시저장(Draft) & 이어쓰기, 해시태그·카테고리 라우트
- **사용자**: 온보딩(알레르기·식단·지역), 마이페이지, 타 유저 프로필, 팔로우
- **홈 콘텐츠**: 오늘의 랜덤 레시피, 인기 레시피 TOP 5, 유튜브 레시피 연동, 만개의레시피 크롤러 보강
- **UI/UX 디자인**: Figma 프로토타입, 컬러·폰트 테마, 마스코트(햄스터·펭귄), 랜딩/홈 대시보드
- **성능**: `annotate(Count)` · `select_related` · `Prefetch`로 N+1 제거, 공통 쿼리셋 구조화
- **데이터**: `bulk_create` 기반 대량 픽스처 적재 커맨드, 데모용 더미 데이터 생성

### 현승준
- **최저가 비교 & 지도**: 한국소비자원 OpenAPI 연동, 전국 매장/상품 카탈로그 캐시, 좌표 이상치 정규화, 카카오맵 마커·오버레이
- **영수증 → 냉장고 자동 등록**: CLOVA OCR 좌표 기반 줄 복원 + LLM 정제 + 표준 식재료 매칭
- **AI 레시피**: 식단·알레르기 프롬프트 주입, JSON 구조화 응답, 결과 캐싱, LLM 식단 검수, OpenAI(gpt-image-1-mini) 이미지 생성
- **인증**: 토큰 인증, 카카오·네이버·구글 OAuth2 Authorization Code 플로우
- **프론트**: 식재료 피커(카테고리 테마, 1,000종 아이콘 비동기 로드), 반응형 레이아웃
- **성능**: 복합 인덱스 마이그레이션, 추천 쿼리 평가 횟수 축소, Vite 코드 스플리팅
- **기타**: 데모 일괄 세팅(`setup_demo`), 약관·법적 고지, 브랜드 에셋

### 공동
- 추천 알고리즘 수식 설계 및 튜닝, 기획 · DB 모델링 · 발표

---

## 3. 기술 스택 & 아키텍처

| 구분     | 기술                                                                                                   |
| :------- | :----------------------------------------------------------------------------------------------------- |
| Backend  | Python 3.11, Django 5.2, Django REST Framework, dj-rest-auth, django-allauth, SQLite                   |
| Frontend | Vue 3, Vue Router, Vite, Fetch API                                                                     |
| AI       | OpenAI API — 영수증 정제 · 레시피 생성 · 식단/게시글 검수 (GPT), 레시피 이미지 생성 (gpt-image-1-mini) |
| 외부 API | NAVER CLOVA OCR, 한국소비자원 생필품 가격 OpenAPI, Kakao Maps JS SDK, Kakao/Naver/Google OAuth         |
| 협업     | GitLab, Figma                                                                                          |

![아키텍처](docs/screenshots/01-architecture.png)

---

## 4. 주요 기능

### 4-1. 랜딩 & 홈 대시보드

|                   랜딩                   |             홈 대시보드             |
| :--------------------------------------: | :---------------------------------: |
| ![랜딩](docs/screenshots/02-landing.gif) | ![홈](docs/screenshots/03-home.gif) |

- 오늘의 랜덤 레시피, 인기 레시피 TOP 5, 스타 셰프, 카테고리 바로가기
- 소비기한 임박 재료 알림 배너

### 4-2. 회원가입 · 소셜 로그인 · 온보딩

|                  로그인                  |                    온보딩                     |
| :--------------------------------------: | :-------------------------------------------: |
| ![로그인](docs/screenshots/04-login.gif) | ![온보딩](docs/screenshots/05-onboarding.gif) |

- 카카오 · 네이버 · 구글 OAuth2 (인가 코드는 프론트, 토큰 교환은 서버)
- 가입 직후 알레르기 · 선호 식단 · 거주 지역 설정 → 추천/가격 비교에 바로 반영

### 4-3. 냉장고 관리 & 소비기한 알림

|                 내 냉장고                 |                    재료 추가                    |
| :---------------------------------------: | :---------------------------------------------: |
| ![냉장고](docs/screenshots/06-fridge.gif) | ![재료추가](docs/screenshots/07-fridge-add.gif) |

- 1,000종 표준 식재료 + 카테고리별 아이콘 피커
- 소비기한 D-day 계산, 5일 이하 임박 재료 우선 노출

### 4-4. 영수증 OCR 자동 등록 ⭐

<p align="center"><img src="docs/screenshots/08-receipt-ocr.gif" width="50%"/></p>

- CLOVA OCR 바운딩 박스 **y좌표 기반 줄 병합**으로 품목 행 복원
- LLM이 `국산 삼겹살 500g` → `삼겹살 / 500 / g` 형태로 구조화, 배송비·적립 등 노이즈 제거
- 별칭 테이블(10만+ 표기)로 표준 식재료에 매칭 후 일괄 등록

### 4-5. 재료 기반 레시피 추천 ⭐

|                     재료 선택                      |                      추천 결과                      |
| :------------------------------------------------: | :-------------------------------------------------: |
| ![재료선택](docs/screenshots/09-recipe-select.gif) | ![추천결과](docs/screenshots/10-recipe-results.gif) |

- 매칭(IDF 가중) + 인기도 + 요리 종류 − 패널티 → **100점 정규화 점수** ([상세](#5-추천-알고리즘))
- 알레르기 · 식단 금지 재료는 쿼리 단계에서 제외, LLM 식단 검수로 2차 확인

### 4-6. AI 맞춤 레시피 & 이미지 생성 ⭐

|                    생성 중                    |                  생성 결과                   |
| :-------------------------------------------: | :------------------------------------------: |
| ![AI로딩](docs/screenshots/11-ai-loading.gif) | ![AI결과](docs/screenshots/12-ai-result.gif) |

- 냉장고 재료 + 식단 + 알레르기를 프롬프트에 주입, JSON 스키마로 응답 고정
- 동일 조건 요청은 캐시로 재사용해 비용·지연 절감
- LLM이 만든 영어 이미지 프롬프트로 완성 요리 이미지 생성 (OpenAI gpt-image-1-mini, 장당 약 $0.005), 같은 프롬프트는 파일 캐시로 재사용

### 4-7. 최저가 비교 & 마트 지도 ⭐

|                     가격 비교                      |                  마트 지도                  |
| :------------------------------------------------: | :-----------------------------------------: |
| ![가격비교](docs/screenshots/13-price-compare.gif) | ![지도](docs/screenshots/14-market-map.gif) |

- 레시피 부족 재료 / 냉장고 추가 구매 품목 → 내 지역 매장별 총액 비교
- 위경도 반전 · `(1,1)` 더미 좌표 등 공공데이터 이상치 정규화
- 미매칭 품목은 안내 모달로 처리하고, API 실패 시에도 다른 기능은 정상 동작

### 4-8. 커뮤니티

|                      메인                      |                       글 작성                       |                       상세                        |
| :--------------------------------------------: | :-------------------------------------------------: | :-----------------------------------------------: |
| ![커뮤니티](docs/screenshots/15-community.gif) | ![글작성](docs/screenshots/16-community-create.gif) | ![상세](docs/screenshots/17-community-detail.gif) |

- 레시피 게시글 CRUD, 댓글, 좋아요, 해시태그, 팔로우
- 작성 중 이탈 대비 **임시저장 & 이어쓰기**
- 등록 전 규칙 + LLM 기반 **게시글 자동 검수** (욕설·비요리 글 차단)
- 유튜브 레시피 영상 연동

### 4-9. 마이페이지

|                  마이페이지                   |                     유저 프로필                     |
| :-------------------------------------------: | :-------------------------------------------------: |
| ![마이페이지](docs/screenshots/18-mypage.gif) | ![유저프로필](docs/screenshots/19-user-profile.gif) |

- 찜한 레시피·게시글, 작성 글, 팔로워/팔로잉, 프로필 수정 · 비밀번호 변경 · 탈퇴

---

## 5. 추천 알고리즘

$$S_{\text{final}} = \text{clip}_{[0,100]}\left(S_{\text{match}} + S_{\text{popularity}} + S_{\text{type}} - P_{\text{penalty}}\right)$$

| 요소      | 배점  | 내용                                               |
| :-------- | :---: | :------------------------------------------------- |
| 재료 매칭 |  30   | 레시피 커버리지 15 + IDF 가중 유저 커버리지 15     |
| 인기도    |  40   | (스크랩 + 추천 수) / 후보군 최댓값                 |
| 요리 종류 |  30   | 메인 식사 30 / 소스·음료·디저트 10                 |
| 패널티    |   −   | 제목 키워드(소스·양념장 등) −20, 재료 2개 이하 −15 |

- 사전 필터: 알레르기 재료 포함 레시피 제외, 식단 프리셋(비건·락토·오보) 금지 재료 제외
- IDF: $IDF_i = \ln\left(1 + \frac{N}{\max(DF_i, 1)}\right)$ — 소금·물보다 소고기·오징어 같은 재료 매칭에 가중

---

## 6. 데이터 & ERD

| 데이터                     | 규모        |
| :------------------------- | :---------- |
| 레시피 (만개의레시피 정제) | 234,538건   |
| 레시피-재료 관계           | 약 210만 건 |
| 식재료 별칭                | 106,994건   |
| 전체 픽스처                | 약 254만 건 |

![ERD](ERD.png)

자세한 스키마는 [ERD.md](ERD.md) 참고.

---

## 7. 실행 방법

```bash
# 1) 환경 변수 (기존 .env가 있으면 덮어쓰지 않음)
cp -n .env.example .env   # 이후 API 키 입력

# 2) Backend
cd backend
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
python manage.py setup_demo     # 마이그레이션 + 데모 데이터 일괄 적재
python manage.py runserver

# 3) Frontend
cd ../frontend
npm install
npm run dev                     # http://localhost:5173
```

필요한 환경 변수는 [.env.example](.env.example) 참고. API 키가 없어도 서버는 뜨며, 해당 기능만 비활성화됩니다.
