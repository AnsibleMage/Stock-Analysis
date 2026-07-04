# AI 과열/암흑 지수 — 산출 수식 · 데이터 출처 · 자동 갱신 가이드

> 작성: Mir (Claude) · 기준일 2026-06-23
> 목적: 대화에서 그린 3개 차트의 **계산식**, **데이터 출처**, **분기 자동 갱신 파이프라인**을 한 문서로 정리.
> ⚠️ 투자 조언이 아니라 현황 분석용 자료입니다. 저는 면허 있는 투자자문가가 아닙니다.

---

## 0. 개요 — 무엇을, 어떤 성격으로

| #   | 차트                         | 성격             | 자동 갱신                |
| --- | -------------------------- | -------------- | -------------------- |
| ①   | 하이퍼스케일러 capex 규모 + 전년比 증가율 | **객관** (보고 수치) | 가능 (분기)              |
| ②   | AI 과열/암흑 합성 지수 (밸류에이션 가중)  | **주관** (판단)    | 불가 (수동 리뷰)           |
| ③   | capex ÷ 영업현금흐름(OCF) 괴리 배수  | **객관** (보고 수치) | 가능 (분기) — *권장 트립와이어* |

핵심 구분: ①③은 기업이 10-K/10-Q에 보고하는 실제 숫자로 만들 수 있어 자동화 가능. ②는 제 판단을 수치화한 것이라 자동화 불가(분기마다 사람이 갱신).

---

## 1. 차트별 산출 수식

### 1-1. capex & 전년比 증가율 (객관)

```
YoY_growth(t) = ( CapEx(t) / CapEx(t-1) - 1 ) * 100   [%]
```

- `CapEx(t)` = 빅5(Amazon, Microsoft, Alphabet, Meta, Oracle) 합산 연간 capex
- 단위: 10억 달러(USD bn)

| 연도    | CapEx ($B) | YoY (%) | 신뢰도                |
| ----- | ---------- | ------- | ------------------ |
| 2020  | ~120       | —       | 근사                 |
| 2021  | ~150       | +25     | 근사                 |
| 2022  | ~160       | +7      | 근사                 |
| 2023  | ~157       | -2      | 파생(2024 −63%)      |
| 2024  | 256        | +63     | 고 (다출처 일치)         |
| 2025  | 443        | +73     | 고                  |
| 2026E | 602        | +36     | 추정 (출처별 $602~805B) |

> 2024–2026 = CreditSights/IEEE ComSoc 컨센서스. 2020–2023 = 여러 차트의 근사. 2026E는 추정이라 출처별 폭이 큼(Apollo $646B · Goldman 4사 $725B · American Century 7사 $775B · Morgan Stanley 5사 $805B).

### 1-2. AI 과열/암흑 합성 지수 (주관)

> ⚠️ **측정 데이터 아님.** 제 시장 판단을 점수로 옮긴 합성 지표. 다른 분석가는 다르게 그림. 매매 신호로 쓰지 말 것.

개념식(가중치는 주관):

```
Index(t) = w1 * z(capex 증가율)
         + w2 * z(밸류에이션 스트레치)     # fwd P/E, EV/EBITDA vs 역사
         + w3 * z(IPO·딜 광기)             # 메가 IPO, 전액주식 M&A 빈도
         + w4 * z(AI ROI 괴리)             # 지출 대비 실현 매출/생산성
         + w5 * z(시장 심리)               # 내러티브 강도
   기준선 = 0 (적정가치) · 위 = 과열 · 아래 = 암흑기
```

| 연도   | 지수  | 근거 요약                                         |
| ---- | --- | --------------------------------------------- |
| 2020 | +3  | COVID 저금리·언택트 랠리 시작                           |
| 2021 | +9  | "모든 것의 버블"(SPAC·밈·크립토·ARKK) — capex 아닌 밸류에이션發 |
| 2022 | -6  | 금리인상 폭락장 = 암흑기                                |
| 2023 | +1  | ChatGPT가 AI 서사 점화, 리셋                         |
| 2024 | +4  | AI capex 변곡, 그러나 과열은 완만                       |
| 2025 | +8  | 광기 정점(사상 최고가, P/E 정점 ~10월)                    |
| 2026 | +5  | 6월 조정·증가율 감속·ROI 의심 — 롤오버                     |

