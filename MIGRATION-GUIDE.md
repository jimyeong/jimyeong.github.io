# 블로그 마이그레이션 가이드

## 전체 파일 구조

```
jimyeong.github.io/
├── _config.yml            ← 사이트 설정
├── _layouts/
│   ├── default.html       ← 전체 틀 (헤더 + 푸터)
│   ├── post.html          ← 글 본문 레이아웃
│   └── page.html          ← 정적 페이지 (about, projects)
├── _includes/
│   ├── header.html        ← 네비게이션
│   └── footer.html        ← 푸터
├── _posts/
│   └── 2026-02-14-why-mqtt-over-http-polling.md  ← 글 (예시)
├── assets/
│   └── css/
│       └── style.css      ← 스타일 전부
├── index.html             ← 홈 (글 목록)
├── projects.md            ← 프로젝트 페이지
├── about.md               ← 소개 페이지
└── Gemfile
```

---

## Step 1: 기존 블로그 백업

```bash
cd jimyeong.github.io
git checkout -b backup-minimal-mistakes
git push origin backup-minimal-mistakes
```

이러면 기존 Minimal Mistakes 블로그가 브랜치로 보존돼. 언제든 돌아갈 수 있어.

---

## Step 2: main 브랜치 비우기

```bash
git checkout main
```

아래 파일/폴더를 **전부 삭제**해:

```bash
rm -rf _pages _data _sass _includes _layouts assets _posts
rm -f index.html Gemfile Gemfile.lock _config.yml
rm -rf .jekyll-cache _site
```

`.git` 폴더는 절대 건드리지 마.

---

## Step 3: 새 파일 넣기

내가 만든 파일들을 전부 레포 루트에 복사해.
다운로드한 파일들의 구조 그대로 넣으면 돼:

```bash
# 다운받은 blog 폴더 내용물을 레포 루트에 복사
cp -r blog/* jimyeong.github.io/
```

---

## Step 4: _config.yml 수정

`_config.yml`에서 이메일 등 네 정보 확인:

```yaml
title: jimmy jung
url: "https://jimyeong.github.io"
```

---

## Step 5: index.html에서 링크 수정

`index.html`의 intro-links 부분에서 이메일 주소를 네 걸로 바꿔:

```html
<a href="mailto:your@email.com">Email</a>
```

---

## Step 6: 로컬에서 확인 (선택)

```bash
bundle install
bundle exec jekyll serve
```

http://localhost:4000 에서 확인.

---

## Step 7: 배포

```bash
git add .
git commit -m "Migrate to custom minimal theme"
git push origin main
```

GitHub Pages가 자동으로 빌드해. 1~2분 후 jimyeong.github.io에서 확인.

---

## 새 글 쓰는 법

`_posts/` 폴더에 마크다운 파일 추가. 파일명 형식:

```
YYYY-MM-DD-제목-영어로.md
```

글 맨 위에 이것만 넣어:

```yaml
---
layout: post
title: "글 제목"
date: 2026-02-14
summary: "한 줄 요약 — 홈 목록에 표시됨"
tags: [태그1, 태그2]
project: 프로젝트이름
---
```

그 아래에 마크다운으로 본문 작성.

---

## Decision Box 사용법

글 본문에서 의사결정 포인트를 강조하고 싶을 때:

```html
<div class="decision-box">
  <div class="decision-label">Decision Point</div>
  <p><strong>Option A:</strong> 설명</p>
  <p><strong>Option B:</strong> 설명</p>
  <p><strong>Option C:</strong> 설명</p>
</div>
```

마크다운 안에 HTML 그대로 넣으면 됨. Jekyll이 알아서 렌더링해.
