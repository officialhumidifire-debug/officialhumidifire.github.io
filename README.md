[NextSet.html](https://github.com/user-attachments/files/25801993/NextSet.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="Next Set">
<title>Next Set</title>
<style>
  :root {
    --bg: #080e10;
    --surface: #0f1a1e;
    --surface2: #162228;
    --border: #1e3038;
    --accent: #00e5ff;
    --accent-dim: #00b8cc;
    --accent-glow: rgba(0,229,255,0.15);
    --accent-glow-strong: rgba(0,229,255,0.25);
    --text: #e8f8fb;
    --text-muted: #5a8a95;
    --radius: 14px;
    --safe-bottom: env(safe-area-inset-bottom, 20px);
  }
  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
  body { background: var(--bg); color: var(--text); font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', sans-serif; min-height: 100vh; overflow-x: hidden; }
  .screen { display: none; padding: 0 16px; padding-bottom: calc(90px + var(--safe-bottom)); min-height: 100vh; }
  .screen.active { display: block; }
  .header { padding: 56px 0 20px; }
  .header h1 { font-size: 32px; font-weight: 800; letter-spacing: -1px; }
  .header h1 span { color: var(--accent); text-shadow: 0 0 20px var(--accent), 0 0 40px rgba(0,229,255,0.4); }
  .header p { color: var(--text-muted); font-size: 14px; margin-top: 4px; }
  .back-btn { background: none; border: none; color: var(--accent); font-size: 16px; font-weight: 600; padding: 52px 0 12px; display: flex; align-items: center; gap: 6px; cursor: pointer; }
  .bottom-nav { position: fixed; bottom: 0; left: 0; right: 0; background: var(--surface); border-top: 1px solid var(--border); display: flex; padding: 10px 0; padding-bottom: calc(10px + var(--safe-bottom)); z-index: 100; }
  .nav-btn { flex: 1; background: none; border: none; color: var(--text-muted); font-size: 10px; font-weight: 500; display: flex; flex-direction: column; align-items: center; gap: 4px; cursor: pointer; padding: 4px 0; transition: color 0.2s; }
  .nav-btn.active { color: var(--accent); text-shadow: 0 0 8px var(--accent); }
  .nav-btn.active svg { filter: drop-shadow(0 0 4px var(--accent)); }
  .nav-btn svg { width: 22px; height: 22px; }
  .day-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 20px; margin-bottom: 14px; cursor: pointer; transition: border-color 0.2s, box-shadow 0.2s; display: flex; align-items: center; justify-content: space-between; }
  .day-card:active { border-color: var(--accent); box-shadow: 0 0 20px var(--accent-glow); }
  .day-label { font-size: 11px; font-weight: 700; letter-spacing: 2px; text-transform: uppercase; color: var(--accent); margin-bottom: 4px; }
  .day-title { font-size: 22px; font-weight: 700; }
  .day-subtitle { font-size: 13px; color: var(--text-muted); margin-top: 2px; }
  .day-arrow { color: var(--accent); font-size: 20px; }
  .exercise-item { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 16px; margin-bottom: 10px; cursor: pointer; transition: border-color 0.2s, box-shadow 0.2s; }
  .exercise-item:active { border-color: var(--accent); box-shadow: 0 0 16px var(--accent-glow); }
  .exercise-name { font-size: 16px; font-weight: 600; }
  .exercise-last { font-size: 12px; color: var(--text-muted); margin-top: 4px; }
  .exercise-controls { display: flex; align-items: center; justify-content: space-between; }
  .delete-ex-btn { background: none; border: none; color: var(--text-muted); font-size: 20px; padding: 4px 8px; cursor: pointer; }
  .modal-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.85); z-index: 300; display: flex; align-items: flex-end; opacity: 0; pointer-events: none; transition: opacity 0.25s; backdrop-filter: blur(6px); }
  .modal-overlay.open { opacity: 1; pointer-events: all; }
  .modal { background: var(--surface); border-radius: 24px 24px 0 0; border: 1px solid var(--border); border-bottom: none; padding: 24px 20px; padding-bottom: calc(28px + var(--safe-bottom)); width: 100%; transform: translateY(100%); transition: transform 0.3s cubic-bezier(0.4,0,0.2,1); box-shadow: 0 -8px 40px rgba(0,229,255,0.1); }
  .modal-overlay.open .modal { transform: translateY(0); }
  .modal-handle { width: 36px; height: 4px; background: var(--border); border-radius: 2px; margin: 0 auto 20px; }
  .modal-title { font-size: 20px; font-weight: 700; margin-bottom: 6px; }
  .modal-sub { font-size: 13px; color: var(--text-muted); margin-bottom: 28px; }
  .log-input-row { display: flex; gap: 14px; margin-bottom: 20px; }
  .log-input-group { flex: 1; }
  .log-input-group label { font-size: 11px; font-weight: 700; letter-spacing: 1px; text-transform: uppercase; color: var(--text-muted); display: block; margin-bottom: 10px; text-align: center; }
  .log-input-group input { width: 100%; background: var(--surface2); border: 1px solid var(--border); border-radius: 14px; color: var(--text); font-size: 42px; font-weight: 800; text-align: center; padding: 18px 8px; outline: none; -webkit-appearance: none; transition: border-color 0.2s, box-shadow 0.2s; }
  .log-input-group input:focus { border-color: var(--accent); box-shadow: 0 0 16px var(--accent-glow); }
  .sets-badge { display: flex; justify-content: center; margin-bottom: 20px; }
  .sets-badge span { background: var(--accent-glow); border: 1px solid rgba(0,229,255,0.3); border-radius: 20px; padding: 5px 14px; font-size: 12px; color: var(--accent); font-weight: 600; }
  .save-btn { width: 100%; background: var(--accent); color: #060e10; border: none; border-radius: 12px; padding: 16px; font-size: 17px; font-weight: 800; cursor: pointer; box-shadow: 0 4px 24px var(--accent-glow-strong); transition: background 0.2s; }
  .save-btn:active { background: var(--accent-dim); }
  .delete-btn { width: 100%; background: transparent; border: 1px solid #ff4d6d; border-radius: 12px; padding: 14px; font-size: 15px; font-weight: 700; color: #ff4d6d; cursor: pointer; margin-top: 10px; transition: background 0.2s; }
  .delete-btn:active { background: rgba(255,77,109,0.12); }
  .add-btn { width: 100%; background: transparent; border: 1px dashed var(--accent-dim); border-radius: var(--radius); padding: 14px; font-size: 15px; font-weight: 600; color: var(--accent); cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 8px; margin-top: 4px; }
  .add-btn:active { background: var(--accent-glow); }
  .pill { display: inline-flex; align-items: center; gap: 4px; background: var(--surface2); border-radius: 20px; padding: 4px 10px; font-size: 12px; color: var(--text-muted); border: 1px solid var(--border); }
  .pill.green { background: var(--accent-glow); color: var(--accent); border-color: rgba(0,229,255,0.3); }
  .stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 16px; }
  .stat-box { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 14px; }
  .stat-val { font-size: 28px; font-weight: 800; color: var(--accent); text-shadow: 0 0 12px var(--accent); }
  .stat-lbl { font-size: 12px; color: var(--text-muted); margin-top: 2px; }
  .banner { background: linear-gradient(135deg, rgba(0,229,255,0.08), rgba(0,229,255,0.04)); border: 1px solid rgba(0,229,255,0.35); border-radius: var(--radius); padding: 14px 16px; margin-bottom: 16px; display: flex; align-items: flex-start; gap: 12px; }
  .banner-icon { font-size: 20px; }
  .banner-text { font-size: 13px; line-height: 1.5; }
  .banner-text strong { color: var(--accent); display: block; margin-bottom: 2px; }
  .banner-close { background: none; border: none; color: var(--text-muted); font-size: 18px; cursor: pointer; margin-left: auto; padding: 0 0 0 8px; }
  .section-title { font-size: 11px; font-weight: 700; letter-spacing: 1.5px; text-transform: uppercase; color: var(--text-muted); margin: 20px 0 10px; }
  .accent-line { height: 1px; background: linear-gradient(90deg, transparent, var(--accent), transparent); opacity: 0.3; margin: 4px 0 16px; }
  .text-input { width: 100%; background: var(--surface2); border: 1px solid var(--border); border-radius: 10px; color: var(--text); font-size: 17px; padding: 14px 16px; outline: none; margin-bottom: 16px; transition: border-color 0.2s, box-shadow 0.2s; }
  .text-input:focus { border-color: var(--accent); box-shadow: 0 0 12px var(--accent-glow); }
  .chart-container { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 16px; margin-bottom: 14px; }
  .chart-title { font-size: 14px; font-weight: 600; margin-bottom: 14px; }
  .bar-chart { display: flex; align-items: flex-end; gap: 6px; height: 80px; }
  .bar-wrap { flex: 1; display: flex; flex-direction: column; align-items: center; gap: 4px; height: 100%; justify-content: flex-end; }
  .bar { width: 100%; background: linear-gradient(180deg, var(--accent), var(--accent-dim)); border-radius: 4px 4px 0 0; min-height: 2px; box-shadow: 0 0 8px var(--accent-glow); }
  .bar.dim { background: var(--surface2); border: 1px solid var(--border); box-shadow: none; }
  .bar-lbl { font-size: 9px; color: var(--text-muted); text-align: center; }
  .bar-val { font-size: 10px; color: var(--accent); font-weight: 700; }
  .empty { text-align: center; padding: 40px 20px; color: var(--text-muted); }
  .empty-icon { font-size: 40px; margin-bottom: 10px; }
  .empty p { font-size: 14px; }
  .ex-select { width: 100%; background: var(--surface2); border: 1px solid var(--border); border-radius: 10px; color: var(--text); font-size: 15px; padding: 12px 16px; outline: none; margin-bottom: 16px; -webkit-appearance: none; }
  /* TOAST */
  .toast { position: fixed; top: 60px; left: 50%; transform: translateX(-50%) translateY(-20px); background: var(--surface); border: 1px solid var(--accent); border-radius: 50px; padding: 12px 22px; display: flex; align-items: center; gap: 10px; font-size: 15px; font-weight: 700; color: var(--accent); box-shadow: 0 0 30px var(--accent-glow-strong), 0 8px 32px rgba(0,0,0,0.5); z-index: 999; opacity: 0; pointer-events: none; transition: opacity 0.25s, transform 0.25s; white-space: nowrap; }
  .toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }
  .toast-icon { font-size: 18px; }
  .toast-detail { font-size: 12px; color: var(--text-muted); font-weight: 500; margin-top: 1px; }
  .toast-inner { display: flex; flex-direction: column; }
  /* CALENDAR */
  .cal-wrap { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 14px; margin-bottom: 16px; }
  .cal-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 12px; }
  .cal-title { font-size: 14px; font-weight: 700; }
  .cal-nav { background: none; border: none; color: var(--accent); font-size: 22px; cursor: pointer; padding: 0 4px; line-height: 1; }
  .cal-days-row { display: grid; grid-template-columns: repeat(7,1fr); gap: 3px; margin-bottom: 4px; }
  .cal-days-row span { font-size: 9px; color: var(--text-muted); text-align: center; font-weight: 600; padding: 2px 0; }
  .cal-grid { display: grid; grid-template-columns: repeat(7,1fr); gap: 3px; }
  .cal-cell { aspect-ratio: 1; border-radius: 7px; display: flex; align-items: center; justify-content: center; font-size: 11px; font-weight: 600; color: var(--text-muted); background: var(--surface2); position: relative; flex-direction: column; gap: 1px; }
  .cal-cell.worked { background: var(--accent-glow); border: 1px solid rgba(0,229,255,0.45); color: var(--accent); text-shadow: 0 0 6px var(--accent); }
  .cal-cell.today { outline: 1.5px solid var(--accent); }
  .cal-cell.empty-cell { background: transparent; }
  .cal-dot { width: 3px; height: 3px; background: var(--accent); border-radius: 50%; box-shadow: 0 0 4px var(--accent); }
  /* HISTORY */
  .history-item { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 14px 16px; margin-bottom: 10px; }
  .history-date { font-size: 11px; color: var(--text-muted); font-weight: 600; letter-spacing: 0.5px; margin-bottom: 8px; text-transform: uppercase; }
  .history-entry { display: flex; align-items: center; justify-content: space-between; padding: 8px 0; border-bottom: 1px solid var(--border); }
  .history-entry:last-child { border-bottom: none; padding-bottom: 0; }
  .history-ex { font-size: 14px; font-weight: 600; }
  .history-val { font-size: 13px; color: var(--accent); font-weight: 700; }
  .history-day-badge { display: inline-flex; border-radius: 20px; padding: 2px 8px; font-size: 10px; font-weight: 700; letter-spacing: 1px; text-transform: uppercase; margin-left: 6px; }
  .badge-push { background: rgba(255,120,50,0.15); color: #ff7832; border: 1px solid rgba(255,120,50,0.3); }
  .badge-pull { background: rgba(160,80,255,0.15); color: #a050ff; border: 1px solid rgba(160,80,255,0.3); }
  .badge-legs { background: rgba(0,229,255,0.12); color: var(--accent); border: 1px solid rgba(0,229,255,0.3); }
  .edit-log-btn { background: none; border: 1px solid var(--border); color: var(--text-muted); font-size: 13px; cursor: pointer; padding: 5px 10px; border-radius: 8px; margin-left: 8px; }
  .edit-log-btn:active { background: var(--surface2); }
</style>
</head>
<body>

<div class="toast" id="toast">
  <div class="toast-icon" id="toast-icon">✅</div>
  <div class="toast-inner">
    <span id="toast-title">Session Saved</span>
    <span class="toast-detail" id="toast-detail"></span>
  </div>
</div>

<!-- HOME -->
<div class="screen active" id="screen-home">
  <div class="header"><h1>Next <span>Set</span></h1><p id="home-date"></p></div>
  <div class="accent-line"></div>
  <div id="progress-banner"></div>
  <div class="section-title">Choose Your Day</div>
  <div id="day-cards"></div>
</div>

<!-- WORKOUT -->
<div class="screen" id="screen-workout">
  <button class="back-btn" onclick="goBack()">‹ Back</button>
  <div id="workout-header"></div>
  <div class="accent-line"></div>
  <div id="exercise-list"></div>
  <button class="add-btn" onclick="openAddExercise()">＋ Add Exercise</button>
</div>

<!-- HISTORY -->
<div class="screen" id="screen-history">
  <div class="header"><h1><span style="color:var(--accent);text-shadow:0 0 20px var(--accent)">History</span></h1><p>Every session logged</p></div>
  <div class="accent-line"></div>
  <div id="cal-container"></div>
  <div class="section-title">Session Log</div>
  <div id="history-list"></div>
</div>

<!-- PROGRESS -->
<div class="screen" id="screen-progress">
  <div class="header"><h1><span style="color:var(--accent);text-shadow:0 0 20px var(--accent)">Progress</span></h1><p>Your improvement over time</p></div>
  <div class="accent-line"></div>
  <div class="stat-grid" id="stat-grid"></div>
  <div class="section-title">Workout Frequency</div>
  <div id="freq-chart"></div>
  <div class="section-title">Exercise Progress</div>
  <select class="ex-select" id="ex-select" onchange="renderExerciseProgress()"><option value="">Select an exercise...</option></select>
  <div id="ex-progress-chart"></div>
</div>

<!-- NAV -->
<nav class="bottom-nav">
  <button class="nav-btn active" id="nav-home" onclick="navigate('home')">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 9l9-7 9 7v11a2 2 0 01-2 2H5a2 2 0 01-2-2z"/><polyline points="9,22 9,12 15,12 15,22"/></svg>
    Workout
  </button>
  <button class="nav-btn" id="nav-history" onclick="navigate('history')">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="4" width="18" height="18" rx="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/></svg>
    History
  </button>
  <button class="nav-btn" id="nav-progress" onclick="navigate('progress')">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="18" y1="20" x2="18" y2="10"/><line x1="12" y1="20" x2="12" y2="4"/><line x1="6" y1="20" x2="6" y2="14"/></svg>
    Progress
  </button>
</nav>

<!-- LOG MODAL -->
<div class="modal-overlay" id="log-modal">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title" id="log-modal-title"></div>
    <div class="modal-sub" id="log-modal-sub"></div>
    <div class="log-input-row">
      <div class="log-input-group"><label>Weight (lbs)</label><input type="number" inputmode="decimal" placeholder="0" id="log-weight"></div>
      <div class="log-input-group"><label>Reps</label><input type="number" inputmode="numeric" placeholder="0" id="log-reps"></div>
    </div>
    <div class="sets-badge"><span>⚡ Applied to all 3 sets</span></div>
    <button class="save-btn" id="log-save-btn" onclick="saveLog()">Save Session</button>
  </div>
</div>

<!-- EDIT MODAL -->
<div class="modal-overlay" id="edit-modal">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title" id="edit-modal-title">Edit Session</div>
    <div class="modal-sub" id="edit-modal-sub"></div>
    <div class="log-input-row">
      <div class="log-input-group"><label>Weight (lbs)</label><input type="number" inputmode="decimal" placeholder="0" id="edit-weight"></div>
      <div class="log-input-group"><label>Reps</label><input type="number" inputmode="numeric" placeholder="0" id="edit-reps"></div>
    </div>
    <div class="sets-badge"><span>⚡ Applied to all 3 sets</span></div>
    <button class="save-btn" onclick="saveEdit()">Save Changes</button>
    <button class="delete-btn" onclick="confirmDelete()">🗑 Delete Session</button>
  </div>
</div>

<!-- ADD EXERCISE MODAL -->
<div class="modal-overlay" id="add-ex-modal">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title">Add Exercise</div>
    <div class="modal-sub" id="add-ex-day-label"></div>
    <input class="text-input" id="new-ex-name" type="text" placeholder="Exercise name..." autocapitalize="words">
    <button class="save-btn" onclick="saveNewExercise()">Add to Workout</button>
  </div>
</div>

<script>
const DAYS=[{id:'push',label:'Push',subtitle:'Chest · Shoulders · Triceps'},{id:'pull',label:'Pull',subtitle:'Back · Biceps · Rear Delts'},{id:'legs',label:'Legs',subtitle:'Quads · Hamstrings · Calves'}];
let currentDay=null, currentExercise=null, toastTimer=null, editIndex=null, calMonth=new Date();

function getData(){return JSON.parse(localStorage.getItem('nextset_data')||'{}');}
function saveData(d){localStorage.setItem('nextset_data',JSON.stringify(d));}
function initData(){let d=getData();if(!d.exercises)d.exercises={push:[],pull:[],legs:[]};if(!d.logs)d.logs=[];saveData(d);return d;}

function showToast(title,detail,icon){
  document.getElementById('toast-icon').textContent=icon||'✅';
  document.getElementById('toast-title').textContent=title;
  document.getElementById('toast-detail').textContent=detail||'';
  const t=document.getElementById('toast');
  t.classList.add('show');
  if(toastTimer)clearTimeout(toastTimer);
  toastTimer=setTimeout(()=>t.classList.remove('show'),2800);
}

function navigate(screen){
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.querySelectorAll('.nav-btn').forEach(b=>b.classList.remove('active'));
  document.getElementById('screen-'+screen).classList.add('active');
  const nb=document.getElementById('nav-'+screen);
  if(nb)nb.classList.add('active');
  if(screen==='home')renderHome();
  if(screen==='progress')renderProgress();
  if(screen==='history')renderHistory();
}
function goBack(){navigate('home');}

/* ── HOME ── */
function renderHome(){
  const now=new Date();
  document.getElementById('home-date').textContent=now.toLocaleDateString('en-US',{weekday:'long',month:'long',day:'numeric'});
  const d=initData();
  const bannerEl=document.getElementById('progress-banner');
  bannerEl.innerHTML='';
  const dismissed=JSON.parse(localStorage.getItem('nextset_banner_dismissed')||'null');
  if((!dismissed||(Date.now()-dismissed>14*24*60*60*1000))&&d.logs&&d.logs.length>=6){
    bannerEl.innerHTML=`<div class="banner"><div class="banner-icon">⚡</div><div class="banner-text"><strong>Time to Level Up?</strong>You've been consistent — consider adding weight or an extra rep this week.</div><button class="banner-close" onclick="dismissBanner()">×</button></div>`;
  }
  const container=document.getElementById('day-cards');
  container.innerHTML='';
  DAYS.forEach(day=>{
    const exCount=(d.exercises[day.id]||[]).length;
    const cnt=(d.logs||[]).filter(l=>l.day===day.id).length;
    container.innerHTML+=`<div class="day-card" onclick="openDay('${day.id}')"><div><div class="day-label">${day.id}</div><div class="day-title">${day.label}</div><div class="day-subtitle">${day.subtitle}</div><div style="margin-top:10px;display:flex;gap:6px;flex-wrap:wrap;"><span class="pill">${exCount} exercise${exCount!==1?'s':''}</span>${cnt>0?`<span class="pill green">✓ ${cnt} session${cnt!==1?'s':''}</span>`:''}</div></div><div class="day-arrow">›</div></div>`;
  });
}
function dismissBanner(){localStorage.setItem('nextset_banner_dismissed',JSON.stringify(Date.now()));document.getElementById('progress-banner').innerHTML='';}

/* ── WORKOUT ── */
function openDay(dayId){
  currentDay=dayId;
  const day=DAYS.find(d=>d.id===dayId);
  const d=initData();
  document.getElementById('workout-header').innerHTML=`<div style="margin-bottom:16px;"><div class="day-label">${dayId}</div><div style="font-size:28px;font-weight:800;">${day.label}</div><div style="font-size:13px;color:var(--text-muted);margin-top:3px;">${day.subtitle}</div></div>`;
  renderExerciseList(d.exercises[dayId],dayId);
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.getElementById('screen-workout').classList.add('active');
  document.querySelectorAll('.nav-btn').forEach(b=>b.classList.remove('active'));
}
function renderExerciseList(exercises,dayId){
  const d=getData();
  const list=document.getElementById('exercise-list');
  if(!exercises||!exercises.length){list.innerHTML=`<div class="empty"><div class="empty-icon">⚡</div><p>No exercises yet.<br>Tap below to add your first one.</p></div>`;return;}
  list.innerHTML=exercises.map(ex=>{
    const logs=(d.logs||[]).filter(l=>l.day===dayId&&l.exercise===ex);
    const last=logs[logs.length-1];
    const lastText=last?`Last: ${last.weight}lb × ${last.reps} reps × 3 sets`:'No logs yet — tap to start';
    return `<div class="exercise-item" onclick="openLog('${ex.replace(/'/g,"\'")}')"><div class="exercise-controls"><div style="flex:1;"><div class="exercise-name">${ex}</div><div class="exercise-last">${lastText}</div></div><button class="delete-ex-btn" onclick="event.stopPropagation();deleteExercise('${ex.replace(/'/g,"\'")}')">×</button></div></div>`;
  }).join('');
}
function deleteExercise(name){
  if(!confirm(`Remove "${name}"?`))return;
  const d=getData();
  d.exercises[currentDay]=d.exercises[currentDay].filter(e=>e!==name);
  saveData(d);
  renderExerciseList(d.exercises[currentDay],currentDay);
}
function openAddExercise(){
  const day=DAYS.find(d=>d.id===currentDay);
  document.getElementById('add-ex-day-label').textContent=`Adding to ${day.label} day`;
  document.getElementById('new-ex-name').value='';
  document.getElementById('add-ex-modal').classList.add('open');
  setTimeout(()=>document.getElementById('new-ex-name').focus(),300);
}
function saveNewExercise(){
  const name=document.getElementById('new-ex-name').value.trim();
  if(!name)return;
  const d=getData();
  if(!d.exercises[currentDay].includes(name)){d.exercises[currentDay].push(name);saveData(d);}
  document.getElementById('add-ex-modal').classList.remove('open');
  renderExerciseList(d.exercises[currentDay],currentDay);
  showToast('Exercise Added',name,'➕');
}
document.getElementById('add-ex-modal').addEventListener('click',function(e){if(e.target===this)this.classList.remove('open');});

/* ── LOG ── */
function openLog(exerciseName){
  currentExercise=exerciseName;
  document.getElementById('log-modal-title').textContent=exerciseName;
  const d=getData();
  const logs=(d.logs||[]).filter(l=>l.day===currentDay&&l.exercise===exerciseName);
  const last=logs[logs.length-1];
  if(last){
    document.getElementById('log-modal-sub').textContent=`Last: ${last.weight}lb × ${last.reps} reps`;
    document.getElementById('log-weight').value=last.weight||'';
    document.getElementById('log-reps').value=last.reps||'';
  } else {
    document.getElementById('log-modal-sub').textContent='Enter weight and reps for all 3 sets';
    document.getElementById('log-weight').value='';
    document.getElementById('log-reps').value='';
  }
  document.getElementById('log-modal').classList.add('open');
}
function saveLog(){
  const weight=parseFloat(document.getElementById('log-weight').value)||0;
  const reps=parseInt(document.getElementById('log-reps').value)||0;
  if(!weight&&!reps){alert('Enter weight or reps!');return;}
  const d=getData();
  d.logs.push({day:currentDay,exercise:currentExercise,date:new Date().toISOString(),weight,reps,sets:3});
  saveData(d);
  document.getElementById('log-modal').classList.remove('open');
  renderExerciseList(d.exercises[currentDay],currentDay);
  showToast('Session Saved ✓',`${currentExercise} · ${weight}lb × ${reps} reps × 3 sets`,'✅');
}
document.getElementById('log-modal').addEventListener('click',function(e){if(e.target===this)this.classList.remove('open');});

/* ── EDIT / DELETE ── */
function openEdit(idx){
  editIndex=idx;
  const d=getData();
  const log=d.logs[idx];
  if(!log)return;
  document.getElementById('edit-modal-title').textContent=log.exercise;
  document.getElementById('edit-modal-sub').textContent=new Date(log.date).toLocaleDateString('en-US',{weekday:'short',month:'short',day:'numeric'});
  document.getElementById('edit-weight').value=log.weight||'';
  document.getElementById('edit-reps').value=log.reps||'';
  document.getElementById('edit-modal').classList.add('open');
}
function saveEdit(){
  const weight=parseFloat(document.getElementById('edit-weight').value)||0;
  const reps=parseInt(document.getElementById('edit-reps').value)||0;
  if(!weight&&!reps){alert('Enter weight or reps!');return;}
  const d=getData();
  if(!d.logs[editIndex])return;
  const name=d.logs[editIndex].exercise;
  d.logs[editIndex].weight=weight;
  d.logs[editIndex].reps=reps;
  saveData(d);
  document.getElementById('edit-modal').classList.remove('open');
  renderHistory();
  showToast('Session Updated',`${name} · ${weight}lb × ${reps} reps`,'✏️');
}
function confirmDelete(){
  const d=getData();
  if(!d.logs[editIndex]){document.getElementById('edit-modal').classList.remove('open');return;}
  const name=d.logs[editIndex].exercise;
  // Close modal first, then delete
  document.getElementById('edit-modal').classList.remove('open');
  setTimeout(()=>{
    const d2=getData();
    d2.logs.splice(editIndex,1);
    saveData(d2);
    renderHistory();
    showToast('Session Deleted',name,'🗑');
    editIndex=null;
  },300);
}
document.getElementById('edit-modal').addEventListener('click',function(e){if(e.target===this)this.classList.remove('open');});

/* ── HISTORY + CALENDAR ── */
function renderHistory(){
  const d=getData();
  const logs=d.logs||[];
  // Calendar
  renderCalendar(logs);
  // Log list
  const list=document.getElementById('history-list');
  if(!logs.length){list.innerHTML=`<div class="empty"><div class="empty-icon">📋</div><p>No sessions logged yet.<br>Head to Workout to get started.</p></div>`;return;}
  const reversed=[...logs].map((l,i)=>({...l,origIdx:i})).reverse();
  const byDate={};
  reversed.forEach(l=>{
    const key=new Date(l.date).toLocaleDateString('en-US',{weekday:'long',month:'long',day:'numeric',year:'numeric'});
    if(!byDate[key])byDate[key]=[];
    byDate[key].push(l);
  });
  list.innerHTML=Object.entries(byDate).map(([date,entries])=>`
    <div class="history-item">
      <div class="history-date">${date}</div>
      ${entries.map(e=>`
        <div class="history-entry">
          <div><span class="history-ex">${e.exercise}</span><span class="history-day-badge badge-${e.day}">${e.day}</span></div>
          <div style="display:flex;align-items:center;">
            <span class="history-val">${e.weight}lb × ${e.reps}</span>
            <button class="edit-log-btn" onclick="openEdit(${e.origIdx})">✏️</button>
          </div>
        </div>`).join('')}
    </div>`).join('');
}

function renderCalendar(logs){
  const year=calMonth.getFullYear();
  const month=calMonth.getMonth();
  const workedDays=new Set((logs||[]).map(l=>l.date.slice(0,10)));
  const today=new Date().toISOString().slice(0,10);
  const firstDay=new Date(year,month,1).getDay();
  const daysInMonth=new Date(year,month+1,0).getDate();
  const monthName=calMonth.toLocaleDateString('en-US',{month:'long',year:'numeric'});
  let cells='';
  for(let i=0;i<firstDay;i++)cells+=`<div class="cal-cell empty-cell"></div>`;
  for(let day=1;day<=daysInMonth;day++){
    const ds=`${year}-${String(month+1).padStart(2,'0')}-${String(day).padStart(2,'0')}`;
    const worked=workedDays.has(ds);
    const isToday=ds===today;
    cells+=`<div class="cal-cell${worked?' worked':''}${isToday?' today':''}">${day}${worked?'<div class="cal-dot"></div>':''}</div>`;
  }
  document.getElementById('cal-container').innerHTML=`
    <div class="cal-wrap">
      <div class="cal-header">
        <button class="cal-nav" onclick="shiftMonth(-1)">‹</button>
        <div class="cal-title">${monthName}</div>
        <button class="cal-nav" onclick="shiftMonth(1)">›</button>
      </div>
      <div class="cal-days-row"><span>Su</span><span>Mo</span><span>Tu</span><span>We</span><span>Th</span><span>Fr</span><span>Sa</span></div>
      <div class="cal-grid">${cells}</div>
    </div>`;
}
function shiftMonth(dir){
  calMonth=new Date(calMonth.getFullYear(),calMonth.getMonth()+dir,1);
  renderCalendar(getData().logs||[]);
}

/* ── PROGRESS ── */
function renderProgress(){
  const d=getData();const logs=d.logs||[];
  const totalSessions=[...new Set(logs.map(l=>l.date.slice(0,10)))].length;
  const totalSets=logs.reduce((a,l)=>a+(l.sets||3),0);
  const allExercises=[...new Set(logs.map(l=>l.exercise))];
  const pc=logs.filter(l=>l.day==='push').length;
  const plc=logs.filter(l=>l.day==='pull').length;
  const lc=logs.filter(l=>l.day==='legs').length;
  document.getElementById('stat-grid').innerHTML=`<div class="stat-box"><div class="stat-val">${totalSessions}</div><div class="stat-lbl">Workout Days</div></div><div class="stat-box"><div class="stat-val">${totalSets}</div><div class="stat-lbl">Total Sets</div></div><div class="stat-box"><div class="stat-val">${allExercises.length}</div><div class="stat-lbl">Exercises Logged</div></div><div class="stat-box"><div class="stat-val">${logs.length}</div><div class="stat-lbl">Total Logs</div></div>`;
  const mf=Math.max(pc,plc,lc,1);
  document.getElementById('freq-chart').innerHTML=`<div class="chart-container"><div class="chart-title">Sessions per Day Type</div><div class="bar-chart">${[['push','Push',pc],['pull','Pull',plc],['legs','Legs',lc]].map(([id,lbl,cnt])=>`<div class="bar-wrap"><div class="bar-val">${cnt}</div><div class="bar${cnt===0?' dim':''}" style="height:${Math.max(4,(cnt/mf)*64)}px"></div><div class="bar-lbl">${lbl}</div></div>`).join('')}</div></div>`;
  const sel=document.getElementById('ex-select');
  sel.innerHTML='<option value="">Select an exercise...</option>';
  allExercises.forEach(ex=>{const o=document.createElement('option');o.value=ex;o.textContent=ex;sel.appendChild(o);});
  document.getElementById('ex-progress-chart').innerHTML='';
}
function renderExerciseProgress(){
  const ex=document.getElementById('ex-select').value;
  if(!ex){document.getElementById('ex-progress-chart').innerHTML='';return;}
  const d=getData();
  const logs=(d.logs||[]).filter(l=>l.exercise===ex).slice(-8);
  if(!logs.length){document.getElementById('ex-progress-chart').innerHTML='<div class="empty"><p>No logs for this exercise yet.</p></div>';return;}
  const weights=logs.map(l=>l.weight||0);
  const maxVal=Math.max(...weights,1);
  const bars=logs.map((l,i)=>{const dt=new Date(l.date);const lb=(dt.getMonth()+1)+'/'+dt.getDate();const h=Math.max(4,(weights[i]/maxVal)*64);return `<div class="bar-wrap"><div class="bar-val">${weights[i]}</div><div class="bar" style="height:${h}px"></div><div class="bar-lbl">${lb}</div></div>`;}).join('');
  document.getElementById('ex-progress-chart').innerHTML=`<div class="chart-container"><div class="chart-title">${ex} — Weight (lbs)</div><div class="bar-chart">${bars}</div></div>`;
}

initData();renderHome();
setInterval(()=>{const dis=JSON.parse(localStorage.getItem('nextset_banner_dismissed')||'null');if(dis&&Date.now()-dis>14*24*60*60*1000){localStorage.removeItem('nextset_banner_dismissed');renderHome();}},60000);
</script>
</body>
</html>
