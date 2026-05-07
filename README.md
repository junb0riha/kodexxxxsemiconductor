<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SAM ETF Intelligence — 반도체 ETF 보유현황 정밀 분석</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Sans+KR:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500;700&family=Noto+Serif+KR:wght@500;700&display=swap');

:root {
  --bg: #07090f; --bg2: #0c1018;
  --surface: #11151f; --surface2: #161c2a; --surface3: #1c2436;
  --border: #1f2940; --border-strong: #2c3a5a;
  --text: #e5edff; --text-2: #a1adc7; --text-3: #6b7894; --text-dim: #4a5572;
  --accent: #5b8def; --accent-warm: #fbbf24;
  --good: #10b981; --warn: #fbbf24; --bad: #f43f5e;
}
* { margin:0; padding:0; box-sizing:border-box; }
html { scroll-behavior: smooth; }
body {
  background: var(--bg); color: var(--text);
  font-family: 'IBM Plex Sans KR', sans-serif;
  font-size: 13px; line-height: 1.6;
  background-image:
    radial-gradient(ellipse at top left, rgba(91,141,239,0.08) 0%, transparent 40%),
    radial-gradient(ellipse at bottom right, rgba(251,146,60,0.06) 0%, transparent 40%);
  background-attachment: fixed;
}
::-webkit-scrollbar { width: 8px; }
::-webkit-scrollbar-track { background: var(--bg); }
::-webkit-scrollbar-thumb { background: var(--border); border-radius: 4px; }

