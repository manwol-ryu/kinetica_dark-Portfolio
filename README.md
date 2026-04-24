# 영상 포트폴리오 템플릿

정적 HTML/CSS/JS 기반의 영상 포트폴리오 템플릿입니다. 별도 프레임워크나 빌드 과정 없이 `data/site.json` 한 파일로 브랜드, 히어로, 작업물, 가격, 문의, 임베드 카드 정보를 관리합니다.

- **현재 버전:** `v2026.04.24`
- **릴리스 노트:** [`CHANGELOG.md`](CHANGELOG.md)
- **버전 파일:** [`VERSION`](VERSION)
- **권장 사용 방식:** 관리자 페이지에서 편집 → JSON 복사/다운로드 → `data/site.json` 반영 → 정적 페이지 확인

## 주요 기능

### 공개 페이지

- `index.html`: 홈, 히어로, 프로젝트/유튜브 채널 카드, 통계, 영상 포트폴리오 렌더링
- `pricing.html`: 서비스/가격 플랜, 맞춤 작업, 진행 프로세스 렌더링
- `contact.html`: 문의 카드, 연락처, 응답 정책 렌더링
- `script.js`: `data/site.json`을 읽어 공개 페이지 데이터를 구성하는 공통 렌더러

### 관리자 페이지

- `admin/index.html`: 포트폴리오 데이터를 편집하는 관리자 화면
- `admin/admin.js`: 상태 관리, JSON 생성, 목록 편집, 임베드/이미지 편집 로직
- `admin/admin.css`: 관리자 UI, 모바일 탭, 퀵스타트 마법사 스타일

관리자 화면에는 다음 편집 흐름이 포함되어 있습니다.

1. **퀵스타트 마법사**: 사이트 정보, 히어로, 경력/툴, 작업 방식, 가격, 문의, JSON, 임베드 카드, 대표 이미지까지 순서대로 점검합니다.
2. **JSON 편집/복사/다운로드**: 현재 입력값을 `site.json` 구조로 확인하고 복사하거나 다운로드합니다.
3. **임베드 카드 설정**: Discord, 카카오톡, Slack 등에 표시되는 Open Graph/Twitter 카드 HTML을 생성합니다.
4. **대표 이미지 자르기**: 소셜 공유용 `social-preview.png` 이미지를 1200×630 비율로 제작할 수 있습니다.
5. **작업물/가격/문의 편집**: 영상 포트폴리오, 카테고리 표시 방식, 가격 플랜, 진행 프로세스, 문의 정보를 편집합니다.
6. **GitHub 편집 보조 링크**: 저장소 정보가 있으면 GitHub의 `site.json`, `index.html`, `assets/` 업로드 화면으로 이동할 수 있습니다.

## 폴더 구조

```text
.
├── index.html                    # 공개 홈 페이지
├── pricing.html                  # 가격/서비스 페이지
├── contact.html                  # 문의 페이지
├── script.js                     # 공개 페이지 공통 렌더러
├── README.md                     # 프로젝트 설명
├── VERSION                       # 현재 문서화/릴리스 버전
├── CHANGELOG.md                  # 업데이트 내역
├── admin/
│   ├── index.html                # 관리자 화면
│   ├── admin.js                  # 관리자 데이터/이벤트 로직
│   └── admin.css                 # 관리자 스타일
├── assets/
│   ├── social-preview.png        # 링크 미리보기 대표 이미지
│   └── tool-presets/             # 툴 아이콘/프리셋 이미지
├── data/
│   ├── example.site.json         # 커밋되는 예제 데이터
│   └── site.json                 # 실제 사이트 데이터, 기본적으로 git 추적 제외
└── scripts/
    ├── sync-gh-pages.sh          # gh-pages 동기화 보조 스크립트
    └── validate-admin-quickstart.mjs
```

## 로컬 실행

브라우저의 `file://` 직접 열기보다 로컬 HTTP 서버 실행을 권장합니다. JSON 로딩과 상대 경로 확인이 더 안정적입니다.

```bash
cd /path/to/jinyoung-portfolio
python3 -m http.server 8765 --bind 127.0.0.1
```

브라우저에서 아래 주소를 엽니다.

- 공개 홈: <http://127.0.0.1:8765/index.html>
- 가격 페이지: <http://127.0.0.1:8765/pricing.html>
- 문의 페이지: <http://127.0.0.1:8765/contact.html>
- 관리자: <http://127.0.0.1:8765/admin/index.html>

## 기본 편집 흐름

1. `admin/index.html`을 엽니다.
2. 퀵스타트 또는 세부 탭에서 필요한 값을 수정합니다.
3. `JSON` 영역에서 결과를 복사하거나 `site.json`으로 다운로드합니다.
4. 생성한 내용을 `data/site.json`에 반영합니다.
5. `index.html`, `pricing.html`, `contact.html`에서 화면을 확인합니다.
6. 변경이 끝나면 필요한 파일을 커밋하고 GitHub Pages 등 정적 호스팅에 배포합니다.

