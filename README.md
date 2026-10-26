<!doctype html>
<html lang="ko">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="theme-color" content="#000000">
<title>phone</title>
<style>
:root { --sz: min(17vw, 74px); }
* { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
html, body { height: 100%; background: #111; overflow: hidden;
  font-family: -apple-system, "SF Pro Text", "Apple SD Gothic Neo", "Noto Sans KR", "Malgun Gothic", sans-serif; }

.phone { position: relative; width: 100%; max-width: 430px; height: 100dvh; margin: 0 auto; overflow: hidden; background: #000; }
@media (min-width: 500px) {
  .phone { height: min(932px, 100dvh); margin-top: max(0px, calc((100dvh - 932px) / 2)); border-radius: 48px; }
}

.screen { position: absolute; inset: 0; }

/* ---------- 상태바 ---------- */
.status { position: absolute; top: 0; left: 0; right: 0; height: 64px; z-index: 5; color: #fff;
  font-weight: 600; font-size: 17px; pointer-events: none; transition: color .4s; }
.status.dark { color: #1c1c1e; }
.s-time { position: absolute; left: 30px; top: 20px; }
.s-right { position: absolute; right: 24px; top: 20px; display: flex; align-items: center; gap: 8px; }
.island { position: absolute; left: 50%; top: 11px; width: 126px; height: 37px; margin-left: -63px; background: #000; border-radius: 20px; }
.batt { position: relative; display: inline-block; min-width: 30px; height: 15px; padding: 0 5px; border-radius: 5px;
  background: rgba(255,255,255,.92); color: #222; font-size: 13px; line-height: 15px; text-align: center; }
.batt::after { content: ""; position: absolute; right: -3px; top: 5px; width: 2px; height: 5px; border-radius: 0 2px 2px 0; background: currentColor; opacity: .5; }
.status.dark .batt { background: #1c1c1e; color: #fff; }

/* ---------- 잠금화면 ---------- */
.lock { z-index: 4; color: #fff; background: #000;
  transition: transform .55s cubic-bezier(.3,.8,.2,1), opacity .55s; }
.lock.gone { transform: translateY(-105%); opacity: .6; }
.lock.hidden { display: none; }
.lk-main { position: absolute; inset: 0; transition: opacity .3s, transform .3s; cursor: pointer; }
.lk-date { position: absolute; top: 104px; width: 100%; text-align: center; font-size: 20px; font-weight: 500; color: rgba(255,255,255,.82); }
.lk-time { position: absolute; top: 124px; width: 100%; text-align: center; font-size: min(22vw, 98px); font-weight: 600; letter-spacing: -1px; line-height: 1.1; }
.lk-btn { position: absolute; bottom: 56px; width: 50px; height: 50px; border-radius: 50%; display: grid; place-items: center;
  background: rgba(255,255,255,.16); backdrop-filter: blur(10px); -webkit-backdrop-filter: blur(10px); }
.lk-btn.l { left: 46px; } .lk-btn.r { right: 46px; }
.lk-hint { position: absolute; bottom: 24px; width: 100%; text-align: center; font-size: 12px; color: rgba(255,255,255,.55); }
.lk-pass { position: absolute; inset: 0; opacity: 0; pointer-events: none; transition: opacity .3s; }
.lock.pass .lk-pass { opacity: 1; pointer-events: auto; }
.lock.pass .lk-main { opacity: 0; transform: scale(.96); pointer-events: none; }
.lock.pass .lk-hint, .lock.pass .lk-btn { opacity: 0; }
.pp-title { position: absolute; top: 118px; width: 100%; text-align: center; font-size: 20px; font-weight: 500; }
.dots { position: absolute; top: 166px; left: 50%; transform: translateX(-50%); display: flex; gap: 22px; }
.dots i { width: 13px; height: 13px; border-radius: 50%; border: 1.5px solid #fff; transition: background .12s; }
.dots i.on { background: #fff; }
.dots.shake { animation: shake .4s; }
@keyframes shake { 20%,60% { transform: translateX(calc(-50% - 9px)); } 40%,80% { transform: translateX(calc(-50% + 9px)); } }
.keys { position: absolute; top: 232px; left: 50%; transform: translateX(-50%); display: grid; grid-template-columns: repeat(3, 78px); gap: 16px 24px; }
.keys button { width: 78px; height: 78px; border-radius: 50%; border: 0; background: rgba(255,255,255,.17); color: #fff; font-size: 34px; font-weight: 400; cursor: pointer; transition: background .1s; }
.keys button:active { background: rgba(255,255,255,.4); }
.keys .blank { visibility: hidden; }
.pp-foot { position: absolute; bottom: 56px; left: 0; right: 0; display: flex; justify-content: space-between; padding: 0 44px; font-size: 17px; }
.pp-foot button { background: none; border: 0; color: #fff; font-size: 17px; cursor: pointer; }

.indicator { position: absolute; bottom: 9px; left: 50%; width: 36%; height: 5px; margin-left: -18%; border-radius: 3px; background: #fff; }
.indicator.dark { background: #fff; }

/* ---------- 홈화면 ---------- */
.home { z-index: 1; background: #000; }
.grid { position: absolute; top: 112px; left: 0; right: 0; padding: 0 12px; display: grid; grid-template-columns: repeat(4, 1fr); row-gap: 26px; }
.app { display: flex; flex-direction: column; align-items: center; gap: 7px; text-decoration: none; color: rgba(255,255,255,.92); font-size: 12.5px; transition: transform .15s; }
.app:active { transform: scale(.9); }
.ico { width: var(--sz); height: var(--sz); border-radius: 23%; overflow: hidden; display: grid; place-items: center; box-shadow: 0 2px 7px rgba(0,0,0,.18); }
.ico img { width: 100%; height: 100%; object-fit: cover; }
.ico svg.full { width: 100%; height: 100%; display: block; }
.ico .cal { text-align: center; color: #111; line-height: 1; }
.ico .cal small { display: block; font-size: 12px; font-weight: 600; margin-bottom: 1px; color: #222; }
.ico .cal b { font-size: calc(var(--sz) * .55); font-weight: 400; }

.dock { position: absolute; left: 14px; right: 14px; bottom: 30px; height: 100px; border-radius: 38px;
  background: rgba(255,255,255,.2); backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px);
  display: flex; align-items: center; justify-content: space-around; padding: 0 6px; }
.dock .app span { display: none; }

/* 페이지 전환 */
body.leaving .phone { opacity: 0; transition: opacity .18s; }

/* =========================================================
   앱 화면 (제2의 화면)
   ========================================================= */
.status { z-index: 9; }
.ic { width: 24px; height: 24px; fill: none; stroke: currentColor; stroke-width: 1.8; stroke-linecap: round; stroke-linejoin: round; flex: none; }
.ic.lg { width: 30px; height: 30px; } .ic.xs { width: 14px; height: 14px; } .ic.sm { width: 20px; height: 20px; } .ic.fill { fill: currentColor; }

.view { position: absolute; inset: 0; z-index: 6; display: none; flex-direction: column; background: var(--bg, #fff); color: var(--fg, #111);
  --line: rgba(0,0,0,.1); --mut: #8e8e93; --tint: #0a84ff; --ind: #000; }
.view.open { display: flex; animation: vin .22s ease-out; }
@keyframes vin { from { opacity: 0; transform: scale(.94); } }
.view.dk { --bg: #000; --fg: #fff; --line: rgba(255,255,255,.18); --mut: #98989f; --ind: #fff; }
.vtop { flex: none; padding-top: 54px; }
.vscroll { flex: 1; min-height: 0; overflow-y: auto; -webkit-overflow-scrolling: touch; }
.vscroll.pt { padding-top: 54px; }
.vind { position: absolute; left: 50%; bottom: 0; width: 50%; height: 26px; margin-left: -25%; z-index: 7; cursor: pointer; }
.vind::after { content: ""; position: absolute; left: 14%; right: 14%; bottom: 8px; height: 5px; border-radius: 3px; background: var(--ind); opacity: .9; }

/* 직접 입력하는 칸 */
[data-e] { outline: none; cursor: text; -webkit-user-select: text; user-select: text; min-width: 1ch; display: inline-block; max-width: 100%; word-break: break-word; }
[data-e][data-ml] { white-space: pre-wrap; }
[data-e]:empty::before { content: attr(data-ph); color: var(--mut); opacity: .7; pointer-events: none; font-weight: 400; }
[data-e]:focus { background: rgba(10,132,255,.1); border-radius: 4px; }
.slot { position: relative; flex: none; background-color: #e5e5ea; background-size: cover; background-position: center; overflow: hidden; cursor: pointer; display: grid; place-items: center; color: #aeaeb2; }
.slot:not(.has)::after { content: "＋"; font-size: 26px; font-weight: 300; }
.view.dk .slot { background-color: #2c2c2e; }
.av { border-radius: 50%; width: 40px; height: 40px; background-color: #c7c7cc;
  background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 40 40'><circle cx='20' cy='15' r='7' fill='white'/><path d='M6 40c0-9 6-14 14-14s14 5 14 14z' fill='white'/></svg>"); }
.av:not(.has)::after { content: none; }
.av.sm { width: 38px; height: 38px; } .av.lg { width: 78px; height: 78px; } .av.xs { width: 26px; height: 26px; }
.sq { width: 100%; aspect-ratio: 1; }

/* 공통 부품 */
.tabs { display: flex; }
.tabs button { flex: 1; border: 0; background: none; padding: 12px 0 10px; font-size: 16px; font-weight: 600; color: var(--mut); position: relative; font-family: inherit; }
.tabs button.on { color: var(--fg); }
.tabs button.on::after { content: ""; position: absolute; left: 50%; transform: translateX(-50%); bottom: 0; width: 56px; height: 4px; border-radius: 2px; background: var(--tint); }
.tabbar { flex: none; display: flex; justify-content: space-around; padding: 8px 4px 26px; background: rgba(249,249,249,.95); border-top: .5px solid var(--line); }
.view.dk .tabbar { background: rgba(22,22,24,.95); }
.tb { border: 0; background: none; display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 3px; font-size: 10px; color: var(--mut); flex: 1; font-family: inherit; padding: 2px 0; }
.tb.on { color: var(--tint); }
.tabbar.icons .tb span { display: none; }
.pill { border: 0; background: #efefef; border-radius: 10px; padding: 8px 16px; font-size: 13.5px; font-weight: 600; font-family: inherit; color: #111; cursor: pointer; }
.pill.on { background: #fff; box-shadow: inset 0 0 0 1px #dbdbdb; }
.big-t { font-size: 34px; font-weight: 700; padding: 4px 16px 6px; }
.sec { font-size: 21px; font-weight: 700; padding: 14px 16px 8px; }
.srch { margin: 6px 16px 12px; background: rgba(118,118,128,.12); border-radius: 10px; padding: 8px 10px; display: flex; gap: 6px; color: #8e8e93; font-size: 17px; align-items: center; }
.srch .ic { width: 18px; height: 18px; }
.grp { margin: 0 16px 18px; background: #fff; border-radius: 12px; overflow: hidden; }
.grp .row { padding: 11px 16px; border-top: .5px solid #e5e5ea; }
.grp .row:first-child { border-top: 0; }
.row .t { font-weight: 600; font-size: 16px; } .row .s { font-size: 14px; color: #8e8e93; margin-top: 2px; }
.bar2 { display: flex; justify-content: space-between; align-items: center; padding: 6px 16px; font-size: 17px; }
.empty { text-align: center; color: #8e8e93; padding: 40px 0; }

/* ---- Instagram ---- */
.ig-top { display: flex; align-items: center; justify-content: space-between; padding: 6px 16px 8px; }
.ig-logo { font-size: 26px; font-weight: 800; font-style: italic; letter-spacing: -.5px; display: flex; align-items: center; gap: 5px; }
.ig-r { display: flex; gap: 20px; }
.ig-stories { display: flex; gap: 12px; padding: 6px 14px 12px; overflow-x: auto; }
.st { display: flex; flex-direction: column; align-items: center; gap: 5px; width: 76px; flex: none; font-size: 12px; }
.st b { font-weight: 400; max-width: 76px; text-align: center; }
.st .ring { position: relative; padding: 3px; border-radius: 50%; }
.st .ring.seen { box-shadow: inset 0 0 0 2px #dbdbdb; }
.st .ring.on { background: linear-gradient(45deg,#feda75,#fa7e1e,#d62976,#962fbf,#4f5bd5); }
.st .av { width: 66px; height: 66px; box-sizing: border-box; }
.st .ring.on .av, .st .ring.seen .av { border: 3px solid #fff; }
.badge { position: absolute; right: -1px; bottom: -1px; width: 22px; height: 22px; border-radius: 50%; background: #111; color: #fff; border: 2px solid #fff; display: grid; place-items: center; font-style: normal; font-size: 17px; line-height: 1; }
.ig-ph { display: flex; align-items: center; gap: 10px; padding: 8px 14px; }
.ig-ph .who { flex: 1; min-width: 0; font-size: 14px; font-weight: 600; line-height: 1.25; }
.ig-ph small { display: block; font-weight: 400; color: #8e8e93; font-size: 12.5px; }
.ig-act { display: flex; gap: 16px; padding: 10px 14px 4px; } .ig-act .sp { flex: 1; }
.ig-meta, .ig-cap { padding: 2px 14px; font-size: 14px; line-height: 1.45; }
.ig-p { padding-bottom: 14px; }
.ig-tb .tb .ic { width: 27px; height: 27px; }

/* ---- X ---- */
.x { --tint: #1d9bf0; --mut: #536471; }
.x-top { display: flex; align-items: center; justify-content: space-between; padding: 6px 16px; }
.x-logo { font-size: 28px; font-weight: 800; }
.x-sub { border: 1px solid #cfd9de; background: #fff; border-radius: 999px; padding: 8px 16px; font-weight: 700; font-size: 14px; font-family: inherit; }
.x .tabs button.plus { flex: .35; font-size: 22px; padding-top: 8px; }
.tw { display: flex; gap: 10px; padding: 12px 16px; border-bottom: 1px solid #eff3f4; }
.tw-b { flex: 1; min-width: 0; }
.tw-h { font-size: 15px; display: flex; gap: 4px; flex-wrap: wrap; align-items: baseline; }
.tw-h b { font-weight: 800; }
.tw-t { font-size: 15px; line-height: 1.4; margin: 2px 0 8px; word-break: break-word; }
.tw-a { display: flex; justify-content: space-between; color: #536471; font-size: 13px; }
.tw-a span { display: flex; align-items: center; gap: 6px; } .tw-a .ic { width: 19px; height: 19px; }
.fab { position: absolute; right: 16px; bottom: 98px; width: 56px; height: 56px; border-radius: 50%; background: #1d9bf0; color: #fff; display: grid; place-items: center; box-shadow: 0 4px 12px rgba(0,0,0,.25); z-index: 5; }
.x-tb .tb .ic { width: 27px; height: 27px; color: #111; }

/* ---- 포스타입 ---- */
.ps-top { display: flex; align-items: center; gap: 14px; padding: 6px 16px; }
.ps-logo { font-size: 26px; font-weight: 900; letter-spacing: -.5px; flex: 1; margin-left: 4px; }
.dotw { position: relative; display: inline-flex; } .dotw i { position: absolute; right: -2px; top: -2px; width: 8px; height: 8px; border-radius: 50%; background: #ff3b30; }
.ps-tabs { border-bottom: 1px solid #e8e8ec; padding-left: 8px; }
.ps-tabs button { flex: none; padding: 12px 14px 10px; }
.ps-tabs button.on::after { width: calc(100% - 16px); height: 3px; border-radius: 0; background: #111; }
.ps-card { padding: 16px; border-bottom: 8px solid #f3f3f5; }
.ps-h { display: flex; align-items: center; gap: 10px; }
.ps-h .who { flex: 1; min-width: 0; font-size: 15px; font-weight: 600; }
.ps-h small { display: block; color: #8e8e93; font-size: 12.5px; font-weight: 400; margin-top: 2px; }
.ps-t { font-size: 16px; line-height: 1.55; margin: 14px 0 10px; color: #333; }
.ps-t .tt { font-weight: 700; display: block; color: #111; }
.chips { display: flex; flex-wrap: wrap; gap: 6px; } .chip { background: #f1f1f3; border-radius: 999px; padding: 6px 12px; font-size: 13px; color: #666; }
.ps-v { font-size: 13px; color: #8e8e93; margin: 12px 0 10px; }
.ps-a { display: flex; align-items: center; gap: 14px; color: #555; font-size: 15px; } .ps-a span { display: flex; align-items: center; gap: 6px; } .ps-a .sp { flex: 1; }
.ps-cm { background: #f6f6f8; border-radius: 10px; padding: 16px; margin-top: 14px; font-size: 15px; line-height: 1.5; }
.ps-tb .tb { font-size: 11px; color: #222; } .ps-tb .tb.on { color: #111; }

/* ---- 캘린더 ---- */
.cal { --tint: #ff3b30; }
.cal-h { display: flex; align-items: center; justify-content: space-between; padding: 6px 16px; color: #ff3b30; font-size: 17px; }
.cal-i { display: flex; gap: 18px; }
.cal-title { font-size: 32px; font-weight: 700; padding: 0 16px 8px; display: flex; justify-content: space-between; align-items: center; }
.cal-title button { border: 0; background: none; color: #ff3b30; font-size: 30px; padding: 0 8px; font-family: inherit; }
.cal-g { display: grid; grid-template-columns: repeat(7,1fr); padding: 0 6px; text-align: center; }
.cal-g .w { font-size: 12px; color: #8e8e93; padding: 6px 0 8px; }
.cal-g .n { height: 52px; display: flex; justify-content: center; padding-top: 6px; border-top: .5px solid #e5e5ea; font-size: 19px; cursor: pointer; }
.cal-g .n span { width: 34px; height: 34px; display: grid; place-items: center; border-radius: 50%; }
.cal-g .n.sel span { background: #e5e5ea; }
.cal-g .n.today span { background: #ff3b30; color: #fff; }
.cal-ev { padding: 14px 16px; border-top: .5px solid #e5e5ea; font-size: 16px; }
.cal-ev .d { font-weight: 600; margin-bottom: 8px; } .cal-ev [data-e] { color: #111; }
.tools { flex: none; display: flex; justify-content: space-around; padding: 12px 0 30px; color: #ff3b30; font-size: 17px; border-top: .5px solid var(--line); background: rgba(249,249,249,.95); }
.tools span { cursor: pointer; }

/* ---- 사진 ---- */
.seg { display: flex; margin: 4px 16px 10px; background: #eeeef0; border-radius: 9px; padding: 2px; }
.seg button { flex: 1; border: 0; background: none; padding: 6px 0; font-size: 13px; border-radius: 7px; font-family: inherit; color: #111; }
.seg button.on { background: #fff; box-shadow: 0 1px 3px rgba(0,0,0,.15); font-weight: 600; }
.ph-g { display: grid; grid-template-columns: repeat(3,1fr); gap: 2px; } .ph-g .slot { aspect-ratio: 1; }

/* ---- 카메라 ---- */
.cam-top { display: flex; justify-content: space-between; align-items: center; padding: 6px 22px; }
.cam-vf { flex: 1; margin: 6px 0; background: linear-gradient(180deg,#1b1b1d,#2a2a2d); border-radius: 16px; display: grid; place-items: center; color: #555; font-size: 14px; position: relative; overflow: hidden; min-height: 0; }
.cam-vf.flash::before { content: ""; position: absolute; inset: 0; background: #fff; animation: fl .25s forwards; }
@keyframes fl { from { opacity: .95; } to { opacity: 0; } }
.zoom { position: absolute; bottom: 14px; left: 50%; transform: translateX(-50%); display: flex; gap: 6px; background: rgba(0,0,0,.35); padding: 4px; border-radius: 999px; }
.zoom b { width: 32px; height: 32px; border-radius: 50%; display: grid; place-items: center; font-size: 12px; color: #fff; }
.zoom b.on { background: rgba(0,0,0,.6); color: #ffd60a; }
.modes { display: flex; gap: 18px; justify-content: center; padding: 10px 0; font-size: 13px; font-weight: 600; white-space: nowrap; overflow: hidden; }
.modes .on { color: #ffd60a; }
.shut-row { display: flex; align-items: center; justify-content: space-around; padding: 6px 20px 38px; }
.shut { width: 76px; height: 76px; border-radius: 50%; border: 4px solid #fff; display: grid; place-items: center; background: none; padding: 0; cursor: pointer; }
.shut::after { content: ""; width: 62px; height: 62px; border-radius: 50%; background: #fff; transition: transform .1s; } .shut:active::after { transform: scale(.88); }
.flip { width: 46px; height: 46px; border-radius: 50%; background: rgba(255,255,255,.14); display: grid; place-items: center; color: #fff; }
.cam .slot { width: 50px; height: 50px; border-radius: 10px; }

/* ---- 시계 ---- */
.ck { --tint: #ff9f0a; }
.ck-h { display: flex; justify-content: space-between; padding: 6px 18px; color: #ff9f0a; font-size: 17px; }
.ck-t { font-size: 34px; font-weight: 700; padding: 0 18px 8px; }
.ck-row { display: flex; align-items: center; justify-content: space-between; gap: 12px; padding: 14px 18px; border-top: .5px solid var(--line); }
.ck-row .l { font-size: 24px; min-width: 0; } .ck-row .l small { display: block; color: var(--mut); font-size: 13px; margin-bottom: 2px; }
.ck-row .r { font-size: 44px; font-weight: 300; } .ck-row .at { font-size: 46px; font-weight: 300; }
.sw { width: 51px; height: 31px; border-radius: 16px; background: #39393d; position: relative; flex: none; cursor: pointer; transition: background .2s; }
.sw::after { content: ""; position: absolute; left: 2px; top: 2px; width: 27px; height: 27px; border-radius: 50%; background: #fff; transition: left .2s; }
.sw.on { background: #34c759; } .sw.on::after { left: 22px; }
.pane { display: none; } .pane.on { display: block; }
.sw-t { font-size: 76px; font-weight: 200; text-align: center; font-variant-numeric: tabular-nums; padding: 56px 0 26px; }
.sw-b { display: flex; justify-content: space-between; padding: 0 28px 20px; }
.rb { width: 84px; height: 84px; border-radius: 50%; border: 0; font-size: 17px; font-family: inherit; cursor: pointer; box-shadow: inset 0 0 0 2px #000; }
.rb.s { background: #333336; color: #fff; } .rb.g { background: #0c3b1a; color: #30d158; } .rb.r { background: #3b1210; color: #ff453a; } .rb.o { background: #3b2a08; color: #ff9f0a; }
.lap { display: flex; justify-content: space-between; padding: 12px 28px; border-top: .5px solid var(--line); font-variant-numeric: tabular-nums; }
.tm-in { text-align: center; font-size: 18px; padding-bottom: 20px; }
.tm-in input { width: 80px; font-size: 28px; text-align: center; background: #1c1c1e; color: #fff; border: 0; border-radius: 10px; padding: 8px; outline: none; font-family: inherit; }

/* ---- 메모 ---- */
.nt { --tint: #f2b600; }
.nt-h { display: flex; justify-content: space-between; padding: 6px 16px; color: #d9a100; font-size: 17px; }
.tools.nt-t { color: #d9a100; justify-content: space-between; padding: 12px 20px 30px; font-size: 13px; }
.nt-t span { color: #8e8e93; }

/* ---- 날씨 ---- */
.wx { --bg: linear-gradient(180deg,#3a7bd5,#6bb1f0 60%,#9fd0f5); --fg: #fff; --mut: rgba(255,255,255,.78); --ind: #fff; --line: rgba(255,255,255,.3); }
.wx-c { text-align: center; padding-top: 20px; } .wx-city { font-size: 34px; } .wx-temp { font-size: 96px; font-weight: 200; line-height: 1.05; } .wx-cond { font-size: 20px; } .wx-hl { font-size: 19px; margin-top: 2px; }
.glass { margin: 16px 14px 0; background: rgba(255,255,255,.2); border-radius: 16px; padding: 12px 14px; -webkit-backdrop-filter: blur(14px); backdrop-filter: blur(14px); }
.glass h4 { font-size: 12px; letter-spacing: .3px; color: var(--mut); margin-bottom: 10px; font-weight: 600; }
.hr { display: flex; justify-content: space-between; text-align: center; } .hr div { display: flex; flex-direction: column; gap: 8px; align-items: center; font-size: 15px; min-width: 0; }
.dy { display: grid; grid-template-columns: 62px 28px 1fr 1fr; align-items: center; gap: 8px; padding: 9px 0; border-top: .5px solid var(--line); font-size: 18px; }
.dy:first-of-type { border-top: 0; } .dy .hi { text-align: right; }

/* ---- 미리 알림 ---- */
.tiles { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 0 16px 16px; }
.tile { background: #fff; border-radius: 14px; padding: 12px 14px; display: flex; justify-content: space-between; align-items: flex-start; min-height: 88px; }
.tile .c { width: 34px; height: 34px; border-radius: 50%; display: grid; place-items: center; color: #fff; } .tile .c .ic { width: 20px; height: 20px; }
.tile .n { font-size: 32px; font-weight: 700; } .tile .nm { font-size: 16px; font-weight: 600; color: #8e8e93; margin-top: 12px; }
.lrow { display: flex; align-items: center; gap: 12px; } .lrow .dot { width: 30px; height: 30px; border-radius: 50%; flex: none; } .lrow .nm { flex: 1; font-size: 17px; }
.todo-add { display: flex; gap: 8px; margin: 0 16px 12px; }
.todo-add input { flex: 1; border: 0; outline: 0; padding: 12px 14px; border-radius: 12px; font-size: 16px; font-family: inherit; }
.todo-add button { border: 0; background: #0a84ff; color: #fff; border-radius: 12px; padding: 0 16px; font-size: 16px; font-family: inherit; }
.todo-list { list-style: none; margin: 0 16px 12px; background: #fff; border-radius: 14px; overflow: hidden; }
.todo-list li { display: flex; align-items: center; gap: 12px; padding: 14px 16px; border-bottom: 1px solid #ececf0; font-size: 17px; }
.todo-list li:last-child { border-bottom: 0; } .todo-list .dot { width: 22px; height: 22px; border-radius: 50%; border: 2px solid #c7c7cc; flex: none; }
.todo-list li.done .dot { background: #0a84ff; border-color: #0a84ff; } .todo-list li.done span { color: #aeaeb2; text-decoration: line-through; }
.hint { margin: 0 16px 24px; font-size: 13px; color: #8e8e93; }

/* ---- 검색 / 브라우저 ---- */
.sp-in { margin: 6px 16px 8px; background: rgba(118,118,128,.14); border-radius: 12px; padding: 10px 12px; display: flex; gap: 8px; align-items: center; color: #8e8e93; }
.sp-in input, .sf-in input { flex: 1; border: 0; background: none; outline: none; font-size: 17px; font-family: inherit; min-width: 0; color: #111; }
.ap-row { display: flex; align-items: center; gap: 14px; padding: 10px 16px; border-top: .5px solid var(--line); font-size: 17px; cursor: pointer; }
.ap-row .ico { --sz: 44px; width: 44px; height: 44px; flex: none; }
.fav-g { display: grid; grid-template-columns: repeat(4,1fr); gap: 18px 8px; padding: 0 16px; }
.fav { display: flex; flex-direction: column; align-items: center; gap: 6px; font-size: 11.5px; text-align: center; min-width: 0; }
.fav .t { width: 62px; height: 62px; border-radius: 14px; background: #fff; display: grid; place-items: center; font-size: 26px; font-weight: 600; color: #555; }
.sf-bar { flex: none; padding: 8px 12px 0; background: rgba(249,249,249,.95); border-top: .5px solid var(--line); }
.sf-in { background: rgba(118,118,128,.14); border-radius: 12px; padding: 10px 12px; display: flex; gap: 8px; align-items: center; color: #8e8e93; }
.sf-in input { text-align: center; font-size: 16px; }
.sf-tools { display: flex; justify-content: space-around; padding: 12px 0 30px; color: #0a84ff; }

/* ---- 전화 ---- */
.pn-d { text-align: center; font-size: 38px; min-height: 56px; padding: 40px 20px 0; letter-spacing: 1px; word-break: break-all; }
.pad { display: grid; grid-template-columns: repeat(3,78px); gap: 14px 22px; justify-content: center; padding: 24px 0; }
.pad button { width: 78px; height: 78px; border-radius: 50%; border: 0; background: #e5e5ea; font-size: 34px; font-family: inherit; display: flex; flex-direction: column; align-items: center; justify-content: center; line-height: 1; padding: 0; color: #111; cursor: pointer; }
.pad button:active { background: #c7c7cc; }
.pad small { font-size: 10px; letter-spacing: 2px; font-weight: 600; margin-top: 2px; min-height: 11px; }
.pad .call { background: #34c759; color: #fff; grid-column: 2; } .pad .call:active { background: #2aa84a; }
.pad .del { background: none; color: #8e8e93; grid-column: 3; }

/* ---- 문자 ---- */
.ms-row { display: flex; gap: 12px; padding: 10px 16px; align-items: flex-start; position: relative; }
.ms-row::after { content: ""; position: absolute; left: 78px; right: 0; bottom: 0; border-bottom: .5px solid var(--line); }
.ms-row .av { width: 50px; height: 50px; } .ms-row .b { flex: 1; min-width: 0; }
.ms-row .nm { display: flex; justify-content: space-between; gap: 8px; font-weight: 600; font-size: 16px; } .ms-row .nm .tm { font-weight: 400; color: #8e8e93; font-size: 14px; }
.ms-row .pv { font-size: 15px; color: #8e8e93; line-height: 1.3; margin-top: 2px; }

/* ---- 음악 ---- */
.mu { --tint: #fa2d48; }
.mu-h { display: flex; justify-content: space-between; align-items: center; padding: 6px 16px; } .mu-h .big-t { padding: 0; }
.hs { display: flex; gap: 14px; padding: 0 16px; overflow-x: auto; }
.alb { width: 150px; flex: none; font-size: 14px; } .alb .slot { width: 150px; height: 150px; border-radius: 8px; margin-bottom: 6px; } .alb .a { color: #8e8e93; font-size: 13px; }
.feat .slot { margin: 0 16px; width: calc(100% - 32px); aspect-ratio: 1.5; border-radius: 12px; }
.mini { flex: none; display: flex; align-items: center; gap: 12px; margin: 0 10px 6px; padding: 8px 12px; background: rgba(240,240,242,.97); border-radius: 14px; box-shadow: 0 2px 10px rgba(0,0,0,.12); }
.mini .slot { width: 42px; height: 42px; border-radius: 6px; } .mini .t { flex: 1; min-width: 0; font-size: 15px; }
.mini button { border: 0; background: none; color: #111; padding: 4px; cursor: pointer; }
.mini .pa { display: none; } .mini.playing .pa { display: block; } .mini.playing .pl { display: none; }

#toast { position: absolute; left: 50%; bottom: 96px; transform: translateX(-50%); background: rgba(30,30,30,.92); color: #fff; padding: 10px 16px; border-radius: 20px; font-size: 14px; z-index: 20; opacity: 0; pointer-events: none; transition: opacity .25s; white-space: nowrap; }
#toast.on { opacity: 1; }

/* =========================================================
   인스타 스토리 보기 (줌 인)
   ========================================================= */
.sv { position: absolute; inset: 0; z-index: 8; background: #000; color: #fff; display: none; overflow: hidden; --ind: #fff; --mut: #bbb; }
.sv.open { display: block; animation: svIn .42s cubic-bezier(.2,.8,.2,1); }
.sv.closing { display: block; animation: svOut .28s ease-in forwards; }
@keyframes svIn { from { transform: scale(.08); opacity: 0; border-radius: 50%; } to { transform: none; opacity: 1; border-radius: 0; } }
@keyframes svOut { from { transform: none; opacity: 1; } to { transform: scale(.08); opacity: 0; border-radius: 50%; } }
.sv-img { position: absolute; inset: 0; bottom: 92px; background-color: #1a1b1f; background-size: cover; background-position: center; display: none; place-items: center; color: #8a8d95; font-size: 17px; }
.sv-img.on { display: grid; }
.sv-img:not(.has)::after { content: "＋  사진 추가"; }
.sv-shade { position: absolute; left: 0; right: 0; top: 0; height: 170px; background: linear-gradient(rgba(0,0,0,.5), transparent); pointer-events: none; }
.sv-bars { position: absolute; left: 12px; right: 12px; top: 54px; display: flex; gap: 4px; z-index: 2; }
.sv-bars span { flex: 1; height: 3px; border-radius: 2px; background: rgba(255,255,255,.35); overflow: hidden; }
.sv-bars i { display: block; height: 100%; width: 0; background: #fff; }
.sv-head { position: absolute; left: 0; right: 0; top: 68px; padding: 0 14px; display: flex; align-items: center; gap: 10px; z-index: 3; }
.sv-head b { font-size: 15px; font-weight: 600; } .sv-time { color: rgba(255,255,255,.72); font-size: 14px; } .sv-sp { flex: 1; }
.sv-av { width: 36px; height: 36px; }
.sv-btn { border: 0; background: none; color: #fff; padding: 4px; display: grid; place-items: center; cursor: pointer; }
.sv-l, .sv-r { position: absolute; top: 120px; bottom: 92px; z-index: 2; } .sv-l { left: 0; width: 32%; } .sv-r { right: 0; width: 68%; }
.sv-foot { position: absolute; left: 0; right: 0; bottom: 0; height: 92px; padding: 0 14px 24px; display: flex; align-items: center; gap: 14px; background: #16181d; z-index: 3; }
.sv-in { flex: 1; min-width: 0; border: 1.5px solid #6b6f78; border-radius: 999px; padding: 13px 18px; background: none; color: #fff; font-size: 15px; outline: none; font-family: inherit; }
.sv-in::placeholder { color: #d0d2d8; }
.sv-foot .ic { width: 28px; height: 28px; }
.sv-heart.on { color: #ff3040; } .sv-heart.on .ic { fill: #ff3040; }

/* =========================================================
   전화: 최근 기록 / 연락처 / 발신 화면
   ========================================================= */
.blue { color: #0a84ff; cursor: pointer; }
.rc-row { display: flex; align-items: center; gap: 12px; padding: 10px 16px; border-top: .5px solid var(--line); cursor: pointer; }
.rc-row:active, .ct-row:active { background: rgba(0,0,0,.06); }
.rc-b { flex: 1; min-width: 0; } .rc-n { font-size: 17px; font-weight: 600; } .rc-b small { display: block; color: #8e8e93; font-size: 14px; }
.rc-t { color: #8e8e93; font-size: 15px; } .rc-row .ic { color: #0a84ff; }
.ct-row { padding: 13px 18px; border-top: .5px solid var(--line); font-size: 17px; font-weight: 600; cursor: pointer; }
#v-phone .pane:not(.editing) [data-e], #v-phone .ct-detail:not(.editing) [data-e] { pointer-events: none; }
.ct-detail { position: absolute; inset: 0; z-index: 7; background: #f2f2f7; display: none; flex-direction: column; }
.ct-detail.open { display: flex; animation: ctIn .25s ease-out; }
@keyframes ctIn { from { transform: translateX(30px); opacity: 0; } }
.ctd-top { text-align: center; padding: 10px 0 14px; } .ctd-av.av { width: 96px; height: 96px; margin: 0 auto 10px; } .ctd-name { font-size: 28px; font-weight: 600; }
.ctd-act { display: flex; justify-content: center; gap: 10px; padding: 6px 16px 16px; }
.ctd-act button { width: 76px; border: 0; background: #fff; border-radius: 12px; padding: 10px 0 8px; color: #0a84ff; font-size: 11px; display: flex; flex-direction: column; align-items: center; gap: 4px; font-family: inherit; cursor: pointer; }
.hist { display: flex; justify-content: space-between; align-items: center; cursor: pointer; } .hist .nm { font-size: 16px; font-weight: 600; } .hist small { display: block; color: #8e8e93; font-size: 13px; margin-top: 2px; }
.hist > span { color: #8e8e93; font-size: 14px; } .hist.miss .nm { color: #ff3b30; }

.callscr { position: absolute; inset: 0; z-index: 8; display: none; flex-direction: column; align-items: center; color: #fff; background: linear-gradient(180deg,#303545,#12141a); overflow: hidden; }
.callscr.on { display: flex; animation: csIn .3s ease-out; }
@keyframes csIn { from { opacity: 0; transform: scale(1.04); } }
.cs-top { margin-top: 112px; text-align: center; padding: 0 20px; } .cs-name { font-size: 34px; font-weight: 500; min-height: 42px; word-break: break-all; } .cs-st { font-size: 18px; color: rgba(255,255,255,.7); margin-top: 6px; }
.cdots i { font-style: normal; animation: dotb 1.2s infinite; } .cdots i:nth-child(2) { animation-delay: .2s; } .cdots i:nth-child(3) { animation-delay: .4s; }
@keyframes dotb { 0%, 80%, 100% { opacity: .2; } 40% { opacity: 1; } }
.cs-av { position: relative; width: 112px; height: 112px; margin: 42px auto 0; }
.cs-face { position: absolute; inset: 0; border-radius: 50%; background: rgba(255,255,255,.14); display: grid; place-items: center; }
.cs-face .ic { width: 46px; height: 46px; }
.rg { position: absolute; inset: 0; border-radius: 50%; border: 2px solid rgba(255,255,255,.4); opacity: 0; }
.callscr.on .rg { animation: rip 2.4s ease-out infinite; } .rg:nth-child(2) { animation-delay: .8s !important; } .rg:nth-child(3) { animation-delay: 1.6s !important; }
@keyframes rip { 0% { transform: scale(1); opacity: .75; } 100% { transform: scale(2.4); opacity: 0; } }
.callscr.ringing .cs-face .ic { animation: wig 1s ease-in-out infinite; }
@keyframes wig { 0%, 60%, 100% { transform: rotate(0); } 10% { transform: rotate(-16deg); } 20% { transform: rotate(16deg); } 30% { transform: rotate(-12deg); } 40% { transform: rotate(12deg); } 50% { transform: rotate(-6deg); } }
.cs-grid { margin-top: auto; display: grid; grid-template-columns: repeat(3, 74px); gap: 22px 30px; justify-content: center; }
.cs-b { display: flex; flex-direction: column; align-items: center; gap: 6px; font-size: 12px; } .cs-b i { width: 74px; height: 74px; border-radius: 50%; background: rgba(255,255,255,.16); display: grid; place-items: center; font-style: normal; }
.cs-end { margin: 30px 0 62px; width: 74px; height: 74px; border-radius: 50%; border: 0; background: #ff3b30; color: #fff; display: grid; place-items: center; cursor: pointer; transition: opacity .2s; }
.cs-end .ic { width: 32px; height: 32px; transform: rotate(135deg); }
.callscr.ending .cs-end { opacity: .45; }
.cs-top { position: relative; z-index: 2; }
.cs-av { margin-top: 72px; }

</style>
</head>
<body>
<svg width="0" height="0" style="position:absolute" aria-hidden="true"><defs><symbol id="i-heart" viewBox="0 0 24 24"><path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/></symbol><symbol id="i-comment" viewBox="0 0 24 24"><path d="M21 11.5a8.38 8.38 0 0 1-.9 3.8 8.5 8.5 0 0 1-7.6 4.7 8.38 8.38 0 0 1-3.8-.9L3 21l1.9-5.7a8.38 8.38 0 0 1-.9-3.8 8.5 8.5 0 0 1 4.7-7.6 8.38 8.38 0 0 1 3.8-.9h.5a8.48 8.48 0 0 1 8 8v.5z"/></symbol><symbol id="i-send" viewBox="0 0 24 24"><line x1="22" y1="2" x2="11" y2="13"/><polygon points="22 2 15 22 11 13 2 9 22 2"/></symbol><symbol id="i-bookmark" viewBox="0 0 24 24"><path d="M19 21l-7-5-7 5V5a2 2 0 0 1 2-2h10a2 2 0 0 1 2 2z"/></symbol><symbol id="i-home" viewBox="0 0 24 24"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></symbol><symbol id="i-search" viewBox="0 0 24 24"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></symbol><symbol id="i-plus" viewBox="0 0 24 24"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></symbol><symbol id="i-bell" viewBox="0 0 24 24"><path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.73 21a2 2 0 0 1-3.46 0"/></symbol><symbol id="i-mail" viewBox="0 0 24 24"><path d="M4 4h16a2 2 0 0 1 2 2v12a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V6a2 2 0 0 1 2-2z"/><polyline points="22,6 12,13 2,6"/></symbol><symbol id="i-repeat" viewBox="0 0 24 24"><polyline points="17 1 21 5 17 9"/><path d="M3 11V9a4 4 0 0 1 4-4h14"/><polyline points="7 23 3 19 7 15"/><path d="M21 13v2a4 4 0 0 1-4 4H3"/></symbol><symbol id="i-share" viewBox="0 0 24 24"><circle cx="18" cy="5" r="3"/><circle cx="6" cy="12" r="3"/><circle cx="18" cy="19" r="3"/><line x1="8.59" y1="13.51" x2="15.42" y2="17.49"/><line x1="15.41" y1="6.51" x2="8.59" y2="10.49"/></symbol><symbol id="i-chart" viewBox="0 0 24 24"><line x1="18" y1="20" x2="18" y2="10"/><line x1="12" y1="20" x2="12" y2="4"/><line x1="6" y1="20" x2="6" y2="14"/></symbol><symbol id="i-upload" viewBox="0 0 24 24"><path d="M4 12v8a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2v-8"/><polyline points="16 6 12 2 8 6"/><line x1="12" y1="2" x2="12" y2="15"/></symbol><symbol id="i-more-h" viewBox="0 0 24 24"><circle cx="12" cy="12" r="1"/><circle cx="19" cy="12" r="1"/><circle cx="5" cy="12" r="1"/></symbol><symbol id="i-menu" viewBox="0 0 24 24"><line x1="3" y1="12" x2="21" y2="12"/><line x1="3" y1="6" x2="21" y2="6"/><line x1="3" y1="18" x2="21" y2="18"/></symbol><symbol id="i-reels" viewBox="0 0 24 24"><rect x="3" y="3" width="18" height="18" rx="5"/><polygon points="10 8 16 12 10 16"/></symbol><symbol id="i-gift" viewBox="0 0 24 24"><polyline points="20 12 20 22 4 22 4 12"/><rect x="2" y="7" width="20" height="5"/><line x1="12" y1="22" x2="12" y2="7"/><path d="M12 7H7.5a2.5 2.5 0 0 1 0-5C11 2 12 7 12 7z"/><path d="M12 7h4.5a2.5 2.5 0 0 0 0-5C13 2 12 7 12 7z"/></symbol><symbol id="i-plus-sq" viewBox="0 0 24 24"><rect x="3" y="3" width="18" height="18" rx="2"/><line x1="12" y1="8" x2="12" y2="16"/><line x1="8" y1="12" x2="16" y2="12"/></symbol><symbol id="i-user" viewBox="0 0 24 24"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></symbol><symbol id="i-image" viewBox="0 0 24 24"><rect x="3" y="3" width="18" height="18" rx="2"/><circle cx="8.5" cy="8.5" r="1.5"/><polyline points="21 15 16 10 5 21"/></symbol><symbol id="i-archive" viewBox="0 0 24 24"><polyline points="21 8 21 21 3 21 3 8"/><rect x="1" y="3" width="22" height="5"/><line x1="10" y1="12" x2="14" y2="12"/></symbol><symbol id="i-pen" viewBox="0 0 24 24"><path d="M12 20h9"/><path d="M16.5 3.5a2.12 2.12 0 0 1 3 3L7 19l-4 1 1-4z"/></symbol><symbol id="i-compose" viewBox="0 0 24 24"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/></symbol><symbol id="i-chev-down" viewBox="0 0 24 24"><polyline points="6 9 12 15 18 9"/></symbol><symbol id="i-chev-left" viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"/></symbol><symbol id="i-chev-right" viewBox="0 0 24 24"><polyline points="9 18 15 12 9 6"/></symbol><symbol id="i-star" viewBox="0 0 24 24"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></symbol><symbol id="i-clock" viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></symbol><symbol id="i-globe" viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><line x1="2" y1="12" x2="22" y2="12"/><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/></symbol><symbol id="i-timer" viewBox="0 0 24 24"><circle cx="12" cy="13" r="8"/><polyline points="12 9 12 13 14 15"/><line x1="10" y1="2" x2="14" y2="2"/></symbol><symbol id="i-keypad" viewBox="0 0 24 24"><circle cx="6" cy="6" r="1.4"/><circle cx="12" cy="6" r="1.4"/><circle cx="18" cy="6" r="1.4"/><circle cx="6" cy="12" r="1.4"/><circle cx="12" cy="12" r="1.4"/><circle cx="18" cy="12" r="1.4"/><circle cx="6" cy="18" r="1.4"/><circle cx="12" cy="18" r="1.4"/><circle cx="18" cy="18" r="1.4"/></symbol><symbol id="i-voicemail" viewBox="0 0 24 24"><circle cx="6" cy="12" r="4"/><circle cx="18" cy="12" r="4"/><line x1="6" y1="16" x2="18" y2="16"/></symbol><symbol id="i-play" viewBox="0 0 24 24"><polygon points="6 4 20 12 6 20 6 4"/></symbol><symbol id="i-pause" viewBox="0 0 24 24"><rect x="6" y="4" width="4" height="16"/><rect x="14" y="4" width="4" height="16"/></symbol><symbol id="i-fwd" viewBox="0 0 24 24"><polygon points="5 4 15 12 5 20 5 4"/><line x1="19" y1="5" x2="19" y2="19"/></symbol><symbol id="i-flash" viewBox="0 0 24 24"><polygon points="13 2 3 14 12 14 11 22 21 10 12 10 13 2"/></symbol><symbol id="i-flip" viewBox="0 0 24 24"><polyline points="23 4 23 10 17 10"/><polyline points="1 20 1 14 7 14"/><path d="M3.51 9a9 9 0 0 1 14.85-3.36L23 10M1 14l4.64 4.36A9 9 0 0 0 20.49 15"/></symbol><symbol id="i-radio" viewBox="0 0 24 24"><circle cx="12" cy="12" r="2"/><path d="M16.24 7.76a6 6 0 0 1 0 8.49m-8.48-.01a6 6 0 0 1 0-8.49m11.31-2.82a10 10 0 0 1 0 14.14m-14.14 0a10 10 0 0 1 0-14.14"/></symbol><symbol id="i-grid" viewBox="0 0 24 24"><rect x="3" y="3" width="7" height="7"/><rect x="14" y="3" width="7" height="7"/><rect x="14" y="14" width="7" height="7"/><rect x="3" y="14" width="7" height="7"/></symbol><symbol id="i-sun" viewBox="0 0 24 24"><circle cx="12" cy="12" r="4"/></symbol><symbol id="i-flag" viewBox="0 0 24 24"><path d="M4 15s1-1 4-1 5 2 8 2 4-1 4-1V3s-1 1-4 1-5-2-8-2-4 1-4 1z"/><line x1="4" y1="22" x2="4" y2="15"/></symbol><symbol id="i-list" viewBox="0 0 24 24"><line x1="8" y1="6" x2="21" y2="6"/><line x1="8" y1="12" x2="21" y2="12"/><line x1="8" y1="18" x2="21" y2="18"/><line x1="3" y1="6" x2="3.01" y2="6"/><line x1="3" y1="12" x2="3.01" y2="12"/><line x1="3" y1="18" x2="3.01" y2="18"/></symbol><symbol id="i-calendar" viewBox="0 0 24 24"><rect x="3" y="4" width="18" height="18" rx="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/></symbol><symbol id="i-book" viewBox="0 0 24 24"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></symbol><symbol id="i-tabs" viewBox="0 0 24 24"><rect x="7" y="3" width="14" height="14" rx="2"/><path d="M3 7v12a2 2 0 0 0 2 2h12"/></symbol><symbol id="i-back" viewBox="0 0 24 24"><line x1="19" y1="12" x2="5" y2="12"/><polyline points="12 19 5 12 12 5"/></symbol><symbol id="i-fwd2" viewBox="0 0 24 24"><line x1="5" y1="12" x2="19" y2="12"/><polyline points="12 5 19 12 12 19"/></symbol><symbol id="i-delete" viewBox="0 0 24 24"><path d="M21 4H8l-7 8 7 8h13a2 2 0 0 0 2-2V6a2 2 0 0 0-2-2z"/><line x1="18" y1="9" x2="12" y2="15"/><line x1="12" y1="9" x2="18" y2="15"/></symbol><symbol id="i-phone" viewBox="0 0 24 24"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/></symbol><symbol id="i-x" viewBox="0 0 24 24"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></symbol><symbol id="i-info" viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><line x1="12" y1="16" x2="12" y2="12"/><line x1="12" y1="8" x2="12.01" y2="8"/></symbol><symbol id="i-video" viewBox="0 0 24 24"><rect x="2" y="5" width="14" height="14" rx="2"/><polygon points="22 8 16 12 22 16 22 8"/></symbol><symbol id="i-mic" viewBox="0 0 24 24"><path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"/><path d="M19 10v2a7 7 0 0 1-14 0v-2"/><line x1="12" y1="19" x2="12" y2="23"/></symbol><symbol id="i-volume" viewBox="0 0 24 24"><polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"/><path d="M15.54 8.46a5 5 0 0 1 0 7.07"/><path d="M19.07 4.93a10 10 0 0 1 0 14.14"/></symbol></defs></svg>
<main class="phone" id="phone">

  <!-- 상태바 (잠금/홈 공통) -->
  <div class="status" id="status">
    <span class="s-time" id="sTime">--:--</span>
    <span class="island"></span>
    <span class="s-right">5G <span class="batt"><b id="battNum">74</b></span></span>
  </div>

  <!-- 홈화면 -->
  <section class="screen home" id="home">
    <div class="grid" id="appGrid"></div>
    <nav class="dock" id="dock"></nav>
    <div class="indicator dark"></div>
  </section>

  <!-- 잠금화면 -->
  <section class="screen lock" id="lock">
    <div class="lk-main" id="lkMain">
      <div class="lk-date" id="lockDate"></div>
      <div class="lk-time" id="lockTime"></div>
      <div class="lk-btn l" aria-hidden="true">
        <svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="#fff" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M8 3h8v4l-1.5 2.5V21h-5V9.5L8 7z"/><line x1="12" y1="12" x2="12" y2="14.5"/></svg>
      </div>
      <div class="lk-btn r" aria-hidden="true">
        <svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="#fff" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h4l2-3h6l2 3h4a2 2 0 0 1 2 2z"/><circle cx="12" cy="13" r="4"/></svg>
      </div>
      <div class="lk-hint">위로 쓸어올리거나 눌러서 열기</div>
    </div>
    <div class="lk-pass" id="lkPass">
      <div class="pp-title">암호 입력</div>
      <div class="dots" id="dots"></div>
      <div class="keys" id="keys"></div>
      <div class="pp-foot"><span>긴급</span><button type="button" id="pwCancel">취소</button></div>
    </div>
    <div class="indicator"></div>
  </section>


<section class="view " id="v-instagram" data-status="dark" style="--tint:#111"><div class="vtop"><div class="ig-top"><svg class="ic lg"><use href="#i-plus"/></svg><div class="ig-logo">Instagram <svg class="ic xs"><use href="#i-chev-down"/></svg></div><div class="ig-r"><svg class="ic "><use href="#i-reels"/></svg><svg class="ic "><use href="#i-heart"/></svg></div></div></div><div class="vscroll"><div class="ig-stories"><div class="st"><div class="ring"><div class="slot av" data-img></div><i class="badge">+</i></div><b>내 스토리</b></div><div class="st" data-sv="sv-1"><div class="ring on"><div class="slot av st-av" data-mirror="sv-1"></div></div><b><span data-e data-ph="아이디"></span></b></div><div class="st" data-sv="sv-2"><div class="ring on"><div class="slot av st-av" data-mirror="sv-2"></div></div><b><span data-e data-ph="아이디"></span></b></div><div class="st"><div class="ring seen"><div class="slot av" data-img></div></div><b><span data-e data-ph="아이디"></span></b></div><div class="st"><div class="ring seen"><div class="slot av" data-img></div></div><b><span data-e data-ph="아이디"></span></b></div></div><article class="ig-p"><div class="ig-ph"><div class="slot av sm" data-img></div><div class="who"><span data-e data-ph="아이디"></span><small>회원님을 위한 추천</small></div><button class="pill" data-a="팔로우" data-b="팔로잉">팔로우</button><svg class="ic "><use href="#i-more-h"/></svg></div><div class="slot sq" data-img></div><div class="ig-act"><svg class="ic "><use href="#i-heart"/></svg><svg class="ic "><use href="#i-comment"/></svg><svg class="ic "><use href="#i-send"/></svg><span class="sp"></span><svg class="ic "><use href="#i-bookmark"/></svg></div><div class="ig-meta">좋아요 <span data-e data-ph="0"></span>개</div><div class="ig-cap"><b><span data-e data-ph="아이디"></span></b> <span data-e data-ph="내용을 입력하세요" data-ml></span></div></article><article class="ig-p"><div class="ig-ph"><div class="slot av sm" data-img></div><div class="who"><span data-e data-ph="아이디"></span></div><svg class="ic "><use href="#i-more-h"/></svg></div><div class="slot sq" data-img></div><div class="ig-act"><svg class="ic "><use href="#i-heart"/></svg><svg class="ic "><use href="#i-comment"/></svg><svg class="ic "><use href="#i-send"/></svg><span class="sp"></span><svg class="ic "><use href="#i-bookmark"/></svg></div><div class="ig-meta">좋아요 <span data-e data-ph="0"></span>개</div><div class="ig-cap"><b><span data-e data-ph="아이디"></span></b> <span data-e data-ph="내용을 입력하세요" data-ml></span></div></article></div><nav class="tabbar icons ig-tb"><button class="tb on"><svg class="ic fill"><use href="#i-home"/></svg></button><button class="tb"><svg class="ic "><use href="#i-reels"/></svg></button><button class="tb"><svg class="ic "><use href="#i-send"/></svg></button><button class="tb"><svg class="ic "><use href="#i-search"/></svg></button><button class="tb"><div class="slot av xs" data-img></div></button></nav><div class="sv" id="sv-1"><div class="sv-img on" data-img></div><div class="sv-img" data-img></div><div class="sv-shade"></div><div class="sv-bars"><span><i></i></span><span><i></i></span></div><div class="sv-head"><div class="slot av sv-av" data-img></div><b class="sv-name"></b><span class="sv-time"><span data-e data-ph="29분"></span></span><span class="sv-sp"></span><button class="sv-btn" data-change aria-label="사진 변경"><svg class="ic "><use href="#i-image"/></svg></button><button class="sv-btn" data-close aria-label="닫기"><svg class="ic "><use href="#i-x"/></svg></button></div><div class="sv-l"></div><div class="sv-r"></div><div class="sv-foot"><input class="sv-in" placeholder="메시지 보내기" enterkeyhint="send"><button class="sv-btn sv-heart" aria-label="좋아요"><svg class="ic "><use href="#i-heart"/></svg></button><span class="sv-btn"><svg class="ic "><use href="#i-comment"/></svg></span><span class="sv-btn"><svg class="ic "><use href="#i-send"/></svg></span></div><a class="vind" data-back aria-label="홈으로"></a></div><div class="sv" id="sv-2"><div class="sv-img on" data-img></div><div class="sv-img" data-img></div><div class="sv-shade"></div><div class="sv-bars"><span><i></i></span><span><i></i></span></div><div class="sv-head"><div class="slot av sv-av" data-img></div><b class="sv-name"></b><span class="sv-time"><span data-e data-ph="29분"></span></span><span class="sv-sp"></span><button class="sv-btn" data-change aria-label="사진 변경"><svg class="ic "><use href="#i-image"/></svg></button><button class="sv-btn" data-close aria-label="닫기"><svg class="ic "><use href="#i-x"/></svg></button></div><div class="sv-l"></div><div class="sv-r"></div><div class="sv-foot"><input class="sv-in" placeholder="메시지 보내기" enterkeyhint="send"><button class="sv-btn sv-heart" aria-label="좋아요"><svg class="ic "><use href="#i-heart"/></svg></button><span class="sv-btn"><svg class="ic "><use href="#i-comment"/></svg></span><span class="sv-btn"><svg class="ic "><use href="#i-send"/></svg></span></div><a class="vind" data-back aria-label="홈으로"></a></div><a class="vind" data-back aria-label="홈으로"></a></section>
<section class="view x" id="v-x" data-status="dark" style=""><div class="vtop"><div class="x-top"><div class="slot av xs" data-img></div><div class="x-logo">X</div><button class="pill x-sub" data-a="구독하기" data-b="구독 중" style="background:#fff">구독하기</button></div><div class="tabs"><button class="on">추천</button><button>팔로잉</button><button class="plus">+</button></div></div><div class="vscroll" style="border-top:1px solid #eff3f4"><article class="tw"><div class="slot av" data-img></div><div class="tw-b"><div class="tw-h"><b><span data-e data-ph="이름"></span></b><span class="mut"><span data-e data-ph="@아이디"></span></span><span class="mut">· <span data-e data-ph="1시간"></span></span></div><div class="tw-t"><span data-e data-ph="트윗 내용을 입력하세요" data-ml></span></div><div class="tw-a"><span><svg class="ic "><use href="#i-comment"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-repeat"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-heart"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-chart"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-bookmark"/></svg><svg class="ic "><use href="#i-upload"/></svg></span></div></div></article><article class="tw"><div class="slot av" data-img></div><div class="tw-b"><div class="tw-h"><b><span data-e data-ph="이름"></span></b><span class="mut"><span data-e data-ph="@아이디"></span></span><span class="mut">· <span data-e data-ph="1시간"></span></span></div><div class="tw-t"><span data-e data-ph="트윗 내용을 입력하세요" data-ml></span></div><div class="tw-a"><span><svg class="ic "><use href="#i-comment"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-repeat"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-heart"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-chart"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-bookmark"/></svg><svg class="ic "><use href="#i-upload"/></svg></span></div></div></article><article class="tw"><div class="slot av" data-img></div><div class="tw-b"><div class="tw-h"><b><span data-e data-ph="이름"></span></b><span class="mut"><span data-e data-ph="@아이디"></span></span><span class="mut">· <span data-e data-ph="1시간"></span></span></div><div class="tw-t"><span data-e data-ph="트윗 내용을 입력하세요" data-ml></span></div><div class="tw-a"><span><svg class="ic "><use href="#i-comment"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-repeat"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-heart"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-chart"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-bookmark"/></svg><svg class="ic "><use href="#i-upload"/></svg></span></div></div></article><article class="tw"><div class="slot av" data-img></div><div class="tw-b"><div class="tw-h"><b><span data-e data-ph="이름"></span></b><span class="mut"><span data-e data-ph="@아이디"></span></span><span class="mut">· <span data-e data-ph="1시간"></span></span></div><div class="tw-t"><span data-e data-ph="트윗 내용을 입력하세요" data-ml></span></div><div class="tw-a"><span><svg class="ic "><use href="#i-comment"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-repeat"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-heart"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-chart"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-bookmark"/></svg><svg class="ic "><use href="#i-upload"/></svg></span></div></div></article></div><div class="fab"><svg class="ic "><use href="#i-compose"/></svg></div><nav class="tabbar icons x-tb"><button class="tb"><svg class="ic fill"><use href="#i-home"/></svg></button><button class="tb"><svg class="ic "><use href="#i-search"/></svg></button><button class="tb"><svg class="ic "><use href="#i-bell"/></svg></button><button class="tb"><svg class="ic "><use href="#i-mail"/></svg></button></nav><a class="vind" data-back aria-label="홈으로"></a></section>
<section class="view " id="v-postype" data-status="dark" style="--tint:#111"><div class="vtop"><div class="ps-top"><svg class="ic "><use href="#i-menu"/></svg><div class="ps-logo">POSTYPE</div><svg class="ic "><use href="#i-plus-sq"/></svg><span class="dotw"><svg class="ic "><use href="#i-gift"/></svg><i></i></span><span class="dotw"><svg class="ic "><use href="#i-bell"/></svg><i></i></span></div><div class="tabs ps-tabs"><button class="on">발견</button><button>구독·참여</button></div></div><div class="vscroll"><article class="ps-card"><div class="ps-h"><div class="slot av" data-img></div><div class="who"><span data-e data-ph="작가 이름"></span><small><span data-e data-ph="2026. 1. 1."></span> · <span data-e data-ph="시리즈"></span></small></div><svg class="ic "><use href="#i-more-h"/></svg></div><div class="ps-t"><span class="tt"><span data-e data-ph="제목"></span></span><span data-e data-ph="본문이 여기에 보여요" data-ml></span></div><div class="chips"><span class="chip"><span data-e data-ph="태그"></span></span><span class="chip"><span data-e data-ph="태그"></span></span><span class="chip"><span data-e data-ph="태그"></span></span><span class="chip"><span data-e data-ph="태그"></span></span></div><div class="ps-v">조회 <span data-e data-ph="0"></span></div><div class="ps-a"><span><svg class="ic "><use href="#i-heart"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-comment"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-repeat"/></svg><span data-e data-ph="0"></span></span><span class="sp"></span><svg class="ic "><use href="#i-bookmark"/></svg><svg class="ic "><use href="#i-upload"/></svg></div><div class="ps-cm"><span data-e data-ph="댓글 미리보기" data-ml></span></div></article><article class="ps-card"><div class="ps-h"><div class="slot av" data-img></div><div class="who"><span data-e data-ph="작가 이름"></span><small><span data-e data-ph="2026. 1. 1."></span> · <span data-e data-ph="시리즈"></span></small></div><button class="pill" data-a="구독" data-b="구독 중">구독</button><svg class="ic "><use href="#i-more-h"/></svg></div><div class="ps-t"><span class="tt"><span data-e data-ph="제목"></span></span><span data-e data-ph="본문이 여기에 보여요" data-ml></span></div><div class="chips"><span class="chip"><span data-e data-ph="태그"></span></span><span class="chip"><span data-e data-ph="태그"></span></span><span class="chip"><span data-e data-ph="태그"></span></span><span class="chip"><span data-e data-ph="태그"></span></span></div><div class="ps-v">조회 <span data-e data-ph="0"></span></div><div class="ps-a"><span><svg class="ic "><use href="#i-heart"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-comment"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-repeat"/></svg><span data-e data-ph="0"></span></span><span class="sp"></span><svg class="ic "><use href="#i-bookmark"/></svg><svg class="ic "><use href="#i-upload"/></svg></div><div class="ps-cm"><span data-e data-ph="댓글 미리보기" data-ml></span></div></article><article class="ps-card"><div class="ps-h"><div class="slot av" data-img></div><div class="who"><span data-e data-ph="작가 이름"></span><small><span data-e data-ph="2026. 1. 1."></span> · <span data-e data-ph="시리즈"></span></small></div><svg class="ic "><use href="#i-more-h"/></svg></div><div class="ps-t"><span class="tt"><span data-e data-ph="제목"></span></span><span data-e data-ph="본문이 여기에 보여요" data-ml></span></div><div class="chips"><span class="chip"><span data-e data-ph="태그"></span></span><span class="chip"><span data-e data-ph="태그"></span></span><span class="chip"><span data-e data-ph="태그"></span></span><span class="chip"><span data-e data-ph="태그"></span></span></div><div class="ps-v">조회 <span data-e data-ph="0"></span></div><div class="ps-a"><span><svg class="ic "><use href="#i-heart"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-comment"/></svg><span data-e data-ph="0"></span></span><span><svg class="ic "><use href="#i-repeat"/></svg><span data-e data-ph="0"></span></span><span class="sp"></span><svg class="ic "><use href="#i-bookmark"/></svg><svg class="ic "><use href="#i-upload"/></svg></div><div class="ps-cm"><span data-e data-ph="댓글 미리보기" data-ml></span></div></article></div><nav class="tabbar ps-tb"><button class="tb"><svg class="ic "><use href="#i-comment"/></svg><span>캐릭터톡</span></button><button class="tb on"><svg class="ic fill"><use href="#i-home"/></svg><span>홈</span></button><button class="tb"><svg class="ic "><use href="#i-pen"/></svg><span>리퀘스트</span></button><button class="tb"><svg class="ic "><use href="#i-search"/></svg><span>검색</span></button><button class="tb"><svg class="ic "><use href="#i-archive"/></svg><span>보관함</span></button><button class="tb"><svg class="ic "><use href="#i-user"/></svg><span>마이메뉴</span></button></nav><a class="vind" data-back aria-label="홈으로"></a></section>
<section class="view cal" id="v-calendar" data-status="dark" style=""><div class="vtop"><div class="cal-h"><span>‹ <span id="calYear"></span></span><span class="cal-i"><svg class="ic "><use href="#i-search"/></svg><svg class="ic "><use href="#i-plus"/></svg></span></div></div><div class="vscroll"><div class="cal-title"><span id="calTitle"></span><span><button id="calPrev">‹</button><button id="calNext">›</button></span></div><div class="cal-g" id="cal"></div><div class="cal-ev"><div class="d" id="calSel"></div><span data-e data-ph="일정 없음" data-ml></span></div></div><div class="tools"><span id="calToday">오늘</span><span>캘린더</span><span>받은 편지함</span></div><a class="vind" data-back aria-label="홈으로"></a></section>
<section class="view " id="v-photos" data-status="dark" style=""><div class="vtop"><div class="bar2"><span></span><span style="color:#0a84ff">선택</span></div></div><div class="vscroll"><div class="big-t">보관함</div><div class="seg"><button>연</button><button>월</button><button>일</button><button class="on">모든 사진</button></div><div class="ph-g"><div class="slot " data-img></div><div class="slot " data-img></div><div class="slot " data-img></div><div class="slot " data-img></div><div class="slot " data-img></div><div class="slot " data-img></div><div class="slot " data-img></div><div class="slot " data-img></div><div class="slot " data-img></div><div class="slot " data-img></div><div class="slot " data-img></div><div class="slot " data-img></div><div class="slot " data-img></div><div class="slot " data-img></div><div class="slot " data-img></div></div></div><nav class="tabbar "><button class="tb on"><svg class="ic "><use href="#i-image"/></svg><span>보관함</span></button><button class="tb"><svg class="ic "><use href="#i-heart"/></svg><span>나를 위한</span></button><button class="tb"><svg class="ic "><use href="#i-archive"/></svg><span>앨범</span></button><button class="tb"><svg class="ic "><use href="#i-search"/></svg><span>검색</span></button></nav><a class="vind" data-back aria-label="홈으로"></a></section>
<section class="view dk cam" id="v-camera" data-status="light" style=""><div class="vtop"><div class="cam-top"><svg class="ic "><use href="#i-flash"/></svg><svg class="ic "><use href="#i-chev-down"/></svg><svg class="ic "><use href="#i-timer"/></svg></div></div><div class="cam-vf" id="camVf">카메라 미리보기<div class="zoom"><b>.5</b><b class="on">1×</b><b>2</b></div></div><div class="modes"><span>타임랩스</span><span>슬로모</span><span>비디오</span><span class="on">사진</span><span>인물사진</span><span>파노라마</span></div><div class="shut-row"><div class="slot " data-img></div><button class="shut" id="shut" aria-label="촬영"></button><div class="flip"><svg class="ic "><use href="#i-flip"/></svg></div></div><a class="vind" data-back aria-label="홈으로"></a></section>
<section class="view dk ck" id="v-clock" data-status="light" style=""><div class="vtop"><div class="ck-h"><span>편집</span><svg class="ic "><use href="#i-plus"/></svg></div></div><div class="vscroll"><div class="pane on" id="p-world"><div class="ck-t">세계 시계</div><div class="ck-row"><div class="l"><small><span data-e data-ph="오늘, +0시간"></span></small><span data-e data-ph="도시"></span></div><div class="r"><span data-e data-ph="--:--"></span></div></div><div class="ck-row"><div class="l"><small><span data-e data-ph="오늘, +0시간"></span></small><span data-e data-ph="도시"></span></div><div class="r"><span data-e data-ph="--:--"></span></div></div><div class="ck-row"><div class="l"><small><span data-e data-ph="오늘, +0시간"></span></small><span data-e data-ph="도시"></span></div><div class="r"><span data-e data-ph="--:--"></span></div></div><div class="ck-row"><div class="l"><small><span data-e data-ph="오늘, +0시간"></span></small><span data-e data-ph="도시"></span></div><div class="r"><span data-e data-ph="--:--"></span></div></div></div><div class="pane" id="p-alarm"><div class="ck-t">알람</div><div class="ck-row"><div class="l"><div class="at"><span data-e data-ph="--:--"></span></div><small style="margin:0"><span data-e data-ph="알람"></span></small></div><i class="sw"></i></div><div class="ck-row"><div class="l"><div class="at"><span data-e data-ph="--:--"></span></div><small style="margin:0"><span data-e data-ph="알람"></span></small></div><i class="sw"></i></div><div class="ck-row"><div class="l"><div class="at"><span data-e data-ph="--:--"></span></div><small style="margin:0"><span data-e data-ph="알람"></span></small></div><i class="sw"></i></div></div><div class="pane" id="p-sw"><div class="sw-t" id="swT">00:00.00</div><div class="sw-b"><button class="rb s" id="swLap">랩</button><button class="rb g" id="swGo">시작</button></div><div id="laps"></div></div><div class="pane" id="p-tm"><div class="sw-t" id="tmT">05:00</div><div class="tm-in"><input id="tmIn" type="number" min="1" value="5" inputmode="numeric"> 분</div><div class="sw-b"><button class="rb s" id="tmReset">취소</button><button class="rb g" id="tmGo">시작</button></div></div></div><nav class="tabbar "><button class="tb on" data-pane="p-world"><svg class="ic "><use href="#i-globe"/></svg><span>세계 시계</span></button><button class="tb" data-pane="p-alarm"><svg class="ic "><use href="#i-bell"/></svg><span>알람</span></button><button class="tb" data-pane="p-sw"><svg class="ic "><use href="#i-clock"/></svg><span>스톱워치</span></button><button class="tb" data-pane="p-tm"><svg class="ic "><use href="#i-timer"/></svg><span>타이머</span></button></nav><a class="vind" data-back aria-label="홈으로"></a></section>
<section class="view nt" id="v-notes" data-status="dark" style="--bg:#f2f2f7"><div class="vtop"><div class="nt-h"><span>‹ 폴더</span><svg class="ic "><use href="#i-more-h"/></svg></div></div><div class="vscroll"><div class="big-t">메모</div><div class="srch"><svg class="ic "><use href="#i-search"/></svg> 검색</div><div class="sec" style="font-size:19px">오늘</div><div class="grp"><div class="row"><div class="t"><span data-e data-ph="제목"></span></div><div class="s"><span data-e data-ph="날짜"></span> <span data-e data-ph="미리보기"></span></div></div><div class="row"><div class="t"><span data-e data-ph="제목"></span></div><div class="s"><span data-e data-ph="날짜"></span> <span data-e data-ph="미리보기"></span></div></div><div class="row"><div class="t"><span data-e data-ph="제목"></span></div><div class="s"><span data-e data-ph="날짜"></span> <span data-e data-ph="미리보기"></span></div></div></div><div class="sec" style="font-size:19px">지난 30일</div><div class="grp"><div class="row"><div class="t"><span data-e data-ph="제목"></span></div><div class="s"><span data-e data-ph="날짜"></span> <span data-e data-ph="미리보기"></span></div></div><div class="row"><div class="t"><span data-e data-ph="제목"></span></div><div class="s"><span data-e data-ph="날짜"></span> <span data-e data-ph="미리보기"></span></div></div><div class="row"><div class="t"><span data-e data-ph="제목"></span></div><div class="s"><span data-e data-ph="날짜"></span> <span data-e data-ph="미리보기"></span></div></div></div></div><div class="tools nt-t"><span></span><span>메모 <span data-e data-ph="0"></span>개</span><svg class="ic "><use href="#i-compose"/></svg></div><a class="vind" data-back aria-label="홈으로"></a></section>
<section class="view wx" id="v-weather" data-status="light" style=""><div class="vscroll pt"><div class="wx-c"><div class="wx-city"><span data-e data-ph="도시"></span></div><div class="wx-temp"><span data-e data-ph="--°"></span></div><div class="wx-cond"><span data-e data-ph="날씨"></span></div><div class="wx-hl">최고 <span data-e data-ph="--°"></span> · 최저 <span data-e data-ph="--°"></span></div></div><div class="glass"><h4>시간별 예보</h4><div class="hr"><div><span data-e data-ph="시"></span><svg class="ic sm"><use href="#i-sun"/></svg><span data-e data-ph="--°"></span></div><div><span data-e data-ph="시"></span><svg class="ic sm"><use href="#i-sun"/></svg><span data-e data-ph="--°"></span></div><div><span data-e data-ph="시"></span><svg class="ic sm"><use href="#i-sun"/></svg><span data-e data-ph="--°"></span></div><div><span data-e data-ph="시"></span><svg class="ic sm"><use href="#i-sun"/></svg><span data-e data-ph="--°"></span></div><div><span data-e data-ph="시"></span><svg class="ic sm"><use href="#i-sun"/></svg><span data-e data-ph="--°"></span></div></div></div><div class="glass"><h4>6일간의 일기예보</h4><div class="dy"><span data-day="0"></span><svg class="ic sm"><use href="#i-sun"/></svg><span><span data-e data-ph="--°"></span></span><span class="hi"><span data-e data-ph="--°"></span></span></div><div class="dy"><span data-day="1"></span><svg class="ic sm"><use href="#i-sun"/></svg><span><span data-e data-ph="--°"></span></span><span class="hi"><span data-e data-ph="--°"></span></span></div><div class="dy"><span data-day="2"></span><svg class="ic sm"><use href="#i-sun"/></svg><span><span data-e data-ph="--°"></span></span><span class="hi"><span data-e data-ph="--°"></span></span></div><div class="dy"><span data-day="3"></span><svg class="ic sm"><use href="#i-sun"/></svg><span><span data-e data-ph="--°"></span></span><span class="hi"><span data-e data-ph="--°"></span></span></div><div class="dy"><span data-day="4"></span><svg class="ic sm"><use href="#i-sun"/></svg><span><span data-e data-ph="--°"></span></span><span class="hi"><span data-e data-ph="--°"></span></span></div><div class="dy"><span data-day="5"></span><svg class="ic sm"><use href="#i-sun"/></svg><span><span data-e data-ph="--°"></span></span><span class="hi"><span data-e data-ph="--°"></span></span></div></div><div style="height:50px"></div></div><a class="vind" data-back aria-label="홈으로"></a></section>
<section class="view " id="v-reminders" data-status="dark" style="--bg:#f2f2f7"><div class="vtop"><div class="nt-h" style="color:#0a84ff"><span></span><span>편집</span></div></div><div class="vscroll"><div class="srch"><svg class="ic "><use href="#i-search"/></svg> 검색</div><div class="tiles"><div class="tile"><div><div class="c" style="background:#0a84ff"><svg class="ic "><use href="#i-calendar"/></svg></div><div class="nm">오늘</div></div><div class="n"><span data-e data-ph="0"></span></div></div><div class="tile"><div><div class="c" style="background:#ff3b30"><svg class="ic "><use href="#i-clock"/></svg></div><div class="nm">예정</div></div><div class="n"><span data-e data-ph="0"></span></div></div><div class="tile"><div><div class="c" style="background:#3a3a3c"><svg class="ic "><use href="#i-archive"/></svg></div><div class="nm">전체</div></div><div class="n"><span data-e data-ph="0"></span></div></div><div class="tile"><div><div class="c" style="background:#ff9500"><svg class="ic "><use href="#i-flag"/></svg></div><div class="nm">깃발 표시</div></div><div class="n"><span data-e data-ph="0"></span></div></div></div><div class="sec">나의 목록</div><div class="grp"><div class="row lrow"><span class="dot" style="background:#ff9500"></span><span class="nm"><span data-e data-ph="목록 이름"></span></span><span data-e data-ph="0"></span></div><div class="row lrow"><span class="dot" style="background:#af52de"></span><span class="nm"><span data-e data-ph="목록 이름"></span></span><span data-e data-ph="0"></span></div></div><div class="sec">새 미리 알림</div><div data-todo><div class="todo-add"><input placeholder="미리 알림 입력"><button type="button">추가</button></div><ul class="todo-list"></ul></div><div class="hint">항목을 누르면 완료, 길게 누르면 삭제돼요.</div></div><a class="vind" data-back aria-label="홈으로"></a></section>
<section class="view " id="v-search" data-status="dark" style="--bg:#f2f2f7"><div class="vtop"><div class="sp-in"><svg class="ic sm"><use href="#i-search"/></svg><input id="spIn" placeholder="검색"></div></div><div class="vscroll" id="spList"></div><a class="vind" data-back aria-label="홈으로"></a></section>
<section class="view " id="v-browser" data-status="dark" style="--bg:#f2f2f7"><div class="vscroll pt"><div class="big-t">즐겨찾기</div><div class="fav-g"><div class="fav"><div class="t"><span data-e data-ph="＋"></span></div><span data-e data-ph="사이트"></span></div><div class="fav"><div class="t"><span data-e data-ph="＋"></span></div><span data-e data-ph="사이트"></span></div><div class="fav"><div class="t"><span data-e data-ph="＋"></span></div><span data-e data-ph="사이트"></span></div><div class="fav"><div class="t"><span data-e data-ph="＋"></span></div><span data-e data-ph="사이트"></span></div><div class="fav"><div class="t"><span data-e data-ph="＋"></span></div><span data-e data-ph="사이트"></span></div><div class="fav"><div class="t"><span data-e data-ph="＋"></span></div><span data-e data-ph="사이트"></span></div><div class="fav"><div class="t"><span data-e data-ph="＋"></span></div><span data-e data-ph="사이트"></span></div><div class="fav"><div class="t"><span data-e data-ph="＋"></span></div><span data-e data-ph="사이트"></span></div></div></div><div class="sf-bar"><div class="sf-in"><svg class="ic sm"><use href="#i-search"/></svg><input id="sfIn" placeholder="검색 또는 웹 사이트 이름 입력" enterkeyhint="go"></div><div class="sf-tools"><svg class="ic "><use href="#i-back"/></svg><svg class="ic "><use href="#i-fwd2"/></svg><svg class="ic "><use href="#i-upload"/></svg><svg class="ic "><use href="#i-book"/></svg><svg class="ic "><use href="#i-tabs"/></svg></div></div><a class="vind" data-back aria-label="홈으로"></a></section>
<section class="view " id="v-phone" data-status="dark" style=""><div class="vscroll pt"><div class="pane" id="p-fav"><div class="bar2"><span class="pn-edit blue">편집</span><span></span></div><div class="big-t">즐겨찾기</div><div class="rc-row" data-call><div class="rc-b"><div class="rc-n"><span data-e data-nm data-ph="이름"></span></div><small>휴대전화</small></div><span class="rc-t"><span data-e data-ph="오후 1:00"></span></span><svg class="ic "><use href="#i-info"/></svg></div><div class="rc-row" data-call><div class="rc-b"><div class="rc-n"><span data-e data-nm data-ph="이름"></span></div><small>휴대전화</small></div><span class="rc-t"><span data-e data-ph="오후 1:00"></span></span><svg class="ic "><use href="#i-info"/></svg></div><div class="rc-row" data-call><div class="rc-b"><div class="rc-n"><span data-e data-nm data-ph="이름"></span></div><small>휴대전화</small></div><span class="rc-t"><span data-e data-ph="오후 1:00"></span></span><svg class="ic "><use href="#i-info"/></svg></div></div><div class="pane" id="p-rec"><div class="bar2"><span class="pn-edit blue">편집</span><span></span></div><div class="big-t">최근 기록</div><div class="seg"><button class="on">전체</button><button>부재중</button></div><div class="rc-row" data-call><div class="rc-b"><div class="rc-n"><span data-e data-nm data-ph="이름"></span></div><small>휴대전화</small></div><span class="rc-t"><span data-e data-ph="오후 1:00"></span></span><svg class="ic "><use href="#i-info"/></svg></div><div class="rc-row" data-call><div class="rc-b"><div class="rc-n"><span data-e data-nm data-ph="이름"></span></div><small>휴대전화</small></div><span class="rc-t"><span data-e data-ph="오후 1:00"></span></span><svg class="ic "><use href="#i-info"/></svg></div><div class="rc-row" data-call><div class="rc-b"><div class="rc-n"><span data-e data-nm data-ph="이름"></span></div><small>휴대전화</small></div><span class="rc-t"><span data-e data-ph="오후 1:00"></span></span><svg class="ic "><use href="#i-info"/></svg></div><div class="rc-row" data-call><div class="rc-b"><div class="rc-n"><span data-e data-nm data-ph="이름"></span></div><small>휴대전화</small></div><span class="rc-t"><span data-e data-ph="오후 1:00"></span></span><svg class="ic "><use href="#i-info"/></svg></div><div class="rc-row" data-call><div class="rc-b"><div class="rc-n"><span data-e data-nm data-ph="이름"></span></div><small>휴대전화</small></div><span class="rc-t"><span data-e data-ph="오후 1:00"></span></span><svg class="ic "><use href="#i-info"/></svg></div><div class="rc-row" data-call><div class="rc-b"><div class="rc-n"><span data-e data-nm data-ph="이름"></span></div><small>휴대전화</small></div><span class="rc-t"><span data-e data-ph="오후 1:00"></span></span><svg class="ic "><use href="#i-info"/></svg></div><div class="rc-row" data-call><div class="rc-b"><div class="rc-n"><span data-e data-nm data-ph="이름"></span></div><small>휴대전화</small></div><span class="rc-t"><span data-e data-ph="오후 1:00"></span></span><svg class="ic "><use href="#i-info"/></svg></div></div><div class="pane" id="p-con"><div class="bar2"><span class="pn-edit blue">편집</span><span></span></div><div class="big-t">연락처</div><div class="srch"><svg class="ic "><use href="#i-search"/></svg> 검색</div><div class="ct-row" data-ct="ct-0"><span data-e data-ph="이름"></span></div><div class="ct-row" data-ct="ct-1"><span data-e data-ph="이름"></span></div><div class="ct-row" data-ct="ct-2"><span data-e data-ph="이름"></span></div><div class="ct-row" data-ct="ct-3"><span data-e data-ph="이름"></span></div><div class="ct-row" data-ct="ct-4"><span data-e data-ph="이름"></span></div><div class="ct-row" data-ct="ct-5"><span data-e data-ph="이름"></span></div></div><div class="pane on" id="p-key"><div class="pn-d" id="pnD"></div><div class="pad"><button data-k="1">1<small></small></button><button data-k="2">2<small>ABC</small></button><button data-k="3">3<small>DEF</small></button><button data-k="4">4<small>GHI</small></button><button data-k="5">5<small>JKL</small></button><button data-k="6">6<small>MNO</small></button><button data-k="7">7<small>PQRS</small></button><button data-k="8">8<small>TUV</small></button><button data-k="9">9<small>WXYZ</small></button><button data-k="*">*<small></small></button><button data-k="0">0<small>+</small></button><button data-k="#">#<small></small></button><button class="call" aria-label="전화 걸기"><svg class="ic fill lg"><use href="#i-phone"/></svg></button><button class="del" id="pnDel" aria-label="지우기"><svg class="ic lg"><use href="#i-delete"/></svg></button></div></div><div class="pane" id="p-vm"><div class="big-t" style="padding-top:30px">음성 사서함</div><div class="empty">음성 사서함 없음</div></div></div><nav class="tabbar "><button class="tb" data-pane="p-fav"><svg class="ic "><use href="#i-star"/></svg><span>즐겨찾기</span></button><button class="tb" data-pane="p-rec"><svg class="ic "><use href="#i-clock"/></svg><span>최근 기록</span></button><button class="tb" data-pane="p-con"><svg class="ic "><use href="#i-user"/></svg><span>연락처</span></button><button class="tb on" data-pane="p-key"><svg class="ic "><use href="#i-keypad"/></svg><span>키패드</span></button><button class="tb" data-pane="p-vm"><svg class="ic "><use href="#i-voicemail"/></svg><span>음성 사서함</span></button></nav><div class="ct-detail" id="ct-0"><div class="vtop"><div class="bar2"><span class="blue" data-ctback>‹ 연락처</span><span class="pn-edit blue">편집</span></div></div><div class="vscroll" style="padding-bottom:40px"><div class="ctd-top"><div class="av ctd-av"></div><div class="ctd-name"></div></div><div class="ctd-act"><button data-call><svg class="ic fill"><use href="#i-phone"/></svg><span>전화</span></button><button><svg class="ic "><use href="#i-comment"/></svg><span>메시지</span></button><button><svg class="ic "><use href="#i-video"/></svg><span>FaceTime</span></button><button><svg class="ic "><use href="#i-mail"/></svg><span>메일</span></button></div><div class="grp"><div class="row"><div class="s">휴대전화</div><div class="t blue"><span data-e data-ph="전화번호"></span></div></div></div><div class="sec" style="font-size:15px;color:#8e8e93">최근 기록</div><div class="grp"><div class="row hist" data-call><div><span class="nm">발신 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div><div class="row hist" data-call><div><span class="nm">수신 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div><div class="row hist miss" data-call><div><span class="nm">부재중 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div></div></div></div><div class="ct-detail" id="ct-1"><div class="vtop"><div class="bar2"><span class="blue" data-ctback>‹ 연락처</span><span class="pn-edit blue">편집</span></div></div><div class="vscroll" style="padding-bottom:40px"><div class="ctd-top"><div class="av ctd-av"></div><div class="ctd-name"></div></div><div class="ctd-act"><button data-call><svg class="ic fill"><use href="#i-phone"/></svg><span>전화</span></button><button><svg class="ic "><use href="#i-comment"/></svg><span>메시지</span></button><button><svg class="ic "><use href="#i-video"/></svg><span>FaceTime</span></button><button><svg class="ic "><use href="#i-mail"/></svg><span>메일</span></button></div><div class="grp"><div class="row"><div class="s">휴대전화</div><div class="t blue"><span data-e data-ph="전화번호"></span></div></div></div><div class="sec" style="font-size:15px;color:#8e8e93">최근 기록</div><div class="grp"><div class="row hist" data-call><div><span class="nm">발신 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div><div class="row hist" data-call><div><span class="nm">수신 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div><div class="row hist miss" data-call><div><span class="nm">부재중 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div></div></div></div><div class="ct-detail" id="ct-2"><div class="vtop"><div class="bar2"><span class="blue" data-ctback>‹ 연락처</span><span class="pn-edit blue">편집</span></div></div><div class="vscroll" style="padding-bottom:40px"><div class="ctd-top"><div class="av ctd-av"></div><div class="ctd-name"></div></div><div class="ctd-act"><button data-call><svg class="ic fill"><use href="#i-phone"/></svg><span>전화</span></button><button><svg class="ic "><use href="#i-comment"/></svg><span>메시지</span></button><button><svg class="ic "><use href="#i-video"/></svg><span>FaceTime</span></button><button><svg class="ic "><use href="#i-mail"/></svg><span>메일</span></button></div><div class="grp"><div class="row"><div class="s">휴대전화</div><div class="t blue"><span data-e data-ph="전화번호"></span></div></div></div><div class="sec" style="font-size:15px;color:#8e8e93">최근 기록</div><div class="grp"><div class="row hist" data-call><div><span class="nm">발신 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div><div class="row hist" data-call><div><span class="nm">수신 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div><div class="row hist miss" data-call><div><span class="nm">부재중 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div></div></div></div><div class="ct-detail" id="ct-3"><div class="vtop"><div class="bar2"><span class="blue" data-ctback>‹ 연락처</span><span class="pn-edit blue">편집</span></div></div><div class="vscroll" style="padding-bottom:40px"><div class="ctd-top"><div class="av ctd-av"></div><div class="ctd-name"></div></div><div class="ctd-act"><button data-call><svg class="ic fill"><use href="#i-phone"/></svg><span>전화</span></button><button><svg class="ic "><use href="#i-comment"/></svg><span>메시지</span></button><button><svg class="ic "><use href="#i-video"/></svg><span>FaceTime</span></button><button><svg class="ic "><use href="#i-mail"/></svg><span>메일</span></button></div><div class="grp"><div class="row"><div class="s">휴대전화</div><div class="t blue"><span data-e data-ph="전화번호"></span></div></div></div><div class="sec" style="font-size:15px;color:#8e8e93">최근 기록</div><div class="grp"><div class="row hist" data-call><div><span class="nm">발신 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div><div class="row hist" data-call><div><span class="nm">수신 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div><div class="row hist miss" data-call><div><span class="nm">부재중 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div></div></div></div><div class="ct-detail" id="ct-4"><div class="vtop"><div class="bar2"><span class="blue" data-ctback>‹ 연락처</span><span class="pn-edit blue">편집</span></div></div><div class="vscroll" style="padding-bottom:40px"><div class="ctd-top"><div class="av ctd-av"></div><div class="ctd-name"></div></div><div class="ctd-act"><button data-call><svg class="ic fill"><use href="#i-phone"/></svg><span>전화</span></button><button><svg class="ic "><use href="#i-comment"/></svg><span>메시지</span></button><button><svg class="ic "><use href="#i-video"/></svg><span>FaceTime</span></button><button><svg class="ic "><use href="#i-mail"/></svg><span>메일</span></button></div><div class="grp"><div class="row"><div class="s">휴대전화</div><div class="t blue"><span data-e data-ph="전화번호"></span></div></div></div><div class="sec" style="font-size:15px;color:#8e8e93">최근 기록</div><div class="grp"><div class="row hist" data-call><div><span class="nm">발신 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div><div class="row hist" data-call><div><span class="nm">수신 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div><div class="row hist miss" data-call><div><span class="nm">부재중 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div></div></div></div><div class="ct-detail" id="ct-5"><div class="vtop"><div class="bar2"><span class="blue" data-ctback>‹ 연락처</span><span class="pn-edit blue">편집</span></div></div><div class="vscroll" style="padding-bottom:40px"><div class="ctd-top"><div class="av ctd-av"></div><div class="ctd-name"></div></div><div class="ctd-act"><button data-call><svg class="ic fill"><use href="#i-phone"/></svg><span>전화</span></button><button><svg class="ic "><use href="#i-comment"/></svg><span>메시지</span></button><button><svg class="ic "><use href="#i-video"/></svg><span>FaceTime</span></button><button><svg class="ic "><use href="#i-mail"/></svg><span>메일</span></button></div><div class="grp"><div class="row"><div class="s">휴대전화</div><div class="t blue"><span data-e data-ph="전화번호"></span></div></div></div><div class="sec" style="font-size:15px;color:#8e8e93">최근 기록</div><div class="grp"><div class="row hist" data-call><div><span class="nm">발신 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div><div class="row hist" data-call><div><span class="nm">수신 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div><div class="row hist miss" data-call><div><span class="nm">부재중 전화</span><small>휴대전화</small></div><span><span data-e data-ph="어제 오후 3:00"></span></span></div></div></div></div><div class="callscr" id="callScr"><div class="cs-top"><div class="cs-name" id="csName"></div><div class="cs-st" id="csSt"></div></div><div class="cs-av"><span class="rg"></span><span class="rg"></span><span class="rg"></span><div class="cs-face"><svg class="ic fill"><use href="#i-phone"/></svg></div></div><div class="cs-grid"><div class="cs-b"><i><svg class="ic "><use href="#i-mic"/></svg></i><span>소리 끔</span></div><div class="cs-b"><i><svg class="ic "><use href="#i-keypad"/></svg></i><span>키패드</span></div><div class="cs-b"><i><svg class="ic "><use href="#i-volume"/></svg></i><span>스피커</span></div><div class="cs-b"><i><svg class="ic "><use href="#i-plus"/></svg></i><span>추가</span></div><div class="cs-b"><i><svg class="ic "><use href="#i-video"/></svg></i><span>FaceTime</span></div><div class="cs-b"><i><svg class="ic "><use href="#i-user"/></svg></i><span>연락처</span></div></div><button class="cs-end" id="csEnd" aria-label="통화 종료"><svg class="ic fill"><use href="#i-phone"/></svg></button></div><a class="vind" data-back aria-label="홈으로"></a></section>
<section class="view " id="v-messages" data-status="dark" style=""><div class="vtop"><div class="nt-h" style="color:#0a84ff"><span>편집</span><svg class="ic "><use href="#i-compose"/></svg></div></div><div class="vscroll"><div class="big-t">메시지</div><div class="srch"><svg class="ic "><use href="#i-search"/></svg> 검색</div><div class="ms-row"><div class="slot av" data-img></div><div class="b"><div class="nm"><span><span data-e data-ph="이름"></span></span><span class="tm"><span data-e data-ph="오후 1:00"></span></span></div><div class="pv"><span data-e data-ph="메시지 내용" data-ml></span></div></div></div><div class="ms-row"><div class="slot av" data-img></div><div class="b"><div class="nm"><span><span data-e data-ph="이름"></span></span><span class="tm"><span data-e data-ph="오후 1:00"></span></span></div><div class="pv"><span data-e data-ph="메시지 내용" data-ml></span></div></div></div><div class="ms-row"><div class="slot av" data-img></div><div class="b"><div class="nm"><span><span data-e data-ph="이름"></span></span><span class="tm"><span data-e data-ph="오후 1:00"></span></span></div><div class="pv"><span data-e data-ph="메시지 내용" data-ml></span></div></div></div><div class="ms-row"><div class="slot av" data-img></div><div class="b"><div class="nm"><span><span data-e data-ph="이름"></span></span><span class="tm"><span data-e data-ph="오후 1:00"></span></span></div><div class="pv"><span data-e data-ph="메시지 내용" data-ml></span></div></div></div><div class="ms-row"><div class="slot av" data-img></div><div class="b"><div class="nm"><span><span data-e data-ph="이름"></span></span><span class="tm"><span data-e data-ph="오후 1:00"></span></span></div><div class="pv"><span data-e data-ph="메시지 내용" data-ml></span></div></div></div><div class="ms-row"><div class="slot av" data-img></div><div class="b"><div class="nm"><span><span data-e data-ph="이름"></span></span><span class="tm"><span data-e data-ph="오후 1:00"></span></span></div><div class="pv"><span data-e data-ph="메시지 내용" data-ml></span></div></div></div></div><a class="vind" data-back aria-label="홈으로"></a></section>
<section class="view mu" id="v-music" data-status="dark" style="--bg:#fff"><div class="vtop"><div class="mu-h"><div class="big-t">지금 듣기</div><div class="slot av xs" data-img></div></div></div><div class="vscroll"><div class="feat"><div class="slot " data-img></div></div><div class="sec">최근 들은 항목</div><div class="hs"><div class="alb"><div class="slot " data-img></div><div><span data-e data-ph="앨범 제목"></span></div><div class="a"><span data-e data-ph="아티스트"></span></div></div><div class="alb"><div class="slot " data-img></div><div><span data-e data-ph="앨범 제목"></span></div><div class="a"><span data-e data-ph="아티스트"></span></div></div><div class="alb"><div class="slot " data-img></div><div><span data-e data-ph="앨범 제목"></span></div><div class="a"><span data-e data-ph="아티스트"></span></div></div><div class="alb"><div class="slot " data-img></div><div><span data-e data-ph="앨범 제목"></span></div><div class="a"><span data-e data-ph="아티스트"></span></div></div></div><div class="sec">추천 믹스</div><div class="hs"><div class="alb"><div class="slot " data-img></div><div><span data-e data-ph="앨범 제목"></span></div><div class="a"><span data-e data-ph="아티스트"></span></div></div><div class="alb"><div class="slot " data-img></div><div><span data-e data-ph="앨범 제목"></span></div><div class="a"><span data-e data-ph="아티스트"></span></div></div><div class="alb"><div class="slot " data-img></div><div><span data-e data-ph="앨범 제목"></span></div><div class="a"><span data-e data-ph="아티스트"></span></div></div><div class="alb"><div class="slot " data-img></div><div><span data-e data-ph="앨범 제목"></span></div><div class="a"><span data-e data-ph="아티스트"></span></div></div></div><div style="height:16px"></div></div><div class="mini"><div class="slot " data-img></div><div class="t"><span data-e data-ph="재생 중이 아님"></span></div><button class="pp" aria-label="재생"><svg class="ic fill pl"><use href="#i-play"/></svg><svg class="ic fill pa"><use href="#i-pause"/></svg></button><button aria-label="다음"><svg class="ic fill"><use href="#i-fwd"/></svg></button></div><nav class="tabbar "><button class="tb on"><svg class="ic "><use href="#i-play"/></svg><span>지금 듣기</span></button><button class="tb"><svg class="ic "><use href="#i-grid"/></svg><span>둘러보기</span></button><button class="tb"><svg class="ic "><use href="#i-radio"/></svg><span>라디오</span></button><button class="tb"><svg class="ic "><use href="#i-archive"/></svg><span>보관함</span></button><button class="tb"><svg class="ic "><use href="#i-search"/></svg><span>검색</span></button></nav><a class="vind" data-back aria-label="홈으로"></a></section>
<div id="toast"></div>
</main>
<script>
/* ===== 설정 (여기만 고치면 돼요) ===== */
const CONFIG = {
  password: "0000",   // 잠금화면 암호
  battery: 74         // 상태바에 표시할 배터리 숫자
};

// 홈화면 아이콘. page = 눌렀을 때 이동할 주소, img = 직접 만든 아이콘 이미지 경로(선택)
const APPS = [
  { id: "calendar",  name: "캘린더",    page: "pages/calendar.html",  img: "icons/calendar.png" },
  { id: "photos",    name: "사진",      page: "pages/photos.html",    img: "icons/photos.png" },
  { id: "camera",    name: "카메라",    page: "pages/camera.html" },
  { id: "clock",     name: "시계",      page: "pages/clock.html" },
  { id: "notes",     name: "메모",      page: "pages/notes.html",     img: "icons/notes.png" },
  { id: "weather",   name: "날씨",      page: "pages/weather.html" },
  { id: "instagram", name: "Instagram", page: "pages/instagram.html", img: "icons/instagram.png" },
  { id: "reminders", name: "미리 알림", page: "pages/reminders.html" },
  { id: "postype",   name: "포스타입",  page: "pages/postype.html",   img: "icons/postype.png" },
  { id: "search",    name: "검색",      page: "pages/search.html" },
  { id: "x",         name: "X",         page: "pages/x.html",         img: "icons/x.png" }
];
const DOCK = [
  { id: "phone",    name: "전화",     page: "pages/phone.html",    img: "icons/phone.png" },
  { id: "messages", name: "문자",     page: "pages/messages.html", img: "icons/messages.png" },
  { id: "browser",  name: "브라우저", page: "pages/browser.html" },
  { id: "music",    name: "음악",     page: "pages/music.html" }
];

/* ===== 아이콘 (직접 그린 범용 아이콘) ===== */
const DAYS = ["일요일","월요일","화요일","수요일","목요일","금요일","토요일"];
const S = (inner, stroke = "#fff", size = "56%") =>
  `<svg viewBox="0 0 24 24" width="${size}" height="${size}" fill="none" stroke="${stroke}" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">${inner}</svg>`;

function ticks() {
  let t = "";
  for (let i = 0; i < 12; i++) t += `<line x1="50" y1="9" x2="50" y2="${i % 3 ? 13 : 16}" stroke="#111" stroke-width="${i % 3 ? 1.5 : 2.5}" transform="rotate(${i * 30} 50 50)"/>`;
  return t;
}

const ICONS = {
  calendar: () => { const d = new Date(); return { bg: "#fff", html: `<div class="cal"><small>${DAYS[d.getDay()]}</small><b>${d.getDate()}</b></div>` }; },
  photos:   () => ({ bg: "linear-gradient(160deg,#7fd1ff,#3b82f6)", html: S(`<rect x="3" y="3" width="18" height="18" rx="2"/><circle cx="8.5" cy="8.5" r="1.5"/><polyline points="21 15 16 10 5 21"/>`) }),
  camera:   () => ({ bg: "linear-gradient(160deg,#d8dadf,#a9acb4)", html: S(`<path d="M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h4l2-3h6l2 3h4a2 2 0 0 1 2 2z"/><circle cx="12" cy="13" r="4"/>`, "#2b2d33") }),
  clock:    () => ({ bg: "#000", html: `<svg class="full" viewBox="0 0 100 100"><circle cx="50" cy="50" r="40" fill="#fff"/>${ticks()}
      <line class="hh" x1="50" y1="50" x2="50" y2="28" stroke="#111" stroke-width="4" stroke-linecap="round"/>
      <line class="mh" x1="50" y1="50" x2="50" y2="18" stroke="#111" stroke-width="3" stroke-linecap="round"/>
      <line class="sh" x1="50" y1="56" x2="50" y2="14" stroke="#ff9500" stroke-width="1.8" stroke-linecap="round"/>
      <circle cx="50" cy="50" r="2.5" fill="#ff9500"/></svg>` }),
  notes:    () => ({ bg: "#fff", html: `<svg class="full" viewBox="0 0 60 60"><rect width="60" height="17" fill="#f8d548"/><line x1="8" y1="31" x2="52" y2="31" stroke="#cfcfd4"/><line x1="8" y1="42" x2="52" y2="42" stroke="#cfcfd4"/></svg>` }),
  weather:  () => ({ bg: "linear-gradient(160deg,#5aa7ff,#2f6fd6)", html: `<svg class="full" viewBox="0 0 60 60"><circle cx="40" cy="22" r="11" fill="#ffd54a"/><path d="M18 44h22a8 8 0 0 0 0-16 11 11 0 0 0-21 3 6.5 6.5 0 0 0-1 13z" fill="#fff" opacity=".95"/></svg>` }),
  feed:     () => ({ bg: "linear-gradient(160deg,#3ddad7,#1e6fe0)", html: S(`<path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/>`) }),
  reminders:() => ({ bg: "#fff", html: `<svg class="full" viewBox="0 0 60 60"><g stroke="#d4d4d9" stroke-width="1.5"><line x1="29" y1="16" x2="50" y2="16"/><line x1="29" y1="30" x2="50" y2="30"/><line x1="29" y1="44" x2="50" y2="44"/></g><circle cx="17" cy="16" r="4.5" fill="#ff9500"/><circle cx="17" cy="30" r="4.5" fill="#0a84ff"/><circle cx="17" cy="44" r="4.5" fill="#ff3b30"/></svg>` }),
  community:() => ({ bg: "linear-gradient(160deg,#1a2150,#080b22)", html: S(`<path d="M21 11.5a8.38 8.38 0 0 1-.9 3.8 8.5 8.5 0 0 1-7.6 4.7 8.38 8.38 0 0 1-3.8-.9L3 21l1.9-5.7a8.38 8.38 0 0 1-.9-3.8 8.5 8.5 0 0 1 4.7-7.6 8.38 8.38 0 0 1 3.8-.9h.5a8.48 8.48 0 0 1 8 8v.5z"/>`) }),
  search:   () => ({ bg: "linear-gradient(160deg,#9b7bff,#6a45e8)", html: S(`<circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/>`) }),
  news:     () => ({ bg: "linear-gradient(160deg,#4db8ff,#1580e8)", html: S(`<path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.73 21a2 2 0 0 1-3.46 0"/>`) }),
  phone:    () => ({ bg: "linear-gradient(160deg,#7fe57f,#2fbf4a)", html: `<svg viewBox="0 0 24 24" width="56%" height="56%" fill="#fff"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/></svg>` }),
  messages: () => ({ bg: "linear-gradient(160deg,#ffe066,#ffc400)", html: S(`<path d="M21 11.5a8.38 8.38 0 0 1-.9 3.8 8.5 8.5 0 0 1-7.6 4.7 8.38 8.38 0 0 1-3.8-.9L3 21l1.9-5.7a8.38 8.38 0 0 1-.9-3.8 8.5 8.5 0 0 1 4.7-7.6 8.38 8.38 0 0 1 3.8-.9h.5a8.48 8.48 0 0 1 8 8v.5z"/>`, "#4a3200") }),
  browser:  () => ({ bg: "#fff", html: S(`<circle cx="12" cy="12" r="10"/><line x1="2" y1="12" x2="22" y2="12"/><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>`, "#1a7cf0") }),
  music:    () => ({ bg: "linear-gradient(160deg,#a99cf0 0%,#e9e3b6 100%)", html: S(`<path d="M9 18V5l12-2v13"/><circle cx="6" cy="18" r="3"/><circle cx="18" cy="16" r="3"/>`, "#2c2350") })
};

ICONS.instagram = ICONS.feed;
ICONS.postype = () => ({ bg: "linear-gradient(160deg,#ffb36b,#ff7a45)", html: S(`<path d="M12 20h9"/><path d="M16.5 3.5a2.12 2.12 0 0 1 3 3L7 19l-4 1 1-4z"/>`) });
ICONS.x = ICONS.news;

function buildApp(a) {
  const link = document.createElement("a");
  link.className = "app";
  link.href = a.page;
  const ico = ICONS[a.id] ? ICONS[a.id]() : { bg: "#888", html: "" };
  const box = document.createElement("div");
  box.className = "ico";
  const generic = () => { box.style.background = ico.bg; box.innerHTML = ico.html; };
  generic();              // 이미지 확인 전/없을 때는 범용 아이콘 표시
  if (a.img) {
    const im = new Image();
    im.alt = "";
    im.onload = () => { box.style.background = "none"; box.innerHTML = ""; box.appendChild(im); };
    im.src = a.img;
  }
  const label = document.createElement("span");
  label.textContent = a.name;
  link.append(box, label);
  link.addEventListener("click", e => {
    e.preventDefault();
    openView(a.id);
  });
  return link;
}

/* ===== 시계/날짜 ===== */
const pad = n => String(n).padStart(2, "0");
function tick() {
  const d = new Date();
  const hm = `${d.getHours() % 12 || 12}:${pad(d.getMinutes())}`;
  document.getElementById("sTime").textContent = hm;
  document.getElementById("lockTime").textContent = hm;
  document.getElementById("lockDate").textContent = `${d.getMonth() + 1}월 ${d.getDate()}일 ${DAYS[d.getDay()]}`;
  const s = d.getSeconds(), m = d.getMinutes(), h = d.getHours() % 12;
  const rot = (cls, deg) => document.querySelectorAll("." + cls).forEach(el => el.setAttribute("transform", `rotate(${deg} 50 50)`));
  rot("sh", s * 6);
  rot("mh", m * 6 + s * 0.1);
  rot("hh", h * 30 + m * 0.5);
}

/* ===== 잠금 / 해제 ===== */
const lock = document.getElementById("lock");
const dotsEl = document.getElementById("dots");
const keysEl = document.getElementById("keys");
let entered = "";

for (let i = 0; i < CONFIG.password.length; i++) dotsEl.appendChild(document.createElement("i"));
["1","2","3","4","5","6","7","8","9","","0",""].forEach(k => {
  const b = document.createElement("button");
  b.type = "button";
  if (k === "") { b.className = "blank"; b.tabIndex = -1; }
  else { b.textContent = k; b.addEventListener("click", () => press(k)); }
  keysEl.appendChild(b);
});

function drawDots() { [...dotsEl.children].forEach((d, i) => d.classList.toggle("on", i < entered.length)); }
function press(k) {
  if (entered.length >= CONFIG.password.length) return;
  entered += k; drawDots();
  if (entered.length === CONFIG.password.length) setTimeout(check, 120);
}
function check() {
  if (entered === CONFIG.password) {
    try { sessionStorage.setItem("unlocked", "1"); } catch (_) {}
    showHome(true);
  } else {
    dotsEl.classList.remove("shake"); void dotsEl.offsetWidth; dotsEl.classList.add("shake");
    entered = ""; setTimeout(drawDots, 350);
  }
}
function showPass() { lock.classList.add("pass"); }
function hidePass() { lock.classList.remove("pass"); entered = ""; drawDots(); }

function showHome(animate) {
  if (animate) {
    lock.classList.add("gone");
    setTimeout(() => lock.classList.add("hidden"), 600);
  } else {
    lock.classList.add("hidden");
  }
}

// 위로 쓸어올리기 또는 탭 → 암호 입력 화면
const lkMain = document.getElementById("lkMain");
let y0 = null;
lkMain.addEventListener("touchstart", e => { y0 = e.touches[0].clientY; }, { passive: true });
lkMain.addEventListener("touchend", e => {
  if (y0 !== null && y0 - e.changedTouches[0].clientY > 40) showPass();
  y0 = null;
});
lkMain.addEventListener("click", showPass);
document.getElementById("pwCancel").addEventListener("click", hidePass);
document.addEventListener("keydown", e => {
  if (lock.classList.contains("hidden")) return;
  if (!lock.classList.contains("pass")) { if (e.key === "Enter" || e.key === " ") showPass(); return; }
  if (/^\d$/.test(e.key)) press(e.key);
  else if (e.key === "Backspace") { entered = entered.slice(0, -1); drawDots(); }
  else if (e.key === "Escape") hidePass();
});

/* ===== 시작 ===== */
document.getElementById("battNum").textContent = CONFIG.battery;
const grid = document.getElementById("appGrid");
APPS.forEach(a => grid.appendChild(buildApp(a)));
const dock = document.getElementById("dock");
DOCK.forEach(a => dock.appendChild(buildApp(a)));
tick(); setInterval(tick, 1000);

let unlocked = false;
try { unlocked = sessionStorage.getItem("unlocked") === "1"; } catch (_) {}
if (unlocked || location.hash === "#home") showHome(false);



/* =========================================================
   앱 화면 (제2의 화면) 동작
   ========================================================= */
const statusEl = document.getElementById("status");
function openView(id) {
  const v = document.getElementById("v-" + id);
  if (!v) return;
  document.querySelectorAll(".view.open").forEach(x => x.classList.remove("open"));
  v.classList.add("open");
  statusEl.classList.toggle("dark", v.dataset.status === "dark");
  const s = v.querySelector(".vscroll"); if (s) s.scrollTop = 0;
}
function closeViews() {
  if (window.__resetOverlays) window.__resetOverlays();
  document.querySelectorAll(".view.open").forEach(x => x.classList.remove("open"));
  statusEl.classList.remove("dark");
}
let toastTimer;
function toast(m) {
  const t = document.getElementById("toast");
  t.textContent = m; t.classList.add("on");
  clearTimeout(toastTimer); toastTimer = setTimeout(() => t.classList.remove("on"), 2200);
}

/* ---------- 직접 입력(글/사진) + 자동 저장 ---------- */
(function () {
  const KEY = "phone-v1";
  let store = {};
  try { store = JSON.parse(localStorage.getItem(KEY) || "{}"); } catch (_) {}
  const save = () => {
    try { localStorage.setItem(KEY, JSON.stringify(store)); }
    catch (_) { toast("저장 공간이 부족해서 저장하지 못했어요"); }
  };
  const setImg = (el, d) => { el.style.backgroundImage = 'url("' + d + '")'; el.classList.add("has"); };

  document.querySelectorAll(".view").forEach(v => {
    v.querySelectorAll("[data-e],[data-img]").forEach((el, i) => {
      const k = v.id + "#" + i;
      el.dataset.k = k;
      if (el.hasAttribute("data-e")) {
        el.contentEditable = "true"; el.spellcheck = false;
        if (store[k]) el.textContent = store[k];
      } else if (store[k]) setImg(el, store[k]);
    });
  });

  syncMirrors();
  document.addEventListener("input", e => {
    const el = e.target.closest && e.target.closest("[data-e]");
    if (!el) return;
    if (!el.textContent.trim()) { el.innerHTML = ""; delete store[el.dataset.k]; }
    else store[el.dataset.k] = el.innerText.replace(/\n$/, "");
    save();
  });
  document.addEventListener("keydown", e => {
    const el = e.target.closest && e.target.closest("[data-e]");
    if (el && e.key === "Enter" && !el.hasAttribute("data-ml")) { e.preventDefault(); el.blur(); }
  });
  document.addEventListener("paste", e => {
    const el = e.target.closest && e.target.closest("[data-e]");
    if (!el) return;
    e.preventDefault();
    const t = (e.clipboardData || window.clipboardData).getData("text/plain");
    document.execCommand("insertText", false, el.hasAttribute("data-ml") ? t : t.replace(/\s+/g, " "));
  });

  const picker = document.createElement("input");
  picker.type = "file"; picker.accept = "image/*"; picker.hidden = true;
  document.body.appendChild(picker);
  let target = null;
  document.addEventListener("click", e => {
    const s = e.target.closest("[data-img]");
    if (!s) return;
    e.preventDefault(); target = s; picker.value = ""; picker.click();
  });
  function shrink(file, max, cb) {
    const r = new FileReader();
    r.onload = () => {
      const im = new Image();
      im.onload = () => {
        const sc = Math.min(1, max / Math.max(im.width, im.height));
        const c = document.createElement("canvas");
        c.width = Math.round(im.width * sc); c.height = Math.round(im.height * sc);
        c.getContext("2d").drawImage(im, 0, 0, c.width, c.height);
        cb(c.toDataURL("image/jpeg", 0.82));
      };
      im.src = r.result;
    };
    r.readAsDataURL(file);
  }
  window.pickImage = el => { target = el; picker.value = ""; picker.click(); };
  picker.addEventListener("change", () => {
    const f = picker.files[0], el = target;
    if (!f || !el) return;
    shrink(f, el.classList.contains("av") || el.classList.contains("xs") ? 256 : (el.classList.contains("sv-img") ? 1280 : 900), d => {
      setImg(el, d); store[el.dataset.k] = d; save();
      syncMirrors(); if (window.onStoryImg) onStoryImg(el);
    });
  });
})();

/* ---------- 공통 클릭: 탭, 스위치, 팔로우, 홈으로 ---------- */
document.addEventListener("click", e => {
  const b = e.target.closest(".tabs button,.seg button,.tb[data-pane]");
  if (b) {
    [...b.parentNode.children].forEach(c => c.classList.toggle("on", c === b));
    if (b.dataset.pane) {
      const v = b.closest(".view");
      v.querySelectorAll(".pane").forEach(p => p.classList.toggle("on", p.id === b.dataset.pane));
      const s = v.querySelector(".vscroll"); if (s) s.scrollTop = 0;
    }
    return;
  }
  const sw = e.target.closest(".sw"); if (sw) { sw.classList.toggle("on"); return; }
  const pill = e.target.closest(".pill");
  if (pill) { const on = pill.classList.toggle("on"); pill.textContent = on ? pill.dataset.b : pill.dataset.a; return; }
  if (e.target.closest("[data-back]")) { e.preventDefault(); closeViews(); }
});
document.addEventListener("keydown", e => { if (e.key === "Escape") closeViews(); });
(function () {   // 화면 아래에서 위로 쓸어올리면 홈으로
  const ph = document.getElementById("phone"); let y0 = null;
  ph.addEventListener("touchstart", e => { const r = ph.getBoundingClientRect(), t = e.touches[0]; y0 = t.clientY > r.bottom - 90 ? t.clientY : null; }, { passive: true });
  ph.addEventListener("touchend", e => { if (y0 !== null && y0 - e.changedTouches[0].clientY > 50) closeViews(); y0 = null; });
})();

/* ---------- 캘린더 ---------- */
(function () {
  const grid = document.getElementById("cal"); if (!grid) return;
  const DOW = ["일", "월", "화", "수", "목", "금", "토"];
  const today = new Date();
  let cur = new Date(today.getFullYear(), today.getMonth(), 1);
  let sel = new Date(today.getFullYear(), today.getMonth(), today.getDate());
  const same = (a, b) => a.getFullYear() === b.getFullYear() && a.getMonth() === b.getMonth() && a.getDate() === b.getDate();
  function draw() {
    const y = cur.getFullYear(), m = cur.getMonth(), first = new Date(y, m, 1).getDay(), last = new Date(y, m + 1, 0).getDate();
    document.getElementById("calYear").textContent = y + "년";
    document.getElementById("calTitle").textContent = (m + 1) + "월";
    let h = DOW.map(w => `<div class="w">${w}</div>`).join("");
    for (let i = 0; i < first; i++) h += "<div></div>";
    for (let d = 1; d <= last; d++) {
      const dt = new Date(y, m, d);
      h += `<div class="n${same(dt, today) ? " today" : ""}${same(dt, sel) ? " sel" : ""}" data-d="${d}"><span>${d}</span></div>`;
    }
    grid.innerHTML = h;
    document.getElementById("calSel").textContent = `${sel.getMonth() + 1}월 ${sel.getDate()}일 ${DOW[sel.getDay()]}요일`;
  }
  grid.addEventListener("click", e => { const n = e.target.closest(".n"); if (!n) return; sel = new Date(cur.getFullYear(), cur.getMonth(), +n.dataset.d); draw(); });
  document.getElementById("calPrev").onclick = () => { cur.setMonth(cur.getMonth() - 1); draw(); };
  document.getElementById("calNext").onclick = () => { cur.setMonth(cur.getMonth() + 1); draw(); };
  document.getElementById("calToday").onclick = () => { cur = new Date(today.getFullYear(), today.getMonth(), 1); sel = new Date(today); draw(); };
  draw();
})();

/* ---------- 시계: 스톱워치 / 타이머 ---------- */
(function () {
  const t = document.getElementById("swT"); if (!t) return;
  const go = document.getElementById("swGo"), lap = document.getElementById("swLap"), lapsEl = document.getElementById("laps");
  let run = false, start = 0, acc = 0, iv, laps = [];
  const fmt = ms => `${pad(Math.floor(ms / 60000))}:${pad(Math.floor(ms / 1000) % 60)}.${pad(Math.floor(ms / 10) % 100)}`;
  const now = () => acc + (run ? Date.now() - start : 0);
  const drawLaps = () => { lapsEl.innerHTML = laps.map((l, i) => `<div class="lap"><span>랩 ${laps.length - i}</span><span>${l}</span></div>`).join(""); };
  go.onclick = () => {
    if (!run) { start = Date.now(); run = true; iv = setInterval(() => t.textContent = fmt(now()), 30); go.textContent = "정지"; go.className = "rb r"; lap.textContent = "랩"; }
    else { acc = now(); run = false; clearInterval(iv); go.textContent = "시작"; go.className = "rb g"; lap.textContent = "재설정"; }
  };
  lap.onclick = () => {
    if (run) { laps.unshift(fmt(now())); drawLaps(); }
    else { acc = 0; laps = []; t.textContent = "00:00.00"; drawLaps(); }
  };

  const tmT = document.getElementById("tmT"), tmIn = document.getElementById("tmIn"), tg = document.getElementById("tmGo"), tr = document.getElementById("tmReset");
  let end = 0, rem = null, iv2 = null;
  const show = s => { tmT.textContent = pad(Math.floor(s / 60)) + ":" + pad(s % 60); };
  const mins = () => Math.min(999, Math.max(1, parseInt(tmIn.value) || 1));
  tmIn.addEventListener("input", () => { if (!iv2) show(mins() * 60); });
  tg.onclick = () => {
    if (iv2) { clearInterval(iv2); iv2 = null; rem = Math.ceil((end - Date.now()) / 1000); tg.textContent = "계속"; tg.className = "rb g"; return; }
    end = Date.now() + (rem !== null ? rem : mins() * 60) * 1000; rem = null;
    tg.textContent = "일시정지"; tg.className = "rb o";
    iv2 = setInterval(() => {
      const r = Math.ceil((end - Date.now()) / 1000);
      if (r <= 0) { clearInterval(iv2); iv2 = null; show(0); tg.textContent = "시작"; tg.className = "rb g"; toast("타이머가 끝났어요"); return; }
      show(r);
    }, 250);
  };
  tr.onclick = () => { clearInterval(iv2); iv2 = null; rem = null; tg.textContent = "시작"; tg.className = "rb g"; show(mins() * 60); };
  show(mins() * 60);
})();

/* ---------- 날씨 요일 ---------- */
document.querySelectorAll("[data-day]").forEach(el => {
  const n = +el.dataset.day;
  if (n === 0) { el.textContent = "오늘"; return; }
  const d = new Date(); d.setDate(d.getDate() + n);
  el.textContent = DAYS[d.getDay()];
});

/* ---------- 미리 알림(할 일) ---------- */
(function () {
  const box = document.querySelector("[data-todo]"); if (!box) return;
  let items = [];
  try { items = JSON.parse(localStorage.getItem("phone-todo") || "[]"); } catch (_) {}
  const save = () => { try { localStorage.setItem("phone-todo", JSON.stringify(items)); } catch (_) {} };
  const input = box.querySelector("input"), list = box.querySelector("ul");
  function draw() {
    list.innerHTML = "";
    items.forEach((it, i) => {
      const li = document.createElement("li");
      li.className = it.done ? "done" : "";
      li.innerHTML = '<i class="dot"></i><span></span>';
      li.querySelector("span").textContent = it.text;
      li.onclick = () => { it.done = !it.done; save(); draw(); };
      li.oncontextmenu = e => { e.preventDefault(); items.splice(i, 1); save(); draw(); };
      let tm; li.addEventListener("touchstart", () => { tm = setTimeout(() => { items.splice(i, 1); save(); draw(); }, 700); }, { passive: true });
      ["touchend", "touchmove"].forEach(ev => li.addEventListener(ev, () => clearTimeout(tm)));
      list.appendChild(li);
    });
  }
  const add = () => { const v = input.value.trim(); if (!v) return; items.push({ text: v, done: false }); input.value = ""; save(); draw(); };
  box.querySelector("button").onclick = add;
  input.addEventListener("keydown", e => { if (e.key === "Enter") add(); });
  draw();
})();

/* ---------- 검색 ---------- */
(function () {
  const list = document.getElementById("spList"), inp = document.getElementById("spIn"); if (!list) return;
  const all = APPS.concat(DOCK).filter(a => a.id !== "search");
  function render() {
    const q = inp.value.trim().toLowerCase();
    const rs = all.filter(a => a.name.toLowerCase().includes(q));
    list.innerHTML = "";
    if (!rs.length) { list.innerHTML = '<div class="empty">결과 없음</div>'; return; }
    list.insertAdjacentHTML("beforeend", '<div class="sec" style="font-size:15px;color:#8e8e93">' + (q ? "앱" : "Siri 앱 제안") + "</div>");
    rs.forEach(a => {
      const row = document.createElement("div"); row.className = "ap-row";
      const ico = buildApp(a).querySelector(".ico");
      const nm = document.createElement("span"); nm.textContent = a.name;
      row.append(ico, nm);
      row.onclick = () => { closeViews(); openView(a.id); };
      list.appendChild(row);
    });
  }
  inp.addEventListener("input", render); render();
})();

/* ---------- 브라우저 / 전화 / 음악 / 카메라 ---------- */
(function () {
  const sf = document.getElementById("sfIn");
  if (sf) sf.addEventListener("keydown", e => {
    if (e.key !== "Enter") return;
    const v = sf.value.trim(); if (!v) return;
    const url = /^https?:\/\//i.test(v) ? v : (/^[^\s]+\.[^\s]{2,}$/.test(v) ? "https://" + v : "https://www.google.com/search?q=" + encodeURIComponent(v));
    const a = document.createElement("a"); a.href = url; a.target = "_blank"; a.rel = "noopener";
    document.body.appendChild(a); a.click(); a.remove();
  });
  const pnD = document.getElementById("pnD");
  if (pnD) {
    document.querySelectorAll(".pad [data-k]").forEach(b => b.onclick = () => { pnD.textContent += b.dataset.k; });
    document.getElementById("pnDel").onclick = () => { pnD.textContent = pnD.textContent.slice(0, -1); };
  }
  const pp = document.querySelector(".mini .pp");
  if (pp) pp.onclick = () => document.querySelector(".mini").classList.toggle("playing");
  const shut = document.getElementById("shut");
  if (shut) shut.onclick = () => { const vf = document.getElementById("camVf"); vf.classList.remove("flash"); void vf.offsetWidth; vf.classList.add("flash"); };
})();

/* =========================================================
   인스타 스토리 보기 (색 고리를 누르면 확대되어 열림)
   ========================================================= */
function syncMirrors() {
  document.querySelectorAll("[data-mirror]").forEach(el => {
    const src = document.querySelector("#" + el.dataset.mirror + " .sv-av"); if (!src) return;
    el.style.backgroundImage = src.style.backgroundImage;
    el.classList.toggle("has", src.classList.contains("has"));
  });
}
(function () {
  const igv = document.getElementById("v-instagram"); if (!igv) return;
  const DUR = 5000, phone = document.getElementById("phone");
  let cur = null, idx = 0, timer = null, remain = DUR;
  const slidesOf = s => s.querySelectorAll(".sv-img"), barsOf = s => s.querySelectorAll(".sv-bars span");
  const hasImg = () => slidesOf(cur)[idx].classList.contains("has");
  function showSlide(n) {
    clearTimeout(timer);
    const sl = slidesOf(cur), bs = barsOf(cur);
    if (n >= sl.length) return closeStory();
    if (n < 0) n = 0;
    idx = n;
    sl.forEach((s, i) => s.classList.toggle("on", i === n));
    bs.forEach((b, i) => { const f = b.firstElementChild; f.style.transition = "none"; f.style.width = i < n ? "100%" : "0%"; });
    remain = DUR;
    if (hasImg()) run();
  }
  function run() {
    const f = barsOf(cur)[idx].firstElementChild;
    void f.offsetWidth; f.style.transition = "width " + remain + "ms linear"; f.style.width = "100%";
    clearTimeout(timer); timer = setTimeout(() => showSlide(idx + 1), remain);
  }
  function freeze() {
    if (!cur) return; clearTimeout(timer);
    const f = barsOf(cur)[idx].firstElementChild, w = f.getBoundingClientRect().width, tw = f.parentNode.getBoundingClientRect().width;
    const r = tw ? w / tw : 0;
    f.style.transition = "none"; f.style.width = (r * 100) + "%"; remain = Math.max(300, (1 - r) * DUR);
  }
  function openStory(st) {
    const sv = document.getElementById(st.dataset.sv); if (!sv) return;
    cur = sv;
    const nm = st.querySelector("[data-e]");
    sv.querySelector(".sv-name").textContent = (nm && nm.textContent.trim()) || "아이디";
    const pr = phone.getBoundingClientRect(), rr = st.querySelector(".ring").getBoundingClientRect();
    sv.style.transformOrigin = (rr.left + rr.width / 2 - pr.left) + "px " + (rr.top + rr.height / 2 - pr.top) + "px";
    sv.classList.remove("closing"); sv.classList.add("open");
    statusEl.classList.remove("dark");
    showSlide(0);
  }
  function closeStory(instant) {
    if (!cur) return;
    clearTimeout(timer);
    const sv = cur; cur = null;
    if (instant) sv.classList.remove("open", "closing");
    else { sv.classList.add("closing"); setTimeout(() => sv.classList.remove("open", "closing"), 280); statusEl.classList.add("dark"); }
  }
  window.__closeStory = closeStory;
  window.onStoryImg = el => { if (cur && el.classList.contains("sv-img")) showSlide(idx); };

  igv.addEventListener("click", e => {
    const st = e.target.closest(".st[data-sv]");
    if (st && !e.target.closest("[data-e]")) { openStory(st); return; }
    if (!cur) return;
    if (e.target.closest("[data-close]")) { closeStory(); return; }
    if (e.target.closest("[data-change]")) { pickImage(slidesOf(cur)[idx]); return; }
    const hb = e.target.closest(".sv-heart"); if (hb) { hb.classList.toggle("on"); return; }
    const z = e.target.closest(".sv-l,.sv-r");
    if (z) {
      const right = z.classList.contains("sv-r");
      if (!hasImg() && right) { pickImage(slidesOf(cur)[idx]); return; }
      showSlide(idx + (right ? 1 : -1));
    }
  });
  igv.addEventListener("focusin", e => { if (cur && e.target.closest(".sv-in,.sv-time")) freeze(); });
  igv.addEventListener("focusout", e => { if (cur && e.target.closest(".sv-in,.sv-time") && hasImg()) run(); });
  igv.addEventListener("keydown", e => {
    if (e.key === "Enter" && e.target.classList && e.target.classList.contains("sv-in")) {
      if (!e.target.value.trim()) return;
      e.target.value = ""; e.target.blur(); toast("메시지를 보냈어요");
    }
  });
  let ty = null;
  igv.addEventListener("touchstart", e => { ty = cur ? e.touches[0].clientY : null; }, { passive: true });
  igv.addEventListener("touchend", e => { if (ty !== null && cur && e.changedTouches[0].clientY - ty > 90) closeStory(); ty = null; });
})();

/* =========================================================
   전화: 연락처 → 최근 기록 → 발신 화면
   ========================================================= */
(function () {
  const v = document.getElementById("v-phone"); if (!v) return;
  const scr = document.getElementById("callScr"), nameEl = document.getElementById("csName"), st = document.getElementById("csSt");
  const dots = '<span class="cdots"><i>.</i><i>.</i><i>.</i></span>';
  let tms = [];
  function startCall(name) {
    nameEl.textContent = name || "알 수 없음";
    st.innerHTML = "연결 중" + dots;
    scr.classList.remove("ending", "ringing"); scr.classList.add("on");
    statusEl.classList.remove("dark");
    tms.forEach(clearTimeout);
    tms = [setTimeout(() => { st.innerHTML = "발신 중" + dots; scr.classList.add("ringing"); }, 1400)];
  }
  function endCall(instant) {
    if (!scr.classList.contains("on")) return;
    tms.forEach(clearTimeout);
    if (instant) { scr.classList.remove("on", "ending", "ringing"); return; }
    st.textContent = "통화 종료"; scr.classList.add("ending"); scr.classList.remove("ringing");
    tms = [setTimeout(() => { scr.classList.remove("on", "ending"); if (v.classList.contains("open")) statusEl.classList.add("dark"); }, 900)];
  }
  window.__endCall = endCall;
  document.getElementById("csEnd").onclick = () => endCall();

  const nameOf = row => {
    const n = row.querySelector("[data-nm]");
    if (n && n.textContent.trim()) return n.textContent.trim();
    const d = row.closest(".ct-detail");
    return d ? d.querySelector(".ctd-name").textContent : "";
  };
  v.addEventListener("click", e => {
    const edit = e.target.closest(".pn-edit");
    if (edit) { const host = edit.closest(".pane,.ct-detail"), on = host.classList.toggle("editing"); edit.textContent = on ? "완료" : "편집"; return; }
    const call = e.target.closest("[data-call]");
    if (call) { const host = call.closest(".pane,.ct-detail"); if (host && host.classList.contains("editing")) return; startCall(nameOf(call)); return; }
    const ct = e.target.closest("[data-ct]");
    if (ct) {
      if (ct.closest(".pane").classList.contains("editing")) return;
      const d = document.getElementById(ct.dataset.ct), n = ct.querySelector("[data-e]");
      d.querySelector(".ctd-name").textContent = (n && n.textContent.trim()) || "이름";
      d.classList.add("open"); const s = d.querySelector(".vscroll"); if (s) s.scrollTop = 0;
      return;
    }
    const back = e.target.closest("[data-ctback]"); if (back) { back.closest(".ct-detail").classList.remove("open"); return; }
    if (e.target.closest(".pad .call")) startCall(document.getElementById("pnD").textContent || "번호 입력 없음");
  });
})();

window.__resetOverlays = () => {
  if (window.__closeStory) window.__closeStory(true);
  if (window.__endCall) window.__endCall(true);
  document.querySelectorAll(".ct-detail.open").forEach(d => d.classList.remove("open"));
};

</script>
</body>
</html>
