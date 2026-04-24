<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>每日喝水记录</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    :root {
      --card: #ffffff;
      --text: #1f2a44;
      --subtle: #6b7892;
      --primary: #3b82f6;
      --primary-soft: #dbeafe;
      --success-bg: #dcfce7;
      --success-text: #166534;
      --danger-bg: #fee2e2;
      --danger-text: #b91c1c;
      --radius: 16px;
    }
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "PingFang SC", "Microsoft YaHei", sans-serif;
      color: var(--text);
      background: linear-gradient(180deg, #eef5ff 0%, #f9fbff 100%);
      min-height: 100vh;
      padding: 14px;
    }
    .container {
      max-width: 560px;
      margin: 0 auto;
      display: grid;
      gap: 12px;
    }
    .card {
      background: var(--card);
      border-radius: var(--radius);
      padding: 14px;
      box-shadow: 0 8px 24px rgba(31, 42, 68, 0.08);
    }
    h1 { margin: 0 0 6px; font-size: 22px; }
    .subtitle, .small-text { color: var(--subtle); font-size: 13px; margin: 0; }
    .row {
      display: grid;
      grid-template-columns: 1fr auto;
      align-items: center;
      gap: 8px;
      margin: 10px 0;
    }
    .multi-row {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
    }
    label { font-size: 13px; color: var(--subtle); }
    input[type="number"] {
      width: 100%;
      border: 1px solid #d6e3ff;
      border-radius: 10px;
      padding: 8px 10px;
      font-size: 14px;
      color: var(--text);
      outline: none;
      margin-top: 4px;
    }
    .progress-wrap {
      background: #edf2ff;
      border-radius: 999px;
      height: 14px;
      overflow: hidden;
      margin: 8px 0 10px;
    }
    .progress-bar {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, #60a5fa, #2563eb);
      transition: width 0.25s ease;
    }
    .progress-text { font-weight: 600; font-size: 14px; }
    .btn-grid {
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 8px;
      margin-top: 10px;
    }
    button {
      border: none;
      border-radius: 11px;
      padding: 10px;
      font-size: 14px;
      font-weight: 600;
      cursor: pointer;
    }
    .btn-primary { background: var(--primary); color: #fff; }
    .btn-soft { background: var(--primary-soft); color: #1e40af; }
    .btn-danger { background: var(--danger-bg); color: var(--danger-text); }
    .btn-plain {
      background: #f3f6ff;
      color: #1f2a44;
      border: 1px solid #dbe6ff;
    }
    .status-ok { color: #15803d; }
    .status-bad { color: #b91c1c; }
    .history-list, .log-list {
      list-style: none;
      margin: 10px 0 0;
      padding: 0;
      display: grid;
      gap: 8px;
    }
    .history-item, .log-item {
      border: 1px solid #e2ebff;
      background: #f8faff;
      border-radius: 12px;
      padding: 8px 10px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 13px;
    }
    .calendar-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 10px;
    }
    .calendar-title { font-weight: 700; }
    .weekdays, .calendar-grid {
      display: grid;
      grid-template-columns: repeat(7, 1fr);
      gap: 6px;
    }
    .weekdays div {
      text-align: center;
      font-size: 12px;
      color: var(--subtle);
    }
    .day-cell {
      min-height: 44px;
      border-radius: 10px;
      padding: 6px 4px;
      text-align: center;
      font-size: 12px;
      border: 1px solid #e5ecff;
      background: #fff;
    }
    .day-other { opacity: 0.4; }
    .day-hit {
      background: var(--success-bg);
      color: var(--success-text);
      border-color: #86efac;
    }
    .day-miss {
      background: var(--danger-bg);
      color: var(--danger-text);
      border-color: #fca5a5;
    }
    .badge-wrap {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
      margin-top: 8px;
    }
    .badge {
      font-size: 12px;
      padding: 6px 8px;
      border-radius: 999px;
      background: #e0e7ff;
      color: #3730a3;
      border: 1px solid #c7d2fe;
    }
    .badge.locked {
      background: #f1f5f9;
      color: #64748b;
      border-color: #e2e8f0;
    }
    .empty { color: var(--subtle); font-size: 13px; margin-top: 8px; }
    .drink-grid {
      display: grid;
      grid-template-columns: repeat(5, minmax(0, 1fr));
      gap: 8px;
      margin-top: 10px;
    }
    .drink-btn {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 70px;
      border-radius: 999px;
      border: 1.5px solid #dbeafe;
      background: #f8fbff;
      transition: all 0.2s ease;
      font-size: 20px;
      cursor: pointer;
      padding: 8px 2px;
    }
    .drink-btn .drink-label {
      font-size: 10px;
      color: #475569;
      margin-top: 3px;
      font-weight: 600;
    }
    .drink-btn.active {
      border-color: #93c5fd;
      background: #dbeafe;
      transform: scale(1.03);
      box-shadow: 0 4px 10px rgba(59, 130, 246, 0.2);
    }
    input[type="datetime-local"] {
      width: 100%;
      border: 1px solid #d6e3ff;
      border-radius: 10px;
      padding: 8px 10px;
      font-size: 14px;
      color: var(--text);
      outline: none;
      margin-top: 4px;
      background: #fff;
    }
    .score-number {
      transition: transform 0.35s ease, color 0.35s ease;
    }
    .score-number.drop {
      animation: scoreDrop 0.45s ease;
    }
    @keyframes scoreDrop {
      0% { transform: translateY(0); }
      30% { transform: translateY(4px); }
      65% { transform: translateY(-2px); }
      100% { transform: translateY(0); }
    }
    .health-warn {
      animation: breathe 2.2s ease-in-out infinite;
    }
    @keyframes breathe {
      0% { box-shadow: 0 0 0 0 rgba(251, 146, 60, 0.12); }
      50% { box-shadow: 0 0 0 9px rgba(251, 146, 60, 0.03); }
      100% { box-shadow: 0 0 0 0 rgba(251, 146, 60, 0.12); }
    }
    .toast {
      position: fixed;
      left: 50%;
      bottom: 24px;
      transform: translateX(-50%) translateY(24px);
      background: rgba(255, 255, 255, 0.78);
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
      color: #334155;
      border: 1px solid rgba(148, 163, 184, 0.3);
      border-radius: 14px;
      padding: 10px 14px;
      max-width: 88%;
      font-size: 13px;
      line-height: 1.4;
      opacity: 0;
      pointer-events: none;
      transition: all 0.35s ease;
      z-index: 999;
    }
    .toast.show {
      opacity: 1;
      transform: translateX(-50%) translateY(0);
    }
    @media (max-width: 420px) {
      .drink-grid { grid-template-columns: repeat(4, minmax(0, 1fr)); }
    }
  </style>
</head>
<body>
  <main class="container">
    <section id="healthCard" class="rounded-3xl p-4 transition-all duration-500 bg-sky-100/90 border border-sky-200 shadow-sm">
      <div class="flex items-center justify-between gap-3">
        <div>
          <p class="text-xs text-slate-500">健康状态</p>
          <p id="healthStatusText" class="text-lg font-semibold text-sky-700">✨ 纯净状态</p>
        </div>
        <div class="text-right">
          <p class="text-xs text-slate-500">Daily Health Score</p>
          <p id="healthScoreNumber" class="score-number text-3xl font-extrabold text-sky-700">100</p>
        </div>
      </div>
      <p id="healthHintText" class="mt-2 text-sm text-slate-600"></p>
    </section>

    <section class="card">
      <h1>每日喝水记录</h1>
      <p class="subtitle" id="todayText"></p>
      <p class="small-text" id="streakText"></p>
      <div class="badge-wrap" id="badgeWrap"></div>
    </section>

    <section class="card">
      <label>选择饮品</label>
      <div class="drink-grid" id="drinkGrid"></div>
      <div class="multi-row">
        <div>
          <label for="goalInput">每日目标（ml）</label>
          <input id="goalInput" type="number" min="200" step="100" />
        </div>
        <div>
          <label for="cupInput">杯子容量（ml）</label>
          <input id="cupInput" type="number" min="100" step="50" />
        </div>
      </div>
      <div class="progress-wrap" aria-label="今日喝水进度">
        <div class="progress-bar" id="progressBar"></div>
      </div>
      <div class="progress-text" id="progressText"></div>
      <div class="multi-row">
        <div>
          <label for="amountInput">录入毫升（ml）</label>
          <input id="amountInput" type="number" min="1" step="10" placeholder="例如 300" />
        </div>
        <div>
          <label for="recordTimeInput">记录时间（可补录）</label>
          <input id="recordTimeInput" type="datetime-local" />
        </div>
      </div>
      <div class="btn-grid">
        <button class="btn-primary" id="submitRecordBtn">确认记录</button>
        <button class="btn-soft" id="cupBtn">+1杯（当前饮品）</button>
        <button class="btn-danger" id="resetBtn">重置今天</button>
        <button class="btn-plain" id="notifyBtn">开启每小时提醒</button>
      </div>
      <p class="small-text" id="notifyStatus"></p>
    </section>

    <section class="card">
      <strong>今天喝水时间点</strong>
      <ul class="log-list" id="logList"></ul>
      <p class="empty" id="logEmptyHint">今天还没记录，喝一口就会出现时间点。</p>
    </section>

    <section class="card">
      <strong>最近 7 天记录</strong>
      <ul class="history-list" id="historyList"></ul>
      <p class="empty" id="historyEmptyHint">暂无记录，先喝第一杯水吧。</p>
    </section>

    <section class="card">
      <div class="calendar-header">
        <button class="btn-plain" id="prevMonthBtn">上月</button>
        <div class="calendar-title" id="calendarTitle"></div>
        <button class="btn-plain" id="nextMonthBtn">下月</button>
      </div>
      <p class="small-text" id="monthSummary"></p>
      <div class="weekdays">
        <div>一</div><div>二</div><div>三</div><div>四</div><div>五</div><div>六</div><div>日</div>
      </div>
      <div class="calendar-grid" id="calendarGrid"></div>
    </section>
  </main>
  <div id="toast" class="toast"></div>

  <script>
    const STORAGE_KEY = "water_tracker_v3";
    const defaultGoal = 2000;
    const defaultCup = 350;
    const retentionDays = 180;
    const drinkTypes = {
      "水": 1.0,
      "运动饮料": 1.1,
      "养生茶": 0.9,
      "汤": 0.9,
      "牛奶": 0.8,
      "养乐多": 0.7,
      "咖啡": 0.6,
      "碳酸饮料": 0.6,
      "奶茶": 0.5,
      "酒类": -0.5
    };
    const drinkOptions = [
      { key: "水", icon: "💧", label: "水" },
      { key: "运动饮料", icon: "🏃", label: "运动饮料" },
      { key: "养生茶", icon: "🍵", label: "养生茶" },
      { key: "汤", icon: "🥣", label: "汤" },
      { key: "牛奶", icon: "🥛", label: "牛奶" },
      { key: "养乐多", icon: "🧴", label: "养乐多" },
      { key: "咖啡", icon: "☕️", label: "咖啡" },
      { key: "碳酸饮料", icon: "🥤", label: "碳酸饮料" },
      { key: "奶茶", icon: "🧋", label: "奶茶" },
      { key: "酒类", icon: "🍺", label: "酒类" }
    ];

    function pad(num) { return String(num).padStart(2, "0"); }
    function getDateKey(d) { return `${d.getFullYear()}-${pad(d.getMonth() + 1)}-${pad(d.getDate())}`; }
    function getTodayKey() { return getDateKey(new Date()); }
    function formatDateLabel(dateKey) { return dateKey.slice(5); }
    function formatTime(iso) { return new Date(iso).toLocaleTimeString("zh-CN", { hour: "2-digit", minute: "2-digit" }); }
    function parseKey(key) {
      const [y, m, d] = key.split("-").map(Number);
      return new Date(y, m - 1, d);
    }

    function getDefaultState() {
      return {
        goal: defaultGoal,
        cupSize: defaultCup,
        selectedDrink: "水",
        reminderEnabled: false,
        lastReminderHour: null,
        days: {}
      };
    }

    function normalizeDays(rawDays) {
      const normalized = {};
      Object.keys(rawDays || {}).forEach((key) => {
        const val = rawDays[key];
        if (typeof val === "number") {
          normalized[key] = { total: Math.max(0, Math.round(val)), logs: [] };
          return;
        }
        if (val && typeof val === "object") {
          normalized[key] = {
            total: Math.max(0, Math.round(Number(val.total) || 0)),
            logs: Array.isArray(val.logs)
              ? val.logs
                  .filter((item) => item && typeof item.amount === "number" && item.time)
                  .map((item) => {
                    const rawAmount = Math.round(Number(item.rawAmount || item.amount) || 0);
                    const drinkType = item.drinkType || "水";
                    const factor = Number(drinkTypes[drinkType] ?? 1);
                    const effectiveAmount = Math.round(
                      Number(item.effectiveAmount) || (rawAmount * factor)
                    );
                    return {
                      amount: Math.round(item.amount),
                      rawAmount,
                      effectiveAmount,
                      drinkType,
                      time: item.time
                    };
                  })
              : []
          };
        }
      });
      return normalized;
    }

    function loadData() {
      try {
        const raw = localStorage.getItem(STORAGE_KEY);
        if (!raw) return getDefaultState();
        const parsed = JSON.parse(raw);
        return {
          goal: Number(parsed.goal) > 0 ? Number(parsed.goal) : defaultGoal,
          cupSize: Number(parsed.cupSize) > 0 ? Number(parsed.cupSize) : defaultCup,
          selectedDrink: parsed.selectedDrink && drinkTypes[parsed.selectedDrink] !== undefined
            ? parsed.selectedDrink
            : "水",
          reminderEnabled: Boolean(parsed.reminderEnabled),
          lastReminderHour: typeof parsed.lastReminderHour === "string" ? parsed.lastReminderHour : null,
          days: normalizeDays(parsed.days)
        };
      } catch (e) {
        return getDefaultState();
      }
    }

    function saveData() { localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); }

    const goalInput = document.getElementById("goalInput");
    const cupInput = document.getElementById("cupInput");
    const amountInput = document.getElementById("amountInput");
    const recordTimeInput = document.getElementById("recordTimeInput");
    const progressBar = document.getElementById("progressBar");
    const progressText = document.getElementById("progressText");
    const todayText = document.getElementById("todayText");
    const streakText = document.getElementById("streakText");
    const badgeWrap = document.getElementById("badgeWrap");
    const drinkGrid = document.getElementById("drinkGrid");
    const submitRecordBtn = document.getElementById("submitRecordBtn");
    const notifyBtn = document.getElementById("notifyBtn");
    const notifyStatus = document.getElementById("notifyStatus");
    const cupBtn = document.getElementById("cupBtn");
    const resetBtn = document.getElementById("resetBtn");
    const historyList = document.getElementById("historyList");
    const historyEmptyHint = document.getElementById("historyEmptyHint");
    const logList = document.getElementById("logList");
    const logEmptyHint = document.getElementById("logEmptyHint");
    const prevMonthBtn = document.getElementById("prevMonthBtn");
    const nextMonthBtn = document.getElementById("nextMonthBtn");
    const calendarTitle = document.getElementById("calendarTitle");
    const monthSummary = document.getElementById("monthSummary");
    const calendarGrid = document.getElementById("calendarGrid");
    const healthCard = document.getElementById("healthCard");
    const healthStatusText = document.getElementById("healthStatusText");
    const healthScoreNumber = document.getElementById("healthScoreNumber");
    const healthHintText = document.getElementById("healthHintText");
    const toast = document.getElementById("toast");

    const badges = [
      { name: "连胜 3 天", days: 3, emoji: "🥉" },
      { name: "连胜 7 天", days: 7, emoji: "🥈" },
      { name: "连胜 30 天", days: 30, emoji: "🥇" }
    ];

    let state = loadData();
    let viewingMonth = new Date(new Date().getFullYear(), new Date().getMonth(), 1);
    let previousHealthScore = 100;
    let toastTimer = null;

    function getDayRecord(dateKey) {
      if (!state.days[dateKey]) {
        state.days[dateKey] = { total: 0, logs: [] };
      }
      return state.days[dateKey];
    }

    function getTodayRecord() { return getDayRecord(getTodayKey()); }

    function trimOldRecords() {
      const keys = Object.keys(state.days).sort((a, b) => b.localeCompare(a));
      keys.slice(retentionDays).forEach((k) => delete state.days[k]);
    }

    function addDrink(rawAmount, drinkType, customDate) {
      const now = customDate || new Date();
      const factor = Number(drinkTypes[drinkType] ?? 1);
      const effectiveAmount = Math.round(rawAmount * factor);
      const rec = getDayRecord(getDateKey(now));
      rec.total = Math.max(0, Math.round(rec.total + effectiveAmount));
      rec.logs.push({
        time: now.toISOString(),
        amount: Math.round(effectiveAmount),
        rawAmount: Math.round(rawAmount),
        effectiveAmount: Math.round(effectiveAmount),
        drinkType
      });
      rec.logs.sort((a, b) => new Date(a.time) - new Date(b.time));
      maybeShowDrinkToast(drinkType);
      saveData();
      render();
    }

    function resetToday() {
      state.days[getTodayKey()] = { total: 0, logs: [] };
      saveData();
      render();
    }

    function getCompletion(dateKey) {
      const total = Number((state.days[dateKey] && state.days[dateKey].total) || 0);
      return total >= state.goal;
    }

    function calcStreak() {
      let streak = 0;
      let cursor = new Date();
      while (true) {
        const key = getDateKey(cursor);
        if (getCompletion(key)) {
          streak += 1;
          cursor.setDate(cursor.getDate() - 1);
        } else {
          break;
        }
      }
      return streak;
    }

    function renderBadges(streak) {
      badgeWrap.innerHTML = "";
      badges.forEach((b) => {
        const div = document.createElement("div");
        div.className = `badge ${streak >= b.days ? "" : "locked"}`;
        div.textContent = `${b.emoji} ${b.name}`;
        badgeWrap.appendChild(div);
      });
    }

    function renderTodayLogs() {
      const logs = getTodayRecord().logs.slice().reverse();
      logList.innerHTML = "";
      if (!logs.length) {
        logEmptyHint.style.display = "block";
        return;
      }
      logEmptyHint.style.display = "none";
      logs.forEach((log) => {
        const li = document.createElement("li");
        li.className = "log-item";
        const type = log.drinkType || "水";
        const raw = Number(log.rawAmount || log.effectiveAmount || log.amount || 0);
        const effective = Number(log.effectiveAmount || log.amount || 0);
        li.innerHTML = `<span>${formatTime(log.time)}</span><strong>喝了 ${raw}ml ${type}（有效补水 ${effective}ml）</strong>`;
        logList.appendChild(li);
      });
    }

    function renderHistory() {
      const keys = Object.keys(state.days).sort((a, b) => b.localeCompare(a)).slice(0, 7);
      historyList.innerHTML = "";
      if (!keys.length) {
        historyEmptyHint.style.display = "block";
        return;
      }
      historyEmptyHint.style.display = "none";
      keys.forEach((key) => {
        const total = Number(state.days[key].total || 0);
        const done = total >= state.goal;
        const li = document.createElement("li");
        li.className = "history-item";
        li.innerHTML = `<span>${formatDateLabel(key)}</span><strong class="${done ? "status-ok" : "status-bad"}">${total} ml</strong>`;
        historyList.appendChild(li);
      });
    }

    function renderCalendar() {
      const year = viewingMonth.getFullYear();
      const month = viewingMonth.getMonth();
      const first = new Date(year, month, 1);
      const firstWeekday = (first.getDay() + 6) % 7;
      const daysInMonth = new Date(year, month + 1, 0).getDate();
      const prevMonthDays = new Date(year, month, 0).getDate();
      const cells = [];
      for (let i = firstWeekday - 1; i >= 0; i -= 1) {
        cells.push({ day: prevMonthDays - i, offset: -1 });
      }
      for (let d = 1; d <= daysInMonth; d += 1) {
        cells.push({ day: d, offset: 0 });
      }
      while (cells.length % 7 !== 0) {
        cells.push({ day: cells.length, offset: 1 });
      }

      calendarGrid.innerHTML = "";
      let completed = 0;
      for (let d = 1; d <= daysInMonth; d += 1) {
        const key = getDateKey(new Date(year, month, d));
        if (getCompletion(key)) completed += 1;
      }
      monthSummary.textContent = `本月达标 ${completed}/${daysInMonth} 天`;
      calendarTitle.textContent = `${year} 年 ${month + 1} 月`;

      cells.forEach((cell) => {
        const date = new Date(year, month + cell.offset, cell.day);
        const key = getDateKey(date);
        const rec = state.days[key];
        const hasRecord = rec && rec.total > 0;
        const done = hasRecord && rec.total >= state.goal;
        const div = document.createElement("div");
        div.className = "day-cell";
        if (cell.offset !== 0) div.classList.add("day-other");
        if (hasRecord) div.classList.add(done ? "day-hit" : "day-miss");
        div.innerHTML = `<div>${date.getDate()}</div><div>${hasRecord ? rec.total : "-"}</div>`;
        calendarGrid.appendChild(div);
      });
    }

    function calculateHealthScore() {
      const logs = getTodayRecord().logs || [];
      let penalty = 0;
      let pureWaterMl = 0;
      let sugaryCount = 0;
      logs.forEach((log) => {
        const type = log.drinkType || "水";
        if (type === "碳酸饮料" || type === "奶茶") {
          penalty += 15;
          sugaryCount += 1;
        }
        if (type === "咖啡") {
          penalty += 5;
        }
        if (type === "水") {
          pureWaterMl += Number(log.rawAmount || log.effectiveAmount || 0);
        }
      });
      const bonus = Math.floor(pureWaterMl / 500) * 10;
      const score = Math.max(0, Math.min(100, 100 - penalty + bonus));
      const sugaryRatio = logs.length ? sugaryCount / logs.length : 0;
      return { score, sugaryRatio };
    }

    function renderHealthCard() {
      const { score, sugaryRatio } = calculateHealthScore();
      const shouldDrop = score < previousHealthScore;
      healthScoreNumber.textContent = `${score}`;

      healthCard.className = "rounded-3xl p-4 transition-all duration-500 border shadow-sm";
      healthCard.classList.remove("health-warn");
      if (score >= 85) {
        healthStatusText.textContent = "✨ 纯净状态";
        healthCard.classList.add("bg-sky-100/90", "border-sky-200");
        healthStatusText.className = "text-lg font-semibold text-sky-700";
        healthScoreNumber.className = "score-number text-3xl font-extrabold text-sky-700";
      } else if (score >= 60) {
        healthStatusText.textContent = "⚖️ 状态平衡";
        healthCard.classList.add("bg-emerald-100/90", "border-emerald-200");
        healthStatusText.className = "text-lg font-semibold text-emerald-700";
        healthScoreNumber.className = "score-number text-3xl font-extrabold text-emerald-700";
      } else {
        healthStatusText.textContent = "⚠️ 糖分警戒";
        healthCard.classList.add("bg-orange-100/90", "border-orange-200", "health-warn");
        healthStatusText.className = "text-lg font-semibold text-orange-700";
        healthScoreNumber.className = "score-number text-3xl font-extrabold text-orange-700";
      }
      if (shouldDrop) {
        void healthScoreNumber.offsetWidth;
        healthScoreNumber.classList.add("drop");
      }
      previousHealthScore = score;
      if (score < 60 && sugaryRatio >= 0.4) {
        healthHintText.textContent = "今日摄入过多糖分，皮肤屏障可能压力山大，快喝杯白水中和一下吧！";
      } else if (score >= 85) {
        healthHintText.textContent = "继续保持，今天的补水结构很干净。";
      } else {
        healthHintText.textContent = "保持平衡，优先补充纯水会更稳。";
      }
    }

    function renderDrinkSelector() {
      drinkGrid.innerHTML = "";
      drinkOptions.forEach((item) => {
        const btn = document.createElement("button");
        btn.type = "button";
        btn.className = `drink-btn ${state.selectedDrink === item.key ? "active" : ""}`;
        btn.innerHTML = `<span>${item.icon}</span><span class="drink-label">${item.label}</span>`;
        btn.addEventListener("click", () => {
          state.selectedDrink = item.key;
          renderDrinkSelector();
          saveData();
        });
        drinkGrid.appendChild(btn);
      });
    }

    function showToast(message) {
      toast.textContent = message;
      toast.classList.add("show");
      if (toastTimer) clearTimeout(toastTimer);
      toastTimer = setTimeout(() => toast.classList.remove("show"), 3000);
    }

    function maybeShowDrinkToast(drinkType) {
      if (drinkType === "碳酸饮料") {
        showToast("⚠️ 气泡虽爽，但多余的糖分会加速胶原蛋白糖化哦。");
      } else if (drinkType === "咖啡") {
        showToast("☕️ 咖啡因正在加速水分代谢，别忘了之后多补一杯纯水。");
      }
    }

    function getRecordDateFromInput() {
      const value = recordTimeInput.value;
      if (!value) return new Date();
      const date = new Date(value);
      if (Number.isNaN(date.getTime())) return new Date();
      return date;
    }

    function setRecordInputNow() {
      const now = new Date();
      const local = `${now.getFullYear()}-${pad(now.getMonth() + 1)}-${pad(now.getDate())}T${pad(now.getHours())}:${pad(now.getMinutes())}`;
      recordTimeInput.value = local;
    }

    function renderReminderStatus() {
      const hasNotification = "Notification" in window;
      if (!hasNotification) {
        notifyStatus.textContent = "当前浏览器不支持通知提醒。";
        notifyBtn.disabled = true;
        return;
      }
      const permission = Notification.permission;
      const enabledText = state.reminderEnabled ? "已开启每小时提醒" : "未开启每小时提醒";
      notifyStatus.textContent = `${enabledText}（通知权限：${permission}）`;
      notifyBtn.textContent = state.reminderEnabled ? "关闭每小时提醒" : "开启每小时提醒";
    }

    function showDrinkReminder() {
      if (!state.reminderEnabled || !("Notification" in window)) return;
      if (document.visibilityState === "visible") return;
      if (Notification.permission !== "granted") return;
      const now = new Date();
      const hourKey = `${getTodayKey()}-${now.getHours()}`;
      if (state.lastReminderHour === hourKey) return;
      state.lastReminderHour = hourKey;
      saveData();
      new Notification("喝水提醒", { body: "已经过了一个小时，记得喝水哦～" });
    }

    function runReminderLoop() {
      showDrinkReminder();
      setInterval(showDrinkReminder, 60 * 1000);
    }

    function render() {
      trimOldRecords();
      const todayKey = getTodayKey();
      const rec = getDayRecord(todayKey);
      const total = rec.total;
      const progress = Math.min(100, (total / state.goal) * 100);
      goalInput.value = state.goal;
      cupInput.value = state.cupSize;
      progressBar.style.width = `${progress}%`;
      progressText.textContent = `今天已喝 ${total} / ${state.goal} ml（${Math.round(progress)}%）`;
      todayText.textContent = `${todayKey} · 坚持喝水更健康`;
      const streak = calcStreak();
      streakText.textContent = `连续达标：${streak} 天`;
      renderBadges(streak);
      renderDrinkSelector();
      renderHealthCard();
      renderTodayLogs();
      renderHistory();
      renderCalendar();
      renderReminderStatus();
      saveData();
    }

    submitRecordBtn.addEventListener("click", () => {
      const raw = Number(amountInput.value);
      if (!raw || raw <= 0) {
        showToast("请输入有效毫升数再记录。");
        return;
      }
      addDrink(raw, state.selectedDrink, getRecordDateFromInput());
      amountInput.value = "";
      setRecordInputNow();
    });
    cupBtn.addEventListener("click", () => {
      addDrink(state.cupSize, state.selectedDrink, getRecordDateFromInput());
      setRecordInputNow();
    });
    resetBtn.addEventListener("click", resetToday);

    goalInput.addEventListener("change", () => {
      const val = Number(goalInput.value);
      state.goal = val >= 200 ? Math.round(val) : defaultGoal;
      render();
    });
    cupInput.addEventListener("change", () => {
      const val = Number(cupInput.value);
      state.cupSize = val >= 100 ? Math.round(val) : defaultCup;
      render();
    });

    notifyBtn.addEventListener("click", async () => {
      if (!("Notification" in window)) return;
      if (state.reminderEnabled) {
        state.reminderEnabled = false;
        render();
        return;
      }
      const permission = await Notification.requestPermission();
      state.reminderEnabled = permission === "granted";
      render();
    });

    prevMonthBtn.addEventListener("click", () => {
      viewingMonth = new Date(viewingMonth.getFullYear(), viewingMonth.getMonth() - 1, 1);
      renderCalendar();
    });
    nextMonthBtn.addEventListener("click", () => {
      viewingMonth = new Date(viewingMonth.getFullYear(), viewingMonth.getMonth() + 1, 1);
      renderCalendar();
    });

    setRecordInputNow();
    render();
    runReminderLoop();
  </script>
</body>
</html>
