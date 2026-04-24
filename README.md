# MIRAI 미르 상점 프로토타입

미르(재화) 상점 페이지 프로토타입. 정적 HTML/CSS/JS 단일 파일 기반.

## 🔗 라이브 데모

- 상점: https://seullyeee-art.github.io/mirai-store/
- 일일 작업: https://seullyeee-art.github.io/mirai-store/daily-tasks.html

## 📁 파일 구조

```
mirai-store/
├── index.html          # 메인 상점 페이지
├── daily-tasks.html    # 일일 작업 페이지
├── images/
│   ├── mir.png         # 미르 아이콘 (공용)
│   ├── 1.png ~ 6.png   # 미르 패키지 티어별 보석 이미지
│   ├── max.png         # MAX 구독권 배지
│   └── 380.png         # 왕관 아이콘 (MAX 타이틀 앞)
└── README.md
```

## 🎨 디자인 토큰 (index.html)

```css
/* Brand */
--primary: #14c391;        /* 메인 민트 */
--primary-heavy: #0fa87d;  /* 어두운 민트 */
--primary-glow: rgba(20, 195, 145, 0.35);
--mint-light: #5fffcf;
--cyan: #28d0ed;

/* Text */
--label-strong: #ffffff;
--label-normal: #f7f7f8;
--label-neutral: rgba(194, 196, 200, 0.88);
--label-alt: rgba(174, 176, 182, 0.61);
--label-assist: rgba(174, 176, 182, 0.28);

/* Background */
--bg-deep: #0f0f10;
--bg-base: #1b1c1e;
--bg-card: #222224;
--bg-card-alt: #28282b;

/* Line */
--line: rgba(20, 195, 145, 0.08);
--line-solid: #333335;

/* Status */
--negative: #ff6363;
--cautionary: #ffa938;
```

### 디폴트 CTA 버튼 (Figma `fill/normal` 기준)

```css
background: rgba(112, 115, 124, 0.22);
backdrop-filter: blur(32px);
-webkit-backdrop-filter: blur(32px);
/* :active */
background: rgba(112, 115, 124, 0.32);
```

> 기존 `rgba(255,255,255,0.06)` → 비활성화(opacity 0.5)와 구분 안 됨 피드백으로 교체.

## 🧱 주요 섹션 구조 (index.html)

```
<header class="header">                     헤더 (56px 고정)
<div class="banner ai d1">                  첫 구독 20% 할인 + 타이머
<div class="sec-hd">플랜 구매</div>
<div class="max-card">                      MAX 구독권
<div class="sec-hd col">미르 구매</div>
<div class="pkgs">                          미르 패키지 그리드 (6종)
  .grid3 × 2 (상/하 3개씩)
  .card / .card.prem                        일반 / 프리미엄
<div class="free-hd">무료 미르 충전소</div>
<div class="daily">                         일일 룰렛
  .roulette-wrap                            룰렛 휠 (JS 회전)
  .days                                     Day 1~7 체크
  .dbtn.spin / .dbtn.ad-retry              CTA
<div class="earn">                          광고 보기 / 일일 작업 카드
<div class="alist">                         액션 리스트
  - 친구 초대, 트위터, Discord, 알림, 인센티브 전환
<div class="hist-row">                      미르 내역 + 구독 관리
<div class="footer">                        약관/환불 정책
<div class="bs">                            바텀시트 (알림 동의)
<div class="cv">                            인센티브 전환 바텀시트
```

## 🎯 주요 인터랙션

### 1. 일일 룰렛
- `spinRoulette()` — 5칸 확률 가중치 (1/3/5/10/50 미르)
- 4초간 회전 애니메이션 (CSS transform + cubic-bezier)
- **첫 스핀**: 무료 → 버튼이 `🎬 광고보고 다시돌리기 (0/3)`로 변경
- **광고 3회 소진**: "오늘 완료!" → 룰렛 슬라이드 숨김 + 바텀시트(알림 동의) 노출
- 결과: 컨페티 + 토스트 + Day N에 +미르 표시

### 2. 카운트다운 타이머 (배너)
- `updateTimers()` — 1초마다 업데이트
- 24시간에서 카운트다운
- `#btH #btM #btS` 세그먼트에 `pad()` 2자리 숫자
- 모바일: SEC 세그먼트 + 라벨 자동 숨김 (`@media (min-width: 768px)` 제외)

### 3. 토스트
```js
showToast(메시지, 미르값?)  // mir 생략하면 일반 메시지만
```

### 4. 액션 리스트 클릭 → 사라짐
```js
claimAction(btn, '완료 메시지', 미르)  // 아이템 접히며 제거
```

### 5. 인센티브 전환 모달
- `cvAdd(N)` — 칩 클릭 시 입력값 +N
- 입력 이벤트 리스너로 실시간 미르 표시 동기화

## 📱 반응형

- **모바일 (<768px)**: 기본
- **데스크탑 (≥768px)**: `max-width: 960px`, 컴포넌트별 폰트/패딩 확장

## 🔧 로컬 실행

```bash
cd mirai-store
python3 -m http.server 8080
# 같은 네트워크 폰 접속: http://<로컬IP>:8080
```

## 🚀 배포

GitHub Pages (branch: main, root). `git push` 시 자동 배포.
Repo: https://github.com/seullyeee-art/mirai-store

## 📝 최근 주요 작업 이력

1. **더블찬스/따따블 이벤트 제거** — 로직 복잡도 이슈로 원복, 단순 룰렛 + 광고 3회로 복귀
2. **CTA 버튼 Figma 토큰 적용** — `fill/normal` (112,115,124,0.22) + backdrop-blur
3. **헤더 56px 고정** + 꺽쇠 배경 제거
4. **배너 프리미엄 리디자인** — 골드 20% + 세그먼트 타이머
5. **미르 아이콘 PNG 교체** — SVG polygon → `mir.png` / 티어별 1~6.png
6. **일일 룰렛 플로우**: `돌리기 → 광고보고 다시돌리기(N/3) → 오늘 완료 → 바텀시트`

## 💡 다음 세션 참고사항

- **수정 전 반드시 파일 전체 구조 파악** (단일 HTML 파일이 60KB+, CSS/JS 모두 인라인)
- **CSS 수정 시 미디어 쿼리도 함께 확인** (`@media (min-width: 768px)` 데스크탑 대응)
- **id/class 참조는 JS ↔ HTML 양방향 체크** (삭제한 요소 JS에서 참조하면 런타임 에러)
- **이미지 추가 시**: `images/` 폴더 + HTML 상대경로 `images/파일명`
- **미르 가격/패키지 수정**: index.html의 `.pkgs` 섹션 (6장 카드 하드코딩)
- **배포**: `git add -A && git commit -m "..." && git push`

## 🧰 사용 스택

- Vanilla HTML/CSS/JS (프레임워크 없음)
- Pretendard Variable (CDN)
- GitHub Pages (정적 호스팅)