핵심: ①의 capex 증가율과 *달라야* 정상. 2021(버블이나 capex 잠잠), 2024(capex 폭발이나 과열 완만)에서 갈리며 **두 봉우리**(2021 광역 버블 + 2025 AI 버블) 모양이 나옴.

### 1-3. capex ÷ 영업현금흐름 (객관) — 권장 트립와이어

```
CapexToOCF(t) = ( CapEx(t) / OperatingCashFlow(t) ) * 100   [%]
   기준선 = 40%  (10년 역사 평균)
   한계선 = 100% (영업현금 전액 소진 → 초과분은 부채로 조달)
```

- `OperatingCashFlow(t)` = 빅5 합산 연간 영업활동현금흐름

| 연도    | CapEx ($B) | OCF ($B) | 비율 (%) | OCF 비고                |
| ----- | ---------- | -------- | ------ | --------------------- |
| 2020  | ~120       | 262.5    | ~46    | Calcbench 기점          |
| 2021  | ~150       | 294      | ~51    | +12.1%                |
| 2022  | ~160       | 293      | ~55    | −0.3%                 |
| 2023  | ~157       | 395      | ~40    | +34.6% (급반등 → 기준선 복귀) |
| 2024  | 256        | ~490     | ~52    | 보간 추정                 |
| 2025  | 443        | 602.8    | ~73    | Calcbench             |
| 2026E | 602        | ~640     | ~95    | OCF 추정 (가이던스 부재)      |

해석:

- **40% 위** = 평균 이상 투자(과열 방향).
- **100% 접근/돌파** = 버는 영업현금을 거의 다 capex에 투입 → 초과는 **부채**로. 실제 빅5 부채 발행 2025년 $121B(2020–24 연평균 $28B 대비 급증), 2026년 차입 전망 ~$400B(Morgan Stanley).
- **암흑기 신호** = 이 선이 **꺾여 내려갈 때**(capex 삭감 or 시장이 부채 거부). 한 방향 지표라, 하락 전환이 곧 buildout 후퇴 신호.

---

## 2. 데이터 출처

| 출처                        | 제공                        | 갱신  | 신뢰  | URL                                                                                                                                                                |
| ------------------------- | ------------------------- | --- | --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **SEC EDGAR** (원본 filing) | capex, OCF, 매출 (1차)       | 분기  | ★★★ | https://www.sec.gov/cgi-bin/browse-edgar                                                                                                                           |
| CreditSights              | capex 컨센서스·AI비중           | 수시  | ★★★ | (유료/2차 인용)                                                                                                                                                         |
| Calcbench                 | 빅5 capex·OCF 시계열          | 분기  | ★★★ | https://www.calcbench.com/blog/post/blogger9065882471048947069/The-Stunning-Capex-of-AI-Hyperscalers-in-Context                                                    |
| ProCap Insights           | capex/OCF ~100% vs 40% 평균 | 수시  | ★★  | https://www.procapinsights.com/reports/stock-picks-large-cap-free-cash-flow-hyperscaler-capex-ai                                                                   |
| American Century          | capex 시계열·강도              | 수시  | ★★  | https://www.americancentury.com/insights/hyperscaler-ai-capex-spending-cycle/                                                                                      |
| IEEE ComSoc               | $600B+/+36% 컨센서스          | 1회  | ★★  | https://techblog.comsoc.org/2025/12/22/hyperscaler-capex-600-bn-in-2026-a-36-increase-over-2025-while-global-spending-on-cloud-infrastructure-services-skyrockets/ |
| Value Add VC              | 기업별 capex·MSFT 비율>50%     | 수시  | ★★  | https://valueaddvc.com/blog/ai-hyperscaler-capex-compared-why-microsoft-google-meta-and-amazon-are-all-spending-at-once                                            |
| Decoding Discontinuity    | FCF 잠식·부채($121B/$400B)    | 1회  | ★★  | https://www.decodingdiscontinuity.com/p/the-agentic-reckoning-are-hyperscalers-spending-trillions-utility-moats-disappear                                          |
| MUFG                      | 자금조달·레버리지                 | 주간  | ★★  | https://www.mufgamericas.com/sites/default/files/document/2025-12/AI_Chart_Weekly_12_19_Financing_the_AI_Supercycle.pdf                                            |

