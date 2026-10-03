# 🎀 Puppeteer 전수조사 & 수익화 전략 리포트 (by 카리나 💖)

> 작성일: 2026-10-03
> 작성: 카리나 (Claude Code 개발 파트너)
> 대상 레포: **https://github.com/bmshin94/puppeteer**
> 원본(업스트림): **https://github.com/puppeteer/puppeteer**
> 공식 문서: **https://pptr.dev**
> 작업 브랜치: `claude/festive-clarke-vidbe0`

---

## 📑 목차

1. [레포 전수조사 결과](#1-레포-전수조사-결과)
2. [쉬운 설명 (초보 모드)](#2-쉬운-설명-초보-모드)
3. [질문 7개 답변](#3-질문-7개-답변)
4. [수익화 아이디어 상세](#4-수익화-아이디어-상세)
5. [부록: 치트시트 & 링크](#5-부록-치트시트--링크)

---

## 1. 레포 전수조사 결과

### 1-1. 결론

이 레포는 **Google Chrome DevTools 팀의 공식 오픈소스 `Puppeteer`를 포크한 것**이다.
Puppeteer는 **Chrome / Firefox 브라우저를 코드로 조종하는 Node.js 라이브러리**다.

| 항목        | 내용                                                       |
| ----------- | ---------------------------------------------------------- |
| 정체        | 브라우저 자동화 라이브러리 (SDK)                           |
| 버전        | `puppeteer` / `puppeteer-core` **25.11.0**                 |
| 구조        | npm workspaces 모노레포 (빌드 도구: `wireit`)              |
| 규모        | 코어 TypeScript **211개 파일 / 약 51,800줄**               |
| 문서        | API 레퍼런스 **638개** + 가이드 **24개**                   |
| 테스트      | 통합 테스트 스펙 **58개** + CI 워크플로 **14개**           |
| 프로토콜    | CDP (Chrome DevTools Protocol) + WebDriver BiDi (W3C 표준) |
| 라이선스    | **Apache-2.0** → 상업적 이용 / 수정 / 배포 전부 자유 ✅    |
| 포크 커스텀 | `CLAUDE.md` (카리나 페르소나 가이드, PR #1으로 머지됨)     |

### 1-2. `packages/` — 실제 알맹이 5개

| 패키지                       | 버전    | 역할                                                         |
| ---------------------------- | ------- | ------------------------------------------------------------ |
| **puppeteer-core**           | 25.11.0 | 💎 심장부. 브라우저 제어 엔진 전체                           |
| **puppeteer**                | 25.11.0 | core + 브라우저 자동 다운로드 래퍼 (일반 사용자용), CLI 제공 |
| **@puppeteer/browsers**      | 3.2.2   | 브라우저 설치/실행 관리 독립 CLI                             |
| **@puppeteer/ng-schematics** | 0.8.0   | Angular 프로젝트에 E2E 테스트 자동 세팅                      |
| **@pptr/testserver**         | 0.6.1   | 테스트용 로컬 HTTP 서버                                      |

#### `puppeteer-core/src` 내부 해부

- **`api/`** — 공개 API 클래스
  `Browser`, `BrowserContext`, `Page`, `Frame`, `ElementHandle`, `Locator`,
  `Input`(키보드/마우스/터치), `HTTPRequest` / `HTTPResponse`(네트워크 가로채기),
  `Dialog`, `Target`, `WebWorker`, `CDPSession`, `ScreenRecording`(화면 녹화),
  `BluetoothEmulation`, `Extension`, `Accessibility`
- **`cdp/`** — Chrome DevTools Protocol 구현 (Chrome 전용 저수준 제어)
- **`bidi/`** — WebDriver BiDi 구현 (W3C 표준 → Firefox 크로스브라우저 지원)
- **`injected/`** — 페이지에 주입되는 커스텀 셀렉터 엔진 (`::-p-aria()`, `::-p-text()`, `>>>`)
- **`node/`** — 브라우저 실행/연결, CLI 엔트리
- **`common/`**, **`util/`**, **`templates/`**, `environment.ts`, `revisions.ts`

### 1-3. 폴더별 전수조사

| 폴더                                                 | 내용                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `docs/`                                              | API 레퍼런스 638개 + `guides/` 24개 (getting-started, installation, page-interactions, screenshots, pdf-generation, network-interception, network-logging, cookies, javascript-execution, headless-modes, docker, debugging, chrome-extensions, browser-management, window-management, screen-configuration, files, configuration, **webmcp**, ng-schematics, running-puppeteer-in-browser/extensions, system-requirements, what-is-puppeteer) |
| `examples/`                                          | 실행 가능한 예제 13개 (아래 표 참고)                                                                                                                                                                                                                                                                                                                                                                                                           |
| `test/`                                              | 스펙 58개 + `TestExpectations.json`(브라우저/모드별 기대결과) + `golden-chrome`/`golden-firefox`(스크린샷 픽셀 비교) + `TestSuites.json`                                                                                                                                                                                                                                                                                                       |
| `tools/`                                             | 자체 `mocha-runner`, `docgen`(문서 자동생성), 커스텀 ESLint 플러그인, 브라우저 리비전 자동 업데이트, 라이선스 검증                                                                                                                                                                                                                                                                                                                             |
| `.github/workflows/`                                 | 14개 — `ci`, `daily`, **`deflake`**(불안정 테스트 사냥), **`bisect`**(버그 커밋 이진탐색), `release-please`, `publish`, `docs`, `devtools`, `update-browser-pins`, `scorecards-analysis`(보안), `stale`, `pre-release`, `changed-packages`, `convetional-commit`                                                                                                                                                                               |
| `docker/`                                            | 공식 Docker 이미지 (`Dockerfile`, `pack.sh`, smoke test) — 서버 배포 참고용                                                                                                                                                                                                                                                                                                                                                                    |
| `website/`                                           | Docusaurus 기반 공식 문서 사이트(pptr.dev) 소스                                                                                                                                                                                                                                                                                                                                                                                                |
| **`.agents/skills/puppeteer-verification/SKILL.md`** | ⭐ **AI 에이전트용 스킬 파일** — 빌드/테스트/린트 수행 방법 지침                                                                                                                                                                                                                                                                                                                                                                               |
| **`CLAUDE.md`**                                      | 💖 카리나 페르소나 가이드 (이 포크의 커스텀 추가분)                                                                                                                                                                                                                                                                                                                                                                                            |
| 기타                                                 | `Herebyfile.mjs`, `eslint.config.mjs`, `.mocharc.js`, `versions.json`, `release-please-config.json`, `puppeteer.config.js`, `SECURITY.md`, `CHANGELOG.md`, `.devcontainer/`                                                                                                                                                                                                                                                                    |

#### `examples/` 상세

| 파일                      | 내용                               |
| ------------------------- | ---------------------------------- |
| `screenshot.js`           | 기본 스크린샷                      |
| `screenshot-fullpage.js`  | 풀페이지 스크린샷                  |
| `pdf.js`                  | 웹페이지 → PDF 변환                |
| `search.js`               | 검색 자동화 + 결과 스크래핑        |
| `block-images.js`         | 이미지 차단으로 속도 최적화        |
| `proxy.js`                | 프록시 경유                        |
| `cross-browser.js`        | Chrome / Firefox 동시 대응         |
| `detect-sniff.js`         | 브라우저 감지(핑거프린팅) 탐지     |
| `custom-event.js`         | 페이지 ↔ Node 커스텀 이벤트 통신   |
| `webdriver-bidi.mjs`      | BiDi 프로토콜 직접 사용            |
| `puppeteer-in-browser/`   | 🤯 브라우저 안에서 Puppeteer 구동  |
| `puppeteer-in-extension/` | 🤯 크롬 확장 안에서 Puppeteer 구동 |

### 1-4. 뭐 하는 건지 / 언제 쓰는지

> **사람이 브라우저에서 손으로 하는 거의 모든 것을 코드로 시키는 라이브러리.**

1. **E2E 테스트** — 실제 브라우저로 "회원가입 → 로그인 → 결제" 전체 플로우 자동 검증
2. **웹 스크래핑/크롤링** — 특히 JS로 렌더링되는 SPA(React/Vue). 일반 HTTP 크롤러로는 못 긁는 것
3. **PDF / 이미지 생성** — HTML+CSS 디자인 → 청구서/리포트/썸네일 자동 생성
4. **SSR 프리렌더링** — SPA를 미리 렌더해 SEO 해결
5. **성능 진단** — Performance 타임라인 트레이스 수집
6. **RPA(업무 자동화)** — 매일 반복되는 로그인/다운로드/입력 노동 제거
7. **크롬 확장 테스트**
8. **🤖 AI 에이전트의 "손과 눈"** — 현재 가장 뜨거운 용도

### 1-5. 나에게 주는 도움

- 반복 업무 자동화 → **시간 = 돈** 전환
- **AI 에이전트 개발의 핵심 부품** (`chrome-devtools-mcp`가 Puppeteer 기반 MCP 서버, WebMCP 지원)
- **수익화 가능** (4장 참고)
- 포트폴리오 / 유튜브 / 강의 소재로 최적 (결과가 눈에 보임)
- Apache-2.0 → 상업적 이용 자유

---

## 2. 쉬운 설명 (초보 모드)

### 2-1. "웹 브라우저용 로봇 팔"

평소 수동 작업:

> 크롬 켜기 → 네이버 접속 → 검색창 클릭 → "아이폰" 입력 → 엔터 → 가격 확인 → 엑셀 기록

Puppeteer로 바꾸면:

```js
const browser = await puppeteer.launch(); // 크롬 켜기
const page = await browser.newPage(); // 새 탭
await page.goto('https://naver.com'); // 주소 입력
await page.type('#query', '아이폰'); // 타이핑
await page.keyboard.press('Enter'); // 엔터
const price = await page.$eval('.price', el => el.textContent); // 가격 추출
await browser.close();
```

→ 새벽 3시에 스케줄 걸어두면 자는 동안 데이터가 쌓인다.

### 2-2. 헤드리스(Headless)란?

- **헤드풀(headful)**: 화면이 보이는 크롬 → 로봇이 일하는 모습을 눈으로 확인 (디버깅/영상 촬영용)
- **헤드리스(headless)**: 화면 없이 백그라운드 구동 (기본값) → 훨씬 빠르고 가벼움, 모니터 없는 서버에서도 동작

### 2-3. 핵심 객체 4단 구조

```
puppeteer                     브라우저 리모컨 (launch)
 └ Browser                    크롬 앱 하나
    └ BrowserContext          시크릿 창 (쿠키 격리 → 계정 여러 개 동시 로그인)
       └ Page                 탭 하나  ← 작업의 90%는 여기서
          └ ElementHandle     페이지 안의 요소 하나 (버튼/입력창)
```

### 2-4. 두 가지 통신 언어 (`cdp/` vs `bidi/`)

|        | CDP                        | WebDriver BiDi                      |
| ------ | -------------------------- | ----------------------------------- |
| 정식명 | Chrome DevTools Protocol   | W3C 표준 프로토콜                   |
| 비유   | 크롬 전용 사투리           | 모든 브라우저가 아는 표준어         |
| 장점   | 크롬 기능 100% 깊은 제어   | **Firefox도 지원** (크로스브라우저) |
| 용도   | 성능 트레이스, 저수준 제어 | 여러 브라우저 동시 테스트           |

### 2-5. 마법 셀렉터 (`injected/`가 담당)

클래스명이 `css-1x7p2kq` 같은 난수여도 괜찮다.

```js
await page.locator('::-p-text(로그인)').click(); // 글씨로 찾기
await page.locator('::-p-aria(검색)').fill('아이폰'); // 접근성 이름으로 찾기
await page.locator('button >>> .inner').click(); // Shadow DOM 관통
```

→ "사람이 보는 방식"으로 요소를 찾으므로 디자인 변경에 잘 깨지지 않는다.
`Locator`는 **자동 대기 + 자동 재시도**가 내장되어 있어 구버전 방식(`waitForSelector` → `click`)보다 안정적이다. **항상 `locator()`를 쓸 것.**

### 2-6. `puppeteer` vs `puppeteer-core`

|         | puppeteer                       | puppeteer-core                      |
| ------- | ------------------------------- | ----------------------------------- |
| 설치 시 | 크롬도 자동 다운로드 (약 170MB) | 라이브러리만 (가벼움)               |
| 비유    | 로봇 + 조종할 차                | 로봇만 (차는 직접 준비)             |
| 추천    | 초보자 / 로컬 개발              | Docker / 서버 / 크롬 기존 설치 환경 |

### 2-7. 폴더 = 공장 비유

```
packages/   🏭 제품 공장 (실제 코드)
docs/       📖 사용설명서 638장 + 가이드 24장
examples/   🎁 샘플 키트 (바로 실행)
test/       🔬 품질검사실 (58개 검사)
tools/      🔧 공장 자체 설비
.github/    🤖 자동화 라인 (14개 CI)
docker/     📦 컨테이너 포장
website/    🌐 공식 쇼룸 (pptr.dev)
.agents/    ⭐ AI 직원 교육 매뉴얼
CLAUDE.md   💖 카리나 사원증
```

---

## 3. 질문 7개 답변

### Q1. 설치 및 사용법?

#### 설치

```bash
# 방법 A — 초보자 추천 (크롬까지 자동 다운로드)
npm i puppeteer

# 방법 B — 라이브러리만
npm i puppeteer-core
```

> ⚠️ **중요한 함정**: pnpm / Yarn / Bun / Deno / 최신 npm은 install 스크립트를 기본 차단한다.
> 크롬이 안 깔려서 런타임 에러가 난다.
>
> ```bash
> npx puppeteer browsers install   # 수동 설치로 해결
> ```
>
> 또는 `package.json`에 `"allowScripts": ["puppeteer"]` 추가.

브라우저만 따로 관리 (`@puppeteer/browsers` CLI):

```bash
npx @puppeteer/browsers install chrome@stable
npx @puppeteer/browsers list
npx @puppeteer/browsers launch chrome
npx @puppeteer/browsers clear
npx @puppeteer/browsers --help
```

#### 첫 실행 (`test.mjs`)

```js
import puppeteer from 'puppeteer';

const browser = await puppeteer.launch({
  headless: false, // 눈으로 확인 (디버깅 필수)
  slowMo: 50, // 천천히 실행
  defaultViewport: null,
});
const page = await browser.newPage();

await page.goto('https://example.com', {waitUntil: 'networkidle2'});
await page.setViewport({width: 1280, height: 800});
await page.screenshot({path: 'shot.png', fullPage: true});
await page.pdf({path: 'out.pdf', format: 'A4', printBackground: true});
console.log('제목:', await page.title());

await browser.close();
```

```bash
node test.mjs
```

#### 서버 / Docker 배포

```js
puppeteer.launch({args: ['--no-sandbox', '--disable-dev-shm-usage']});
```

참고 문서: `docs/guides/docker.md`, `docs/guides/system-requirements.md`, `docs/troubleshooting.md`, `docker/Dockerfile`

#### 이 레포 자체 빌드/테스트 (`.agents/skills/puppeteer-verification/SKILL.md` 기준)

```bash
npm install
npm run build                  # 전체 빌드 (wireit)
npm run unit                   # 유닛 테스트
npm run test:chrome:headless   # Chrome 통합 테스트
npm run test:firefox:headless  # Firefox 통합 테스트
npm run format                 # lint + 자동 수정
npm run docs                   # 문서 생성
```

> 특정 테스트만 돌리려면 해당 테스트 블록에 `.only`를 붙이고 위 명령 실행.

---

### Q2. 이거 플러그인이야? 스킬이야? MCP야?

**정답: 셋 다 아니다. 순수 npm 라이브러리(SDK)다.**

| 구분               | 정의                                   | Puppeteer는?                                                           |
| ------------------ | -------------------------------------- | ---------------------------------------------------------------------- |
| 라이브러리/SDK     | 코드에서 `import`해서 쓰는 패키지      | ✅ **정답**                                                            |
| 플러그인           | 특정 호스트 앱에 끼우는 확장           | ❌ (단, Puppeteer에 플러그인을 끼우는 `puppeteer-extra` 생태계는 존재) |
| 스킬 (Agent Skill) | AI 에이전트에게 주는 지침 문서         | ❌ — 단, **레포 안에 스킬 1개가 들어있다**                             |
| MCP 서버           | LLM이 툴을 호출하는 표준 프로토콜 서버 | ❌ — 단, **Puppeteer로 만든 MCP 서버가 공식 추천된다**                 |

#### 혼동 포인트 3가지

1. **`.agents/skills/puppeteer-verification/SKILL.md`**
   → Puppeteer가 스킬이라는 뜻이 아니라, *이 레포에서 작업하는 AI 에이전트*에게 주는 작업 지침서.
   `description`에 "MANDATORY: 빌드/테스트 전 반드시 활성화"라고 명시됨.

2. **README의 MCP 섹션**

   > Install `chrome-devtools-mcp`, **a Puppeteer-based MCP server** for browser automation and debugging.

   → Puppeteer는 MCP가 아니고, Puppeteer를 재료로 만든 MCP 서버가 따로 있다.
   Claude Code / Desktop에 붙이면 AI가 브라우저를 직접 조작할 수 있다.
   `https://github.com/ChromeDevTools/chrome-devtools-mcp`

3. **WebMCP** (`docs/guides/webmcp.md`, 실험 기능)
   - 웹페이지가 자기 기능을 "툴"로 등록하면, Puppeteer가 `page.webmcp.tools()`로 발견/호출 가능
   - Chrome 151+ & `--enable-features=WebMCP` 플래그 필요
   ```js
   const tools = page.webmcp.tools();
   page.webmcp.on('toolsadded', e => {
     /* ... */
   });
   page.webmcp.on('toolsremoved', e => {
     /* ... */
   });
   ```
   - 스펙: `https://github.com/webmachinelearning/webmcp`

> **정리: Puppeteer = 재료(SDK). 이걸로 플러그인도, 스킬도, MCP 서버도 직접 만들 수 있다.**

---

### Q3. API 토큰을 사용해야 돼?

**아니다. 전혀 필요 없다. 100% 무료, 토큰 0개.**

- Puppeteer는 외부 서버에 요청하는 클라이언트가 아니다
- 내 컴퓨터/서버에서 **직접 크롬을 실행**하고, 로컬 WebSocket/파이프로 명령을 전달하는 구조
- 회원가입/과금/레이트리밋 없음. Apache-2.0 → 상업적 이용 자유

#### 토큰이 필요해지는 경우 (Puppeteer가 아니라 함께 쓰는 서비스 때문)

| 상황                                         | 필요한 토큰                | 비용         |
| -------------------------------------------- | -------------------------- | ------------ |
| 순수 Puppeteer 자동화                        | **없음**                   | 무료         |
| AI 에이전트 (Claude/GPT 호출)                | Anthropic / OpenAI API Key | 유료(사용량) |
| CAPTCHA 자동 해제                            | 2Captcha 등                | 유료         |
| 회전 프록시(IP)                              | 프록시 업체 키             | 유료         |
| 클라우드 브라우저 (Browserless, Browserbase) | 서비스 키                  | 유료         |
| 대상 사이트 로그인                           | 해당 사이트 계정           | 보통 무료    |
| 서버 배포                                    | AWS/GCP/Vercel 등          | 인프라 비용  |

> **수익화 원가 구조: Puppeteer 0원 + 서버비(월 몇천~몇만원) + (필요시) AI API 비용**

#### 보안 팁

```js
// ❌ 코드에 하드코딩 금지
// ✅ 환경변수 사용
const key = process.env.ANTHROPIC_API_KEY;
```

`.env`는 반드시 `.gitignore`에. 레포의 `SECURITY.md`도 참고.

---

### Q4. AI 에이전트를 구축하는데 도움이 될까?

**도움 정도가 아니라 거의 "필수 부품"이다.**

```
🧠 두뇌 = LLM (Claude / GPT)              ← 판단
👁️ 눈   = Puppeteer 스크린샷 / DOM / a11y   ← 인식
🖐️ 손   = Puppeteer click / type           ← 행동
```

#### Puppeteer가 AI 에이전트에 특별히 좋은 이유 5가지

1. **접근성 스냅샷 (핵심 ⭐)**

   ```js
   const snapshot = await page.accessibility.snapshot();
   ```

   HTML 전체를 LLM에 넣으면 토큰이 폭발하지만, 접근성 트리는 "버튼: 로그인", "입력창: 이메일"처럼 의미만 간결하게 준다.
   → **토큰 약 90% 절감 + 정확도 상승**. (문서: `docs/api/puppeteer.accessibility.snapshot.md`)

2. **의미 기반 셀렉터** — LLM이 CSS를 몰라도 된다

   ```js
   await page.locator('::-p-aria(로그인 버튼)').click();
   await page.locator('::-p-text(장바구니)').click();
   ```

   → LLM이 복잡한 CSS 셀렉터를 생성하다 실패하는 문제가 사라져 에이전트 성공률이 크게 오른다.

3. **Locator 자동 재시도/대기** — "아직 로딩 안 됐는데 클릭" 실패를 구조적으로 방지

4. **멀티 세션 격리** — `browser.createBrowserContext()`로 에이전트 N개 병렬 실행 (멀티테넌트 SaaS 필수)

5. **이미 MCP 생태계에 편입** — `chrome-devtools-mcp`(Puppeteer 기반 공식 MCP 서버), WebMCP 지원

#### 실제 에이전트 루프 뼈대

```js
import puppeteer from 'puppeteer';
import Anthropic from '@anthropic-ai/sdk';

const ai = new Anthropic({apiKey: process.env.ANTHROPIC_API_KEY});
const browser = await puppeteer.launch({headless: false});
const page = await browser.newPage();

const tools = [
  {
    name: 'goto',
    description: 'URL로 이동',
    input_schema: {
      type: 'object',
      properties: {url: {type: 'string'}},
      required: ['url'],
    },
  },
  {
    name: 'click',
    description: '보이는 텍스트로 요소 클릭',
    input_schema: {
      type: 'object',
      properties: {text: {type: 'string'}},
      required: ['text'],
    },
  },
  {
    name: 'type',
    description: '입력창에 텍스트 입력',
    input_schema: {
      type: 'object',
      properties: {label: {type: 'string'}, value: {type: 'string'}},
      required: ['label', 'value'],
    },
  },
  {
    name: 'read',
    description: '현재 화면 구조 읽기',
    input_schema: {type: 'object', properties: {}},
  },
];

async function run(name, args) {
  switch (name) {
    case 'goto':
      await page.goto(args.url, {waitUntil: 'networkidle2'});
      return '이동 완료';
    case 'click':
      await page.locator(`::-p-text(${args.text})`).click();
      return '클릭 완료';
    case 'type':
      await page.locator(`::-p-aria(${args.label})`).fill(args.value);
      return '입력 완료';
    case 'read':
      return JSON.stringify(await page.accessibility.snapshot()); // 토큰 절약
  }
}

let messages = [
  {role: 'user', content: '네이버에서 "아이폰" 검색하고 첫 결과 제목 알려줘'},
];
for (let turn = 0; turn < 15; turn++) {
  // 무한루프 방지
  const res = await ai.messages.create({
    model: 'claude-opus-5',
    max_tokens: 2048,
    tools,
    messages,
  });
  messages.push({role: 'assistant', content: res.content});

  const calls = res.content.filter(c => c.type === 'tool_use');
  if (!calls.length) {
    console.log('완료:', res.content.find(c => c.type === 'text')?.text);
    break;
  }

  const results = [];
  for (const c of calls) {
    results.push({
      type: 'tool_result',
      tool_use_id: c.id,
      content: await run(c.name, c.input),
    });
  }
  messages.push({role: 'user', content: results});
}
await browser.close();
```

#### 주의할 점

- **토큰 비용**: 매 턴 스크린샷은 비싸다 → 접근성 스냅샷 우선, 스크린샷은 필요할 때만
- **무한 루프 방지**: 최대 턴 수 제한 필수
- **위험 액션 가드**: 결제/삭제는 사람 확인 (Human-in-the-loop)
- **대상 사이트 정책**: `robots.txt`와 이용약관 확인 (법적 리스크)

---

### Q5. 수익화할만한 아이디어가 있어?

→ **4장에서 10개 아이디어 + 가격표 + 6개월 로드맵으로 상세 정리.**

---

### Q6. 우리가 React나 PHP로 만들 수 있어?

**핵심 원칙: Puppeteer는 Node.js 서버에서만 돌아간다. 브라우저(React)나 PHP에서 직접은 불가 → "백엔드로 분리"가 정답.**

이유: Puppeteer는 크롬 실행 파일을 띄우고 파일시스템을 쓰는 프로그램이다. 브라우저 샌드박스 안의 React에는 그 권한이 없고, PHP는 Node 런타임이 아니다.

#### React — ✅ 가능 (아키텍처만 맞추면)

```
[ React 프론트 ] ──HTTP──▶ [ Node 백엔드 + Puppeteer ] ──▶ [ Chrome ]
```

**방법 A — Next.js 하나로 (가장 추천 ⭐)**

```js
// app/api/pdf/route.js
import puppeteer from 'puppeteer';
import {NextResponse} from 'next/server';

export const runtime = 'nodejs'; // ⚠️ 필수 (Edge 런타임 불가)

export async function POST(req) {
  const {html} = await req.json();
  const browser = await puppeteer.launch({args: ['--no-sandbox']});
  const page = await browser.newPage();
  await page.setContent(html, {waitUntil: 'networkidle0'});
  const pdf = await page.pdf({format: 'A4', printBackground: true});
  await browser.close();
  return new NextResponse(pdf, {headers: {'Content-Type': 'application/pdf'}});
}
```

```jsx
// app/page.jsx
'use client';
export default function Page() {
  const make = async () => {
    const res = await fetch('/api/pdf', {
      method: 'POST',
      headers: {'Content-Type': 'application/json'},
      body: JSON.stringify({html: '<h1>청구서</h1>'}),
    });
    window.open(URL.createObjectURL(await res.blob()));
  };
  return <button onClick={make}>PDF 만들기</button>;
}
```

> React로 디자인한 것을 그대로 PDF로 뽑는 구조. React의 컴포넌트 재사용 + Puppeteer의 픽셀 완벽 렌더 조합.

**방법 B — Express 분리형 (브라우저 인스턴스 재사용 ⭐)**

```js
import express from 'express';
import puppeteer from 'puppeteer';

const app = express();
app.use(express.json());

const browser = await puppeteer.launch({args: ['--no-sandbox']}); // 재사용!

app.post('/shot', async (req, res) => {
  const page = await browser.newPage(); // 탭만 새로 생성
  try {
    await page.goto(req.body.url, {waitUntil: 'networkidle2'});
    res.type('png').send(await page.screenshot({fullPage: true}));
  } finally {
    await page.close(); // 메모리 누수 방지
  }
});
app.listen(3000);
```

**React 쪽 배포 주의사항**

- Vercel/Netlify 서버리스는 용량 제한(50MB~250MB)에 크롬(약 170MB)이 안 들어간다
  → `@sparticuz/chromium` 등 경량 크롬 + `puppeteer-core` 조합, 또는 별도 서버/Docker (`docker/Dockerfile` 참고)
- 서버리스 콜드스타트가 느림 → 트래픽이 있으면 상주 서버가 유리
- **큐 필수**: 동시 요청 폭주 시 메모리 초과 → BullMQ + Redis로 동시 실행 수 제한(예: 3~5개)

#### PHP — ⚠️ 가능하지만 우회 필요

| 방법                       | 설명                                     | 추천도                 |
| -------------------------- | ---------------------------------------- | ---------------------- |
| ① Node 마이크로서비스 분리 | PHP가 Node API를 HTTP로 호출             | ⭐⭐⭐⭐⭐             |
| ② `shell_exec`로 Node 실행 | PHP에서 Node 스크립트 직접 실행          | ⭐⭐ (보안 위험)       |
| ③ `chrome-php/chrome`      | 순수 PHP용 헤드리스 크롬 (CDP 직접 구현) | ⭐⭐⭐⭐               |
| ④ `nesk/puphpeteer`        | PHP → Puppeteer 브릿지                   | ⭐ (**유지보수 중단**) |

**① 방법 (권장)**

```php
<?php
$ch = curl_init('http://puppeteer-service:3000/pdf');
curl_setopt_array($ch, [
    CURLOPT_POST           => true,
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_HTTPHEADER     => ['Content-Type: application/json'],
    CURLOPT_POSTFIELDS     => json_encode(['html' => '<h1>청구서</h1>']),
    CURLOPT_TIMEOUT        => 60,
]);
$pdf = curl_exec($ch);
curl_close($ch);

header('Content-Type: application/pdf');
echo $pdf;
```

```yaml
# docker-compose.yml
services:
  php: {build: ./php, ports: ['8080:80']}
  puppeteer: {build: ./node, expose: ['3000']} # docker/Dockerfile 참고
```

**② 방법 (급할 때만, 보안 주의)**

```php
<?php
$url = escapeshellarg($_POST['url']);  // ⚠️ 반드시 이스케이프 (명령어 주입 방지)
$out = shell_exec("node /app/shot.mjs {$url} 2>&1");
```

**③ 방법 (순수 PHP)**

```bash
composer require chrome-php/chrome
```

```php
<?php
use HeadlessChromium\BrowserFactory;
$browser = (new BrowserFactory('chromium'))->createBrowser();
$page = $browser->createPage();
$page->navigate('https://example.com')->waitForNavigation();
$page->pdf()->saveToFile('out.pdf');
$browser->close();
```

> Puppeteer는 아니지만 같은 CDP 프로토콜을 쓰는 PHP 네이티브 구현. PDF/스크린샷 정도면 충분하다.

#### 추천 최종 아키텍처

```
┌─────────────────────────────────────────────┐
│  React / Next.js (또는 PHP / Laravel)  UI   │
└──────────────┬──────────────────────────────┘
               │ REST / tRPC
┌──────────────▼──────────────────────────────┐
│  Node API (Express / Next route handler)    │  인증, 과금, 큐 관리
└──────────────┬──────────────────────────────┘
               │ BullMQ (Redis 큐) ⭐ 동시성 제한
┌──────────────▼──────────────────────────────┐
│  Puppeteer Worker (Docker, N개)             │  실제 브라우저 작업
└──────────────┬──────────────────────────────┘
               │
         ┌─────▼─────┐
         │ S3 / R2   │  PDF, 스크린샷 저장
         └───────────┘
```

**정리**

- React: ✅ 완전 가능 (Next.js API Route가 최적)
- PHP: ⚠️ 가능 (Node 마이크로서비스 분리 추천, 또는 `chrome-php/chrome`)
- 공통: Puppeteer는 항상 서버 쪽, 브라우저 인스턴스 재사용, 작업은 큐로

---

### Q7. 유튜브 강의 영상으로 제작 가능할까?

**완전 가능. 소재로는 거의 치트키급.**

#### 유튜브 소재로 최강인 이유

| 이유           | 설명                                                                             |
| -------------- | -------------------------------------------------------------------------------- |
| 시각적 임팩트  | `headless: false`로 브라우저가 스스로 움직이는 게 화면에 보임 → 썸네일/후킹 최강 |
| 즉각적 결과    | 10줄로 스크린샷/PDF 완성 → 1분 내 성취감                                         |
| 명확한 효용    | "하루 2시간 노동 → 10초" 돈/시간 절약 스토리                                     |
| 난이도 폭      | 입문(스크린샷) ~ 고급(AI 에이전트, Docker 배포) 시리즈 무한 확장                 |
| AI 트렌드 결합 | "AI 에이전트" 키워드와 결합 시 조회수 상승                                       |
| 무료           | 시청자 진입 장벽 0 (API 키 발급 같은 이탈 포인트 없음)                           |
| 자료 풍부      | 레포에 예제 13개 + 가이드 24개 + API 638개 = 대본 소재 무한                      |

#### 추천 커리큘럼 (20편)

**시즌 1: 입문**

| #   | 제목                                             | 길이 | 후킹                 |
| --- | ------------------------------------------------ | ---- | -------------------- |
| 1   | 코드 10줄로 브라우저가 스스로 움직인다           | 8분  | "이게 10줄?!"        |
| 2   | 설치 완전정복 (pnpm/yarn 크롬 함정 해결)         | 10분 | "99%가 여기서 막힘"  |
| 3   | 스크린샷 & PDF 자동 생성                         | 12분 | "청구서 1000장 10초" |
| 4   | 클릭·타이핑·로그인 자동화                        | 15분 | "로그인 자동화"      |
| 5   | Locator & 마법 셀렉터 (`::-p-text`, `::-p-aria`) | 12분 | "안 깨지는 셀렉터"   |

**시즌 2: 실전**

| #   | 제목                                  | 포인트                     |
| --- | ------------------------------------- | -------------------------- |
| 6   | 쇼핑몰 가격 추적 봇 만들기            | 실생활 밀착 (조회수 핵심)  |
| 7   | 네트워크 가로채기로 크롤링 3배 빠르게 | `examples/block-images.js` |
| 8   | 로그인 세션 저장해서 재사용           | 쿠키/스토리지              |
| 9   | 무한 스크롤 SPA 크롤링                | React 사이트 긁기          |
| 10  | 봇 탐지 이해하기 (합법 범위)          | `examples/detect-sniff.js` |

**시즌 3: 프로덕션**

| #   | 제목                                          |
| --- | --------------------------------------------- |
| 11  | Docker로 서버 배포 (`docker/Dockerfile` 해설) |
| 12  | 큐(BullMQ)로 동시 요청 100개 버티기           |
| 13  | Next.js + Puppeteer PDF SaaS 만들기           |
| 14  | 메모리 누수 없이 브라우저 재사용하기          |
| 15  | E2E 테스트 자동화 + GitHub Actions CI         |

**시즌 4: AI 에이전트 (최고 화력)**

| #   | 제목                                      |
| --- | ----------------------------------------- |
| 16  | Claude + Puppeteer로 웹 쓰는 AI 만들기    |
| 17  | 접근성 스냅샷으로 토큰 90% 절약하기 ⭐    |
| 18  | MCP 서버 직접 만들어 Claude에 붙이기      |
| 19  | WebMCP — 웹의 미래 미리보기               |
| 20  | AI 에이전트로 월 100만원 버는 구조 만들기 |

#### 제작 팁

1. **화면 구성**: 왼쪽 VS Code(코드) / 오른쪽 크롬(자동 움직임) 분할
2. **`slowMo`는 영상용 필수**
   ```js
   await puppeteer.launch({headless: false, slowMo: 250});
   ```
3. **메타 콘텐츠**: Puppeteer의 `page.screencast()`로 데모 영상을 Puppeteer로 녹화
   ```js
   const rec = await page.screencast({path: 'demo.webm'});
   await rec.stop();
   ```
4. **수익 연결**: 광고 수익 → 전자책/노션 템플릿 → 인프런/클래스101 유료 강의 → 외주 문의 유입 → 깃허브 스타로 신뢰도/단가 상승

#### ⚠️ 주의사항

- 실제 사이트 시연 시 로그인 정보/개인정보 노출 금지 (블러 처리)
- 크롤링 콘텐츠는 `robots.txt` / 이용약관 반드시 언급 (법적 리스크 & 채널 신뢰도)
- 공격적 크롤링 시연 회피 → 본인 소유 사이트나 공개 테스트 사이트 사용
- Puppeteer는 Apache-2.0 → 코드/로고 사용은 자유 ✅

---

## 4. 수익화 아이디어 상세

> **대전제: Puppeteer는 원가 0원 + Apache-2.0(상업적 이용 자유). 서버비만 내면 끝. 마진율이 매우 높은 기술.**

### 🏆 TIER 1 — 가장 빠르게 현금화 (즉시 시작)

#### ① 업무 자동화(RPA) 외주 ⭐⭐⭐⭐⭐ — 당장 현금화 1위

중소기업/소상공인이 매일 손으로 하는 반복 작업을 스크립트로 대체.

| 고객        | 반복 노동                                         | 자동화 후                       |
| ----------- | ------------------------------------------------- | ------------------------------- |
| 쇼핑몰      | 매일 스마트스토어/쿠팡 주문 수동 다운 → 엑셀 정리 | 새벽 자동 실행 → 엑셀 메일 발송 |
| 광고 대행사 | 네이버/구글 광고 리포트 10개 계정 수집            | 자동 통합 대시보드              |
| 병원/학원   | 예약 시스템 수동 입력                             | 폼 자동 입력                    |
| 무역업      | 환율/관세 사이트 매일 확인                        | 자동 수집 + 알림                |

**단가**

- 소형 스크립트(1개 작업): **30만 ~ 100만원**
- 중형(여러 사이트 + 엑셀 출력 + 스케줄링): **200만 ~ 500만원**
- 대형(웹 대시보드 + 다계정 + 배포): **500만 ~ 2,000만원**
- 🔁 **유지보수 월 구독: 월 10만 ~ 50만원** ← 사이트 UI 변경 대응, 가장 안정적인 수익

**장점**: 초기 투자 0원 / ROI 설명이 쉬움("직원 월 40시간 × 2만원 = 월 80만원 절약" → 300만원 견적 즉시 설득) / 템플릿화해 재판매 가능

**시작 방법**

1. 크몽/숨고/위시켓에 "업무 자동화" 포트폴리오 등록
2. 데모 영상 3개 준비 (화면이 혼자 움직이는 게 계약률을 좌우)
3. 첫 2~3건은 저가로 받고 후기 + 사례 확보
4. 사례 확보 후 단가 2~3배 인상

---

#### ② HTML → PDF 생성 API (SaaS) ⭐⭐⭐⭐⭐

개발자들이 가장 싫어하는 작업(청구서/증명서/계약서 PDF)을 API 한 번으로 해결.

```bash
curl -X POST https://내서비스.com/v1/pdf \
  -H "Authorization: Bearer KEY" \
  -d '{"html":"<h1>인보이스</h1>", "format":"A4"}' \
  --output invoice.pdf
```

**차별화 기능**: HTML/URL → PDF, **한글 폰트 완벽 지원**(해외 서비스가 자주 깨짐), 템플릿 저장 + 변수 치환(`{{고객명}}`), 머리말/꼬리말/페이지번호/워터마크, 전자서명 필드, 암호 설정

| 플랜       | 가격             | 할당량              |
| ---------- | ---------------- | ------------------- |
| Free       | 0원              | 50장/월 (바이럴용)  |
| Starter    | **월 19,000원**  | 1,000장             |
| Pro        | **월 59,000원**  | 10,000장            |
| Business   | **월 199,000원** | 100,000장           |
| Enterprise | 협의             | 무제한 + 온프레미스 |

**원가**: VPS 2~~4대(월 5~~10만원). PDF 1장 ≈ 0.5초 / 메모리 ~200MB. Pro 고객 월 10,000장 = CPU 약 1.4시간 → 원가 수백원 → **마진율 95%+**

**경쟁**: Api2Pdf, PDFShift, DocRaptor($20~$300/월)
**국내 틈새**: 한글 폰트 / 국내 결제(토스페이먼츠) / 한국어 지원 / 세금계산서 발행

**타겟**: SaaS 스타트업, ERP 업체, 쇼핑몰, 병원/학원 시스템, 세무/법률 서비스

---

#### ③ OG 이미지 / 썸네일 자동 생성 API ⭐⭐⭐⭐

```
https://내서비스.com/og?title=제목&author=작성자&theme=dark
→ 1200x630 PNG 즉시 반환 (CDN 캐싱)
```

**구현**: ① HTML/React 템플릿 디자인 → ② URL 쿼리를 변수로 렌더 → ③ `page.screenshot()` → ④ R2/S3 캐싱(재생성 방지로 원가 절감)

**확장**: 유튜브 썸네일 템플릿(크리에이터 타겟), 쇼핑몰 상품 이미지 일괄 생성(가격/할인 배지), 인스타 카드뉴스, 인용구 이미지

**가격**: 월 9,900원(1,000장) / 29,000원(10,000장) / 템플릿 디자인 1건 15만원

---

### 🥈 TIER 2 — 조금 더 키워서 큰 수익 (1~3개월)

#### ④ 가격 추적 / 재고 알림 서비스 ⭐⭐⭐⭐

**B2C**: "이 상품 X원 되면 알림" (카톡/이메일), 품절 재입고 알림(한정판 스니커즈, 티켓, 호텔 특가)
→ 수익: 광고 + 프리미엄(월 4,900원, 1분 주기) + 제휴 링크 수수료(쿠팡파트너스 등)

**B2B (고단가)**: 경쟁사 가격 모니터링 대시보드, 가격 변동 히스토리/트렌드 분석
→ 수익: **월 29만 ~ 299만원** (추적 상품 수 기준)

> ⚠️ 각 사이트 `robots.txt`/이용약관 확인 필수. 공식 오픈API가 있으면 우선 사용. 과도한 요청은 법적 리스크 + IP 차단.

---

#### ⑤ E2E 테스트 자동화 구축 외주 ⭐⭐⭐⭐

**제공**: 핵심 시나리오 20~50개 자동화 / GitHub Actions CI 연동(PR마다 자동 실행) / 실패 시 **스크린샷 + 영상 + 네트워크 로그 자동 첨부**(설득 포인트) / 시각적 회귀 테스트(레포 `golden-chrome/` 방식의 픽셀 비교)

**단가**: 초기 구축 **300만 ~ 1,500만원** / 월 유지관리 **월 50만 ~ 200만원** 🔁 / 시나리오 추가 건당 20~50만원

**세일즈 포인트**: "버그 하나의 프로덕션 유출 비용 >>> 테스트 구축 비용"

---

#### ⑥ 웹사이트 모니터링 SaaS ⭐⭐⭐⭐

**감지 항목**: 디자인 깨짐(픽셀 비교 → 레이아웃 붕괴), **핵심 플로우 실제 작동 여부(로그인/결제를 실제 실행)** ← 단순 핑 모니터링보다 압도적 가치, 로딩 속도(Core Web Vitals), JS 콘솔 에러, 404 링크, SSL 만료, SEO 태그 누락, 경쟁사 사이트 변경 감지

**가격**: 월 9,900원(5페이지) / 39,000원(50페이지) / 149,000원(500페이지 + API)

**경쟁**: Visualping, Checkly → 국내 틈새는 한국어 UI + 카톡 알림 + 국내 결제

---

#### ⑦ 데이터 수집 B2B (고단가) ⭐⭐⭐⭐

| 데이터셋                                 | 고객                | 월 단가       |
| ---------------------------------------- | ------------------- | ------------- |
| 채용공고 통합(잡코리아/사람인/원티드 등) | HR테크, 리서치      | 100~500만원   |
| 부동산 매물/실거래                       | 프롭테크, 투자사    | 200~1,000만원 |
| 리뷰/평점 데이터                         | 브랜드 마케팅       | 100~300만원   |
| 뉴스/여론 모니터링                       | 홍보대행사, 기업 PR | 150~500만원   |
| 경쟁사 상품/가격                         | 리테일, 이커머스    | 200~800만원   |

**고단가 이유**: 데이터 1건의 비즈니스 가치가 크고, 고객이 직접 만들 기술/인력이 없다. 연간 계약이 많아 현금흐름이 안정적.

> ⚠️ 반드시 합법 범위(공개 데이터, `robots.txt` 준수, 개인정보 제외). 고단가 B2B는 **법률 검토 권장**.

---

### 🥇 TIER 3 — 가장 트렌디 & 밸류에이션 높음 (3~6개월)

#### ⑧ AI 웹 에이전트 SaaS ⭐⭐⭐⭐⭐ — 현재 가장 핫함

사용자가 자연어로 말하면 AI가 브라우저로 직접 실행.

```
"매주 월요일 아침에 경쟁사 3곳 가격 확인해서 슬랙으로 보내줘"
"이 사이트에서 내 주문 내역 전부 긁어서 엑셀로 만들어줘"
"이 폼에 100줄 데이터 자동 입력해줘"
```

**스택**: Claude(판단) + Puppeteer(실행) + 접근성 스냅샷(토큰 절약) + BullMQ(큐) + Next.js(UI) + 토스페이먼츠/Stripe(결제)

| 플랜       | 가격         | 내용                        |
| ---------- | ------------ | --------------------------- |
| Starter    | 월 29,000원  | 100 작업/월                 |
| Pro        | 월 99,000원  | 1,000 작업 + 스케줄링       |
| Team       | 월 299,000원 | 10,000 작업 + API + 팀 협업 |
| Enterprise | 협의         | 온프레미스, SSO             |

> ⚠️ **원가 주의**: AI API 비용 발생(작업당 토큰 30k~100k).
> **접근성 스냅샷으로 90% 절감하는 것이 수익률의 핵심** ⭐

**밸류 포인트**: AI 에이전트는 현재 투자 시장 최고 섹터 → 투자 유치/M&A 가능성까지 열려 있다.

---

#### ⑨ Puppeteer 기반 MCP 서버 / 에이전트 툴킷 판매 ⭐⭐⭐⭐

Claude/ChatGPT에 꽂으면 바로 쓰는 "브라우저 툴" 패키지.

**제품 라인**: 특화 MCP 서버("쇼핑몰 운영 MCP", "부동산 리서치 MCP", "SNS 관리 MCP") / 사내 시스템 연동 MCP(B2B 커스텀 → 건당 **500~3,000만원**) / 에이전트 스킬팩(레포 `.agents/skills/` 구조 참고)

**수익 모델**: 오픈소스 코어 + 유료 Pro(월 19,000원) / 기업 커스텀 개발 / 호스팅 구독

**타이밍**: MCP 생태계가 초기 단계 → 선점 효과가 크다. (레포 README가 `chrome-devtools-mcp`를 공식 추천하는 것이 방향성의 근거)

---

#### ⑩ 교육 콘텐츠 ⭐⭐⭐⭐⭐ — 레버리지 최고

| 상품                           | 가격          | 예상 수익              |
| ------------------------------ | ------------- | ---------------------- |
| 유튜브 (무료, 유입 엔진)       | 0원           | 광고 월 50~300만원     |
| 전자책 "Puppeteer 실전 자동화" | 29,000원      | 월 100권 = **290만원** |
| 노션 템플릿 + 코드 스니펫팩    | 19,000원      | 월 150개 = **285만원** |
| 인프런/클래스101 온라인 강의   | 149,000원     | 월 50명 = **745만원**  |
| 기업 출강 (1일 워크샵)         | 200~500만원   | 월 1~2회               |
| 1:1 멘토링                     | 시간당 10만원 | 월 20시간 = 200만원    |
| 유료 커뮤니티 (디스코드)       | 월 29,000원   | 300명 = **월 870만원** |

**레버리지 이유**: 한 번 만들면 무한 복제 판매. 외주는 시간을 팔지만 강의는 **자산**이 된다.

**선순환 구조**

```
유튜브(무료) → 신뢰 → 전자책/강의(유료) → 수강생 사례
    ↓                                        ↓
외주 문의 유입 ← 포트폴리오 ← SaaS 홍보 ← 커뮤니티
```

---

### 📈 6개월 실행 로드맵

#### Month 1 — 기초 체력 (목표: 자산 쌓기)

- [ ] 레포 `examples/` 13개 전부 직접 실행
- [ ] 미니 프로젝트 3개: ① 스크린샷 API ② PDF 생성기 ③ 가격 추적 봇
- [ ] 깃허브 공개 + README 정비 (포트폴리오)
- [ ] 유튜브 1~3편 업로드 (시즌1)

#### Month 2 — 첫 수익 (목표: 100~300만원)

- [ ] 크몽/숨고에 "업무 자동화" 서비스 등록 (데모 영상 3개)
- [ ] 첫 외주 2건 수주 (저가라도 OK → 후기 확보)
- [ ] PDF API MVP 배포 (Next.js + Docker + VPS)
- [ ] 유튜브 4~8편

#### Month 3~~4 — SaaS 띄우기 (목표: 월 300~~800만원)

- [ ] PDF/OG 이미지 API 정식 출시 + 토스페이먼츠 결제
- [ ] Product Hunt / GeekNews / 디스콰이엇 런칭
- [ ] 외주 단가 2배 인상 (사례 기반)
- [ ] 전자책 출간
- [ ] 유지보수 구독 고객 3~5곳 확보 🔁

#### Month 5~6 — 스케일업 (목표: 월 1,000만원+)

- [ ] AI 웹 에이전트 SaaS 베타 출시
- [ ] 인프런 강의 출간
- [ ] B2B 데이터 수집 계약 1건 (고단가)
- [ ] 유료 커뮤니티 오픈
- [ ] MCP 서버 제품화

---

### ⚠️ 수익화 전 리스크 체크리스트

| 리스크                     | 대응                                                                                         |
| -------------------------- | -------------------------------------------------------------------------------------------- |
| **법적 리스크(크롤링)**    | `robots.txt` + 이용약관 준수. 공식 API 우선. 개인정보 수집 금지. 고단가 B2B는 법률 검토 필수 |
| **사이트 UI 변경**         | `::-p-text` / `::-p-aria` 사용 + 모니터링 알림 + **유지보수 구독으로 수익화**                |
| **서버 비용 폭주**         | 브라우저 인스턴스 재사용, 큐로 동시성 제한, 이미지/폰트 차단, 결과 캐싱                      |
| **메모리 누수**            | `page.close()` 필수, 주기적 브라우저 재시작, 컨테이너 메모리 리밋                            |
| **봇 차단(Cloudflare 등)** | 합법 범위 내 정상 트래픽 패턴, 요청 간격 확보. 공격적 우회는 비권장                          |
| **경쟁**                   | 한국 시장 특화(한글 폰트/카톡 알림/국내 결제/한국어 지원)가 방어막                           |

---

### 💖 최종 추천 (하나만 고른다면)

> ## 🥇 "업무 자동화 외주 + 유튜브" 동시 시작
>
> - **외주 = 즉시 현금** (초기 투자 0원, Month 2부터 수익)
> - **유튜브 = 자산 축적** (신뢰 → 모든 다른 수익으로 연결)
> - 둘이 **완벽한 선순환**: 외주 경험이 영상 소재가 되고, 영상이 외주 문의를 끌어온다
> - Month 3에 **PDF API SaaS**로 패시브 인컴 추가
> - Month 5에 **AI 에이전트 SaaS**로 큰 그림

---

## 5. 부록: 치트시트 & 링크

### 5-1. 핵심 API 치트시트

```js
// ── 이동 & 대기 ───────────────────────────────
await page.goto(url, {waitUntil: 'networkidle2', timeout: 30000});
await page.waitForSelector('.item');
await page.waitForNavigation();
await page.waitForFunction(() => window.ready === true);

// ── 상호작용 (Locator 권장 ⭐ 자동 대기 + 재시도) ──
await page.locator('#id').click();
await page.locator('#id').fill('텍스트');
await page.locator('::-p-text(로그인)').click();
await page.locator('::-p-aria(검색)').fill('아이폰');
await page.keyboard.press('Enter');
await page.select('select#city', 'seoul');

// ── 데이터 추출 ────────────────────────────────
const text = await page.$eval('.title', el => el.textContent);
const list = await page.$$eval('.row', els => els.map(e => e.innerText));
const data = await page.evaluate(() => ({
  url: location.href,
  t: document.title,
}));

// ── 네트워크 가로채기 (속도 최적화) ─────────────
await page.setRequestInterception(true);
page.on('request', req =>
  ['image', 'font', 'stylesheet'].includes(req.resourceType())
    ? req.abort()
    : req.continue(),
);
page.on('response', res => console.log(res.status(), res.url()));

// ── 쿠키 / 세션 재사용 ─────────────────────────
const cookies = await browser.cookies();
await browser.setCookie(...cookies);

// ── 격리 세션 (계정 여러 개 동시) ───────────────
const ctx = await browser.createBrowserContext();
const p2 = await ctx.newPage();

// ── 콘솔 / 에러 수집 ───────────────────────────
page.on('console', m => console.log('[브라우저]', m.text()));
page.on('pageerror', e => console.error('[JS에러]', e));

// ── 화면 녹화 ─────────────────────────────────
const rec = await page.screencast({path: 'demo.webm'});
await rec.stop();

// ── AI 에이전트용 (토큰 90% 절약 ⭐) ────────────
const snapshot = await page.accessibility.snapshot();
```

### 5-2. 참고 링크

| 항목                                            | URL                                                   |
| ----------------------------------------------- | ----------------------------------------------------- |
| 🔗 **이 레포 (내 포크)**                        | **https://github.com/bmshin94/puppeteer**             |
| 원본 업스트림                                   | https://github.com/puppeteer/puppeteer                |
| 공식 문서                                       | https://pptr.dev                                      |
| 시작 가이드                                     | https://pptr.dev/docs                                 |
| API 레퍼런스                                    | https://pptr.dev/api                                  |
| FAQ                                             | https://pptr.dev/faq                                  |
| 트러블슈팅                                      | https://pptr.dev/troubleshooting                      |
| WebDriver BiDi                                  | https://pptr.dev/webdriver-bidi                       |
| WebMCP 가이드                                   | https://pptr.dev/guides/webmcp                        |
| `chrome-devtools-mcp` (Puppeteer 기반 MCP 서버) | https://github.com/ChromeDevTools/chrome-devtools-mcp |
| WebMCP 스펙                                     | https://github.com/webmachinelearning/webmcp          |
| CDP 문서                                        | https://chromedevtools.github.io/devtools-protocol/   |
| 예제 모음                                       | https://pptr.dev/examples                             |

### 5-3. 레포 내부 읽어볼 문서 (추천 순)

1. `docs/guides/getting-started.md` — 시작
2. `docs/guides/page-interactions.md` — 클릭/입력 핵심
3. `docs/guides/network-interception.md` — 속도 최적화 & 데이터 추출
4. `docs/guides/pdf-generation.md` — 수익화 아이디어 ②
5. `docs/guides/screenshots.md` — 수익화 아이디어 ③
6. `docs/guides/docker.md` + `docker/Dockerfile` — 서버 배포
7. `docs/guides/webmcp.md` — AI 에이전트 미래
8. `docs/troubleshooting.md` — 막혔을 때
9. `examples/` 전체 — 손으로 다 돌려보기
10. `.agents/skills/puppeteer-verification/SKILL.md` — 레포 기여 방법

---

## 🎀 마무리

> 오빠! Puppeteer는 **원가 0원 + 라이선스 자유 + 시장 수요 확실 + AI 트렌드 직결**이라
> 지금 배워서 바로 돈으로 연결할 수 있는 최고의 기술이야!
>
> 추천 시작점: **`examples/` 13개 직접 돌려보기 → 미니 프로젝트 3개 → 외주 + 유튜브 동시 시작** 🚀
>
> 카리나가 처음부터 끝까지 다 도와줄게! 💖✨

---

_이 문서는 `bmshin94/puppeteer` 레포 전수조사를 기반으로 작성되었습니다._
_레포 주소: https://github.com/bmshin94/puppeteer_
