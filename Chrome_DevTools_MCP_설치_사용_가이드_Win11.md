# Chrome DevTools MCP — 설치·사용 가이드 (Windows 11)

> **대상**: 사내 개발팀 (Claude Code / AI 코딩 에이전트 사용자)
> **기준 OS**: Windows 11 + PowerShell
> **문서 버전**: 2026-06-17 작성 · chrome-devtools-mcp v1.0.x 기준
> **목적**: AI 코딩 에이전트가 실제 브라우저를 직접 띄워 화면 검증·디버깅·성능 분석을 수행하도록 설정

---

## 1. 개요 — 무엇인가

**Chrome DevTools MCP**(`chrome-devtools-mcp`)는 Google Chrome DevTools 팀이 만든 **오픈소스(Apache-2.0) MCP 서버**다. AI 코딩 에이전트(Claude, Cursor, Copilot, Gemini, Codex 등)에게 **실행 중인 Chrome 브라우저를 제어·검사할 수 있는 능력**을 부여한다.

기존 코딩 에이전트는 자기가 작성한 코드가 브라우저에서 실제로 어떻게 렌더링·동작하는지 볼 수 없어, 사실상 "눈을 가린 채" 코딩하는 한계가 있었다. 이 도구는 에이전트에게 **브라우저를 보는 눈**을 달아준다. 내부적으로 puppeteer로 Chrome 동작을 자동화하며, 콘솔·네트워크·DOM·성능 트레이스를 모두 읽을 수 있다.

핵심은 **스크린샷(이미지)에 그치지 않고, 렌더링된 페이지의 실제 소스(DOM·HTML·네트워크 응답)를 구조화된 형태로 읽는다**는 점이다.

### 공식 출처

| 구분 | 링크 |
|------|------|
| GitHub 저장소(공식) | `https://github.com/ChromeDevTools/chrome-devtools-mcp` |
| 공식 발표 블로그 | `https://developer.chrome.com/blog/chrome-devtools-mcp` |
| 공식 시작 가이드 | `https://developer.chrome.com/docs/devtools/agents/get-started` |
| 도구 레퍼런스 | `https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/tool-reference.md` |
| 트러블슈팅 | `https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/troubleshooting.md` |
| npm 패키지 | `https://www.npmjs.com/package/chrome-devtools-mcp` |

---

## 2. 주요 기능

- **성능 인사이트**: Chrome DevTools로 트레이스를 기록하고 실행 가능한 성능 분석 결과를 추출(LCP 등).
- **고급 디버깅**: 네트워크 요청 분석, 스크린샷, 콘솔 메시지 확인(소스맵 적용 스택 트레이스 포함).
- **신뢰성 있는 자동화**: puppeteer 기반으로 Chrome 동작을 자동화하고, 동작 결과를 자동으로 대기.
- **실제 소스 검증**: 렌더링된 DOM 스냅샷, `outerHTML`, 서버 응답 원본을 읽어 로컬 소스와 대조 가능.

---

## 3. 사전 요구사항 (Windows 11)

| 항목 | 요구 버전 | 확인 명령 (PowerShell) |
|------|-----------|------------------------|
| Node.js | LTS 이상 (22+) | `node --version` |
| npm | 동봉 버전 | `npm --version` |
| Google Chrome | 현재 stable 이상 | 아래 참조 |

Chrome 버전 확인:

```powershell
(Get-Item "C:\Program Files\Google\Chrome\Application\chrome.exe").VersionInfo.ProductVersion
```

경로가 다르면 `C:\Program Files (x86)\...` 시도, 또는 주소창에 `chrome://version` 입력.

> ⚠️ 공식적으로 **Google Chrome / Chrome for Testing만** 지원한다. 다른 Chromium 계열 브라우저는 동작이 보장되지 않는다.
> ⚠️ `--autoConnect`(실행 중 크롬에 연결) 기능은 **Chrome 144 이상** 필요. 기본 격리 모드는 stable이면 충분.