> **주의 — "AI 매출"은 보고되지 않음.** 기업이 AI 매출을 따로 쪼개지 않아 그 항목은 *추정치*(예: Azure AI 성장률 등 부분 지표만 공개). 그래서 ③의 분모로 *보고되는* OCF를 채택. capex÷AI매출 배수는 분모가 추정이라 객관 지표에서 제외.

---

## 3. 분기 자동 갱신 파이프라인

**솔직한 전제:** filing 기반이라 *진짜 "실시간"이 아니라 분기 갱신*입니다(어닝 시즌마다 새 10-Q/10-K 반영). 진짜 실시간이 필요한 건 *주가* 정도이고, 그건 별도 시세 API가 필요합니다. 아래는 ①③(capex·OCF)을 분기마다 자동으로 끌어와 차트가 다시 그려지게 하는 구성입니다.

### 3-1. 데이터 수집 — SEC EDGAR XBRL API (무료·공식)

엔드포인트:

```
https://data.sec.gov/api/xbrl/companyconcept/CIK{10자리}/us-gaap/{concept}.json
```

| 항목     | us-gaap concept 태그                                    |
| ------ | ----------------------------------------------------- |
| CapEx  | `PaymentsToAcquirePropertyPlantAndEquipment`          |
| 영업현금흐름 | `NetCashProvidedByUsedInOperatingActivities`          |
| 매출     | `RevenueFromContractWithCustomerExcludingAssessedTax` |

CIK 조회(정답 소스): `https://www.sec.gov/files/company_tickers.json`
참고 CIK(반드시 위 파일로 검증): MSFT 0000789019 · AMZN 0001018724 · GOOGL 0001652044 · META 0001326801 · ORCL 0001341439
요청 시 `User-Agent` 헤더 필수(SEC 정책). 호출 간 0.1s 이상 간격 권장.

Python 예제 (capex/OCF 끌어와 합산·비율 → `data.json` 출력):

```python
import requests, json, time

HEADERS = {"User-Agent": "Ann ann@example.com"}  # 본인 연락처로 교체
CIKS = {  # company_tickers.json 으로 검증할 것
    "MSFT": "0000789019", "AMZN": "0001018724", "GOOGL": "0001652044",
    "META": "0001326801", "ORCL": "0001341439",
}
CONCEPTS = {
    "capex": "PaymentsToAcquirePropertyPlantAndEquipment",
    "ocf":   "NetCashProvidedByUsedInOperatingActivities",
}

def fetch_fy(cik, concept):
    """회계연도(FY, 10-K) 연간값을 {연도: 값} 으로 반환"""
    url = f"https://data.sec.gov/api/xbrl/companyconcept/CIK{cik}/us-gaap/{concept}.json"
    r = requests.get(url, headers=HEADERS, timeout=30); r.raise_for_status()
    out = {}
    for it in r.json().get("units", {}).get("USD", []):
        if it.get("form") == "10-K" and it.get("fp") == "FY" and it.get("frame"):
            out[it["fy"]] = it["val"]      # 단위: USD
    return out

agg = {}  # {연도: {"capex":합, "ocf":합}}
for tic, cik in CIKS.items():
    for key, concept in CONCEPTS.items():
        for fy, val in fetch_fy(cik, concept).items():
            agg.setdefault(fy, {}).setdefault(key, 0)
            agg[fy][key] += val
        time.sleep(0.2)

rows = []
for fy in sorted(agg):
    d = agg[fy]
    if "capex" in d and "ocf" in d and d["ocf"]:
        rows.append({
            "year": fy,
            "capex_bn": round(d["capex"]/1e9, 1),
            "ocf_bn":   round(d["ocf"]/1e9, 1),
            "capex_to_ocf_pct": round(d["capex"]/d["ocf"]*100, 1),
        })

json.dump({"updated": time.strftime("%Y-%m-%d"), "rows": rows},
          open("data.json", "w"), ensure_ascii=False, indent=2)
print("saved data.json:", rows)
```

