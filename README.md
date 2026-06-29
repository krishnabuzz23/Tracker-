<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Habit Tracker</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Sora:wght@300;400;600;700&display=swap');

  :root {
    --bg: #0f0f13;
    --surface: #17171f;
    --card: #1e1e2a;
    --border: #2a2a38;
    --text: #e8e8f0;
    --muted: #6b6b80;
    --accent: #7c6af7;
    --accent2: #f76a8f;
    --green: #4ade80;
    --yellow: #fbbf24;

    --h1: #workout;
    --c0: #7c6af7;
    --c1: #f76a8f;
    --c2: #38bdf8;
    --c3: #4ade80;
    --c4: #fbbf24;
    --c5: #fb923c;
    --c6: #e879f9;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Sora', sans-serif;
    min-height: 100vh;
    padding: 24px 16px 60px;
  }

  /* HEADER */
  .header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 28px;
    flex-wrap: wrap;
    gap: 12px;
  }
  .header-left h1 {
    font-size: 22px;
    font-weight: 700;
    letter-spacing: -0.5px;
  }
  .header-left p {
    color: var(--muted);
    font-size: 12px;
    margin-top: 3px;
    font-family: 'DM Mono', monospace;
  }
  .overall-ring {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4px;
  }
  .overall-ring svg { width: 70px; height: 70px; }
  .ring-label {
    font-size: 11px;
    color: var(--muted);
    font-family: 'DM Mono', monospace;
    text-align: center;
  }
  .ring-pct {
    font-size: 14px;
    font-weight: 700;
    fill: var(--text);
    font-family: 'Sora', sans-serif;
  }

  /* SECTION LABEL */
  .section-label {
    font-size: 10px;
    font-family: 'DM Mono', monospace;
    color: var(--muted);
    letter-spacing: 1.5px;
    text-transform: uppercase;
    margin-bottom: 10px;
  }

  /* WEEKLY RINGS */
  .week-rings {
    display: flex;
    gap: 10px;
    margin-bottom: 28px;
    overflow-x: auto;
    padding-bottom: 4px;
  }
  .week-ring-wrap {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 5px;
    flex-shrink: 0;
  }
  .week-ring-wrap svg { width: 60px; height: 60px; }
  .week-ring-label {
    font-size: 10px;
    color: var(--muted);
    font-family: 'DM Mono', monospace;
  }
  .week-ring-pct {
    font-size: 12px;
    font-weight: 700;
    fill: var(--text);
  }

  /* DAILY GRID */
  .grid-section { margin-bottom: 28px; }
  .grid-scroll { overflow-x: auto; }
  .grid-table {
    border-collapse: separate;
    border-spacing: 0;
    width: 100%;
    min-width: 600px;
  }
  .grid-table th {
    font-size: 10px;
    font-family: 'DM Mono', monospace;
    color: var(--muted);
    padding: 6px 4px;
    text-align: center;
    white-space: nowrap;
    font-weight: 400;
  }
  .grid-table th.habit-col {
    text-align: left;
    padding-left: 0;
    min-width: 110px;
  }
  .grid-table th.week-header {
    color: var(--accent);
    font-size: 9px;
    padding-bottom: 2px;
  }
  .grid-table td {
    padding: 3px 3px;
    text-align: center;
    vertical-align: middle;
  }
  .grid-table td.habit-name {
    text-align: left;
    font-size: 12px;
    padding-right: 10px;
    white-space: nowrap;
    font-weight: 600;
  }
  .habit-dot {
    display: inline-block;
    width: 8px; height: 8px;
    border-radius: 50%;
    margin-right: 6px;
    vertical-align: middle;
    flex-shrink: 0;
  }
  .cb-wrap {
    width: 22px; height: 22px;
    margin: 0 auto;
    position: relative;
  }
  .cb-wrap input[type="checkbox"] {
    opacity: 0;
    position: absolute;
    width: 100%; height: 100%;
    cursor: pointer;
    margin: 0;
    z-index: 2;
  }
  .cb-visual {
    position: absolute;
    inset: 0;
    border: 1.5px solid var(--border);
    border-radius: 5px;
    background: var(--surface);
    transition: all 0.15s ease;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .cb-visual svg {
    width: 11px; height: 11px;
    opacity: 0;
    transition: opacity 0.15s;
  }
  .cb-wrap input:checked ~ .cb-visual {
    border-color: transparent;
  }
  .cb-wrap input:checked ~ .cb-visual svg {
    opacity: 1;
  }
  .day-future .cb-visual {
    opacity: 0.35;
    pointer-events: none;
  }

  /* STATS TABLE */
  .stats-section { margin-bottom: 28px; }
  .stats-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 14px;
    overflow: hidden;
  }
  .stats-header-row {
    display: grid;
    grid-template-columns: 1fr 80px 70px 70px;
    padding: 10px 14px;
    border-bottom: 1px solid var(--border);
    font-size: 9px;
    font-family: 'DM Mono', monospace;
    color: var(--muted);
    letter-spacing: 0.8px;
    text-transform: uppercase;
  }
  .stats-row {
    display: grid;
    grid-template-columns: 1fr 80px 70px 70px;
    padding: 11px 14px;
    border-bottom: 1px solid var(--border);
    align-items: center;
  }
  .stats-row:last-child { border-bottom: none; }
  .stats-habit {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 12px;
    font-weight: 600;
  }
  .bar-wrap {
    display: flex;
    align-items: center;
    gap: 6px;
  }
  .bar-track {
    flex: 1;
    height: 5px;
    background: var(--border);
    border-radius: 99px;
    overflow: hidden;
  }
  .bar-fill {
    height: 100%;
    border-radius: 99px;
    transition: width 0.4s ease;
  }
  .bar-pct {
    font-size: 10px;
    font-family: 'DM Mono', monospace;
    color: var(--muted);
    width: 32px;
    text-align: right;
  }
  .stat-count {
    font-size: 11px;
    font-family: 'DM Mono', monospace;
    color: var(--text);
    text-align: center;
  }
  .stat-streak {
    font-size: 11px;
    font-family: 'DM Mono', monospace;
    color: var(--yellow);
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 3px;
  }

  /* WEEKLY BAR CHART */
  .chart-section { margin-bottom: 28px; }
  .chart-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 16px;
  }
  .chart-bars {
    display: flex;
    gap: 6px;
    align-items: flex-end;
    height: 80px;
    margin-top: 10px;
  }
  .chart-week-group {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4px;
  }
  .chart-week-bars {
    display: flex;
    gap: 2px;
    align-items: flex-end;
    height: 66px;
    width: 100%;
    justify-content: center;
  }
  .chart-bar {
    flex: 1;
    max-width: 12px;
    border-radius: 3px 3px 0 0;
    min-height: 3px;
    transition: height 0.4s ease;
  }
  .chart-week-label {
    font-size: 9px;
    font-family: 'DM Mono', monospace;
    color: var(--muted);
  }

  /* MOTIVATIONAL BANNER */
  .moti-banner {
    background: linear-gradient(135deg, #1a1730, #1e1226);
    border: 1px solid #2d2050;
    border-radius: 14px;
    padding: 16px;
    text-align: center;
    margin-bottom: 28px;
  }
  .moti-banner p {
    font-size: 13px;
    color: #a89af7;
    font-style: italic;
  }
  .moti-banner .streak-total {
    font-size: 28px;
    font-weight: 700;
    color: var(--accent);
    font-family: 'DM Mono', monospace;
    display: block;
    margin-bottom: 4px;
  }

  .today-marker {
    background: var(--accent);
    color: white;
    border-radius: 4px;
    font-size: 9px;
    padding: 1px 3px;
  }

  /* SCROLLBAR */
  ::-webkit-scrollbar { height: 4px; width: 4px; }
  ::-webkit-scrollbar-track { background: transparent; }
  ::-webkit-scrollbar-thumb { background: var(--border); border-radius: 99px; }
</style>
</head>
<body>

<div class="header">
  <div class="header-left">
    <h1>Habit Tracker</h1>
    <p id="dateRange">Loading...</p>
  </div>
  <div class="overall-ring">
    <svg viewBox="0 0 70 70">
      <circle cx="35" cy="35" r="28" fill="none" stroke="#2a2a38" stroke-width="6"/>
      <circle id="overallRingCircle" cx="35" cy="35" r="28" fill="none" stroke="#7c6af7"
        stroke-width="6" stroke-linecap="round"
        stroke-dasharray="175.93" stroke-dashoffset="175.93"
        transform="rotate(-90 35 35)" style="transition:stroke-dashoffset 0.6s ease"/>
      <text x="35" y="39" text-anchor="middle" class="ring-pct" id="overallPct">0%</text>
    </svg>
    <div class="ring-label">overall</div>
  </div>
</div>

<!-- WEEK RINGS -->
<div class="section-label">weekly overview</div>
<div class="week-rings" id="weekRings"></div>

<!-- DAILY GRID -->
<div class="grid-section">
  <div class="section-label">daily check-in</div>
  <div class="grid-scroll">
    <table class="grid-table" id="gridTable"></table>
  </div>
</div>

<!-- STATS -->
<div class="stats-section">
  <div class="section-label">progress report</div>
  <div class="stats-card" id="statsCard"></div>
</div>

<!-- CHART -->
<div class="chart-section">
  <div class="section-label">weekly completion</div>
  <div class="chart-card">
    <div class="chart-bars" id="weeklyChart"></div>
  </div>
</div>

<!-- MOTIVATIONAL -->
<div class="moti-banner">
  <span class="streak-total" id="totalChecks">0</span>
  <p id="motiText">Check off your first habit to get started 🚀</p>
</div>

<script>
const HABITS = [
  { name: 'Workout',    color: '#7c6af7' },
  { name: 'Learning',   color: '#f76a8f' },
  { name: 'Reading',    color: '#38bdf8' },
  { name: 'Running',    color: '#4ade80' },
  { name: 'Good Sleep', color: '#fbbf24' },
  { name: 'No Sugar',   color: '#fb923c' },
  { name: 'No Junk',    color: '#e879f9' },
];

const DAYS = 30;
const STORAGE_KEY = 'habit_tracker_v1';

// State
let state = {};  // state[day][habitIdx] = true/false

function getStartDate() {
  const s = localStorage.getItem(STORAGE_KEY + '_start');
  if (s) return new Date(s);
  const d = new Date();
  d.setHours(0,0,0,0);
  localStorage.setItem(STORAGE_KEY + '_start', d.toISOString());
  return d;
}

function loadState() {
  try {
    const s = localStorage.getItem(STORAGE_KEY);
    if (s) state = JSON.parse(s);
  } catch(e) { state = {}; }
}

function saveState() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
}