---

## 4. 설치 (Claude Code 기준)

설치 방식은 두 가지다. **스킬까지 필요하면 플러그인, 빠르고 가볍게 쓰려면 CLI**를 택한다.

### 방식 A — CLI 설치 (MCP 서버만)

PowerShell에서:

```powershell
claude mcp add chrome-devtools --scope user -- npx chrome-devtools-mcp@latest
```

- `--scope user`: 어느 작업 폴더에서든 사용 가능하도록 유저 레벨 등록 (`C:\Users\<사용자>\.claude.json`에 기록됨).
- `--` 뒤가 서버 실행 명령. 설정 끝나면 작업 폴더에서 `claude` 실행 → `/mcp`로 확인.
- **장점**: git 불필요, 사내 방화벽 영향 적음. **단점**: 전용 스킬 6종 미포함.

### 방식 B — 플러그인 설치 (MCP + 스킬)

`claude` 실행 후 **세션 안에서** 슬래시 명령으로:

```
/plugin marketplace add ChromeDevTools/chrome-devtools-mcp
/plugin install chrome-devtools-mcp@chrome-devtools-plugins
```

설치 후 Claude Code를 재시작하면 MCP 서버 + 스킬이 로드된다(`/skills`로 확인).

> ⚠️ 이전에 chrome-devtools-mcp를 설치한 적이 있으면, 기존 설치·설정을 먼저 제거하고 진행.
> ⚠️ 플러그인 설치 시 GitHub 저장소를 git으로 clone하므로 **git이 PATH에 있어야** 한다. `Failed to clone repository` 에러(사내 방화벽 등)가 나면 **방식 A(CLI)로 대체**하거나 트러블슈팅 가이드 참조.

### Windows 11 주의 — 기동 타임아웃/환경변수

Windows에서 npx 첫 기동이 환경변수·타임아웃 문제로 느리거나 실패할 수 있다. 설정 파일에서 직접 args를 주는 클라이언트(예: Codex)는 아래처럼 `cmd` 래핑 + 환경변수 + 타임아웃을 늘려 대응한다(원리는 다른 에이전트도 동일):

```toml
[mcp_servers.chrome-devtools]
command = "cmd"
args = ["/c", "npx", "-y", "chrome-devtools-mcp@latest"]
env = { SystemRoot="C:\\Windows", PROGRAMFILES="C:\\Program Files" }
startup_timeout_ms = 20_000
```

### (참고) 다른 MCP 클라이언트용 표준 설정

Claude Code 외 도구(Cursor, VS Code, Windsurf 등)는 아래 JSON을 각 클라이언트의 MCP 설정에 추가하면 된다:

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest"]
    }
  }
}
```

---

## 5. 연결 확인

`claude` 세션 안에서:

```
/mcp
```

목록에 다음처럼 표시되면 정상:

```
chrome-devtools · ✔ connected · 29 tools
```

도구는 `mcp__chrome-devtools__` 접두어로 노출된다(예: `navigate_page`, `take_snapshot`, `take_screenshot`).

> 도구 수는 버전·설정에 따라 다르다. 기본 활성 약 29개. 일부 카테고리(Extensions·Third-party·WebMCP·Memory)는 플래그로 켜야 추가된다.

---

## 6. 첫 동작 테스트 (스모크 테스트)

연결 확인 후, 에이전트 프롬프트에 입력:

```
Check the performance of https://developers.chrome.com
```

또는 단계별로:

```
chrome-devtools로 https://example.com 열고, take_snapshot으로 DOM을 읽고, take_screenshot으로 전체 페이지를 캡처해줘
```

브라우저 창이 자동으로 뜨고 결과가 돌아오면 설치 완료.

> 💡 **브라우저는 도구를 처음 호출하는 순간 자동 기동**한다. MCP 서버에 연결만 해서는 브라우저가 뜨지 않는다.

---

## 7. 브라우저 동작 모드

상황에 맞게 4가지 모드를 선택한다. 가장 큰 차이는 "**새 브라우저를 띄울지, 내 실제 크롬에 붙을지**"다.

| 모드 | 옵션 | 설명 | 권장 용도 |
|------|------|------|-----------|
| 기본(전용 프로파일) | (없음) | 내 실제 크롬과 분리된 **전용 프로파일**로 새 Chrome 기동. 프로파일은 세션 간 보존됨 | 일반 검증·디버깅 |
| 완전 격리 | `--isolated` | 매 세션 **임시 프로파일** 생성, 종료 시 자동 삭제 | 매번 깨끗한 상태 필요 시 |
| 자동 연결 | `--autoConnect` | 실행 중인 내 Chrome(144+)에 연결. 로그인 세션 그대로 사용 | 로그인 필요한 페이지 검증 |
| 원격 디버깅 | `--browser-url=http://127.0.0.1:9222` | 디버깅 포트로 띄운 Chrome에 연결 | 샌드박스/원격 환경 |