> **회계연도 정렬 주의:** MSFT(6월 결산)·ORCL(5월 결산)은 캘린더연도와 어긋남 → 엄밀히는 분기(10-Q)를 캘린더 기준으로 재합산해야 정확. 위 예제는 FY 기준 근사. 정밀화하려면 `companyconcept`의 분기 프레임(`CY2025Q1` 등)을 합산.

### 3-2. 차트 렌더 — `data.json` 읽어 자동 redraw (HTML + Chart.js)

```html
<!-- index.html : data.json 과 같은 폴더. 갱신되면 새로고침만으로 반영 -->
<canvas id="c" height="360"></canvas>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
<script>
async function draw() {
  const { rows, updated } = await (await fetch('data.json?_=' + Date.now())).json();
  const labels = rows.map(r => r.year);
  new Chart(document.getElementById('c'), {
    type: 'line',
    data: { labels, datasets: [
      { label: 'capex÷OCF %', data: rows.map(r => r.capex_to_ocf_pct),
        borderColor: '#D85A30', borderWidth: 3, tension: .3,
        fill: { target: { value: 40 }, above: 'rgba(216,90,48,.14)' } },
      { label: '기준선 40%', data: labels.map(() => 40),
        borderColor: '#888', borderDash: [6,5], pointRadius: 0 },
      { label: '한계 100%', data: labels.map(() => 100),
        borderColor: '#A32D2D', borderDash: [4,4], pointRadius: 0 },
    ]},
    options: { plugins: { title: { display: true, text: '갱신: ' + updated } },
               scales: { y: { suggestedMax: 110, ticks: { callback: v => v + '%' } } } }
  });
}
draw();
// 자동 주기 갱신을 원하면: setInterval(draw, 1000 * 60 * 60 * 6);  // 6시간마다
</script>
```

### 3-3. 운영 권장

| 작업                             | 주기                                                                      |
| ------------------------------ | ----------------------------------------------------------------------- |
| `fetch.py` 실행 → `data.json` 갱신 | 분기 어닝 직후(대략 1·4·7·10월 말~다음달)                                            |
| 차트 확인(새로고침)                    | 갱신 후 즉시                                                                 |
| **주관 지수(②) 수동 리뷰**             | 분기 1회 — 밸류에이션·딜 광기·ROI를 사람이 재평가                                         |
| 자동화 옵션                         | cron / GitHub Actions 로 `fetch.py` 스케줄 → `data.json` 커밋 → 정적 페이지가 자동 반영 |

`data.json` 스키마:

```json
{
  "updated": "2026-06-23",
  "rows": [
    { "year": 2025, "capex_bn": 443.0, "ocf_bn": 602.8, "capex_to_ocf_pct": 73.5 }
  ]
}
```

---

## 4. 한계 · 주의

- **객관 라인도 수준은 근사.** 4사 vs 5사, 연간 vs 런레이트, 순(net) vs 총(gross) capex에 따라 비율이 흔들림 → *정확한 높이보다 추세*가 신호.
- **주관 지수(②)는 의견.** 객관 근거 없는 점수. 사이클 모양을 보는 용도이지 타이밍 신호가 아님.
- **2026E는 추정.** 가이던스·애널리스트 전망 혼합이라 실제 어닝으로 계속 교체 필요.
- **타이밍 예측 불가.** "꺾이는 중"은 보여도 "언제 0/100%를 깰지"는 누구도 못 맞힘.
- 본 문서는 정보 제공용이며 투자 권유가 아닙니다.







좋아요, 사이트부터 보고 스택에 맞춰 안내할게요.

차트 소스 비교 중 (TradingView vs 네이버 금융)

차트 소스 비교 중 (TradingView vs 네이버 금융)

사이트 잘 봤어요 — "Stock Foresight, Ann × Mir Edition", 49종목·8섹터 추적 대시보드네요. Vercel + Next.js/TypeScript에 `lib/...ts`를 SSOT로 쓰는 구조고, 메뉴에 이미 **차트** 페이지가 있으니 거기가 자연스러운 자리예요. `/chart`에 뭐가 들었는지 잠깐 보고 맞춰 안내할게요.