function toggle(day, hIdx) {
  if (!state[day]) state[day] = {};
  state[day][hIdx] = !state[day][hIdx];
  saveState();
  render();
}

const startDate = getStartDate();

function getDayDate(i) {
  const d = new Date(startDate);
  d.setDate(d.getDate() + i);
  return d;
}

function todayIdx() {
  const now = new Date();
  now.setHours(0,0,0,0);
  const diff = Math.floor((now - startDate) / 86400000);
  return Math.max(0, Math.min(diff, DAYS-1));
}

function getChecked(day, hIdx) {
  return !!(state[day] && state[day][hIdx]);
}

function render() {
  const today = todayIdx();

  // Date range label
  const endDate = getDayDate(DAYS - 1);
  document.getElementById('dateRange').textContent =
    `${fmt(startDate)} — ${fmt(endDate)}`;

  // Compute stats per habit
  const habitsStats = HABITS.map((h, hIdx) => {
    let count = 0, streak = 0, maxStreak = 0, cur = 0;
    for (let d = 0; d <= today; d++) {
      if (getChecked(d, hIdx)) { count++; cur++; maxStreak = Math.max(maxStreak, cur); }
      else { streak = Math.max(streak, cur); cur = 0; }
    }
    streak = Math.max(streak, cur);
    const pct = today >= 0 ? Math.round(count / (today + 1) * 100) : 0;
    return { count, streak, maxStreak, pct };
  });

  // Overall %
  let totalPossible = (today + 1) * HABITS.length;
  let totalDone = 0;
  HABITS.forEach((h, hIdx) => { totalDone += habitsStats[hIdx].count; });
  const overallPct = totalPossible > 0 ? Math.round(totalDone / totalPossible * 100) : 0;

  document.getElementById('overallPct').textContent = overallPct + '%';
  const circ = 2 * Math.PI * 28;
  document.getElementById('overallRingCircle').setAttribute('stroke-dasharray', circ);
  document.getElementById('overallRingCircle').setAttribute('stroke-dashoffset', circ * (1 - overallPct/100));

  // Week rings
  const weekColors = ['#7c6af7','#f76a8f','#38bdf8','#fbbf24','#4ade80'];
  let wHTML = '';
  for (let w = 0; w < Math.ceil(DAYS/7); w++) {
    const wStart = w * 7;
    const wEnd = Math.min(wStart + 7, DAYS) - 1;
    let wDone = 0, wPoss = 0;
    for (let d = wStart; d <= Math.min(wEnd, today); d++) {
      HABITS.forEach((h, hIdx) => {
        wPoss++;
        if (getChecked(d, hIdx)) wDone++;
      });
    }
    const wPct = wPoss > 0 ? Math.round(wDone / wPoss * 100) : 0;
    const wCirc = 2 * Math.PI * 23;
    const color = weekColors[w % weekColors.length];
    wHTML += `
      <div class="week-ring-wrap">
        <svg viewBox="0 0 60 60">
          <circle cx="30" cy="30" r="23" fill="none" stroke="#2a2a38" stroke-width="5"/>
          <circle cx="30" cy="30" r="23" fill="none" stroke="${color}"
            stroke-width="5" stroke-linecap="round"
            stroke-dasharray="${wCirc}" stroke-dashoffset="${wCirc*(1-wPct/100)}"
            transform="rotate(-90 30 30)" style="transition:stroke-dashoffset 0.5s"/>
          <text x="30" y="34" text-anchor="middle" class="week-ring-pct" fill="white" font-family="Sora,sans-serif" font-size="11" font-weight="700">${wPct}%</text>
        </svg>
        <div class="week-ring-label">wk ${w+1}</div>
      </div>`;
  }
  document.getElementById('weekRings').innerHTML = wHTML;

  // Grid table
  const dayNames = ['S','M','T','W','T','F','S'];
  let thead = '<thead><tr><th class="habit-col">habit</th>';
  
  // Week group headers
  for (let w = 0; w < Math.ceil(DAYS/7); w++) {
    const wStart = w * 7;
    const wEnd = Math.min(wStart + 7, DAYS);
    thead += `<th colspan="${wEnd-wStart}" class="week-header">WK${w+1}</th>`;
  }
  thead += '</tr><tr><th class="habit-col"></th>';
  for (let d = 0; d < DAYS; d++) {
    const dd = getDayDate(d);
    const dn = dayNames[dd.getDay()];
    const isToday = d === today;
    thead += `<th>${isToday ? `<span class="today-marker">${dn}</span>` : dn}</th>`;
  }
  thead += '</tr></thead>';

  let tbody = '<tbody>';
  HABITS.forEach((h, hIdx) => {
    tbody += `<tr>
      <td class="habit-name"><span class="habit-dot" style="background:${h.color}"></span>${h.name}</td>`;
    for (let d = 0; d < DAYS; d++) {
      const isFuture = d > today;
      const checked = getChecked(d, hIdx);
      const color = h.color;
      tbody += `<td class="${isFuture ? 'day-future' : ''}">
        <div class="cb-wrap">
          <input type="checkbox" ${checked?'checked':''} ${isFuture?'disabled':''}
            onchange="toggle(${d},${hIdx})">
          <div class="cb-visual" style="${checked ? `background:${color}22;border-color:${color}` : ''}">
            <svg viewBox="0 0 12 12" fill="none">
              <polyline points="2,6 5,9 10,3" stroke="${color}" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
        </div>
      </td>`;
    }
    tbody += '</tr>';
  });
  tbody += '</tbody>';
  document.getElementById('gridTable').innerHTML = thead + tbody;

  // Stats card
  const sorted = HABITS.map((h,i) => ({...h, ...habitsStats[i], idx: i}))
    .sort((a,b) => b.pct - a.pct);
  let statsHTML = `
    <div class="stats-header-row">
      <div>habit</div>
      <div>completion</div>
      <div style="text-align:center">days</div>
      <div style="text-align:center">🔥 streak</div>
    </div>`;
  sorted.forEach(h => {
    statsHTML += `
      <div class="stats-row">
        <div class="stats-habit">
          <span class="habit-dot" style="background:${h.color}"></span>
          ${h.name}
        </div>
        <div class="bar-wrap">
          <div class="bar-track">
            <div class="bar-fill" style="width:${h.pct}%;background:${h.color}"></div>
          </div>
          <div class="bar-pct">${h.pct}%</div>
        </div>
        <div class="stat-count">${h.count} / ${today+1}</div>
        <div class="stat-streak">${h.streak > 0 ? '🔥' : '—'} ${h.streak > 0 ? h.streak+'d' : ''}</div>
      </div>`;
  });
  document.getElementById('statsCard').innerHTML = statsHTML;

  // Weekly chart
  const wColors = ['#7c6af7','#f76a8f','#38bdf8','#fbbf24','#4ade80'];
  let chartHTML = '';
  for (let w = 0; w < Math.ceil(DAYS/7); w++) {
    const wStart = w * 7;
    const wEnd = Math.min(wStart + 7, DAYS);
    let barsHTML = '';
    for (let d = wStart; d < wEnd; d++) {
      if (d > today) {
        barsHTML += `<div class="chart-bar" style="height:3px;background:#2a2a38"></div>`;
        continue;
      }
      let dayDone = 0;
      HABITS.forEach((h,hIdx) => { if (getChecked(d,hIdx)) dayDone++; });
      const pct = dayDone / HABITS.length;
      const h = Math.max(3, Math.round(pct * 60));
      barsHTML += `<div class="chart-bar" style="height:${h}px;background:${wColors[w%wColors.length]}"></div>`;
    }
    chartHTML += `
      <div class="chart-week-group">
        <div class="chart-week-bars">${barsHTML}</div>
        <div class="chart-week-label">wk${w+1}</div>
      </div>`;
  }
  document.getElementById('weeklyChart').innerHTML = chartHTML;

  // Motivational
  document.getElementById('totalChecks').textContent = totalDone;
  const msgs = [
    [0, "Check off your first habit to get started 🚀"],
    [1, "You've begun. That's the hardest part. 💪"],
    [10, "Building momentum! Keep showing up. ⚡"],
    [30, "A solid streak forming. Don't break the chain! 🔥"],
    [70, "You're in the zone. Consistency is your superpower. 🏆"],
    [140, "Incredible discipline. Most people quit by now. 👑"],
    [200, "You're on another level. Absolute machine. 🦾"],
  ];
  let moti = msgs[0][1];
  for (const [t, m] of msgs) { if (totalDone >= t) moti = m; }
  document.getElementById('motiText').textContent = moti;
}

function fmt(d) {
  return d.toLocaleDateString('en-US', { month:'short', day:'numeric', year:'numeric' });
}

loadState();
render();
</script>
</body>
</html>