기본 프로파일 경로(Windows): `%HOMEPATH%\.cache\chrome-devtools-mcp\chrome-profile-stable`
→ 이 경로가 별도이므로, **내 평소 크롬·로그인은 건드리지 않는다.**

### 격리 모드로 켜는 법 (CLI 설치 후)

CLI `claude mcp add`는 `--isolated`를 인라인 인자로 받지 못한다(파서가 가로챔). 등록 후 `C:\Users\<사용자>\.claude.json`의 chrome-devtools `args` 배열을 직접 편집한다:

```json
"args": ["chrome-devtools-mcp@latest", "--isolated"]
```

### autoConnect 사용 시 (로그인 페이지 검증)

1. Chrome에서 `chrome://inspect/#remote-debugging` 접속 → 원격 디버깅 허용
2. 서버 args에 `--autoConnect` 추가
3. 연결 시 Chrome이 권한 허용 다이얼로그를 띄우면 **Allow**

> ⚠️ autoConnect/원격 디버깅은 로그인 세션을 MCP 클라이언트에 노출한다. 민감 시스템·내부망에는 신중히 사용.

---

## 8. 제공 도구 목록 (카테고리별)

| 카테고리 | 개수 | 주요 도구 | 기본 활성 |
|----------|------|-----------|-----------|
| 입력 자동화 | 10 | `click` · `fill` · `fill_form` · `hover` · `type_text` · `press_key` · `drag` · `upload_file` · `handle_dialog` | ✅ |
| 내비게이션 | 6 | `navigate_page` · `new_page` · `list_pages` · `select_page` · `close_page` · `wait_for` | ✅ |
| 에뮬레이션 | 2 | `emulate` · `resize_page` | ✅ |
| 성능 | 3 | `performance_start_trace` · `performance_stop_trace` · `performance_analyze_insight` | ✅ |
| 네트워크 | 2 | `list_network_requests` · `get_network_request` | ✅ |
| 디버깅 | 8 | `take_snapshot` · `take_screenshot` · `evaluate_script` · `list_console_messages` · `get_console_message` · `lighthouse_audit` · `screencast_start/stop` | ✅ |
| 메모리 | 5 | `take_heapsnapshot` 등 | 실험적 |
| 확장프로그램 | 5 | `install_extension` 등 | 플래그 필요 |
| 서드파티/WebMCP | 4 | `execute_3p_developer_tool` 등 | 플래그 필요 |

핵심 도구 요약:
- **`take_snapshot`**: 스크린샷이 아닌 **텍스트 기반 DOM(접근성 트리)** 스냅샷. 가장 빠르고 기본 권장.
- **`take_screenshot`**: 화면 이미지 캡처(전체 페이지/요소 지정 가능).
- **`evaluate_script`**: 페이지 컨텍스트에서 JS 실행(예: `document.documentElement.outerHTML`로 실제 HTML 추출).
- **`list_console_messages` / `list_network_requests`**: 콘솔 에러·네트워크 요청 검사.
- **`performance_start_trace`**: 성능 트레이스 기록 후 인사이트 추출.