분석했고 아키텍처 권장사항과 선택지를 도출했다

분석했고 아키텍처 권장사항과 선택지를 도출했다

`/chart` 구조 파악했어요. 지금은 영리하게 — **외부 라이브 차트**를 박아두셨네요. 한국 종목은 네이버 정적 PNG(`ssl.pstatic.net/.../candle/month/005930.png`), 미국 종목은 TradingView 위젯. 둘 다 _URL만 박으면 알아서 최신_이라 "갱신" 고민이 없는 방식이죠.

그런데 이번 과열 지수는 성격이 달라요 — **네이버처럼 알아서 갱신되는 외부 이미지가 없는, 직접 계산하는 첫 커스텀 차트**예요. 그래서 두 가지가 처음으로 필요해요: ① 차트 렌더 라이브러리, ② 작은 데이터 레이어. 핵심 함정 하나 먼저 —

**⚠️ EDGAR는 브라우저에서 직접 못 불러요 (CORS).** `data.sec.gov`는 브라우저 fetch에 CORS 헤더를 안 주고 User-Agent도 요구해서, 클라이언트 코드에 fetch를 박으면 막혀요. _반드시 서버 쪽_(빌드 스크립트 or Vercel 서버리스/cron)에서 받아야 해요.

**추천 구조 (앤의 SSOT 패턴에 맞춤):**

* `lib/overheat.ts` ← 기존 `lib/research-trend.ts`처럼 **SSOT**. capex·OCF·비율·주관지수를 여기 둠.
* `scripts/fetch-capex.ts` ← Node에서 EDGAR 호출(서버라 CORS 무관) → 계산 → `lib/overheat.ts` 재생성. _분기 어닝 후 1회 실행_이면 충분(EDGAR 자체가 분기 갱신이라 진짜 실시간은 의미 없음).
* `/chart`의 새 섹션이 SSOT를 읽어 렌더.
* _자동화 원하면:_ `vercel.json`에 Cron → `app/api/refresh-capex/route.ts`가 EDGAR 받아 갱신(KV/Blob 저장 or ISR revalidate). 하루 1회 cron이면 차고 넘쳐요.

**자리/맥락:** 이건 _개별 종목_이 아니라 _시장 전체_ 게이지라, 사이트가 이미 매기는 "거품 14종 / 고평가 15종"의 **거시 짝꿍**이에요. `/chart` 맨 위 "시장 과열 게이지" 섹션이나 종합 페이지 상단이 잘 어울려요.

이제 코드를 정확히 뽑으려면 두 갈래만 정해주세요 — 그럼 다음 턴에 `lib/overheat.ts` + 컴포넌트 + `fetch-capex.ts`까지 바로 떨궈드릴게요.

질문: 차트 렌더는 뭘로 할까요? (지금 self-render 차트 라이브러리가 없어 새로 추가하는 거예요)답변: [No preference]

질문: 데이터 갱신은 어떻게?답변: [No preference]

생각해보니 저거 만드는 맥북에서 할거야.. 넌 하지 않아도되

방금 만든 사이트 작업은 집 맥북에서 직접 하실 거군요. 알겠어요 — 코드는 안 뽑을게요.

그럼 지금 손에 들고 가실 건 이거예요: 방법론 문서(`AI_overheat_index_methodology.md`)에 수식·데이터 출처·EDGAR 수집 코드·Chart.js 템플릿까지 다 들어가 있으니, 맥북에서 그대로 참고하시면 돼요. 사이트가 Next.js/TS라 그 문서의 Python 수집부는 Node 스크립트로 옮기시거나 그냥 분기마다 한 번 돌려 `lib/overheat.ts` SSOT만 갱신하는 식이면 충분하고요.

딱 하나만 기억하세요 — **EDGAR는 브라우저에서 직접 fetch 못 해요(CORS)**. 서버 스크립트나 빌드 단계에서 받아서 SSOT에 박는 구조로 가셔야 막히지 않아요.

맥북에서 하다가 막히면 그때 가져오세요. 필요하면 그 자리에서 컴포넌트든 스크립트든 맞춰 뽑을게