> `data/site.json`은 실제 운영 데이터이므로 기본적으로 `.gitignore` 대상입니다. 저장소에 포함되는 샘플 데이터는 `data/example.site.json`입니다.

## 링크 미리보기 카드 수정

Discord, 카카오톡, Slack 같은 앱에서 링크를 붙였을 때 뜨는 카드는 `index.html`의 `<head>` 안에 있는 `OG START/END` 블록을 읽어서 만듭니다.

1. 관리자 사이드바의 `임베드 카드 설정`에서 `임베드카드 수정`을 엽니다.
2. 제목, 설명, 이미지 URL, 사이트 URL을 입력합니다.
3. `카드 미리보기`로 노출 상태를 확인합니다.
4. `HTML 코드 복사` 버튼으로 OG/Twitter 메타 태그를 복사합니다.
5. `index.html`의 `<!-- OG START -->`부터 `<!-- OG END -->`까지를 복사한 코드로 교체합니다.
6. 대표 이미지를 바꿨다면 `이미지 수정`에서 1200×630 비율로 자른 뒤 `social-preview.png`로 다운로드합니다.
7. 다운로드한 이미지를 `assets/social-preview.png`로 업로드하거나 교체합니다.

주의사항:

- `og:image`와 `twitter:image`에는 로컬 경로가 아니라 GitHub Pages 등에서 접근 가능한 절대 URL을 넣어야 합니다.
- 줄바꿈은 플랫폼마다 무시될 수 있습니다. 여러 줄 레이아웃은 설명 텍스트보다 대표 이미지 안에 문구를 디자인하는 편이 안정적입니다.
- Discord 등은 링크 미리보기를 캐시합니다. 수정 후에도 예전 카드가 보이면 URL 뒤에 `?v=2`처럼 쿼리를 붙여 다시 테스트하세요.

## 데이터 파일 정책

- `data/example.site.json`: 템플릿 기본 예시입니다. 저장소에 포함됩니다.
- `data/site.json`: 실제 포트폴리오 데이터입니다. 기본 `.gitignore` 규칙상 커밋되지 않습니다.
- 새 데이터 필드가 생기면 `data/example.site.json`과 관리자 `DEFAULT_DATA` 구조를 함께 맞춰야 합니다.

현재 데이터 구조에는 다음 주요 영역이 있습니다.

- `site`: 사이트 제목, 설명, GitHub 저장소, 브랜드, 내비게이션, 푸터
- `hero`: 히어로 문구, 배경 영상, 버튼, 경력/툴/BGM 패널
- `projects`: 프로젝트 카드와 유튜브 채널 카드
- `stats`: 통계 카드
- `works`: 영상 포트폴리오, 카테고리, 표시 방식
- `pricing`: 가격 플랜, 맞춤 작업, 진행 프로세스
- `contact`: 문의 카드, 연락처, 응답 정책
- `freeContent`: 자유 입력 콘텐츠

## 버전 관리

이 저장소는 날짜 기반 버전을 사용합니다.

- 현재 버전: `v2026.04.24`
- 버전 표기 파일: `VERSION`
- 업데이트 내역: `CHANGELOG.md`
- 정적 파일 캐시 버스터: HTML의 `?v=YYYYMMDD-N` 쿼리 문자열

릴리스 시 권장 작업:

1. `VERSION` 값을 새 버전으로 변경합니다.
2. `CHANGELOG.md` 맨 위에 새 업데이트 내역을 추가합니다.
3. `admin/index.html`, `index.html`, `pricing.html`, `contact.html`의 CSS/JS `?v=` 값을 필요에 따라 갱신합니다.
4. 검증 명령을 실행합니다.
5. 커밋 후 태그를 생성합니다.

예시:

```bash
git tag -a v2026.04.24 -m "Admin quickstart wizard and portfolio editor updates"
git push origin v2026.04.24
```

## 검증 명령

변경 후 아래 명령을 실행해 기본 구조와 문법을 확인합니다.

```bash
node scripts/validate-admin-quickstart.mjs
node --check admin/admin.js
node --check scripts/validate-admin-quickstart.mjs
python3 -m json.tool data/example.site.json >/tmp/example-site-json-check.out
test ! -f data/site.json || python3 -m json.tool data/site.json >/tmp/site-json-check.out
git diff --check
```

추가로 로컬 서버를 띄운 뒤 관리자와 공개 페이지를 브라우저에서 확인하세요.

## 배포 메모

- GitHub Pages 등 정적 호스팅에 그대로 배포할 수 있습니다.
- 실제 운영 데이터인 `data/site.json`을 배포 브랜치에 포함할지, 로컬에서만 관리할지는 저장소 운영 정책에 맞게 정하세요.
- `assets/social-preview.png`를 교체한 경우 링크 미리보기 캐시 때문에 즉시 반영되지 않을 수 있습니다.
- 관리자 화면은 별도 인증이 없으므로 공개 저장소/공개 호스팅에 둘 때는 편집 권한이 아니라 JSON 생성 보조 도구로만 사용하세요.