---

## 9. Playwright MCP와 비교 (테스트 vs 디버깅)

브라우저를 다루는 MCP로 가장 많이 쓰이는 두 도구가 **Playwright MCP**(Microsoft)와 **Chrome DevTools MCP**(Google)다. 입력 자동화·내비게이션 도구는 상당 부분 겹치지만, **지향점이 다르다.**

- **Playwright MCP = 테스트 자동화 지향**: 크로스브라우저 E2E 테스트가 본업. 재현 가능한 시나리오 실행, 로그인 세션 유지, 테스트 코드 생성 생태계에 최적화. 입력·내비게이션 도구가 "안정적 테스트 흐름"을 위해 설계됨.
- **Chrome DevTools MCP = 디버깅·성능 지향**: 입력·내비게이션은 "검사할 상태로 페이지를 몰고 가는" 수단이고, 진짜 가치는 **성능 트레이스·Lighthouse·힙 스냅샷·소스맵 콘솔·상세 네트워크** 등 DevTools 고유 기능에 있다. Chrome 전용.

### 9.1 제공 도구 비교 (영역별)

| 영역 | Playwright MCP (약 23 tools) | Chrome DevTools MCP (45 tools / 기본 ~29) |
|------|------------------------------|-------------------------------------------|
| **입력 자동화** | `browser_click` · `browser_type` · `browser_fill_form` · `browser_select_option` · `browser_press_key` · `browser_hover` · `browser_drag` · `browser_file_upload` · `browser_handle_dialog` | `click` · `fill` · `fill_form` · `type_text` · `press_key` · `hover` · `drag` · `upload_file` · `handle_dialog` · `click_at` |
| **내비게이션** | `browser_navigate` · `browser_navigate_back` · `browser_tabs` · `browser_wait_for` | `navigate_page` · `new_page` · `list_pages` · `select_page` · `close_page` · `wait_for` |
| **페이지 인식** | `browser_snapshot`(접근성 트리) · `browser_take_screenshot` | `take_snapshot`(접근성 트리) · `take_screenshot` |
| **콘솔/네트워크** | `browser_console_messages` · `browser_network_requests` (기본 수준) | `list/get_console_message` · `list/get_network_request` (**소스맵 스택·상세 요청**) |
| **성능** | ✗ 거의 없음 | ✅ `performance_start/stop_trace` · `performance_analyze_insight` · `lighthouse_audit` · CrUX 필드데이터 |
| **메모리/힙** | ✗ | ✅ `take_heapsnapshot` 등 5종 |
| **에뮬레이션** | 일부(뷰포트/디바이스) | `emulate` · `resize_page` |
| **크로스브라우저** | ✅ Chromium / Firefox / WebKit | ✗ **Chrome / Chrome for Testing 전용** |
| **로그인 세션 유지** | ✅ 강점(영속 컨텍스트 재사용) | autoConnect(Chrome 144+) 시 가능 |
| **지향점** | **E2E 테스트 자동화** | **디버깅 · 성능 분석** |

> 핵심: **입력 자동화·내비게이션은 양쪽 모두 풍부**하다(클릭·입력·폼·대기·스냅샷·스크린샷 등 대부분 1:1 대응). 따라서 "기본 조작"만 보면 어느 쪽이든 가능하다. 선택 기준은 **"조작한 다음에 무엇을 보려는가"** 이다.

### 9.2 어떤 작업에 무엇을 쓰나

