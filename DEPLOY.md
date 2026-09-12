# 포트폴리오 배포 안내

작성: 2026-09-12

## 1. 지금 작업물이 어디 있나

| 위치 | 내용 | 주의 |
|---|---|---|
| 이 대화의 파일 카드 `portfolio.zip` | `index.html` + `assets/` + 이 문서 | **이걸 내려받는 게 시작점** |
| 이 대화의 파일 카드 `윤태형_포트폴리오_단일파일.html` | 폰트·사진을 HTML에 심은 단일 파일 (612KB) | 파일 하나만 쓸 때 |
| 아티팩트 링크 (대화의 카드) | 브라우저 미리보기, 비공개 | 배포본과 별개 |
| 클라우드 작업 폴더 | 원본 | **세션이 끝나면 사라짐. zip을 꼭 내려받을 것** |

작업은 Claude 클라우드 컨테이너에서 했고, 내 컴퓨터에는 아직 아무것도 없다. zip을 내려받아 압축을 푸는 것부터 시작한다.

## 2. 파일 구조

```
portfolio/
├─ index.html                      40KB  페이지 전체 (HTML + CSS + JS 한 파일)
├─ DEPLOY.md                             이 문서
└─ assets/
   ├─ GmarketSansMedium.woff2     419KB  지마켓 산스 (무료 배포 웹폰트)
   └─ photo.jpg                    19KB  프로필 사진
```

`assets/`를 같이 올려야 폰트와 사진이 나온다. `index.html`만 올리면 시스템 기본 폰트로 떨어지고 사진이 깨진다.

## 3. 올리는 순서

### 3-1. 준비

```bash
# 압축 푼 폴더로 이동
cd ~/Downloads/portfolio          # 실제 경로로 바꿀 것

# 파일 확인 — index.html, assets/, DEPLOY.md 가 보여야 한다
ls -la
ls -la assets
```

git 최초 사용이면 한 번만:

```bash
git config --global user.name "YunTaeng"
git config --global user.email "howard9166@naver.com"
```

### 3-2. 커밋

```bash
git init -b main

# Jekyll 자동 처리 끄기 (정적 파일만 쓸 때 안전)
touch .nojekyll

git add .
git commit -m "포트폴리오 초기 버전"
```

### 3-3. 레포 생성 + 푸시

레포 이름은 **`YunTaeng.github.io`** 로 정확히. 이름이 다르면 주소가 `yuntaeng.github.io/레포이름` 형태가 된다.

gh CLI가 있으면 한 줄:

```bash
gh repo create YunTaeng.github.io --private --source=. --push
```

없으면 github.com에서 private 레포를 만든 뒤:

```bash
git remote add origin https://github.com/YunTaeng/YunTaeng.github.io.git
git push -u origin main
```

### 3-4. Pages 켜기

레포 → **Settings → Pages**

- **Source**: Deploy from a branch
- **Branch**: `main` / `/(root)` → Save

여기서 갈린다:

- **설정이 저장되면** → Pro가 적용된 상태. 1~2분 뒤 `https://yuntaeng.github.io` 접속
- **업그레이드 안내가 뜨거나 잠겨 있으면** → 무료 플랜은 private 레포에서 Pages를 못 쓴다. 아래 중 하나를 선택
  - 레포를 public으로 전환 (Settings → General → 맨 아래 Change visibility) — 무료, 주소 그대로
  - GitHub Pro 결제 (월 $4) — private 유지, 단 **사이트 자체는 공개됨**
  - Cloudflare Pages / Netlify / Vercel 무료 플랜 — private 레포 연결 가능, 주소는 `*.pages.dev` 등

### 3-5. 확인

```bash
curl -I https://yuntaeng.github.io
```

`HTTP/2 200` 이면 정상. 브라우저에서 볼 것:

- [ ] 글꼴이 지마켓 산스로 나오는가 (아니면 `assets/` 누락)
- [ ] 프로필 사진이 보이는가
- [ ] 창을 넓혔을 때 왼쪽 네비바가 나오고, 스크롤에 따라 현재 섹션이 청록색으로 바뀌는가
- [ ] 폰 크기(400px)에서 가로 스크롤이 생기지 않는가

## 4. 수정하고 다시 올릴 때

```bash
# index.html 수정 후
git add index.html
git commit -m "문구 수정"
git push
```

푸시하면 Pages가 다시 빌드해서 **1분 안에** 사이트가 바뀐다. 안 바뀌어 보이면 강력 새로고침(`Cmd/Ctrl + Shift + R`) — 브라우저 캐시다.

배포 상태는 레포의 **Actions** 탭 또는 Settings → Pages 상단에서 확인할 수 있다.

## 5. 막히는 지점

| 증상 | 원인 |
|---|---|
| 글꼴이 기본 고딕으로 나옴 | `assets/GmarketSansMedium.woff2` 미포함 또는 경로 오타 |
| 사진만 안 나옴 | `assets/photo.jpg` 미포함 |
| 404 | 레포 이름이 `YunTaeng.github.io`가 아니거나, Pages 브랜치가 `main`이 아님 |
| 푸시했는데 안 바뀜 | 브라우저 캐시. 강력 새로고침 |
| 학생 팩 만료 후 사이트 내려감 | private 레포 Pages는 Pro 전용. public 전환 필요 |

## 6. 아직 안 채운 것

- 캡스톤 수상명 — 이력서에 없어서 페이지에서 뺐다. 실제 수상이면 AI-BASED-ETA 섹션에 추가
- AI-based-ETA 조직 프로필의 내 역할이 "Frontend Developer, React & Flask"로 되어 있다. 포트폴리오에는 백엔드(Flask 서버·A*·모델 연동)로 적었으니, 채용 담당자가 교차 확인하기 전에 조직 쪽 표기를 맞추는 게 좋다
- 유레카 과정이 2026.10.28 종료. 최종 프로젝트가 나오면 Project 05로 추가
- Mealiver-IT 발급 이력은 레포 문서상 약 287만 건이라 "약 300만"으로 적었다. 실제 시더 설정이 300만이면 "약"을 뺄 수 있다
