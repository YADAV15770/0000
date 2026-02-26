{
  "name": "2026 Days Tracker",
  "short_name": "2026 Tracker",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#f0f0f0",
  "theme_color": "#ffffff",
  "icons": [
    {
      "src": "https://via.placeholder.com/192x192/e53935/ffffff?text=26",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "https://via.placeholder.com/512x512/e53935/ffffff?text=26",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}const MONTHS = ["January","February","March","April","May","June","July","August","September","October","November","December"];
const MONTH_DAYS = [31,28,31,30,31,30,31,31,30,31,30,31];

function getDayOfYear(date) {
  const start = new Date(2026, 0, 1);
  return Math.floor((date - start) / (1000 * 60 * 60 * 24)) + 1;
}

function getDayInfo(dayNum) {
  let m = 0, d = dayNum;
  for (let i = 0; i < 12; i++) {
    if (d <= MONTH_DAYS[i]) { m = i; break; }
    d -= MONTH_DAYS[i];
  }
  return { month: MONTHS[m], day: d, dayNum };
}

const today = new Date();
const isYear2026 = today.getFullYear() === 2026;
const currentDay = isYear2026 ? getDayOfYear(today) : 0;
const totalDays = 365;
const daysPast = Math.max(0, currentDay - 1);
const pct = Math.round((daysPast / totalDays) * 100);

// Stats update
document.getElementById("past-count").textContent = daysPast;
document.getElementById("pct-val").textContent = pct + "%";
document.getElementById("left-count").textContent = totalDays - daysPast;
document.getElementById("prog-bar").style.width = pct + "%";

// Build months
const container = document.getElementById("months-container");
const tooltip = document.getElementById("tooltip-area");

MONTHS.forEach((mon, mi) => {
  const daysInMonth = MONTH_DAYS[mi];
  const monthStartDay = MONTH_DAYS.slice(0, mi).reduce((a, b) => a + b, 0) + 1;

  const block = document.createElement("div");
  block.className = "month-block";

  const label = document.createElement("div");
  label.className = "month-name";
  label.textContent = mon.toUpperCase();
  block.appendChild(label);

  const row = document.createElement("div");
  row.className = "days-row";

  for (let i = 0; i < daysInMonth; i++) {
    const dayNum = monthStartDay + i;
    const info = getDayInfo(dayNum);
    const sq = document.createElement("div");
    sq.className = "day-sq";
    if (dayNum < currentDay) sq.classList.add("past");
    else if (dayNum === currentDay) sq.classList.add("today");

    sq.addEventListener("mouseenter", () => {
      const isPast = dayNum < currentDay;
      const isCurrent = dayNum === currentDay;
      const color = isPast ? "#e53935" : isCurrent ? "#f57c00" : "#555";
      const bg = isPast ? "#fff5f5" : isCurrent ? "#fff8f0" : "#f9f9f9";
      const border = isPast ? "#ffcdd2" : isCurrent ? "#ffe0b2" : "#eee";
      const msg = isPast ? " · Beet gaya ✓" : isCurrent ? " · Aaj 🔥" : " · Aane wala hai";
      tooltip.innerHTML = `<span style="background:${bg};color:${color};border:1px solid ${border}">${info.day} ${info.month} 2026 — Day ${dayNum}${msg}</span>`;
    });

    sq.addEventListener("mouseleave", () => {
      tooltip.innerHTML = "Kisi bhi square par hover karo";
    });

    row.appendChild(sq);
  }

  block.appendChild(row);
  container.appendChild(block);
});<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>2026 Days Tracker</title>
  <link rel="manifest" href="manifest.json" />
  <meta name="theme-color" content="#ffffff" />
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      background: #f0f0f0;
      font-family: sans-serif;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 24px;
    }
    .card {
      background: #fff;
      border-radius: 16px;
      box-shadow: 0 4px 32px rgba(0,0,0,0.12);
      padding: 32px;
      width: 100%;
      max-width: 680px;
    }
    .title-area {
      text-align: center;
      margin-bottom: 24px;
      border-bottom: 2px solid #f0f0f0;
      padding-bottom: 20px;
    }
    h1 { font-size: 28px; font-weight: 800; color: #111; letter-spacing: 3px; }
    .subtitle { color: #999; font-size: 13px; margin-top: 6px; }
    .stats { display: flex; gap: 12px; margin-bottom: 20px; }
    .stat-box {
      flex: 1;
      border-radius: 10px;
      padding: 12px 8px;
      text-align: center;
    }
    .stat-val { font-size: 22px; font-weight: 700; }
    .stat-lbl { font-size: 11px; color: #999; margin-top: 2px; letter-spacing: 1px; }
    .progress-wrap {
      background: #f5f5f5;
      border-radius: 6px;
      height: 8px;
      margin-bottom: 24px;
      overflow: hidden;
      border: 1px solid #eee;
    }
    .progress-bar {
      height: 100%;
      background: linear-gradient(90deg, #e53935, #f57c00);
      border-radius: 6px;
      transition: width 0.5s;
    }
    .month-block {
      margin-bottom: 16px;
      background: #fafafa;
      border-radius: 10px;
      padding: 12px 14px;
      border: 1px solid #eee;
    }
    .month-name {
      font-size: 12px;
      font-weight: 700;
      color: #555;
      letter-spacing: 2px;
      margin-bottom: 8px;
    }
    .days-row { display: flex; flex-wrap: wrap; gap: 4px; }
    .day-sq {
      width: 18px;
      height: 18px;
      border-radius: 3px;
      cursor: pointer;
      transition: transform 0.12s;
      border: 2px solid #ddd;
      background: #fff;
    }
    .day-sq:hover { transform: scale(1.35); box-shadow: 0 2px 8px rgba(0,0,0,0.2); }
    .day-sq.past { background: #e53935; border-color: #c62828; }
    .day-sq.today { background: #f57c00; border-color: #f57c00; }
    .tooltip {
      min-height: 36px;
      display: flex;
      align-items: center;
      justify-content: center;
      margin: 8px 0;
      font-size: 13px;
      color: #aaa;
    }
    .tooltip span {
      padding: 6px 18px;
      border-radius: 20px;
      font-weight: 600;
    }
    .legend {
      display: flex;
      justify-content: center;
      gap: 20px;
      padding-top: 16px;
      border-top: 1px solid #f0f0f0;
    }
    .legend-item { display: flex; align-items: center; gap: 6px; font-size: 11px; color: #888; }
    .legend-sq { width: 14px; height: 14px; border-radius: 3px; }
  </style>
</head>
<body>
  <div class="card">
    <div class="title-area">
      <h1>2026</h1>
      <p class="subtitle">365 Days Calendar Tracker</p>
    </div>

    <div class="stats">
      <div class="stat-box" style="background:#fff5f5;border:1px solid #ffcdd2">
        <div class="stat-val" style="color:#e53935" id="past-count">0</div>
        <div class="stat-lbl">BEET GAYE</div>
      </div>
      <div class="stat-box" style="background:#fff8f0;border:1px solid #ffe0b2">
        <div class="stat-val" style="color:#f57c00" id="pct-val">0%</div>
        <div class="stat-lbl">SAAL GAYA</div>
      </div>
      <div class="stat-box" style="background:#f1f8e9;border:1px solid #c5e1a5">
        <div class="stat-val" style="color:#2e7d32" id="left-count">365</div>
        <div class="stat-lbl">BAKI HAIN</div>
      </div>
    </div>

    <div class="progress-wrap">
      <div class="progress-bar" id="prog-bar" style="width:0%"></div>
    </div>

    <div id="months-container"></div>

    <div class="tooltip" id="tooltip-area">Kisi bhi square par hover karo</div>

    <div class="legend">
      <div class="legend-item">
        <div class="legend-sq" style="background:#e53935;border:2px solid #c62828"></div> Beet gaye din
      </div>
      <div class="legend-item">
        <div class="legend-sq" style="background:#f57c00;border:2px solid #f57c00"></div> Aaj ka din
      </div>
      <div class="legend-item">
        <div class="legend-sq" style="background:#fff;border:2px solid #ddd"></div> Aane wale din
      </div>
    </div>
  </div>

  <script src="app.js"></script>
</body>
</html>