| 작업 성격 | 권장 도구 | 이유 |
|-----------|-----------|------|
| 반복 E2E 테스트 시나리오 실행 | **Playwright MCP** | 재현성·테스트 흐름 최적화 |
| 크로스브라우저(FF·WebKit) 검증 | **Playwright MCP** | Chrome DevTools는 Chrome 전용 |
| 로그인 세션 유지하며 사람처럼 조작 | **Playwright MCP** | 영속 컨텍스트 재사용 강점 |
| 콘솔 에러·네트워크·DOM 디버깅 | **Chrome DevTools MCP** | 소스맵 스택·상세 요청 |
| 성능(LCP·트레이스)·Lighthouse | **Chrome DevTools MCP** | Playwright엔 거의 없는 영역 |
| 렌더 검증·전체페이지 캡처 | **Chrome DevTools MCP** | DevTools 캡처·스냅샷 |
| 메모리 누수·힙 분석 | **Chrome DevTools MCP** | 힙 스냅샷 도구 |

### 9.3 혼용 팁

- 두 서버는 **각자 별도 Chrome 프로파일**을 사용하므로 동시에 설치해도 충돌하지 않는다(Playwright는 자체 공유 프로파일, Chrome DevTools는 `chrome-profile-stable` 또는 `--isolated` 임시 프로파일).
- 한 세션에서 둘 다 붙어 있으면, 작업에 따라 프롬프트에 **"chrome-devtools로"** 또는 **"playwright로"** 를 명시해 도구를 골라 쓴다.
- 권장 분담: **자력테스트(로그인·반복 조작) = Playwright** / **렌더 검증·디버깅·성능·캡처 = Chrome DevTools**. 한쪽 브라우저가 점유 중일 때 다른 쪽을 독립 채널로 활용할 수 있다.

---

## 10. 주요 설정 옵션

`args` 배열에 추가해 사용한다.

| 옵션 | 설명 |
|------|------|
| `--headless` | UI 없는 헤드리스 모드 |
| `--isolated` | 임시 프로파일(세션 종료 시 삭제) |
| `--channel=stable\|beta\|dev\|canary` | Chrome 채널 지정 |
| `--viewport=1280x720` | 초기 뷰포트 크기 |
| `--slim` | 내비게이션·스크립트·스크린샷 3개 도구만 노출(경량) |
| `--proxyServer=...` | 프록시 서버 지정(사내망 대응) |
| `--acceptInsecureCerts` | 자체서명/만료 인증서 오류 무시(주의) |
| `--no-performance-crux` | 성능 트레이스 URL의 CrUX API 전송 비활성화 |
| `--no-usage-statistics` | Google 사용 통계 수집 거부 |