/* TOPBAR */
.topbar { position: sticky; top:0; z-index:100; background: rgba(7,9,15,0.85); backdrop-filter: blur(20px); border-bottom: 1px solid var(--border); }
.topbar-inner { max-width:1600px; margin:0 auto; padding:16px 32px; display:flex; align-items:center; justify-content:space-between; gap:24px; }
.brand { display:flex; align-items:center; gap:14px; }
.logo { width:36px; height:36px; background:linear-gradient(135deg,#5b8def,#fb923c); border-radius:8px; display:flex; align-items:center; justify-content:center; font-family:'JetBrains Mono',monospace; font-weight:700; font-size:14px; color:#fff; box-shadow:0 4px 16px rgba(91,141,239,0.3); }
.brand-text h1 { font-size:14px; font-weight:600; letter-spacing:-0.2px; }
.brand-text p { font-family:'JetBrains Mono',monospace; font-size:10px; color:var(--text-3); margin-top:1px; letter-spacing:0.5px; }
.nav-links { display:flex; gap:4px; }
.nav-links a { padding:6px 12px; font-size:11px; font-family:'JetBrains Mono',monospace; color:var(--text-3); text-decoration:none; border-radius:4px; transition:all 0.15s; letter-spacing:0.5px; }
.nav-links a:hover { color:var(--text); background:var(--surface2); }
.topbar-meta { display:flex; align-items:center; gap:16px; }
.meta-pill { font-family:'JetBrains Mono',monospace; font-size:10px; color:var(--text-2); letter-spacing:0.6px; }
.meta-pill .dot { display:inline-block; width:6px; height:6px; border-radius:50%; background:var(--good); margin-right:6px; animation:pulse 2s infinite; }
@keyframes pulse { 0%,100%{opacity:1;} 50%{opacity:0.4;} }
.confidential { font-family:'JetBrains Mono',monospace; font-size:10px; letter-spacing:1.5px; color:var(--bad); border:1px solid var(--bad); padding:4px 10px; border-radius:3px; }

/* HERO */
.hero { max-width:1600px; margin:0 auto; padding:80px 32px 60px; }
.hero-tag { display:inline-block; font-family:'JetBrains Mono',monospace; font-size:11px; color:var(--accent); letter-spacing:2px; margin-bottom:24px; padding:6px 14px; border:1px solid rgba(91,141,239,0.3); border-radius:100px; background:rgba(91,141,239,0.05); }
.hero h1 { font-family:'Noto Serif KR',serif; font-size:52px; font-weight:700; line-height:1.18; letter-spacing:-1.2px; margin-bottom:20px; background:linear-gradient(135deg,#fff 0%,#a1adc7 100%); -webkit-background-clip:text; -webkit-text-fill-color:transparent; max-width:1100px; }
.hero h1 em { font-style:italic; background:linear-gradient(135deg,#fbbf24,#fb923c); -webkit-background-clip:text; -webkit-text-fill-color:transparent; }
.hero .lead { font-size:16px; color:var(--text-2); max-width:760px; line-height:1.7; margin-bottom:36px; }
.hero-meta { display:flex; gap:32px; margin-top:48px; padding-top:28px; border-top:1px solid var(--border); flex-wrap:wrap; }
.hero-meta .meta-item { font-family:'JetBrains Mono',monospace; }
.hero-meta .meta-item .l { font-size:10px; color:var(--text-3); letter-spacing:1.2px; text-transform:uppercase; margin-bottom:4px; }
.hero-meta .meta-item .v { font-size:14px; color:var(--text); font-weight:500; }

/* UPCOMING BANNER */
.upcoming-row { margin-top:32px; display:flex; gap:12px; flex-wrap:wrap; }
.upcoming-pill { display:inline-flex; align-items:center; gap:8px; padding:8px 14px; border-radius:100px; font-size:11px; font-family:'JetBrains Mono',monospace; letter-spacing:0.5px; }
.upcoming-pill .dot { width:6px; height:6px; border-radius:50%; }
.upcoming-pill.warm { background:rgba(251,191,36,0.1); border:1px solid rgba(251,191,36,0.3); color:#fcd34d; }
.upcoming-pill.warm .dot { background:#fbbf24; }
.upcoming-pill.orange { background:rgba(251,146,60,0.1); border:1px solid rgba(251,146,60,0.3); color:#fdba74; }
.upcoming-pill.orange .dot { background:#fb923c; }
.upcoming-pill.rose { background:rgba(244,63,94,0.1); border:1px solid rgba(244,63,94,0.3); color:#fda4af; }
.upcoming-pill.rose .dot { background:#f43f5e; }
.upcoming-pill strong { color:#fff; font-weight:600; margin-left:2px; }

/* MAIN */
.container { max-width:1600px; margin:0 auto; padding:0 32px 60px; }

/* KPI */
.kpi-grid { display:grid; grid-template-columns:repeat(5,1fr); gap:14px; margin-bottom:80px; }
.kpi-card { background:linear-gradient(135deg,var(--surface) 0%,var(--surface2) 100%); border:1px solid var(--border); border-radius:10px; padding:20px 22px; position:relative; overflow:hidden; }
.kpi-card::before { content:''; position:absolute; top:0; left:0; width:3px; height:100%; background:var(--kpi-color,var(--accent)); }
.kpi-card .label { font-size:10px; font-family:'JetBrains Mono',monospace; color:var(--text-3); letter-spacing:1px; text-transform:uppercase; margin-bottom:10px; }
.kpi-card .value { font-family:'JetBrains Mono',monospace; font-size:28px; font-weight:700; color:#fff; line-height:1; letter-spacing:-1px; }
.kpi-card .unit { font-size:13px; color:var(--text-3); margin-left:4px; }
.kpi-card .delta { font-size:11px; color:var(--text-2); margin-top:10px; font-family:'JetBrains Mono',monospace; }
.kpi-card .delta strong { color:var(--kpi-color,var(--accent)); font-weight:600; }

/* EXEC SUMMARY */
.exec-summary { background:linear-gradient(135deg,rgba(91,141,239,0.06) 0%,rgba(251,146,60,0.04) 100%); border:1px solid var(--border); border-radius:16px; padding:40px 48px; margin-bottom:80px; position:relative; overflow:hidden; }
.exec-summary::before { content:''; position:absolute; top:0; left:0; right:0; height:1px; background:linear-gradient(90deg,transparent,var(--accent),transparent); }
.exec-summary .ribbon { font-family:'JetBrains Mono',monospace; font-size:10px; letter-spacing:2px; color:var(--accent); margin-bottom:14px; }
.exec-summary h2 { font-family:'Noto Serif KR',serif; font-size:28px; font-weight:700; letter-spacing:-0.8px; margin-bottom:24px; color:#fff; }
.exec-summary p { font-size:14px; color:var(--text-2); line-height:1.85; max-width:1000px; }
.exec-summary p + p { margin-top:14px; }
.exec-summary strong { color:var(--accent-warm); font-weight:600; }
.exec-takeaways { display:grid; grid-template-columns:repeat(3,1fr); gap:20px; margin-top:32px; padding-top:28px; border-top:1px solid var(--border); }
.exec-take { font-size:12px; }
.exec-take .num { font-family:'JetBrains Mono',monospace; font-size:36px; font-weight:700; color:var(--take-color,var(--accent)); line-height:1; margin-bottom:8px; letter-spacing:-1px; }
.exec-take .lab { font-family:'JetBrains Mono',monospace; font-size:10px; color:var(--text-3); letter-spacing:1px; text-transform:uppercase; margin-bottom:6px; }
.exec-take .desc { color:var(--text-2); line-height:1.6; }

/* SECTION */
.section { margin-bottom:64px; scroll-margin-top:80px; }
.section-header { display:flex; align-items:baseline; gap:14px; margin-bottom:24px; padding-bottom:16px; border-bottom:1px solid var(--border); flex-wrap:wrap; }
.section-num { font-family:'JetBrains Mono',monospace; font-size:11px; color:var(--text-dim); letter-spacing:1px; }
.section-title { font-family:'Noto Serif KR',serif; font-size:22px; font-weight:700; letter-spacing:-0.5px; }
.section-desc { font-size:12px; color:var(--text-3); margin-left:auto; }

/* CARDS */
.card { background:var(--surface); border:1px solid var(--border); border-radius:12px; padding:22px; }
.card-head { display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:18px; gap:16px; flex-wrap:wrap; }
.card-head .left h3 { font-size:13px; font-weight:600; color:var(--text); margin-bottom:2px; }
.card-head .left p { font-size:11px; color:var(--text-3); }
.card-head .right { font-family:'JetBrains Mono',monospace; font-size:10px; color:var(--text-dim); letter-spacing:0.5px; }

/* INSIGHT */
.insight-block { margin-top:24px; padding:24px 28px; background:linear-gradient(135deg,var(--surface2) 0%,rgba(91,141,239,0.05) 100%); border-left:3px solid var(--insight-color,var(--accent)); border-radius:0 10px 10px 0; }
.insight-block .label { font-family:'JetBrains Mono',monospace; font-size:9px; letter-spacing:1.5px; color:var(--insight-color,var(--accent)); text-transform:uppercase; margin-bottom:10px; }
.insight-block .label::before { content:'◆ '; }
.insight-block h4 { font-size:16px; font-weight:600; margin-bottom:12px; letter-spacing:-0.2px; color:#fff; }
.insight-block p { font-size:13px; color:var(--text-2); line-height:1.75; }
.insight-block p + p { margin-top:8px; }
.insight-block strong { color:var(--insight-color,var(--accent)); font-weight:600; }
.insight-block .actions { margin-top:16px; padding-top:14px; border-top:1px dashed var(--border); }
.insight-block .actions .a-label { font-family:'JetBrains Mono',monospace; font-size:9px; letter-spacing:1px; color:var(--text-3); margin-bottom:8px; }
.insight-block .actions ul { list-style:none; padding-left:0; }
.insight-block .actions li { padding:5px 0 5px 18px; position:relative; color:var(--text-2); font-size:12px; line-height:1.6; }
.insight-block .actions li::before { content:'→'; position:absolute; left:0; color:var(--insight-color,var(--accent)); font-family:'JetBrains Mono',monospace; }

/* GRID */
.grid-2 { display:grid; grid-template-columns:1fr 1fr; gap:18px; }
.grid-3 { display:grid; grid-template-columns:1fr 1fr 1fr; gap:18px; }
.grid-23 { display:grid; grid-template-columns:2fr 1fr; gap:18px; }

/* FILTER */
.filter-row { display:flex; gap:6px; margin-bottom:14px; flex-wrap:wrap; }
.pill { padding:5px 12px; border-radius:100px; border:1px solid var(--border); background:var(--surface2); font-size:11px; font-family:'JetBrains Mono',monospace; color:var(--text-2); cursor:pointer; transition:all 0.15s; }
.pill:hover { border-color:var(--border-strong); color:var(--text); }
.pill.active { background:var(--accent); border-color:var(--accent); color:#fff; }

/* TABLE */
.tbl { width:100%; border-collapse:collapse; font-size:12px; }
.tbl th { text-align:left; font-family:'JetBrains Mono',monospace; font-weight:500; font-size:10px; color:var(--text-3); letter-spacing:0.5px; padding:8px 10px; border-bottom:1px solid var(--border); text-transform:uppercase; }
.tbl th.num { text-align:right; }
.tbl td { padding:9px 10px; border-bottom:1px solid #0e1320; }
.tbl td.num { text-align:right; font-family:'JetBrains Mono',monospace; color:var(--text); }
.tbl td.rank { font-family:'JetBrains Mono',monospace; color:var(--text-dim); font-size:11px; width:30px; }
.tbl tr:last-child td { border-bottom:none; }
.tbl tr:hover td { background:rgba(91,141,239,0.04); }
.bar-cell { width:120px; padding:0 10px; }
.bar-bg { height:4px; background:var(--surface3); border-radius:100px; overflow:hidden; }
.bar-fill { height:100%; border-radius:100px; }
.tag { display:inline-block; padding:2px 8px; border-radius:100px; font-size:10px; font-family:'JetBrains Mono',monospace; }

/* HEATMAP */
.heatmap { display:grid; grid-template-columns:100px repeat(6,1fr); gap:2px; font-size:10px; font-family:'JetBrains Mono',monospace; }
.heat-h { padding:6px 4px; text-align:center; color:var(--text-3); font-size:9px; letter-spacing:0.3px; }
.heat-h.row { text-align:left; padding-left:8px; display:flex; align-items:center; color:var(--text-2); }
.heat-cell { padding:8px 4px; text-align:center; border-radius:3px; transition:transform 0.15s; color:rgba(255,255,255,0.95); }
.heat-cell:hover { transform:scale(1.06); outline:1px solid rgba(255,255,255,0.3); position:relative; z-index:2; }

/* NARRATIVE */
.narrative { margin:48px 0; padding:32px 40px; background:var(--surface); border-radius:12px; border:1px solid var(--border); border-left:4px solid var(--accent-warm); }
.narrative .lab { font-family:'JetBrains Mono',monospace; font-size:10px; color:var(--accent-warm); letter-spacing:1.5px; margin-bottom:12px; }
.narrative blockquote { font-family:'Noto Serif KR',serif; font-size:22px; font-weight:500; line-height:1.5; letter-spacing:-0.4px; color:#fff; font-style:italic; }
.narrative blockquote::before { content:'"'; color:var(--accent-warm); }
.narrative blockquote::after { content:'"'; color:var(--accent-warm); }
.narrative .src { font-family:'JetBrains Mono',monospace; font-size:10px; color:var(--text-3); margin-top:16px; letter-spacing:0.5px; }

/* STRATEGY */
.strategy-section { margin-top:80px; padding:60px 0; background:linear-gradient(180deg,var(--bg) 0%,rgba(91,141,239,0.03) 100%); border-top:1px solid var(--border); border-bottom:1px solid var(--border); }
.strategy-tag { font-family:'JetBrains Mono',monospace; font-size:11px; letter-spacing:2px; color:var(--accent-warm); margin-bottom:12px; }
.strategy-section h2 { font-family:'Noto Serif KR',serif; font-size:36px; font-weight:700; letter-spacing:-1px; margin-bottom:16px; color:#fff; }
.strategy-section .sub { font-size:14px; color:var(--text-2); max-width:800px; margin-bottom:48px; line-height:1.7; }
.strategy-grid { display:grid; grid-template-columns:repeat(2,1fr); gap:20px; }
.strategy-card { background:var(--surface); border:1px solid var(--border); border-radius:14px; padding:28px; }
.strategy-card .priority { font-family:'JetBrains Mono',monospace; font-size:10px; letter-spacing:1.5px; padding:3px 10px; border-radius:3px; display:inline-block; margin-bottom:16px; }
.strategy-card .priority.high { background:rgba(244,63,94,0.15); color:var(--bad); border:1px solid rgba(244,63,94,0.3); }
.strategy-card .priority.med { background:rgba(251,191,36,0.15); color:var(--warn); border:1px solid rgba(251,191,36,0.3); }
.strategy-card .priority.low { background:rgba(91,141,239,0.15); color:var(--accent); border:1px solid rgba(91,141,239,0.3); }
.strategy-card h3 { font-family:'Noto Serif KR',serif; font-size:20px; font-weight:700; letter-spacing:-0.3px; margin-bottom:12px; color:#fff; }
.strategy-card .opp { font-size:13px; color:var(--text-2); line-height:1.7; margin-bottom:20px; }
.strategy-card .opp strong { color:var(--accent-warm); }
.strategy-card .actions { padding-top:18px; border-top:1px dashed var(--border); }
.strategy-card .actions .a-label { font-family:'JetBrains Mono',monospace; font-size:10px; letter-spacing:1px; color:var(--text-3); margin-bottom:12px; }
.strategy-card .actions ol { list-style:none; padding-left:0; counter-reset:action; }
.strategy-card .actions li { counter-increment:action; padding:8px 0 8px 30px; position:relative; font-size:12px; color:var(--text-2); line-height:1.6; }
.strategy-card .actions li::before { content:counter(action,decimal-leading-zero); position:absolute; left:0; top:8px; font-family:'JetBrains Mono',monospace; font-size:10px; color:var(--accent); font-weight:600; }

/* CHART */
.chart-wrap { position:relative; width:100%; }
.chart-wrap.h-360 { height:360px; }
.chart-wrap.h-400 { height:400px; }
.chart-wrap.h-500 { height:500px; }

/* FOOTER */
footer { margin-top:80px; padding:32px 0; border-top:1px solid var(--border); background:var(--bg2); }
.footer-inner { max-width:1600px; margin:0 auto; padding:0 32px; display:grid; grid-template-columns:2fr 1fr 1fr; gap:40px; }
.footer-brand h4 { font-family:'Noto Serif KR',serif; font-size:16px; margin-bottom:8px; }
.footer-brand p { font-size:11px; color:var(--text-3); line-height:1.6; }
.footer-section h5 { font-family:'JetBrains Mono',monospace; font-size:10px; letter-spacing:1.5px; color:var(--text-3); margin-bottom:14px; text-transform:uppercase; }
.footer-section ul { list-style:none; }
.footer-section li { padding:4px 0; font-family:'JetBrains Mono',monospace; font-size:11px; color:var(--text-2); }
.footer-meta { max-width:1600px; margin:32px auto 0; padding:20px 32px 0; border-top:1px solid var(--border); display:flex; justify-content:space-between; font-family:'JetBrains Mono',monospace; font-size:10px; color:var(--text-dim); flex-wrap:wrap; gap:10px; }

@media (max-width:1200px) {
  .kpi-grid { grid-template-columns:repeat(3,1fr); }
  .grid-2, .grid-23, .strategy-grid { grid-template-columns:1fr; }
  .exec-takeaways { grid-template-columns:1fr; }
  .hero h1 { font-size:36px; }
  .footer-inner { grid-template-columns:1fr; }
}
</style>
</head>
<body>

<!-- TOPBAR -->
<div class="topbar">
  <div class="topbar-inner">
    <div class="brand">
      <div class="logo">SAM</div>
      <div class="brand-text">
        <h1>SAM ETF Intelligence</h1>
        <p>SAMSUNG ASSET MANAGEMENT · ETF PLANNING TEAM</p>
      </div>
    </div>
    <nav class="nav-links">
      <a href="#summary">SUMMARY</a>
      <a href="#products">PRODUCTS</a>
      <a href="#regions">REGIONS</a>
      <a href="#cities">CITIES</a>
      <a href="#strategy">STRATEGY</a>
    </nav>
    <div class="topbar-meta">
      <span class="meta-pill"><span class="dot"></span>2026.05.06</span>
      <span class="confidential">INTERNAL</span>
    </div>
  </div>
</div>

<!-- HERO -->
<div class="hero">
  <span class="hero-tag">◆ INTERNAL ANALYSIS REPORT · v1.0</span>
  <h1>반도체 ETF는 누가, 어디서, 얼마나 사고 있는가 — <em>지역 데이터로 본 점유 지도</em></h1>
  <p class="lead">
    KODEX 3종 · TIGER 3종, 총 6개 반도체 ETF의 전국 보유 현황을 17개 시·도, 230여 시·군·구 단위로 정밀 분해.
    삼성자산운용 ETF 기획팀 시각에서 시장 점유 구조와 지역 침투율, 그리고 다음 한 분기를 위한 실행 전략을 제시합니다.
  </p>
  <div class="hero-meta">
    <div class="meta-item"><div class="l">DATA POINTS</div><div class="v">100+ 시·구 / 6개 ETF</div></div>
    <div class="meta-item"><div class="l">TOTAL AUM</div><div class="v">1조 2,508억원</div></div>
    <div class="meta-item"><div class="l">TOTAL HOLDERS</div><div class="v">175,120명</div></div>
    <div class="meta-item"><div class="l">REPORT DATE</div><div class="v">2026.05.06</div></div>
  </div>

  <div class="upcoming-row">
    <span class="upcoming-pill warm">
      <span class="dot"></span>
      UPCOMING · 2026.05 중 명칭 변경 · <strong>KODEX AI반도체 → KODEX AI반도체TOP2+</strong>
    </span>
    <span class="upcoming-pill orange">
      <span class="dot"></span>
      UPCOMING · 2026.05 신규 출시 · <strong>KODEX 반도체위클리커버드콜</strong>
    </span>
    <span class="upcoming-pill rose">
      <span class="dot"></span>
      UPCOMING · 2026.05말 신규 상장 · <strong>KODEX 삼성전자 레버리지 · KODEX SK하이닉스 레버리지</strong>
    </span>
  </div>
</div>

<div class="container">

  <!-- KPI -->
  <div class="kpi-grid">
    <div class="kpi-card" style="--kpi-color:#5b8def">
      <div class="label">총 보유 평가액</div>
      <div class="value">1,250.8<span class="unit">B</span></div>
      <div class="delta">≒ <strong>1조 2,508억원</strong></div>
    </div>
    <div class="kpi-card" style="--kpi-color:#38bdf8">
      <div class="label">총 보유 인원</div>
      <div class="value">175,120<span class="unit">명</span></div>
      <div class="delta">시·도 17개 · 시·구 230+</div>
    </div>
    <div class="kpi-card" style="--kpi-color:#14b8a6">
      <div class="label">1인당 평균 보유액</div>
      <div class="value">714<span class="unit">만원</span></div>
      <div class="delta">전국 단순 평균</div>
    </div>
    <div class="kpi-card" style="--kpi-color:#fbbf24">
      <div class="label">최대 종목 AUM</div>
      <div class="value">579.6<span class="unit">B</span></div>
      <div class="delta"><strong>TIGER TOP10</strong> · 46.3% 점유</div>
    </div>
    <div class="kpi-card" style="--kpi-color:#f43f5e">
      <div class="label">수도권 집중도</div>
      <div class="value">76.3<span class="unit">%</span></div>
      <div class="delta">서울+경기+인천 / 전국</div>
    </div>
  </div>

  <!-- EXEC SUMMARY -->
  <div class="exec-summary" id="summary">
    <div class="ribbon">◆ EXECUTIVE SUMMARY · 핵심 요약</div>
    <h2>2026년 5월, KODEX 반도체 라인업의 구조적 재편이 시작된다</h2>
    <p>
      이번 5월은 KODEX 반도체 라인업이 단순 보강을 넘어 <strong>구조적으로 재편되는 분기점</strong>입니다.
      ① <strong>KODEX AI반도체 → KODEX AI반도체TOP2+</strong> 명칭 변경으로 "AI 반도체 핵심 2종 + α" 컨셉을 명료화하고,
      ② <strong>KODEX 반도체위클리커버드콜</strong> 신규 출시로 인컴(Income) 라인을 신설하며,
      ③ 5월 말에는 <strong>KODEX 삼성전자 레버리지 · KODEX SK하이닉스 레버리지</strong> 두 종목이 동시 상장하면서 단일 종목 레버리지 시장까지 직접 진입합니다.
    </p>
    <p>
      현재 보유 데이터는 분명한 시그널을 줍니다. <strong>화성·수원·평택·이천 등 반도체 산업 인접 지역</strong>에서 KODEX 반도체 본편 비중이 두드러지게 높습니다 — 이는 곧 삼성전자·SK하이닉스 임직원과 협력사 인력이 거주하는 지역에서 <strong>"개별 종목 직접 노출"에 대한 잠재 수요</strong>가 가장 크다는 의미이며, 5월 말 상장하는 단일 종목 레버리지 2종이 흡수해야 할 1차 모객 풀입니다.
    </p>
    <p>
      따라서 다음 한 분기 과제는 명확합니다.
      <strong>① KODEX AI반도체TOP2+ 리브랜딩을 보유 인원 최다(34,900명) 풀에 정확히 도달시키고</strong>,
      <strong>② 위클리커버드콜로 "성장-핵심-인컴" 라이프사이클 funnel을 완성하며</strong>,
      <strong>③ 삼성전자/하이닉스 레버리지로 반도체 본편의 IT 벨트(화성·수원·이천) 보유자를 단일 종목 레버리지 시장으로 자연 이동시키는 것</strong>입니다.
    </p>

    <div class="exec-takeaways">
      <div class="exec-take" style="--take-color:#fbbf24">
        <div class="num">AI TOP2+</div>
        <div class="lab">5월 명칭 변경 · 가장 두꺼운 보유자 풀</div>
        <div class="desc">KODEX AI반도체(현 116.5B · 34,900명) → AI반도체TOP2+. 보유 인원 기준 KODEX 라인업 1위 — 리브랜딩 임팩트 가장 큰 카드.</div>
      </div>
      <div class="exec-take" style="--take-color:#fb923c">
        <div class="num">위클리CC</div>
        <div class="lab">5월 신규 출시 · 인컴 라인 신설</div>
        <div class="desc">반도체위클리커버드콜로 KODEX 라인업이 "성장-핵심-인컴" 3축 완성. 50대+ 인컴 수요 미충족 시장 직접 공략.</div>
      </div>
      <div class="exec-take" style="--take-color:#f43f5e">
        <div class="num">삼전·하이닉스 LEV</div>
        <div class="lab">5월말 신규 상장 · 단일종목 시장 진입</div>
        <div class="desc">KODEX 삼성전자 레버리지 + KODEX SK하이닉스 레버리지 동시 상장. 화성·수원·이천 IT 벨트 보유자가 1차 타겟 풀.</div>
      </div>
    </div>
  </div>

  <!-- § 1. PRODUCTS -->
  <div class="section" id="products">
    <div class="section-header">
      <span class="section-num">§ 01</span>
      <span class="section-title">종목별 시장 점유 구조</span>
      <span class="section-desc">6종 반도체 ETF의 AUM·인원·1인당 보유금액 비교</span>
    </div>
    <div class="grid-23">
      <div class="card">
        <div class="card-head">
          <div class="left"><h3>종목별 AUM 분포</h3><p>단위: 백만원 · 도넛 차트</p></div>
          <div class="right">TOTAL · 1,250,820M</div>
        </div>
        <div class="chart-wrap h-360"><canvas id="donut"></canvas></div>
      </div>
      <div class="card">
        <div class="card-head">
          <div class="left"><h3>종목 순위 & 1인당 보유금액</h3><p>1인당 평균 보유액</p></div>
        </div>
        <table class="tbl">
          <thead><tr><th>#</th><th>종목</th><th class="num">AUM(B)</th><th class="num">인원</th><th class="num">1인당(만원)</th></tr></thead>
          <tbody id="prodTable"></tbody>
        </table>
      </div>
    </div>

    <div class="insight-block" style="--insight-color:#5b8def">
      <div class="label">SAM Insight · 종목 구조 해석</div>
      <h4>KODEX AI반도체TOP2+ 리브랜딩 — 가장 두꺼운 보유자 풀을 가진 카드</h4>
      <p>
        KODEX AI반도체는 보유 인원 <strong>34,900명</strong>으로 KODEX 라인업 중 가장 많은 보유자를 확보하고 있습니다. 즉 <strong>"진입 장벽이 낮아 신규 유입이 가장 활발한 상품"</strong>입니다.
        5월 중 진행되는 <strong>KODEX AI반도체 → KODEX AI반도체TOP2+</strong> 명칭 변경은 단순 리네이밍이 아니라, "AI 반도체 핵심 2종 + α" 컨셉으로 상품 정체성을 명료화하는 작업이며, 이미 확보된 가장 두꺼운 보유자 풀에 새로운 메시지를 가장 효율적으로 전달할 수 있는 카드입니다.
      </p>
      <p>
        한편 KODEX 반도체(보유 인원 21,740명)는 화성·수원·평택 등 반도체 산업 인접 지역 중심으로 견고한 진지를 형성하고 있습니다.
        이 진지는 5월 신규 출시되는 <strong>KODEX 반도체위클리커버드콜</strong>과 5월 말 상장하는 <strong>삼성전자/SK하이닉스 레버리지 2종</strong>의 1차 모객 풀이 될 가능성이 높습니다 — 한 번 형성된 "반도체 보유 행동"은 인접 신상품 흡수에 결정적 영향을 미치기 때문입니다.
      </p>
      <div class="actions">
        <div class="a-label">▣ 기획팀 ACTION ITEMS</div>
        <ul>
          <li><strong>KODEX AI반도체TOP2+</strong> 명칭 변경 시점에 기존 보유자 34,900명 전원에게 리브랜딩 안내 + "TOP2+" 컨셉 설명 자료 푸시</li>
          <li><strong>KODEX 반도체위클리커버드콜</strong> 출시 시 KODEX 반도체 보유자 21,740명 대상 "성장→인컴 분산" 메시지로 1차 IR</li>
          <li><strong>삼성전자/SK하이닉스 레버리지</strong> 상장 시 KODEX 반도체 보유자의 화성·수원·이천 지역 거주자에 우선 노출 — 이미 반도체 노출 의지가 검증된 풀</li>
          <li>KODEX 라인업 전체를 "성장(레버리지) → 핵심(반도체·AI TOP2+) → 인컴(위클리CC) → 단일종목(삼전·하이닉스 LEV)" 4축 funnel로 시각화한 통합 IR 자료 제작</li>
        </ul>
      </div>
    </div>
  </div>

  <!-- § 2. REGIONS -->
  <div class="section" id="regions">
    <div class="section-header">
      <span class="section-num">§ 02</span>
      <span class="section-title">광역시·도 분포</span>
      <span class="section-desc">17개 시·도별 AUM·인원·1인 평균 종합</span>
    </div>
    <div class="grid-2">
      <div class="card">
        <div class="card-head">
          <div class="left"><h3>광역별 보유 평가액 순위</h3><p>전국 17개 시·도 가로 막대</p></div>
        </div>
        <div class="chart-wrap h-400"><canvas id="regionAmt"></canvas></div>
      </div>
      <div class="card">
        <div class="card-head">
          <div class="left"><h3>광역별 1인당 평균 보유액</h3><p>전국 평균 714만원 기준 비교</p></div>
        </div>
        <div class="chart-wrap h-400"><canvas id="regionPerCap"></canvas></div>
      </div>
    </div>

    <div class="insight-block" style="--insight-color:#fbbf24">
      <div class="label">SAM Insight · 지역 분포 해석</div>
      <h4>"수도권 76% vs 비수도권 24%" — 시장 한계인가, 미개척 기회인가</h4>
      <p>
        서울(41.8%) + 경기(31.2%) + 인천(3.4%) 합산이 <strong>76.3%</strong>. 부산·대구·대전·광주 광역시 4곳을 합쳐도 10.0%에 그칩니다.
        반면 1인당 평균 보유액에서는 <strong>서울 874만원, 경기 740만원, 충남 714만원, 충북 639만원, 대전 605만원</strong>으로 충남·충북·대전이 전국 평균(714만원) 수준 또는 그 이상 — 즉 <strong>지방은 "수가 적을 뿐 고액 소수가 존재"하는 시장</strong>입니다.
      </p>
      <p>
        충남 천안시(22.3B) · 충북 청주시(21.2B) · 경남 창원시(22.2B)는 단일 시 단위로 광주광역시 전체(20.8B)를 능가합니다. 광역 단위 평균값에 가려진 <strong>"지방 거점 도시"의 잠재력</strong>이 확인됩니다.
      </p>
      <div class="actions">
        <div class="a-label">▣ 기획팀 ACTION ITEMS</div>
        <ul>
          <li>천안·청주·창원·전주를 "지방 거점 4대 도시"로 분류, 별도 IR 캠페인 트랙 신설</li>
          <li>광역시별로 일률적 마케팅 대신 시·구 단위 차별화 메시지(대전 유성구·서구는 강남급 대접)</li>
          <li><strong>SK하이닉스 본사가 위치한 청주(충북) · 이천(경기)</strong>는 5월 말 상장 예정인 KODEX SK하이닉스 레버리지의 핵심 거점 — 별도 마케팅 트랙 우선 편성</li>
          <li>제주·강원·전남은 디지털 풀퍼널 채널(MTS 푸시·유튜브 타겟)로 비용효율 진입</li>
        </ul>
      </div>
    </div>
  </div>

  <!-- NARRATIVE -->
  <div class="narrative">
    <div class="lab">◆ FIELD NOTE · 현장 관점</div>
    <blockquote>반도체 ETF 시장은 "전국적 상품"처럼 보이지만, 실제로는 강남 3구·분당·수원·동탄을 잇는 IT 벨트가 절반 이상을 받치고 있다. 이 벨트가 살아있는 한 시장은 안전하지만, 새로운 성장은 이 벨트 바깥에서 만들어야 한다.</blockquote>
    <div class="src">— ETF Planning Team, 내부 검토 메모</div>
  </div>

  <!-- § 3. HEATMAP -->
  <div class="section">
    <div class="section-header">
      <span class="section-num">§ 03</span>
      <span class="section-title">종목 × 광역 매트릭스</span>
      <span class="section-desc">어떤 지역에서 어떤 종목이 강한가</span>
    </div>
    <div class="card">
      <div class="card-head">
        <div class="left"><h3>종목·지역 히트맵</h3><p>각 셀 = 해당 광역의 해당 종목 AUM (백만원). 종목별 상대값으로 색상 강도 결정.</p></div>
        <div class="right">SCALE: 종목내 상대값</div>
      </div>
      <div id="heatmap"></div>
    </div>

    <div class="insight-block" style="--insight-color:#14b8a6">
      <div class="label">SAM Insight · 종목별 지역 강세 패턴</div>
      <h4>KODEX는 IT 벨트, TIGER는 전국 균질 — 종목별 DNA가 다르다</h4>
      <p>
        히트맵을 보면 <strong>KODEX 반도체레버리지는 서울·경기에 95% 이상 집중</strong>되어 있습니다. 비수도권 광역에서 KODEX 레버리지 보유액이 0인 곳이 6개(부산·대구·강원·전남·제주·세종) — 전형적 "수도권 액티브 상품" 분포입니다.
      </p>
      <p>
        반면 TIGER TOP10은 17개 광역 모두에서 의미있는 보유 규모를 보이며, 특히 <strong>강원(70.2%)·전남(82.1%)·제주(95.8%)</strong>에서는 사실상 단일 상품처럼 작동합니다. 비수도권 = TIGER의 무대라는 비대칭 구도가 명확합니다.
      </p>
      <div class="actions">
        <div class="a-label">▣ 기획팀 ACTION ITEMS</div>
        <ul>
          <li>KODEX 레버리지의 비수도권 진입은 단기 무리. 대신 KODEX AI반도체로 비수도권 첫 거점 마련</li>
          <li>"KODEX 반도체 → KODEX AI반도체 → KODEX 레버리지" 단계적 업그레이드 funnel 설계</li>
          <li>TIGER TOP10이 강한 지방 광역에 KODEX 신규 테마 ETF(로보틱스 등) 첫 거점 활용</li>
        </ul>
      </div>
    </div>
  </div>

  <!-- § 4. CITIES -->
  <div class="section" id="cities">
    <div class="section-header">
      <span class="section-num">§ 04</span>
      <span class="section-title">상위 시·구 경쟁구도</span>
      <span class="section-desc">AUM 상위 시·구에서의 6종 점유 비교</span>
    </div>
    <div class="filter-row" id="topNFilter">
      <span class="pill active" data-n="10">TOP 10</span>
      <span class="pill" data-n="15">TOP 15</span>
      <span class="pill" data-n="20">TOP 20</span>
    </div>
    <div class="card">
      <div class="card-head">
        <div class="left"><h3>시·구별 종목 점유 (스택 막대)</h3><p>막대 길이 = 총 AUM, 색상 = 종목별 점유</p></div>
        <div class="right" id="topNLabel">TOP 10</div>
      </div>
      <div class="chart-wrap h-400"><canvas id="cityStack"></canvas></div>
    </div>

    <div class="insight-block" style="--insight-color:#fb923c">
      <div class="label">SAM Insight · 거점 도시 경쟁 양상</div>
      <h4>"강남·서초·성남·수원·화성" — 대한민국 반도체 ETF의 5대 본진</h4>
      <p>
        AUM 상위 5개 시·구의 합계는 약 <strong>3,489억원</strong>으로 전국 AUM의 27.9%. 이 중 4곳(성남·수원·화성·용인)은 경기 남부 IT 벨트입니다.
        흥미로운 건 시·구마다 종목 믹스가 다르다는 점 — <strong>성남시(분당)는 KODEX 레버리지 비중 압도적</strong>, <strong>화성시(동탄)는 KODEX 반도체 본편 강세</strong>, <strong>강남구는 6종 모두 균형 보유</strong>로 가장 다각화되어 있습니다.
      </p>
      <p>
        이는 단순히 "부자 동네"라서가 아니라, <strong>지역 산업 구조와 거주 인구 직군이 종목 선택에 직접 영향</strong>을 준다는 신호입니다. 삼성전자·SK하이닉스 임직원이 많은 화성·수원·이천에서 반도체 본편 ETF 비중이 높은 것은 자연스러운 결과입니다.
      </p>
      <div class="actions">
        <div class="a-label">▣ 기획팀 ACTION ITEMS</div>
        <ul>
          <li>5대 본진(강남·서초·성남·수원·화성)을 "Tier 1 City"로 분류 — <strong>KODEX AI반도체TOP2+ 리브랜딩 안내 1차 노출 권역</strong>으로 활용</li>
          <li>화성·수원·천안 등 반도체 산업 인접 도시는 KODEX AI반도체TOP2+ 컨셉이 가장 잘 통하는 지역 — 산업단지 내 사내방송·게시판 활용 검토</li>
          <li>분당(성남시)는 1인당 보유금액 가장 높은 거점 — <strong>KODEX 반도체위클리커버드콜 출시 시 프리미엄 인컴 상품으로 우선 노출</strong></li>
        </ul>
      </div>
    </div>
  </div>

  <!-- § 5. SCATTER -->
  <div class="section">
    <div class="section-header">
      <span class="section-num">§ 05</span>
      <span class="section-title">고액 vs 다수 — 시·구 산점도</span>
      <span class="section-desc">x: 인원수, y: AUM, 점 크기: 1인당 보유금액</span>
    </div>
    <div class="card">
      <div class="card-head">
        <div class="left"><h3>시·구 분포 (전체)</h3><p>오른쪽 상단 = 고액·다수 / 왼쪽 위 = 소수 고액 / 오른쪽 아래 = 다수 소액</p></div>
        <div class="right" id="scatterCount"></div>
      </div>
      <div class="chart-wrap h-500"><canvas id="scatter"></canvas></div>
      <div id="scatterLegend"></div>
    </div>

    <div class="insight-block" style="--insight-color:#5b8def">
      <div class="label">SAM Insight · 4사분면 분류</div>
      <h4>4가지 시장 유형 — 차별화된 접근이 필요한 이유</h4>
      <p>
        <strong>① 고액·다수 (Q1: 강남·서초·성남·수원)</strong> — 가장 두꺼운 본진. 신규 상품 출시 시 1차 노출 필수.<br>
        <strong>② 소수·고액 (Q2: 분당 일부, 서초)</strong> — 1인당 보유금액이 매우 높지만 인원 절대 수 적음. 프라이빗 IR로 효율적 접근.<br>
        <strong>③ 다수·중액 (Q3: 부평구·구로구·관악구)</strong> — 인원은 많지만 1인당 보유금액 낮음. 디지털 채널과 자동투자 캠페인이 효과적.<br>
        <strong>④ 소수·소액 (Q4: 비수도권 군 단위)</strong> — 점유율 미미하지만 향후 광역 단위 캠페인 노출 시 가성비 평가 영역.
      </p>
      <div class="actions">
        <div class="a-label">▣ 기획팀 ACTION ITEMS</div>
        <ul>
          <li>4사분면 매핑 결과를 마케팅 부서·영업 채널과 공유, 시·구 단위 예산 배분 기준으로 활용</li>
          <li>Q1 본진 도시 6곳에 "신상품 베타 테스트 시범 노출" 권한 부여</li>
          <li>Q3 다수·중액 지역은 적립식·자동투자 상품과 묶어 1인당 보유금액 끌어올리기</li>
        </ul>
      </div>
    </div>
  </div>

  <!-- § 6. PER-CAPITA -->
  <div class="section">
    <div class="section-header">
      <span class="section-num">§ 06</span>
      <span class="section-title">1인당 보유금액 양극단</span>
      <span class="section-desc">시·구 단위 1인당 보유액 — TOP 15 vs BOTTOM 15</span>
    </div>
    <div class="grid-2">
      <div class="card">
        <div class="card-head"><div class="left"><h3 style="color:#fcd34d">▲ TOP 15 — 1인당 보유금액 상위</h3><p>고액 투자자 밀집 지역</p></div></div>
        <table class="tbl">
          <thead><tr><th>#</th><th>시·구</th><th>광역</th><th class="num">1인당(만원)</th><th></th></tr></thead>
          <tbody id="topPerCap"></tbody>
        </table>
      </div>
      <div class="card">
        <div class="card-head"><div class="left"><h3 style="color:#fda4af">▼ BOTTOM 15 — 1인당 보유금액 하위</h3><p>대중 투자자 분포 지역</p></div></div>
        <table class="tbl">
          <thead><tr><th>#</th><th>시·구</th><th>광역</th><th class="num">1인당(만원)</th><th></th></tr></thead>
          <tbody id="botPerCap"></tbody>
        </table>
      </div>
    </div>

    <div class="insight-block" style="--insight-color:#fbbf24">
      <div class="label">SAM Insight · 1인당 보유금액 격차</div>
      <h4>1인당 보유금액 1위와 꼴찌의 차이는 약 5배 — 그 격차가 곧 기회다</h4>
      <p>
        TOP 15 평균 1인당 보유금액은 <strong>약 1,000만원대</strong>, BOTTOM 15 평균은 <strong>약 200만원대</strong>. 같은 ETF 시장 안에서 5배 가까운 보유금액 격차가 존재합니다.
        BOTTOM 권역은 "관심은 있으나 첫 매수 규모가 작은" 초입 투자자가 다수 — <strong>적립식·소액 자동투자 상품과 결합하면 자연스러운 1인당 보유금액 상승</strong>을 유도할 수 있습니다.
      </p>
      <div class="actions">
        <div class="a-label">▣ 기획팀 ACTION ITEMS</div>
        <ul>
          <li>BOTTOM 권역 대상 "월 10만원 자동투자" 캠페인으로 진입 장벽 낮추기</li>
          <li>TOP 권역에는 신상품 첫 배포 시 "프리미엄 얼리액세스" 컨셉 활용</li>
        </ul>
      </div>
    </div>
  </div>

  <!-- § 7. SEOUL & GG -->
  <div class="section">
    <div class="section-header">
      <span class="section-num">§ 07</span>
      <span class="section-title">수도권 심층 분석</span>
      <span class="section-desc">서울 25개 구 + 경기 31개 시·군 종목별 분포</span>
    </div>
    <div class="grid-2">
      <div class="card">
        <div class="card-head">
          <div class="left"><h3>서울 25개 구 — AUM 순위</h3><p>전체 522.6B 중 분포</p></div>
          <div class="right">서울 = 전국 41.8%</div>
        </div>
        <div class="chart-wrap h-500"><canvas id="seoulBar"></canvas></div>
      </div>
      <div class="card">
        <div class="card-head">
          <div class="left"><h3>경기 시·군 — AUM 순위 (TOP 15)</h3><p>전체 390.2B 중 분포</p></div>
          <div class="right">경기 = 전국 31.2%</div>
        </div>
        <div class="chart-wrap h-500"><canvas id="ggBar"></canvas></div>
      </div>
    </div>

    <div class="insight-block" style="--insight-color:#fb923c">
      <div class="label">SAM Insight · 수도권 내부 균열</div>
      <h4>같은 "수도권"이라도 강북과 강남, 북부와 남부는 완전히 다른 시장</h4>
      <p>
        서울 내부에서도 강남구(96B)와 도봉구(2.8B)는 <strong>AUM 격차가 34배</strong>. 경기 내부에서도 성남(80.7B)과 포천(0.05B) 차이가 1,600배가 넘습니다.
        "수도권 마케팅"이라는 단일 카테고리는 더 이상 유효하지 않으며, <strong>구·시 단위로 잘게 쪼갠 마이크로 타겟팅</strong>이 필요합니다.
      </p>
    </div>
  </div>
</div>

<!-- STRATEGY -->
<div class="strategy-section" id="strategy">
  <div class="container">
    <div class="strategy-tag">◆ STRATEGY · 다음 한 분기 실행 과제</div>
    <h2>데이터 기반 5대 전략 과제</h2>
    <p class="sub">위 분석을 바탕으로 ETF 기획팀이 즉시 실행 가능한 5가지 우선순위 과제를 도출했습니다. 각 과제는 우선순위·근거 데이터·구체 액션으로 구성됩니다.</p>

    <div class="strategy-grid">
      <div class="strategy-card">
        <span class="priority high">PRIORITY · HIGH</span>
        <h3>① KODEX AI반도체TOP2+ 리브랜딩 임팩트 극대화</h3>
        <p class="opp">5월 명칭 변경되는 <strong>KODEX AI반도체TOP2+</strong>는 KODEX 라인업 중 보유 인원 최다(34,900명) 상품. 리브랜딩 시점에 기존 보유자 전원 알림 + 신규 유입 캠페인을 동시 가동하면 <strong>"AI 핵심 2종 + α"</strong> 컨셉이 가장 두꺼운 메시지 도달률을 확보합니다.</p>
        <div class="actions">
          <div class="a-label">▣ 실행 단계</div>
          <ol>
            <li>명칭 변경 D-day에 기존 보유자 34,900명 전원 MTS·이메일 알림 발송 ("TOP2+ 컨셉이란 무엇인가" 인포그래픽 동봉)</li>
            <li>천안·화성·수원 등 기존 KODEX AI반도체 강세 지역 영업 채널에 리브랜딩 IR 자료 우선 배포</li>
            <li>리브랜딩 후 4주간 신규 매수 인원·AUM 증가율 추적, 전월 대비 +15% 이상을 KPI로 설정</li>
          </ol>
        </div>
      </div>

      <div class="strategy-card">
        <span class="priority high">PRIORITY · HIGH</span>
        <h3>② KODEX 반도체위클리커버드콜 — 인컴 라인 신설 거점화</h3>
        <p class="opp">5월 신규 출시되는 <strong>KODEX 반도체위클리커버드콜</strong>은 KODEX 라인업 최초의 본격 인컴형 상품. 기존 KODEX 반도체 보유자(21,740명) 중 <strong>50대+ 자산 안정화 니즈를 가진 풀</strong>을 1차 타겟으로 설정해야 모객 효율이 극대화됩니다.</p>
        <div class="actions">
          <div class="a-label">▣ 실행 단계</div>
          <ol>
            <li>KODEX 반도체·KODEX 레버리지 보유자 중 50대+ 추정 세그먼트 추출, "성장→인컴 분산" 메시지로 사전 안내</li>
            <li>대전 유성구·경남 창원시·충북 청주시 등 1인당 보유금액 평균 이상 지방 거점에 인컴형 IR 자료 별도 제작 배포</li>
            <li>출시 후 1개월 내 "기존 KODEX 보유자 → 위클리CC 동시 보유" 비율을 핵심 KPI로 설정 (목표: 20% 이상)</li>
          </ol>
        </div>
      </div>

      <div class="strategy-card">
        <span class="priority high">PRIORITY · HIGH</span>
        <h3>③ 삼성전자 · SK하이닉스 레버리지 — IT 벨트 정조준</h3>
        <p class="opp">5월 말 동시 상장하는 <strong>KODEX 삼성전자 레버리지 · KODEX SK하이닉스 레버리지</strong>는 ETF가 아닌 <strong>"단일 종목"에 대한 직접 노출 의지</strong>를 가진 보유자가 타겟. 데이터상 KODEX 반도체 본편 강세 지역인 <strong>화성·수원·이천·평택·청주(SK하이닉스 본사)</strong>가 1차 모객 풀이며, 이미 반도체 노출 의지가 검증된 풀이라는 점에서 신상품 흡수율이 가장 높을 것으로 예상됩니다.</p>
        <div class="actions">
          <div class="a-label">▣ 실행 단계</div>
          <ol>
            <li>KODEX 반도체 보유자 21,740명 중 화성·수원·이천·평택·청주 거주 세그먼트 추출, 상장 D-7부터 "단일종목 레버리지로 더 직접 노출" 메시지 발송</li>
            <li>삼성전자 레버리지는 화성·수원(삼성전자 캠퍼스 인접), SK하이닉스 레버리지는 이천·청주(SK하이닉스 본사 인접) 우선 노출 — 종목별 지역 매칭 IR</li>
            <li>두 종목 동시 상장 효과를 활용해 "반도체 양대 축 동시 베팅" 컨셉의 묶음 IR 자료 제작</li>
            <li>상장 후 1개월 내 두 종목 합산 AUM 100B 돌파를 KPI로 설정</li>
          </ol>
        </div>
      </div>

      <div class="strategy-card">
        <span class="priority med">PRIORITY · MEDIUM</span>
        <h3>④ KODEX 4축 funnel — 라이프사이클 전 구간 커버</h3>
        <p class="opp">5월 라인업 정비 후 KODEX는 <strong>"성장(레버리지) → 핵심(반도체·AI TOP2+) → 인컴(위클리CC) → 단일종목(삼전·하이닉스 LEV)"</strong> 4축 구조 완성. 보유자 연령·성향에 따른 자연 이동 경로를 명문화하면 각 보유자의 라이프사이클 전 구간을 KODEX 안에서 커버할 수 있습니다.</p>
        <div class="actions">
          <div class="a-label">▣ 실행 단계</div>
          <ol>
            <li>4축 funnel 비주얼을 단일 인포그래픽으로 통합, 모든 KODEX IR 자료에 공통 삽입</li>
            <li>각 단계별 보유자 행동 패턴 분석 후 다음 상품 추천 알고리즘 내재화 (MTS 추천 모듈 연동)</li>
            <li>4축 모두 보유한 고객에게 "KODEX 컬렉터" 등 로열티 프로그램 검토</li>
          </ol>
        </div>
      </div>

      <div class="strategy-card">
        <span class="priority low">PRIORITY · LOW (LONG-TERM)</span>
        <h3>⑤ 비수도권 디지털 풀퍼널 채널 구축</h3>
        <p class="opp">제주·강원·전남 합산이 전국의 <strong>2.0%</strong>. 오프라인 영업으로는 비용효율 확보가 어려운 만큼 <strong>MTS 푸시·유튜브·네이버 타겟 광고</strong>를 활용한 디지털 풀퍼널이 현실적입니다. 신규 위클리CC는 인컴 컨셉이 디지털 광고에서 메시지 전달력이 좋은 상품이라 비수도권 첫 거점 상품으로 적합합니다.</p>
        <div class="actions">
          <div class="a-label">▣ 실행 단계</div>
          <ol>
            <li>지방 광역 거주자 대상 MTS 내 ETF 메뉴 노출 우선순위 조정 — 위클리CC 신상품을 첫 화면 노출</li>
            <li>지역별 인구통계(연령·소득)에 맞춘 디지털 광고 크리에이티브 분리 제작</li>
            <li>1년 후 지방 광역(제주·강원·전남) 합산 AUM 비중 3% 돌파를 중기 KPI로 설정</li>
          </ol>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- FOOTER -->
<footer>
  <div class="footer-inner">
    <div class="footer-brand">
      <h4>SAM ETF Intelligence</h4>
      <p>삼성자산운용 ETF 기획팀의 내부 분석 도구. 전국 시·도·시·군·구 단위 데이터 기반의 정밀 시장 분석 및 실행 전략 제시.</p>
    </div>
    <div class="footer-section">
      <h5>SECTIONS</h5>
      <ul>
        <li>§01 PRODUCTS</li>
        <li>§02 REGIONS</li>
        <li>§03 HEATMAP</li>
        <li>§04 CITIES</li>
        <li>§05 SCATTER</li>
        <li>§06 PER-CAPITA</li>
        <li>§07 SEOUL/GG</li>
      </ul>
    </div>
    <div class="footer-section">
      <h5>METADATA</h5>
      <ul>
        <li>VERSION · 1.0</li>
        <li>DATE · 2026.05.06</li>
        <li>OWNER · ETF PLANNING</li>
        <li>STATUS · CONFIDENTIAL</li>
      </ul>
    </div>
  </div>
  <div class="footer-meta">
    <span>SOURCE: 삼성자산운용 내부 집계 · 6개 반도체 ETF 통합 데이터</span>
    <span>CONFIDENTIAL · DO NOT DISTRIBUTE</span>
  </div>
</footer>

<script>// ═══════ DATA ═══════
const PRODUCTS = [
  { key:'kodex', name:'KODEX 반도체',     aum:250994, cnt:21740, color:'#5b8def', tag:'tag-blue' },
  { key:'klev',  name:'KODEX 레버리지',   aum:143766, cnt:3510,  color:'#38bdf8', tag:'tag-cyan' },
  { key:'kai',   name:'KODEX AI반도체 → TOP2+',   aum:116547, cnt:34900, color:'#14b8a6', tag:'tag-teal' },
  { key:'tiger', name:'TIGER 반도체',     aum:23786,  cnt:1750,  color:'#fbbf24', tag:'tag-amber' },
  { key:'ttop',  name:'TIGER TOP10',     aum:579562, cnt:109550,color:'#fb923c', tag:'tag-orange' },
  { key:'tlev',  name:'TIGER 레버리지',   aum:136165, cnt:3670,  color:'#f43f5e', tag:'tag-rose' },
];
const TOTAL_AUM = 1250820;

const REGIONS = [
  { name:"서울", amt:522648, cnt:59770 }, { name:"경기", amt:390240, cnt:52720 },
  { name:"인천", amt:42615, cnt:8680 },   { name:"부산", amt:39804, cnt:7600 },
  { name:"경남", amt:37665, cnt:6790 },   { name:"대전", amt:34735, cnt:5740 },
  { name:"대구", amt:31173, cnt:5750 },   { name:"충남", amt:29071, cnt:4070 },
  { name:"충북", amt:22549, cnt:3530 },   { name:"광주", amt:20822, cnt:4640 },
  { name:"전북", amt:19444, cnt:3710 },   { name:"경북", amt:18457, cnt:3420 },
  { name:"울산", amt:14999, cnt:2670 },   { name:"전남", amt:13312, cnt:2630 },
  { name:"강원", amt:9831, cnt:2590 },    { name:"제주", amt:1887, cnt:530 },
  { name:"세종", amt:1568, cnt:280 },
];

const REGION_BY_PROD = {
  "서울":[113834,74427,43251,10218,215457,65462],"경기":[81476,48057,36932,11704,167397,44674],
  "인천":[8390,3483,4651,0,23552,2539],"부산":[5889,0,4398,0,26646,2872],
  "경남":[6459,3374,4205,192,19310,4126],"대전":[6598,4078,3984,190,16372,3512],
  "대구":[6710,264,2580,0,18903,2716],"충남":[5168,2757,2636,849,15357,2304],
  "충북":[3218,2347,1890,633,9225,5237],"광주":[3125,1273,2248,0,13928,249],
  "전북":[3135,1942,2171,0,10777,1419],"경북":[2609,949,2973,0,11644,281],
  "울산":[1838,816,1521,0,10051,774],"전남":[804,0,1574,0,10935,0],
  "강원":[1743,0,1189,0,6900,0],"제주":[0,0,80,0,1807,0],"세종":[0,0,266,0,1302,0],
};

const CITIES = [
  {nm:"강남구",rg:"서울",amt:96054,cnt:8650,prods:[16850,15791,6953,3358,41668,11434]},
  {nm:"서초구",rg:"서울",amt:60057,cnt:4830,prods:[16670,9573,5392,1806,18610,8006]},
  {nm:"영등포구",rg:"서울",amt:57532,cnt:4390,prods:[8843,11113,3319,1296,18184,14777]},
  {nm:"송파구",rg:"서울",amt:54759,cnt:4880,prods:[9616,8731,4369,1475,22414,8154]},
  {nm:"마포구",rg:"서울",amt:25024,cnt:2800,prods:[5329,2629,2201,392,9166,5307]},
  {nm:"강서구",rg:"서울",amt:24170,cnt:3230,prods:[6542,5690,1714,0,7881,2343]},
  {nm:"중구",rg:"서울",amt:22238,cnt:2750,prods:[4664,4406,2304,148,8143,2553]},
  {nm:"강동구",rg:"서울",amt:19935,cnt:2350,prods:[4266,5286,1452,412,7125,1394]},
  {nm:"양천구",rg:"서울",amt:18117,cnt:4390,prods:[4851,1063,1566,0,7973,2665]},
  {nm:"종로구",rg:"서울",amt:16350,cnt:1890,prods:[3282,2101,814,268,7220,2666]},
  {nm:"노원구",rg:"서울",amt:15170,cnt:2300,prods:[4164,1182,1592,127,6790,1315]},
  {nm:"용산구",rg:"서울",amt:13118,cnt:1310,prods:[3565,0,805,0,8747,0]},
  {nm:"구로구",rg:"서울",amt:12282,cnt:2120,prods:[2564,1670,932,325,6458,332]},
  {nm:"동작구",rg:"서울",amt:12251,cnt:1670,prods:[3632,441,1429,240,4917,1593]},
  {nm:"서대문구",rg:"서울",amt:10705,cnt:1590,prods:[1679,1545,1476,0,5080,925]},
  {nm:"광진구",rg:"서울",amt:10284,cnt:1550,prods:[3216,1044,591,0,5159,275]},
  {nm:"성동구",rg:"서울",amt:9813,cnt:1650,prods:[3161,741,775,0,4590,546]},
  {nm:"성북구",rg:"서울",amt:8507,cnt:1570,prods:[1910,488,796,370,4199,745]},
  {nm:"관악구",rg:"서울",amt:7701,cnt:1660,prods:[2063,408,1102,0,4128,0]},
  {nm:"은평구",rg:"서울",amt:6774,cnt:1390,prods:[1693,0,913,0,3734,435]},
  {nm:"동대문구",rg:"서울",amt:6438,cnt:1500,prods:[1927,0,892,0,3620,0]},
  {nm:"금천구",rg:"서울",amt:5270,cnt:1200,prods:[1495,528,666,0,2582,0]},
  {nm:"중랑구",rg:"서울",amt:4021,cnt:910,prods:[808,0,480,0,2734,0]},
  {nm:"강북구",rg:"서울",amt:3304,cnt:740,prods:[347,0,322,0,2635,0]},
  {nm:"도봉구",rg:"서울",amt:2774,cnt:730,prods:[678,0,399,0,1698,0]},
  {nm:"성남시",rg:"경기",amt:80674,cnt:7310,prods:[15298,16348,5890,3118,29681,10338]},
  {nm:"수원시",rg:"경기",amt:64890,cnt:7110,prods:[17073,6557,6762,3190,25643,5666]},
  {nm:"용인시",rg:"경기",amt:49404,cnt:5430,prods:[9630,7145,4044,1916,20521,6149]},
  {nm:"화성시",rg:"경기",amt:46497,cnt:4690,prods:[13071,6211,3911,1775,16310,5218]},
  {nm:"고양시",rg:"경기",amt:38575,cnt:5070,prods:[6354,4864,4027,1398,15826,6106]},
  {nm:"안양시",rg:"경기",amt:22368,cnt:2870,prods:[3924,1762,2057,155,8662,5808]},
  {nm:"부천시",rg:"경기",amt:13806,cnt:2560,prods:[2877,1397,1440,153,5825,2115]},
  {nm:"평택시",rg:"경기",amt:8986,cnt:1830,prods:[1886,1743,822,0,4041,494]},
  {nm:"김포시",rg:"경기",amt:6756,cnt:1430,prods:[1141,358,757,0,3270,1230]},
  {nm:"하남시",rg:"경기",amt:6385,cnt:1210,prods:[2206,0,551,0,3194,435]},
  {nm:"파주시",rg:"경기",amt:6045,cnt:1260,prods:[1195,1190,414,0,2977,271]},
  {nm:"남양주시",rg:"경기",amt:6028,cnt:1560,prods:[909,0,1051,0,3603,465]},
  {nm:"안산시",rg:"경기",amt:5980,cnt:1910,prods:[951,173,641,0,4215,0]},
  {nm:"광명시",rg:"경기",amt:5058,cnt:1070,prods:[1180,0,822,0,3056,0]},
  {nm:"시흥시",rg:"경기",amt:4930,cnt:1530,prods:[1271,0,776,0,2883,0]},
  {nm:"의정부시",rg:"경기",amt:4849,cnt:1410,prods:[946,309,701,0,2513,381]},
  {nm:"군포시",rg:"경기",amt:4671,cnt:890,prods:[594,0,564,0,3513,0]},
  {nm:"오산시",rg:"경기",amt:2667,cnt:590,prods:[121,0,504,0,2041,0]},
  {nm:"구리시",rg:"경기",amt:3032,cnt:700,prods:[850,0,439,0,1743,0]},
  {nm:"이천시",rg:"경기",amt:2025,cnt:470,prods:[0,0,263,0,1762,0]},
  {nm:"의왕시",rg:"경기",amt:1594,cnt:370,prods:[0,0,106,0,1488,0]},
  {nm:"과천시",rg:"경기",amt:1424,cnt:230,prods:[0,0,67,0,1358,0]},
  {nm:"안성시",rg:"경기",amt:817,cnt:100,prods:[0,0,0,0,817,0]},
  {nm:"양주시",rg:"경기",amt:713,cnt:420,prods:[0,0,93,0,621,0]},
  {nm:"여주시",rg:"경기",amt:459,cnt:120,prods:[0,0,65,0,394,0]},
  {nm:"연수구",rg:"인천",amt:11850,cnt:1870,prods:[3158,446,1129,0,6327,791]},
  {nm:"인천서구",rg:"인천",amt:10856,cnt:2090,prods:[2131,1907,1390,0,4251,1176]},
  {nm:"남동구",rg:"인천",amt:8655,cnt:1520,prods:[933,1130,729,0,5291,572]},
  {nm:"부평구",rg:"인천",amt:3838,cnt:1150,prods:[821,0,470,0,2547,0]},
  {nm:"인천중구",rg:"인천",amt:3225,cnt:680,prods:[727,0,373,0,2124,0]},
  {nm:"미추홀구",rg:"인천",amt:2493,cnt:850,prods:[477,0,327,0,1690,0]},
  {nm:"계양구",rg:"인천",amt:1699,cnt:520,prods:[142,0,234,0,1323,0]},
  {nm:"해운대구",rg:"부산",amt:11715,cnt:1410,prods:[2957,0,986,0,4900,2872]},
  {nm:"부산진구",rg:"부산",amt:4817,cnt:1260,prods:[623,0,488,0,3706,0]},
  {nm:"동래구",rg:"부산",amt:3521,cnt:650,prods:[482,0,469,0,2570,0]},
  {nm:"연제구",rg:"부산",amt:3400,cnt:590,prods:[513,0,255,0,2632,0]},
  {nm:"부산남구",rg:"부산",amt:3022,cnt:650,prods:[343,0,385,0,2295,0]},
  {nm:"금정구",rg:"부산",amt:2576,cnt:540,prods:[460,0,370,0,1745,0]},
  {nm:"부산북구",rg:"부산",amt:2474,cnt:550,prods:[243,0,484,0,1747,0]},
  {nm:"부산강서구",rg:"부산",amt:2442,cnt:530,prods:[268,0,459,0,1716,0]},
  {nm:"수영구",rg:"부산",amt:2199,cnt:330,prods:[0,0,293,0,1906,0]},
  {nm:"사하구",rg:"부산",amt:1777,cnt:470,prods:[0,0,153,0,1624,0]},
  {nm:"유성구",rg:"대전",amt:15690,cnt:2070,prods:[3147,2811,1522,190,6527,1493]},
  {nm:"대전서구",rg:"대전",amt:14952,cnt:2360,prods:[3045,1268,1782,0,6839,2019]},
  {nm:"대전중구",rg:"대전",amt:2124,cnt:600,prods:[406,0,411,0,1308,0]},
  {nm:"대덕구",rg:"대전",amt:1190,cnt:350,prods:[0,0,231,0,959,0]},
  {nm:"수성구",rg:"대구",amt:11100,cnt:1430,prods:[2982,264,922,0,6281,651]},
  {nm:"달서구",rg:"대구",amt:8229,cnt:1530,prods:[1461,0,586,0,4764,1418]},
  {nm:"대구북구",rg:"대구",amt:5004,cnt:1150,prods:[1143,0,332,0,2882,648]},
  {nm:"대구동구",rg:"대구",amt:3363,cnt:840,prods:[739,0,561,0,2064,0]},
  {nm:"대구중구",rg:"대구",amt:2186,cnt:400,prods:[386,0,92,0,1709,0]},
  {nm:"광주서구",rg:"광주",amt:6470,cnt:1240,prods:[1174,139,891,0,4018,249]},
  {nm:"광산구",rg:"광주",amt:5248,cnt:1300,prods:[573,1135,522,0,3018,0]},
  {nm:"광주북구",rg:"광주",amt:4879,cnt:1280,prods:[1028,0,573,0,3279,0]},
  {nm:"광주남구",rg:"광주",amt:3072,cnt:550,prods:[350,0,250,0,2472,0]},
  {nm:"창원시",rg:"경남",amt:22181,cnt:3420,prods:[3525,3374,2033,192,8931,4126]},
  {nm:"김해시",rg:"경남",amt:5090,cnt:1170,prods:[1243,0,815,0,3032,0]},
  {nm:"진주시",rg:"경남",amt:4327,cnt:1040,prods:[599,0,534,0,3195,0]},
  {nm:"거제시",rg:"경남",amt:3430,cnt:560,prods:[875,0,479,0,2075,0]},
  {nm:"양산시",rg:"경남",amt:1800,cnt:470,prods:[217,0,266,0,1317,0]},
  {nm:"천안시",rg:"충남",amt:22330,cnt:2890,prods:[4187,2757,2331,849,9903,2304]},
  {nm:"아산시",rg:"충남",amt:3733,cnt:530,prods:[981,0,234,0,2518,0]},
  {nm:"청주시",rg:"충북",amt:21202,cnt:3100,prods:[3218,2347,1777,633,7991,5237]},
  {nm:"전주시",rg:"전북",amt:14374,cnt:2600,prods:[2680,1942,1589,0,6745,1419]},
  {nm:"익산시",rg:"전북",amt:2340,cnt:570,prods:[175,0,410,0,1755,0]},
  {nm:"군산시",rg:"전북",amt:1555,cnt:460,prods:[281,0,172,0,1102,0]},
  {nm:"김천시",rg:"경북",amt:8099,cnt:1240,prods:[1228,703,1739,0,4146,281]},
  {nm:"포항시",rg:"경북",amt:6712,cnt:1300,prods:[1381,246,893,0,4192,0]},
  {nm:"경산시",rg:"경북",amt:1226,cnt:300,prods:[0,0,250,0,976,0]},
  {nm:"구미시",rg:"경북",amt:1167,cnt:240,prods:[0,0,30,0,1137,0]},
  {nm:"울산남구",rg:"울산",amt:9858,cnt:1590,prods:[1616,816,874,0,5779,774]},
  {nm:"울산북구",rg:"울산",amt:2041,cnt:430,prods:[222,0,370,0,1449,0]},
  {nm:"울산중구",rg:"울산",amt:1713,cnt:380,prods:[0,0,137,0,1576,0]},
  {nm:"울산동구",rg:"울산",amt:1388,cnt:270,prods:[0,0,141,0,1248,0]},
  {nm:"순천시",rg:"전남",amt:5166,cnt:800,prods:[216,0,596,0,4353,0]},
  {nm:"여수시",rg:"전남",amt:3035,cnt:590,prods:[215,0,242,0,2578,0]},
  {nm:"목포시",rg:"전남",amt:2721,cnt:660,prods:[202,0,411,0,2109,0]},
  {nm:"광양시",rg:"전남",amt:1725,cnt:330,prods:[171,0,226,0,1327,0]},
  {nm:"원주시",rg:"강원",amt:4565,cnt:1110,prods:[787,0,493,0,3285,0]},
  {nm:"춘천시",rg:"강원",amt:3518,cnt:920,prods:[955,0,655,0,1907,0]},
  {nm:"강릉시",rg:"강원",amt:1127,cnt:370,prods:[0,0,41,0,1086,0]},
  {nm:"제주시",rg:"제주",amt:1533,cnt:450,prods:[0,0,80,0,1454,0]},
  {nm:"서귀포시",rg:"제주",amt:354,cnt:80,prods:[0,0,0,0,354,0]},
];

const REGION_COLORS = {
  "서울":"#5b8def","경기":"#38bdf8","인천":"#14b8a6","부산":"#fbbf24","대전":"#fb923c",
  "경남":"#f43f5e","대구":"#8b5cf6","광주":"#ec4899","충남":"#84cc16","충북":"#a78bfa",
  "전북":"#f472b6","경북":"#34d399","울산":"#fcd34d","전남":"#67e8f9","강원":"#fda4af","제주":"#94a3b8"
};

// ═══════ CHART DEFAULTS ═══════
Chart.defaults.color = '#6b7894';
Chart.defaults.font.family = "'IBM Plex Sans KR', sans-serif";
Chart.defaults.font.size = 11;
const tooltipBase = { backgroundColor:'rgba(7,9,15,0.95)', borderColor:'#2c3a5a', borderWidth:1, padding:12, titleColor:'#fff', titleFont:{size:12,weight:'600'}, bodyColor:'#a1adc7', bodyFont:{size:11,family:"'JetBrains Mono', monospace"}, cornerRadius:6, displayColors:true, boxPadding:4 };
const gridLight = { color:'rgba(31,41,64,0.5)', drawBorder:false };
const gridNone = { display:false };

// ═══════ § 1. DONUT ═══════
new Chart(document.getElementById('donut'), {
  type:'doughnut',
  data:{ labels:PRODUCTS.map(p=>p.name), datasets:[{ data:PRODUCTS.map(p=>p.aum), backgroundColor:PRODUCTS.map(p=>p.color), borderColor:'#11151f', borderWidth:3, hoverOffset:8 }] },
  options:{ responsive:true, maintainAspectRatio:false, cutout:'62%',
    plugins:{ legend:{ position:'bottom', labels:{ padding:14, boxWidth:10, boxHeight:10, font:{size:11} } },
      tooltip:{...tooltipBase, callbacks:{ label:ctx=>{const v=ctx.parsed; return   ${v.toLocaleString()} M  ·  ${(v/TOTAL_AUM*100).toFixed(1)}%;}}}}}
});

// § 1. PRODUCT TABLE
const prodSorted = [...PRODUCTS].sort((a,b)=>b.aum-a.aum);
document.getElementById('prodTable').innerHTML = prodSorted.map((p,i)=>{
  const perCap = Math.round(p.aum/p.cnt*10000);
  return <tr><td class="rank">${String(i+1).padStart(2,'0')}</td><td><span class="tag" style="background:${p.color}22;color:${p.color}">${p.name}</span></td><td class="num">${(p.aum/1000).toFixed(1)}</td><td class="num">${p.cnt.toLocaleString()}</td><td class="num">${perCap.toLocaleString()}</td></tr>;
}).join('');

// ═══════ § 2. REGION CHARTS ═══════
const regSorted = [...REGIONS].sort((a,b)=>b.amt-a.amt);
new Chart(document.getElementById('regionAmt'), {
  type:'bar',
  data:{ labels:regSorted.map(r=>r.name), datasets:[{ label:'AUM', data:regSorted.map(r=>r.amt), backgroundColor:regSorted.map((r,i)=>rgba(91,141,239,${Math.max(1-i*0.04,0.25)})), borderRadius:4 }] },
  options:{ indexAxis:'y', responsive:true, maintainAspectRatio:false,
    plugins:{ legend:{display:false},
      tooltip:{...tooltipBase, callbacks:{ label:ctx=>{const r=regSorted[ctx.dataIndex]; return [  AUM: ${r.amt.toLocaleString()} M,   점유율: ${(r.amt/TOTAL_AUM*100).toFixed(2)}%,   인원: ${r.cnt.toLocaleString()}명];}}}},
    scales:{ x:{grid:gridLight, ticks:{callback:v=>(v/1000).toFixed(0)+'B'}}, y:{grid:gridNone, ticks:{color:'#a1adc7',font:{size:11}}} }}
});

const regPerCap = REGIONS.map(r=>({...r, perCap:Math.round(r.amt/r.cnt*10000)})).sort((a,b)=>b.perCap-a.perCap);
new Chart(document.getElementById('regionPerCap'), {
  type:'bar',
  data:{ labels:regPerCap.map(r=>r.name), datasets:[{ data:regPerCap.map(r=>r.perCap), backgroundColor:regPerCap.map(r=>r.perCap>714?'#fbbf24':'rgba(91,141,239,0.55)'), borderRadius:4 }] },
  options:{ indexAxis:'y', responsive:true, maintainAspectRatio:false,
    plugins:{ legend:{display:false},
      tooltip:{...tooltipBase, callbacks:{ label:ctx=>  1인 평균: ${ctx.parsed.x.toLocaleString()}만원}}},
    scales:{ x:{grid:gridLight, ticks:{callback:v=>v.toLocaleString()}}, y:{grid:gridNone, ticks:{color:'#a1adc7',font:{size:11}}} }}
});

// ═══════ § 3. HEATMAP ═══════
const heatRegions = Object.keys(REGION_BY_PROD).sort((a,b)=>{
  const ra = REGIONS.find(r=>r.name===a), rb = REGIONS.find(r=>r.name===b);
  return (rb?rb.amt:0)-(ra?ra.amt:0);
});
const prodMax = PRODUCTS.map((_,pi)=>Math.max(...heatRegions.map(rn=>REGION_BY_PROD[rn][pi])));
let heatHTML = '<div class="heatmap"><div class="heat-h"></div>';
PRODUCTS.forEach(p=>{ heatHTML += <div class="heat-h">${p.name}</div>; });
heatRegions.forEach(rn=>{
  heatHTML += <div class="heat-h row">${rn}</div>;
  PRODUCTS.forEach((p,pi)=>{
    const v = REGION_BY_PROD[rn][pi];
    const ratio = prodMax[pi]>0 ? v/prodMax[pi] : 0;
    let bg, color;
    if (v===0) { bg='#161c2a'; color='#3a4560'; }
    else { bg = p.color + Math.round(ratio*200+30).toString(16).padStart(2,'0'); color = ratio>0.5?'#fff':'#ffffffcc'; }
    const display = v>=1000 ? (v/1000).toFixed(0)+'k' : v;
    heatHTML += <div class="heat-cell" style="background:${bg};color:${color}" title="${rn} · ${p.name}: ${v.toLocaleString()}M">${v===0?'—':display}</div>;
  });
});
heatHTML += '</div>';
document.getElementById('heatmap').innerHTML = heatHTML;

// ═══════ § 4. CITY STACK ═══════
let cityStackChart = null;
function renderCityStack(n){
  if (cityStackChart) cityStackChart.destroy();
  const sorted = [...CITIES].sort((a,b)=>b.amt-a.amt).slice(0,n);
  const datasets = PRODUCTS.map((p,pi)=>({ label:p.name, data:sorted.map(c=>c.prods[pi]), backgroundColor:p.color, borderRadius:2 }));
  cityStackChart = new Chart(document.getElementById('cityStack'), {
    type:'bar',
    data:{ labels:sorted.map(c=>${c.nm} (${c.rg})), datasets },
    options:{ responsive:true, maintainAspectRatio:false,
      plugins:{ legend:{position:'bottom', labels:{boxWidth:10, padding:12, font:{size:10}}},
        tooltip:{...tooltipBase, callbacks:{ label:ctx=>  ${ctx.dataset.label}: ${ctx.parsed.y.toLocaleString()} M}}},
      scales:{ x:{stacked:true, grid:gridNone, ticks:{color:'#a1adc7',font:{size:10}, maxRotation:45}},
        y:{stacked:true, grid:gridLight, ticks:{callback:v=>(v/1000).toFixed(0)+'B'}} }}
  });
}
renderCityStack(10);
document.querySelectorAll('#topNFilter .pill').forEach(pill=>{
  pill.addEventListener('click', ()=>{
    document.querySelectorAll('#topNFilter .pill').forEach(p=>p.classList.remove('active'));
    pill.classList.add('active');
    const n = parseInt(pill.dataset.n);
    document.getElementById('topNLabel').textContent = TOP ${n};
    renderCityStack(n);
  });
});

// ═══════ § 5. SCATTER ═══════
const regionGroups = [...new Set(CITIES.map(c=>c.rg))];
const scatterDS = regionGroups.map(rg=>({
  label:rg,
  data:CITIES.filter(c=>c.rg===rg).map(c=>({ x:c.cnt, y:c.amt, r:Math.max(4,Math.min(20,Math.round(c.amt/c.cnt/200))), nm:c.nm })),
  backgroundColor:(REGION_COLORS[rg]||'#64748b')+'b0',
  borderColor:REGION_COLORS[rg]||'#64748b', borderWidth:1
}));
new Chart(document.getElementById('scatter'), {
  type:'bubble', data:{datasets:scatterDS},
  options:{ responsive:true, maintainAspectRatio:false,
    plugins:{ legend:{display:false},
      tooltip:{...tooltipBase, callbacks:{ label:ctx=>{const d=ctx.raw; return [  ${d.nm} (${ctx.dataset.label}),   인원: ${d.x.toLocaleString()}명,   AUM: ${d.y.toLocaleString()}M,   1인당: ${Math.round(d.y/d.x*10000).toLocaleString()}만원];}}}},
    scales:{ x:{grid:gridLight, title:{display:true, text:'보유 인원 수 (명)', color:'#6b7894'}, ticks:{callback:v=>v.toLocaleString()}},
      y:{grid:gridLight, title:{display:true, text:'AUM (백만원)', color:'#6b7894'}, ticks:{callback:v=>(v/1000).toFixed(0)+'B'}} }}
});
document.getElementById('scatterCount').textContent = ${CITIES.length} 개 시·구 PLOT;
const scatterLeg = document.getElementById('scatterLegend');
regionGroups.forEach(rg=>{
  scatterLeg.innerHTML += <span style="display:inline-flex;align-items:center;gap:6px"><span style="width:8px;height:8px;border-radius:50%;background:${REGION_COLORS[rg]||'#64748b'}"></span>${rg}</span>;
});

// ═══════ § 6. PER-CAPITA TABLES ═══════
const perCapData = CITIES.filter(c=>c.cnt>=100).map(c=>({...c, perCap:Math.round(c.amt/c.cnt*10000)}));
const top15 = [...perCapData].sort((a,b)=>b.perCap-a.perCap).slice(0,15);
const bot15 = [...perCapData].sort((a,b)=>a.perCap-b.perCap).slice(0,15);
const maxPC = top15[0].perCap;
document.getElementById('topPerCap').innerHTML = top15.map((d,i)=>
  <tr><td class="rank">${String(i+1).padStart(2,'0')}</td><td>${d.nm}</td>
  <td><span class="tag" style="background:${(REGION_COLORS[d.rg]||'#64748b')}22;color:${REGION_COLORS[d.rg]||'#64748b'}">${d.rg}</span></td>
  <td class="num">${d.perCap.toLocaleString()}</td>
  <td class="bar-cell"><div class="bar-bg"><div class="bar-fill" style="width:${(d.perCap/maxPC*100).toFixed(0)}%;background:linear-gradient(90deg,#fbbf24,#fb923c)"></div></div></td></tr>
).join('');
document.getElementById('botPerCap').innerHTML = bot15.map((d,i)=>
  <tr><td class="rank">${String(i+1).padStart(2,'0')}</td><td>${d.nm}</td>
  <td><span class="tag" style="background:${(REGION_COLORS[d.rg]||'#64748b')}22;color:${REGION_COLORS[d.rg]||'#64748b'}">${d.rg}</span></td>
  <td class="num">${d.perCap.toLocaleString()}</td>
  <td class="bar-cell"><div class="bar-bg"><div class="bar-fill" style="width:${(d.perCap/maxPC*100).toFixed(0)}%;background:linear-gradient(90deg,#f43f5e,#fb7185)"></div></div></td></tr>
).join('');

// ═══════ § 7. SEOUL & GG ═══════
const seoulCities = CITIES.filter(c=>c.rg==='서울').sort((a,b)=>b.amt-a.amt);
new Chart(document.getElementById('seoulBar'), {
  type:'bar',
  data:{ labels:seoulCities.map(c=>c.nm), datasets:PRODUCTS.map((p,pi)=>({label:p.name, data:seoulCities.map(c=>c.prods[pi]), backgroundColor:p.color, borderRadius:2})) },
  options:{ indexAxis:'y', responsive:true, maintainAspectRatio:false,
    plugins:{ legend:{position:'bottom', labels:{boxWidth:8, padding:8, font:{size:9}}},
      tooltip:{...tooltipBase, callbacks:{label:ctx=>  ${ctx.dataset.label}: ${ctx.parsed.x.toLocaleString()} M}}},
    scales:{ x:{stacked:true, grid:gridLight, ticks:{callback:v=>(v/1000).toFixed(0)+'B', font:{size:9}}},
      y:{stacked:true, grid:gridNone, ticks:{font:{size:10}, color:'#a1adc7'}} }}
});
const ggCities = CITIES.filter(c=>c.rg==='경기').sort((a,b)=>b.amt-a.amt).slice(0,15);
new Chart(document.getElementById('ggBar'), {
  type:'bar',
  data:{ labels:ggCities.map(c=>c.nm), datasets:PRODUCTS.map((p,pi)=>({label:p.name, data:ggCities.map(c=>c.prods[pi]), backgroundColor:p.color, borderRadius:2})) },
  options:{ indexAxis:'y', responsive:true, maintainAspectRatio:false,
    plugins:{ legend:{position:'bottom', labels:{boxWidth:8, padding:8, font:{size:9}}},
      tooltip:{...tooltipBase, callbacks:{label:ctx=>  ${ctx.dataset.label}: ${ctx.parsed.x.toLocaleString()} M}}},
    scales:{ x:{stacked:true, grid:gridLight, ticks:{callback:v=>(v/1000).toFixed(0)+'B', font:{size:9}}},
      y:{stacked:true, grid:gridNone, ticks:{font:{size:10}, color:'#a1adc7'}} }}
});
</script>
</body>
</html>