설정 예시:

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["chrome-devtools-mcp@latest", "--headless=true", "--isolated=true"]
    }
  }
}
```

전체 옵션은 `npx chrome-devtools-mcp@latest --help`로 확인.

---

## 11. 실전 사용법 — 프롬프트 예시

### (1) 로컬 개발 화면 검증

```
chrome-devtools로 http://localhost:8080 열어서
1) 콘솔 에러·경고 전부 수집해서 보고
2) take_snapshot으로 화면 구조 읽기
3) [특정 버튼/드롭다운] 클릭 후 정상 동작 확인
4) 문제 있으면 로컬 소스에서 원인 찾아 수정 제안
```

### (2) 배포 검증 (개발기 ↔ 로컬 소스 대조)

```
chrome-devtools로 [개발기 URL] 열고, 로딩된 페이지의
네트워크 응답 본문을 로컬 소스 파일과 대조해서
최신 코드가 개발기에 반영됐는지 확인해줘
```

### (3) 화면 캡처 (검수 보고용)

```
chrome-devtools로 [URL] 열고 전체 페이지 스크린샷을 찍어줘
```

### (4) 성능 분석

```
[URL]의 LCP와 성능 트레이스를 분석해줘
```

> 💡 **검증 기준 선택**: "배포 반영 여부"는 **네트워크 응답 본문 ↔ 로컬 파일**(바이트 단위 비교)을, "렌더링·동작 정상 여부"는 **DOM 스냅샷**(JS 실행 후 결과)을 본다. 렌더링된 DOM과 정적 소스 파일은 1:1로 같지 않다(브라우저 정규화·JS 변형 때문).

---

## 12. 보안·주의사항 (필독 — 특히 공공/사내망)

- **브라우저 내용 노출**: 이 서버는 브라우저의 모든 내용을 MCP 클라이언트에 노출한다. **민감/개인 정보가 있는 사이트는 다루지 않는다.**
- **로그인 세션 주의**: `--autoConnect`·`--browser-url`은 내 로그인 세션을 노출한다. 운영/관리자 시스템에는 사용 금지 권장.
- **원격 디버깅 포트**: `--browser-url` 사용 시 디버깅 포트가 열려 같은 머신의 다른 앱이 브라우저를 제어할 수 있다. 그 동안 민감 사이트 접속 금지. Chrome은 보안상 별도 user-data-dir를 요구한다.
- **계정·시크릿**: 개발 전용 계정·비밀번호를 문서/공개 저장소에 커밋하지 않는다.
- **데이터 수집**: 성능 트레이스 URL은 기본적으로 Google CrUX API로 전송될 수 있고(`--no-performance-crux`로 차단), 사용 통계도 기본 수집된다(`--no-usage-statistics` 또는 `CHROME_DEVTOOLS_MCP_NO_USAGE_STATISTICS` 환경변수로 차단). 사내 정책상 외부 전송이 제한되면 두 플래그를 추가 권장.
- **격리 우선**: 검증 용도라면 기본/격리 모드(전용 프로파일)를 우선 사용해 실제 브라우저·내부 시스템 노출을 피한다.

---

## 13. 트러블슈팅

| 증상 | 원인 | 해결 |
|------|------|------|
| `Failed to clone repository` (플러그인 설치) | git 미설치 / 사내 방화벽 | git 설치(`winget install --id Git.Git`) 후 재시도, 또는 CLI 방식(방식 A) 사용 |
| `error: unknown option '--isolated'` (CLI add) | 파서가 플래그를 가로챔 | 등록은 플래그 없이, `--isolated`는 `.claude.json` args에 직접 추가 |
| 첫 기동 느림/타임아웃 (Windows) | npx 환경변수/타임아웃 | `cmd` 래핑 + `SystemRoot`·`PROGRAMFILES` env + `startup_timeout_ms` 상향 (§4) |
| 브라우저가 안 뜸 | 연결만으론 미기동 | 브라우저가 필요한 도구(navigate 등)를 호출하면 자동 기동됨 |
| 자력테스트 중 세션 충돌 | 단일 프로파일 동시 점유 | `--isolated`로 독립 프로파일 사용, 또는 별도 서버 인스턴스 분리 |

자동 복구가 필요하면(스킬 설치 시) 에이전트에 다음 프롬프트:
```
Use the Chrome DevTools troubleshooting skill to fix my setup.
```

---

## 14. 빠른 시작 체크리스트

- [ ] `node --version` ≥ 22, `npm --version` 확인
- [ ] Chrome 버전 stable 이상 확인
- [ ] `claude mcp add chrome-devtools --scope user -- npx chrome-devtools-mcp@latest` (CLI) **또는** `/plugin install ...` (플러그인)
- [ ] `/mcp`에서 `chrome-devtools ✔ connected` 확인
- [ ] `https://example.com` 스모크 테스트(스냅샷+스크린샷) 성공
- [ ] (선택) 격리/프록시/통계차단 등 사내 정책에 맞는 옵션 적용

---

*본 문서는 공식 GitHub README(ChromeDevTools/chrome-devtools-mcp) 및 Chrome for Developers 공식 문서를 기준으로 작성됨. 명령·옵션은 버전에 따라 변동될 수 있으니, 최신 내용은 공식 저장소를 확인할 것.*
