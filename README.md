<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bloom — Habit Tracker</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Lora:ital,wght@0,400;0,500;0,600;1,400&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
:root {
  --cream: #faf6f0;
  --cream2: #f3ede3;
  --warm-white: #fffdf9;
  --sand: #e8ddd0;
  --sand2: #d4c4b0;
  --brown: #8b6f57;
  --brown-dark: #5c4a3a;
  --text: #3a2e26;
  --text-muted: #8a7a6e;
  --text-light: #b0a095;
  --accent: #c47c5a;
  --accent2: #e8a87c;
  --accent-light: #f5d5c0;
  --green: #7aad8a;
  --green-light: #d4ead9;
  --blue: #7a9bbf;
  --blue-light: #d4e4f0;
  --purple: #9e86c0;
  --purple-light: #e0d4f5;
  --rose: #c4798a;
  --rose-light: #f5d4dc;
  --yellow: #c4a84e;
  --yellow-light: #f5eac0;
  --teal: #6aada8;
  --teal-light: #d0ecea;
  --card-bg: #fffdf9;
  --card-shadow: 0 2px 20px rgba(60,40,20,0.08);
  --card-shadow-hover: 0 8px 40px rgba(60,40,20,0.14);
  --radius: 20px;
  --radius-sm: 12px;
  --transition: 0.25s cubic-bezier(0.4,0,0.2,1);
}

[data-theme="dark"] {
  --cream: #1e1812;
  --cream2: #251f18;
  --warm-white: #2a231b;
  --sand: #3a3025;
  --sand2: #4a3d30;
  --brown: #c4a078;
  --brown-dark: #e0c4a0;
  --text: #f0e8dc;
  --text-muted: #a89880;
  --text-light: #7a6858;
  --accent: #e09060;
  --accent2: #f0b080;
  --accent-light: #5a3020;
  --green: #7aad8a;
  --green-light: #1a3520;
  --blue: #7a9bbf;
  --blue-light: #1a2535;
  --purple: #9e86c0;
  --purple-light: #25183a;
  --rose: #c4798a;
  --rose-light: #3a1820;
  --yellow: #d4b860;
  --yellow-light: #352a10;
  --teal: #6aada8;
  --teal-light: #153028;
  --card-bg: #2a231b;
  --card-shadow: 0 2px 20px rgba(0,0,0,0.3);
  --card-shadow-hover: 0 8px 40px rgba(0,0,0,0.4);
}

* { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: 'DM Sans', sans-serif;
  background: var(--cream);
  color: var(--text);
  min-height: 100vh;
  transition: background var(--transition), color var(--transition);
  overflow-x: hidden;
}

/* ---- SCROLLBAR ---- */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--sand2); border-radius: 3px; }

/* ---- APP SHELL ---- */
.app { display: flex; flex-direction: column; min-height: 100vh; max-width: 480px; margin: 0 auto; position: relative; }

/* ---- HEADER ---- */
.app-header {
  padding: 20px 20px 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: sticky;
  top: 0;
  z-index: 100;
  background: var(--cream);
  transition: background var(--transition);
}

.logo {
  font-family: 'Lora', serif;
  font-size: 22px;
  font-weight: 600;
  color: var(--brown-dark);
  display: flex;
  align-items: center;
  gap: 8px;
}
.logo-icon { font-size: 22px; }

.header-actions { display: flex; gap: 8px; align-items: center; }

.icon-btn {
  width: 40px; height: 40px;
  border-radius: 50%;
  border: none;
  background: var(--card-bg);
  color: var(--text-muted);
  cursor: pointer;
  display: flex; align-items: center; justify-content: center;
  font-size: 18px;
  box-shadow: var(--card-shadow);
  transition: all var(--transition);
}
.icon-btn:hover { transform: scale(1.05); box-shadow: var(--card-shadow-hover); color: var(--accent); }
.icon-btn:active { transform: scale(0.95); }

/* ---- DATE BAR ---- */
.date-bar {
  padding: 16px 20px 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.date-display { }
.date-today { font-family: 'Lora', serif; font-size: 26px; font-weight: 600; color: var(--text); line-height: 1; }
.date-sub { font-size: 13px; color: var(--text-muted); margin-top: 2px; }

.streak-badge {
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  color: white;
  padding: 6px 14px;
  border-radius: 50px;
  font-size: 13px;
  font-weight: 600;
  display: flex;
  align-items: center;
  gap: 5px;
}

/* ---- WEEK STRIP ---- */
.week-strip {
  display: flex;
  gap: 6px;
  padding: 16px 20px 0;
  overflow-x: auto;
  scrollbar-width: none;
}
.week-strip::-webkit-scrollbar { display: none; }

.week-day {
  flex: 0 0 auto;
  width: 44px;
  height: 56px;
  border-radius: var(--radius-sm);
  background: var(--card-bg);
  box-shadow: var(--card-shadow);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 3px;
  cursor: pointer;
  transition: all var(--transition);
  border: 2px solid transparent;
}
.week-day:hover { transform: translateY(-2px); box-shadow: var(--card-shadow-hover); }
.week-day.today { border-color: var(--accent); background: var(--accent-light); }
.week-day.selected { background: var(--accent); }
.week-day.selected .wd-label, .week-day.selected .wd-num { color: white; }
.week-day.has-completions::after {
  content: '';
  width: 5px; height: 5px;
  border-radius: 50%;
  background: var(--green);
  display: block;
}
.wd-label { font-size: 10px; font-weight: 500; color: var(--text-light); text-transform: uppercase; letter-spacing: 0.05em; }
.wd-num { font-size: 16px; font-weight: 600; color: var(--text); }

/* ---- TABS ---- */
.tabs {
  display: flex;
  gap: 4px;
  padding: 16px 20px 0;
  overflow-x: auto;
  scrollbar-width: none;
}
.tabs::-webkit-scrollbar { display: none; }

.tab-btn {
  flex: 0 0 auto;
  padding: 8px 16px;
  border-radius: 50px;
  border: none;
  background: var(--card-bg);
  color: var(--text-muted);
  font-family: 'DM Sans', sans-serif;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  transition: all var(--transition);
  box-shadow: var(--card-shadow);
}
.tab-btn:hover { color: var(--accent); }
.tab-btn.active { background: var(--accent); color: white; }

/* ---- MAIN CONTENT ---- */
.main { flex: 1; padding: 16px 20px 100px; display: flex; flex-direction: column; gap: 12px; }

/* ---- SEARCH BAR ---- */
.search-bar {
  position: relative;
}
.search-bar input {
  width: 100%;
  padding: 12px 16px 12px 44px;
  border-radius: 50px;
  border: none;
  background: var(--card-bg);
  color: var(--text);
  font-family: 'DM Sans', sans-serif;
  font-size: 14px;
  box-shadow: var(--card-shadow);
  outline: none;
  transition: all var(--transition);
}
.search-bar input:focus { box-shadow: var(--card-shadow-hover); }
.search-bar input::placeholder { color: var(--text-light); }
.search-icon {
  position: absolute;
  left: 16px;
  top: 50%;
  transform: translateY(-50%);
  color: var(--text-light);
  font-size: 16px;
}

/* ---- SECTION HEADER ---- */
.section-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 4px;
}
.section-title { font-family: 'Lora', serif; font-size: 18px; font-weight: 600; color: var(--text); }
.section-sub { font-size: 12px; color: var(--text-muted); }

/* ---- PROGRESS SUMMARY ---- */
.progress-summary {
  background: var(--card-bg);
  border-radius: var(--radius);
  padding: 18px;
  box-shadow: var(--card-shadow);
  display: flex;
  align-items: center;
  gap: 16px;
}
.progress-circle-wrap { position: relative; flex: 0 0 auto; }
.progress-circle {
  width: 72px; height: 72px;
  transform: rotate(-90deg);
}
.progress-circle-bg { fill: none; stroke: var(--sand); stroke-width: 6; }
.progress-circle-fill {
  fill: none;
  stroke: var(--accent);
  stroke-width: 6;
  stroke-linecap: round;
  stroke-dasharray: 200;
  stroke-dashoffset: 200;
  transition: stroke-dashoffset 0.8s cubic-bezier(0.4,0,0.2,1);
}
.progress-num {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%,-50%);
  font-size: 16px;
  font-weight: 700;
  color: var(--accent);
}
.progress-info { flex: 1; }
.progress-info h3 { font-size: 15px; font-weight: 600; color: var(--text); margin-bottom: 4px; }
.progress-info p { font-size: 13px; color: var(--text-muted); line-height: 1.5; }
.progress-stats { display: flex; gap: 16px; margin-top: 8px; }
.pstat { text-align: center; }
.pstat-num { font-size: 18px; font-weight: 700; color: var(--text); display: block; }
.pstat-label { font-size: 11px; color: var(--text-muted); }

/* ---- REMINDER CARD ---- */
.reminder-card {
  background: linear-gradient(135deg, var(--accent-light), #ffe8d6);
  border-radius: var(--radius);
  padding: 16px;
  border-left: 4px solid var(--accent);
  display: flex;
  align-items: flex-start;
  gap: 12px;
  animation: slideIn 0.4s cubic-bezier(0.4,0,0.2,1);
}
[data-theme="dark"] .reminder-card { background: linear-gradient(135deg, #3a1e10, #2a1808); }
.reminder-icon { font-size: 20px; flex: 0 0 auto; }
.reminder-text { flex: 1; }
.reminder-text strong { font-size: 14px; color: var(--accent); display: block; margin-bottom: 2px; }
.reminder-text p { font-size: 13px; color: var(--text-muted); }
.reminder-close { background: none; border: none; color: var(--text-light); cursor: pointer; font-size: 18px; padding: 0; line-height: 1; }

/* ---- CATEGORY CHIPS ---- */
.category-chips {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}
.cat-chip {
  padding: 6px 14px;
  border-radius: 50px;
  border: 2px solid var(--sand);
  background: var(--card-bg);
  color: var(--text-muted);
  font-size: 13px;
  font-weight: 500;
  cursor: pointer;
  transition: all var(--transition);
  display: flex;
  align-items: center;
  gap: 5px;
}
.cat-chip:hover { border-color: var(--accent); color: var(--accent); }
.cat-chip.active { background: var(--accent); border-color: var(--accent); color: white; }

/* ---- HABIT CARD ---- */
.habit-card {
  background: var(--card-bg);
  border-radius: var(--radius);
  padding: 16px;
  box-shadow: var(--card-shadow);
  display: flex;
  align-items: center;
  gap: 14px;
  transition: all var(--transition);
  cursor: default;
  position: relative;
  overflow: hidden;
  animation: slideIn 0.3s cubic-bezier(0.4,0,0.2,1);
  border: 2px solid transparent;
}
.habit-card:hover { box-shadow: var(--card-shadow-hover); transform: translateY(-1px); }
.habit-card.done { border-color: var(--green); }
.habit-card.done::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0; bottom: 0;
  background: var(--green-light);
  opacity: 0.3;
  pointer-events: none;
}
.habit-card.missed { border-color: var(--rose); opacity: 0.7; }
.habit-card.skipped { border-color: var(--sand2); opacity: 0.6; }

.habit-icon-wrap {
  width: 52px; height: 52px;
  border-radius: 16px;
  display: flex; align-items: center; justify-content: center;
  font-size: 24px;
  flex: 0 0 auto;
  transition: transform var(--transition);
}
.habit-card:hover .habit-icon-wrap { transform: scale(1.05); }

.habit-info { flex: 1; min-width: 0; }
.habit-name { font-size: 16px; font-weight: 600; color: var(--text); margin-bottom: 3px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.habit-meta { display: flex; align-items: center; gap: 8px; flex-wrap: wrap; }
.habit-freq { font-size: 12px; color: var(--text-muted); }
.habit-streak { font-size: 12px; color: var(--accent); font-weight: 600; display: flex; align-items: center; gap: 3px; }
.habit-category-tag { font-size: 11px; padding: 2px 8px; border-radius: 50px; font-weight: 500; }
.habit-progress-bar { width: 100%; height: 3px; background: var(--sand); border-radius: 2px; margin-top: 8px; overflow: hidden; }
.habit-progress-fill { height: 100%; border-radius: 2px; transition: width 0.6s cubic-bezier(0.4,0,0.2,1); }

.habit-actions { display: flex; gap: 6px; flex: 0 0 auto; }
.habit-action-btn {
  width: 36px; height: 36px;
  border-radius: 50%;
  border: none;
  cursor: pointer;
  display: flex; align-items: center; justify-content: center;
  font-size: 16px;
  transition: all var(--transition);
}
.habit-action-btn:hover { transform: scale(1.1); }
.habit-action-btn:active { transform: scale(0.9); }
.btn-done { background: var(--green-light); color: var(--green); }
.btn-done.active { background: var(--green); color: white; }
.btn-missed { background: var(--rose-light); color: var(--rose); }
.btn-missed.active { background: var(--rose); color: white; }
.btn-skip { background: var(--sand); color: var(--text-muted); }
.btn-skip.active { background: var(--sand2); color: var(--text); }
.btn-edit { background: var(--blue-light); color: var(--blue); }
.btn-delete { background: var(--rose-light); color: var(--rose); }

/* Status indicator */
.habit-status-dot {
  position: absolute;
  top: 8px; right: 8px;
  width: 8px; height: 8px;
  border-radius: 50%;
}
.status-often { background: var(--green); }
.status-medium { background: var(--yellow); }
.status-rarely { background: var(--rose); }
.status-neglected { background: var(--text-light); }
.status-improving { background: var(--blue); }

/* ---- EMPTY STATE ---- */
.empty-state {
  text-align: center;
  padding: 60px 20px;
  color: var(--text-muted);
}
.empty-icon { font-size: 60px; margin-bottom: 16px; display: block; }
.empty-state h3 { font-family: 'Lora', serif; font-size: 20px; color: var(--text); margin-bottom: 8px; }
.empty-state p { font-size: 14px; line-height: 1.6; }

/* ---- ADD HABIT FAB ---- */
.fab {
  position: fixed;
  bottom: 24px;
  right: 24px;
  width: 60px; height: 60px;
  border-radius: 50%;
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  color: white;
  border: none;
  font-size: 28px;
  cursor: pointer;
  box-shadow: 0 4px 24px rgba(196,124,90,0.4);
  display: flex; align-items: center; justify-content: center;
  transition: all var(--transition);
  z-index: 200;
}
.fab:hover { transform: scale(1.1) rotate(10deg); box-shadow: 0 8px 32px rgba(196,124,90,0.5); }
.fab:active { transform: scale(0.95); }

/* ---- MODAL ---- */
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(30,20,10,0.5);
  backdrop-filter: blur(4px);
  z-index: 1000;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  opacity: 0;
  pointer-events: none;
  transition: opacity var(--transition);
}
.modal-overlay.open { opacity: 1; pointer-events: all; }

.modal {
  background: var(--card-bg);
  border-radius: 28px 28px 0 0;
  padding: 24px 24px 40px;
  width: 100%;
  max-width: 480px;
  max-height: 90vh;
  overflow-y: auto;
  transform: translateY(100%);
  transition: transform 0.4s cubic-bezier(0.4,0,0.2,1);
}
.modal-overlay.open .modal { transform: translateY(0); }

.modal-handle {
  width: 36px; height: 4px;
  background: var(--sand2);
  border-radius: 2px;
  margin: 0 auto 20px;
}
.modal-title { font-family: 'Lora', serif; font-size: 22px; font-weight: 600; color: var(--text); margin-bottom: 20px; }

/* ---- FORM ---- */
.form-group { margin-bottom: 16px; }
.form-label { font-size: 13px; font-weight: 600; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.05em; margin-bottom: 8px; display: block; }
.form-input, .form-select, .form-textarea {
  width: 100%;
  padding: 12px 16px;
  border-radius: var(--radius-sm);
  border: 2px solid var(--sand);
  background: var(--cream2);
  color: var(--text);
  font-family: 'DM Sans', sans-serif;
  font-size: 15px;
  outline: none;
  transition: all var(--transition);
}
.form-input:focus, .form-select:focus, .form-textarea:focus { border-color: var(--accent); background: var(--card-bg); }
.form-textarea { resize: vertical; min-height: 80px; }

.icon-picker {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}
.icon-option {
  width: 44px; height: 44px;
  border-radius: var(--radius-sm);
  background: var(--cream2);
  border: 2px solid transparent;
  cursor: pointer;
  display: flex; align-items: center; justify-content: center;
  font-size: 22px;
  transition: all var(--transition);
}
.icon-option:hover { background: var(--sand); }
.icon-option.selected { border-color: var(--accent); background: var(--accent-light); }

.color-picker { display: flex; gap: 10px; flex-wrap: wrap; }
.color-option {
  width: 32px; height: 32px;
  border-radius: 50%;
  cursor: pointer;
  border: 3px solid transparent;
  transition: all var(--transition);
}
.color-option:hover { transform: scale(1.15); }
.color-option.selected { border-color: var(--text); transform: scale(1.1); }

.form-row { display: flex; gap: 12px; }
.form-row .form-group { flex: 1; }

.btn-primary {
  width: 100%;
  padding: 14px;
  border-radius: 50px;
  border: none;
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  color: white;
  font-family: 'DM Sans', sans-serif;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all var(--transition);
  margin-top: 8px;
}
.btn-primary:hover { transform: translateY(-1px); box-shadow: 0 6px 24px rgba(196,124,90,0.35); }
.btn-primary:active { transform: translateY(0); }

.btn-secondary {
  width: 100%;
  padding: 12px;
  border-radius: 50px;
  border: 2px solid var(--sand2);
  background: transparent;
  color: var(--text-muted);
  font-family: 'DM Sans', sans-serif;
  font-size: 15px;
  font-weight: 500;
  cursor: pointer;
  transition: all var(--transition);
  margin-top: 8px;
}
.btn-secondary:hover { border-color: var(--rose); color: var(--rose); }

/* ---- VIEWS ---- */
.view { display: none; }
.view.active { display: flex; flex-direction: column; gap: 12px; }

/* ---- STATISTICS VIEW ---- */
.stat-card {
  background: var(--card-bg);
  border-radius: var(--radius);
  padding: 18px;
  box-shadow: var(--card-shadow);
}
.stat-card-title { font-family: 'Lora', serif; font-size: 16px; font-weight: 600; color: var(--text); margin-bottom: 14px; display: flex; align-items: center; gap: 8px; }

.big-stats {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
}
.big-stat {
  background: var(--cream2);
  border-radius: var(--radius-sm);
  padding: 16px;
  text-align: center;
}
.big-stat-num { font-size: 32px; font-weight: 700; color: var(--accent); display: block; line-height: 1; }
.big-stat-label { font-size: 12px; color: var(--text-muted); margin-top: 4px; display: block; }

/* ---- HEATMAP ---- */
.heatmap-grid {
  display: flex;
  gap: 3px;
  flex-wrap: wrap;
}
.heatmap-week { display: flex; flex-direction: column; gap: 3px; }
.heatmap-cell {
  width: 12px; height: 12px;
  border-radius: 2px;
  background: var(--sand);
  transition: all var(--transition);
  cursor: pointer;
}
.heatmap-cell:hover { transform: scale(1.3); }
.heatmap-cell.level-1 { background: #d4ead9; }
.heatmap-cell.level-2 { background: #7aad8a; }
.heatmap-cell.level-3 { background: #4a9060; }
.heatmap-cell.level-4 { background: #2a6040; }
[data-theme="dark"] .heatmap-cell.level-1 { background: #1a3520; }
[data-theme="dark"] .heatmap-cell.level-2 { background: #2a5530; }
[data-theme="dark"] .heatmap-cell.level-3 { background: #3a7545; }
[data-theme="dark"] .heatmap-cell.level-4 { background: #4a9060; }

.heatmap-labels { display: flex; gap: 3px; margin-bottom: 4px; }
.heatmap-month-label { font-size: 10px; color: var(--text-muted); }

/* ---- HISTORY VIEW ---- */
.history-month { margin-bottom: 20px; }
.history-month-title {
  font-family: 'Lora', serif;
  font-size: 16px;
  font-weight: 600;
  color: var(--text);
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.history-day {
  background: var(--card-bg);
  border-radius: var(--radius-sm);
  padding: 12px 16px;
  margin-bottom: 6px;
  box-shadow: var(--card-shadow);
  display: flex;
  align-items: center;
  gap: 12px;
}
.history-day-date { font-size: 13px; color: var(--text-muted); flex: 0 0 60px; }
.history-day-habits { display: flex; flex-wrap: wrap; gap: 6px; flex: 1; }
.history-habit-chip {
  padding: 3px 10px;
  border-radius: 50px;
  font-size: 12px;
  font-weight: 500;
}
.history-day-rate { font-size: 13px; font-weight: 600; color: var(--text-muted); }

/* ---- CALENDAR VIEW ---- */
.calendar-nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 12px;
}
.cal-nav-btn {
  width: 36px; height: 36px;
  border-radius: 50%;
  border: none;
  background: var(--card-bg);
  color: var(--text-muted);
  cursor: pointer;
  display: flex; align-items: center; justify-content: center;
  font-size: 18px;
  box-shadow: var(--card-shadow);
  transition: all var(--transition);
}
.cal-nav-btn:hover { color: var(--accent); }
.cal-month-title { font-family: 'Lora', serif; font-size: 18px; font-weight: 600; color: var(--text); }

.calendar-grid {
  background: var(--card-bg);
  border-radius: var(--radius);
  padding: 16px;
  box-shadow: var(--card-shadow);
}
.cal-weekdays { display: grid; grid-template-columns: repeat(7,1fr); gap: 4px; margin-bottom: 8px; }
.cal-weekday { text-align: center; font-size: 12px; font-weight: 600; color: var(--text-muted); padding: 4px; }
.cal-days-grid { display: grid; grid-template-columns: repeat(7,1fr); gap: 4px; }
.cal-day {
  aspect-ratio: 1;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  cursor: pointer;
  transition: all var(--transition);
  position: relative;
  font-weight: 500;
}
.cal-day:hover { background: var(--sand); }
.cal-day.today { background: var(--accent); color: white; }
.cal-day.other-month { color: var(--text-light); }
.cal-day-dot {
  position: absolute;
  bottom: 3px;
  width: 4px; height: 4px;
  border-radius: 50%;
}
.cal-day.full .cal-day-dot { background: var(--green); }
.cal-day.partial .cal-day-dot { background: var(--yellow); }
.cal-day.none .cal-day-dot { background: transparent; }

/* ---- ACHIEVEMENTS ---- */
.achievements-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
.achievement-card {
  background: var(--card-bg);
  border-radius: var(--radius-sm);
  padding: 14px;
  box-shadow: var(--card-shadow);
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  gap: 6px;
  transition: all var(--transition);
}
.achievement-card:hover { transform: translateY(-2px); box-shadow: var(--card-shadow-hover); }
.achievement-card.locked { opacity: 0.4; filter: grayscale(1); }
.achievement-icon { font-size: 32px; }
.achievement-name { font-size: 13px; font-weight: 600; color: var(--text); }
.achievement-desc { font-size: 11px; color: var(--text-muted); }

/* ---- SETTINGS ---- */
.settings-section { background: var(--card-bg); border-radius: var(--radius); box-shadow: var(--card-shadow); overflow: hidden; margin-bottom: 4px; }
.settings-item {
  padding: 16px 18px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px solid var(--sand);
  transition: background var(--transition);
  cursor: pointer;
}
.settings-item:last-child { border-bottom: none; }
.settings-item:hover { background: var(--cream2); }
.settings-item-left { display: flex; align-items: center; gap: 12px; }
.settings-item-icon { font-size: 20px; }
.settings-item-text { }
.settings-item-label { font-size: 15px; font-weight: 500; color: var(--text); }
.settings-item-sub { font-size: 12px; color: var(--text-muted); }
.settings-item-right { color: var(--text-light); font-size: 18px; }

/* ---- TOGGLE ---- */
.toggle {
  width: 44px; height: 24px;
  border-radius: 12px;
  background: var(--sand2);
  position: relative;
  cursor: pointer;
  transition: background var(--transition);
}
.toggle.on { background: var(--green); }
.toggle::after {
  content: '';
  position: absolute;
  width: 18px; height: 18px;
  border-radius: 50%;
  background: white;
  top: 3px; left: 3px;
  transition: left var(--transition);
  box-shadow: 0 1px 4px rgba(0,0,0,0.15);
}
.toggle.on::after { left: 23px; }

/* ---- BOTTOM NAV ---- */
.bottom-nav {
  position: fixed;
  bottom: 0; left: 50%;
  transform: translateX(-50%);
  width: 100%;
  max-width: 480px;
  background: var(--card-bg);
  border-top: 1px solid var(--sand);
  display: flex;
  padding: 8px 0 16px;
  z-index: 100;
  box-shadow: 0 -4px 20px rgba(60,40,20,0.08);
}
.nav-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 3px;
  padding: 6px;
  cursor: pointer;
  transition: all var(--transition);
  border: none;
  background: none;
  color: var(--text-light);
  font-family: 'DM Sans', sans-serif;
}
.nav-item:hover { color: var(--accent); }
.nav-item.active { color: var(--accent); }
.nav-item-icon { font-size: 22px; transition: transform var(--transition); }
.nav-item:hover .nav-item-icon { transform: translateY(-2px); }
.nav-item.active .nav-item-icon { transform: scale(1.1); }
.nav-item-label { font-size: 10px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.04em; }

/* ---- ANIMATIONS ---- */
@keyframes slideIn {
  from { opacity: 0; transform: translateY(16px); }
  to { opacity: 1; transform: translateY(0); }
}
@keyframes pop {
  0% { transform: scale(1); }
  50% { transform: scale(1.3); }
  100% { transform: scale(1); }
}
@keyframes confetti {
  0% { opacity: 1; transform: translateY(0) rotate(0deg); }
  100% { opacity: 0; transform: translateY(-40px) rotate(360deg); }
}

.pop-anim { animation: pop 0.3s cubic-bezier(0.4,0,0.2,1); }

/* ---- TOAST ---- */
.toast-container {
  position: fixed;
  top: 16px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 2000;
  display: flex;
  flex-direction: column;
  gap: 8px;
  align-items: center;
  pointer-events: none;
}
.toast {
  background: var(--brown-dark);
  color: var(--cream);
  padding: 10px 20px;
  border-radius: 50px;
  font-size: 14px;
  font-weight: 500;
  box-shadow: 0 4px 20px rgba(0,0,0,0.2);
  animation: toastAnim 2.5s cubic-bezier(0.4,0,0.2,1) forwards;
  white-space: nowrap;
}
@keyframes toastAnim {
  0% { opacity: 0; transform: translateY(-12px) scale(0.9); }
  15% { opacity: 1; transform: translateY(0) scale(1); }
  75% { opacity: 1; transform: translateY(0) scale(1); }
  100% { opacity: 0; transform: translateY(-8px) scale(0.95); }
}

/* ---- SWIPE ACTIONS ---- */
.habit-card-wrap { position: relative; overflow: hidden; border-radius: var(--radius); }
.swipe-bg {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  padding: 0 20px;
  pointer-events: none;
  opacity: 0;
  transition: opacity var(--transition);
}
.swipe-bg.right { background: linear-gradient(90deg, var(--green), transparent); justify-content: flex-start; }
.swipe-bg.left { background: linear-gradient(270deg, var(--rose), transparent); justify-content: flex-end; }

/* ---- RESPONSIVE ---- */
@media (min-width: 481px) {
  .app { box-shadow: 0 0 60px rgba(60,40,20,0.12); }
  .bottom-nav { border-radius: 0 0 24px 24px; }
}

/* ---- THEME SELECTOR ---- */
.theme-options { display: flex; gap: 10px; flex-wrap: wrap; margin-top: 8px; }
.theme-option {
  width: 44px; height: 44px;
  border-radius: 50%;
  cursor: pointer;
  border: 3px solid transparent;
  transition: all var(--transition);
}
.theme-option.selected { border-color: var(--text); transform: scale(1.1); }
.theme-option:hover { transform: scale(1.1); }

/* ---- WEEKLY CHART ---- */
.weekly-bars {
  display: flex;
  align-items: flex-end;
  gap: 6px;
  height: 80px;
}
.weekly-bar-wrap { flex: 1; display: flex; flex-direction: column; align-items: center; gap: 4px; }
.weekly-bar-bg { width: 100%; flex: 1; background: var(--sand); border-radius: 4px 4px 0 0; position: relative; overflow: hidden; }
.weekly-bar-fill { position: absolute; bottom: 0; left: 0; right: 0; background: linear-gradient(180deg, var(--accent2), var(--accent)); border-radius: 4px 4px 0 0; transition: height 0.8s cubic-bezier(0.4,0,0.2,1); }
.weekly-bar-label { font-size: 11px; color: var(--text-muted); }

/* ---- FILTER BAR ---- */
.filter-bar { display: flex; gap: 6px; align-items: center; }
.filter-select {
  flex: 1;
  padding: 8px 12px;
  border-radius: 50px;
  border: 2px solid var(--sand);
  background: var(--card-bg);
  color: var(--text);
  font-family: 'DM Sans', sans-serif;
  font-size: 13px;
  outline: none;
  transition: border-color var(--transition);
}
.filter-select:focus { border-color: var(--accent); }

/* habit detail expanded */
.habit-detail { padding: 8px 0 0; border-top: 1px solid var(--sand); margin-top: 8px; display: none; }
.habit-card.expanded .habit-detail { display: block; }
.habit-mini-heatmap { display: flex; gap: 2px; margin-top: 6px; }
.mini-cell { width: 10px; height: 10px; border-radius: 2px; background: var(--sand); }
.mini-cell.done { background: var(--green); }
.mini-cell.missed { background: var(--rose-light); }

.habit-notes-text { font-size: 13px; color: var(--text-muted); font-style: italic; margin-top: 4px; }
</style>
</head>
<body>

<div class="app" id="app">

  <!-- HEADER -->
  <header class="app-header">
    <div class="logo">
      <span class="logo-icon">🌸</span>
      <span>Bloom</span>
    </div>
    <div class="header-actions">
      <button class="icon-btn" id="searchToggleBtn" title="Search" onclick="toggleSearch()">🔍</button>
      <button class="icon-btn" id="themeToggleBtn" title="Toggle theme" onclick="toggleTheme()">🌙</button>
    </div>
  </header>

  <!-- DATE BAR -->
  <div class="date-bar" id="dateBar">
    <div class="date-display">
      <div class="date-today" id="dateToday"></div>
      <div class="date-sub" id="dateSub"></div>
    </div>
    <div class="streak-badge" id="globalStreak">🔥 <span id="globalStreakNum">0</span> day streak</div>
  </div>

  <!-- WEEK STRIP -->
  <div class="week-strip" id="weekStrip"></div>

  <!-- TABS (context-based) -->
  <div class="tabs" id="mainTabs" style="display:none">
    <button class="tab-btn active" onclick="setFilter('all')">All</button>
    <button class="tab-btn" onclick="setFilter('morning')">🌅 Morning</button>
    <button class="tab-btn" onclick="setFilter('afternoon')">☀️ Afternoon</button>
    <button class="tab-btn" onclick="setFilter('evening')">🌙 Evening</button>
    <button class="tab-btn" onclick="setFilter('health')">💪 Health</button>
    <button class="tab-btn" onclick="setFilter('mind')">🧠 Mind</button>
    <button class="tab-btn" onclick="setFilter('social')">💬 Social</button>
    <button class="tab-btn" onclick="setFilter('creative')">🎨 Creative</button>
    <button class="tab-btn" onclick="setFilter('other')">📦 Other</button>
  </div>

  <!-- MAIN -->
  <main class="main" id="mainContent">

    <!-- TODAY VIEW -->
    <div class="view active" id="viewToday">
      <div id="searchBarWrap" style="display:none">
        <div class="search-bar">
          <span class="search-icon">🔍</span>
          <input type="text" id="searchInput" placeholder="Search habits..." oninput="renderHabits()">
        </div>
      </div>

      <div id="reminderArea"></div>

      <!-- Progress summary -->
      <div class="progress-summary" id="progressSummary">
        <div class="progress-circle-wrap">
          <svg class="progress-circle" viewBox="0 0 72 72">
            <circle class="progress-circle-bg" cx="36" cy="36" r="30"/>
            <circle class="progress-circle-fill" id="progressCircleFill" cx="36" cy="36" r="30"/>
          </svg>
          <span class="progress-num" id="progressPct">0%</span>
        </div>
        <div class="progress-info">
          <h3 id="progressTitle">Start your day! 🌸</h3>
          <p id="progressDesc">Track your habits to build a better you.</p>
          <div class="progress-stats">
            <div class="pstat"><span class="pstat-num" id="pstatDone">0</span><span class="pstat-label">Done</span></div>
            <div class="pstat"><span class="pstat-num" id="pstatLeft">0</span><span class="pstat-label">Left</span></div>
            <div class="pstat"><span class="pstat-num" id="pstatStreak">0</span><span class="pstat-label">Best streak</span></div>
          </div>
        </div>
      </div>

      <!-- Filter bar -->
      <div class="filter-bar">
        <select class="filter-select" id="sortSelect" onchange="renderHabits()">
          <option value="default">Sort: Default</option>
          <option value="name">Sort: Name</option>
          <option value="streak">Sort: Streak</option>
          <option value="completion">Sort: Completion %</option>
          <option value="category">Sort: Category</option>
        </select>
        <select class="filter-select" id="statusFilter" onchange="renderHabits()">
          <option value="all">All Status</option>
          <option value="pending">Pending</option>
          <option value="done">Done</option>
          <option value="missed">Missed</option>
        </select>
      </div>

      <div class="section-header">
        <span class="section-title">Today's Habits</span>
        <span class="section-sub" id="habitCountLabel"></span>
      </div>

      <div id="habitList"></div>
    </div>

    <!-- STATS VIEW -->
    <div class="view" id="viewStats">
      <div class="section-header">
        <span class="section-title">Statistics</span>
      </div>
      <div class="big-stats" id="bigStats"></div>

      <!-- Weekly chart -->
      <div class="stat-card">
        <div class="stat-card-title">📊 Weekly Overview</div>
        <div class="weekly-bars" id="weeklyBars"></div>
      </div>

      <!-- Heatmap -->
      <div class="stat-card">
        <div class="stat-card-title">🗓 Activity Heatmap</div>
        <div id="heatmapWrap"></div>
      </div>

      <!-- Per habit stats -->
      <div class="stat-card">
        <div class="stat-card-title">📈 Habit Performance</div>
        <div id="habitPerfList"></div>
      </div>
    </div>

    <!-- CALENDAR VIEW -->
    <div class="view" id="viewCalendar">
      <div class="calendar-nav">
        <button class="cal-nav-btn" onclick="calNav(-1)">◀</button>
        <span class="cal-month-title" id="calTitle"></span>
        <button class="cal-nav-btn" onclick="calNav(1)">▶</button>
      </div>
      <div class="calendar-grid" id="calendarGrid"></div>

      <!-- Habit selector for calendar -->
      <div class="stat-card">
        <div class="stat-card-title">📅 Daily Summary</div>
        <div id="calDaySummary">
          <p style="font-size:13px;color:var(--text-muted)">Select a day to see details.</p>
        </div>
      </div>

      <div class="stat-card">
        <div class="stat-card-title">📆 Monthly Stats</div>
        <div id="monthlyStats"></div>
      </div>
    </div>

    <!-- HISTORY VIEW -->
    <div class="view" id="viewHistory">
      <div class="section-header">
        <span class="section-title">History</span>
        <button class="icon-btn" onclick="exportData()" style="width:auto;padding:6px 12px;border-radius:50px;font-size:13px;color:var(--text-muted)">Export 📤</button>
      </div>
      <div id="historyContent"></div>
    </div>

    <!-- ACHIEVEMENTS VIEW -->
    <div class="view" id="viewAchievements">
      <div class="section-header">
        <span class="section-title">Achievements</span>
      </div>
      <div class="achievements-grid" id="achievementsGrid"></div>
    </div>

    <!-- SETTINGS VIEW -->
    <div class="view" id="viewSettings">
      <div class="section-header"><span class="section-title">Settings</span></div>
      <div class="settings-section">
        <div class="settings-item" onclick="toggleTheme()">
          <div class="settings-item-left">
            <span class="settings-item-icon">🌙</span>
            <div class="settings-item-text">
              <div class="settings-item-label">Dark Mode</div>
              <div class="settings-item-sub">Easy on the eyes at night</div>
            </div>
          </div>
          <div class="toggle" id="darkToggle"></div>
        </div>
        <div class="settings-item" onclick="toggleReminders()">
          <div class="settings-item-left">
            <span class="settings-item-icon">🔔</span>
            <div class="settings-item-text">
              <div class="settings-item-label">Smart Reminders</div>
              <div class="settings-item-sub">Get nudged for neglected habits</div>
            </div>
          </div>
          <div class="toggle on" id="reminderToggle"></div>
        </div>
        <div class="settings-item" onclick="toggleSound()">
          <div class="settings-item-left">
            <span class="settings-item-icon">🔊</span>
            <div class="settings-item-text">
              <div class="settings-item-label">Sound Effects</div>
              <div class="settings-item-sub">Satisfying completion sounds</div>
            </div>
          </div>
          <div class="toggle on" id="soundToggle"></div>
        </div>
      </div>

      <div class="section-header" style="margin-top:8px"><span class="section-title">Theme</span></div>
      <div class="stat-card">
        <div class="theme-options" id="themeOptions">
          <div class="theme-option selected" data-theme-color="default" style="background: linear-gradient(135deg,#c47c5a,#faf6f0)" onclick="setThemeColor('default')"></div>
          <div class="theme-option" data-theme-color="sage" style="background: linear-gradient(135deg,#6aad7a,#f0f5f0)" onclick="setThemeColor('sage')"></div>
          <div class="theme-option" data-theme-color="lavender" style="background: linear-gradient(135deg,#9e86c0,#f5f0ff)" onclick="setThemeColor('lavender')"></div>
          <div class="theme-option" data-theme-color="ocean" style="background: linear-gradient(135deg,#4a8abf,#f0f5ff)" onclick="setThemeColor('ocean')"></div>
          <div class="theme-option" data-theme-color="rose" style="background: linear-gradient(135deg,#c4788a,#fff0f3)" onclick="setThemeColor('rose')"></div>
          <div class="theme-option" data-theme-color="mocha" style="background: linear-gradient(135deg,#8b5a3a,#2a1c12)" onclick="setThemeColor('mocha')"></div>
        </div>
      </div>

      <div class="stat-card" style="margin-top:8px">
        <div class="stat-card-title">📊 Data</div>
        <div style="display:flex;flex-direction:column;gap:8px">
          <button class="btn-primary" onclick="exportData()">Export Data 📤</button>
          <button class="btn-secondary" onclick="importData()">Import Data 📥</button>
          <button class="btn-secondary" style="border-color:var(--rose);color:var(--rose)" onclick="confirmClear()">Clear All Data 🗑</button>
        </div>
      </div>

      <div style="text-align:center;padding:20px;color:var(--text-light);font-size:12px">
        Bloom v1.0 • Made with 🌸<br>Your data stays on your device.
      </div>
    </div>

  </main>

  <!-- BOTTOM NAV -->
  <nav class="bottom-nav">
    <button class="nav-item active" onclick="switchNav('today',this)">
      <span class="nav-item-icon">🌸</span>
      <span class="nav-item-label">Today</span>
    </button>
    <button class="nav-item" onclick="switchNav('stats',this)">
      <span class="nav-item-icon">📊</span>
      <span class="nav-item-label">Stats</span>
    </button>
    <button class="nav-item" onclick="switchNav('calendar',this)">
      <span class="nav-item-icon">📅</span>
      <span class="nav-item-label">Calendar</span>
    </button>
    <button class="nav-item" onclick="switchNav('history',this)">
      <span class="nav-item-icon">📖</span>
      <span class="nav-item-label">History</span>
    </button>
    <button class="nav-item" onclick="switchNav('achievements',this)">
      <span class="nav-item-icon">🏆</span>
      <span class="nav-item-label">Awards</span>
    </button>
    <button class="nav-item" onclick="switchNav('settings',this)">
      <span class="nav-item-icon">⚙️</span>
      <span class="nav-item-label">Settings</span>
    </button>
  </nav>

  <!-- FAB -->
  <button class="fab" onclick="openAddHabit()" title="Add habit">+</button>

</div>

<!-- TOAST CONTAINER -->
<div class="toast-container" id="toastContainer"></div>

<!-- ADD/EDIT HABIT MODAL -->
<div class="modal-overlay" id="habitModal">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title" id="modalTitle">Add New Habit</div>

    <div class="form-group">
      <label class="form-label">Habit Name</label>
      <input type="text" class="form-input" id="habitName" placeholder="e.g. Morning meditation" maxlength="60">
    </div>

    <div class="form-group">
      <label class="form-label">Choose an Icon</label>
      <div class="icon-picker" id="iconPicker"></div>
    </div>

    <div class="form-group">
      <label class="form-label">Color</label>
      <div class="color-picker" id="colorPicker"></div>
    </div>

    <div class="form-row">
      <div class="form-group">
        <label class="form-label">Category</label>
        <select class="form-select" id="habitCategory">
          <option value="morning">🌅 Morning</option>
          <option value="afternoon">☀️ Afternoon</option>
          <option value="evening">🌙 Evening</option>
          <option value="health">💪 Health</option>
          <option value="mind">🧠 Mind</option>
          <option value="social">💬 Social</option>
          <option value="creative">🎨 Creative</option>
          <option value="other">📦 Other</option>
        </select>
      </div>
      <div class="form-group">
        <label class="form-label">Frequency</label>
        <select class="form-select" id="habitFreq" onchange="updateFreqCustom()">
          <option value="daily">Every day</option>
          <option value="2days">Every 2 days</option>
          <option value="3days">Every 3 days</option>
          <option value="weekly">Once a week</option>
          <option value="2week">Twice a week</option>
          <option value="3week">3× a week</option>
          <option value="monthly">Once a month</option>
          <option value="2month">2× a month</option>
          <option value="3month">3× a month</option>
          <option value="custom">Custom interval</option>
        </select>
      </div>
    </div>

    <div class="form-group" id="customFreqGroup" style="display:none">
      <label class="form-label">Custom: Every how many days?</label>
      <input type="number" class="form-input" id="customFreqDays" placeholder="e.g. 5" min="1" max="365" value="5">
    </div>

    <div class="form-group">
      <label class="form-label">Notes (optional)</label>
      <textarea class="form-textarea" id="habitNotes" placeholder="Any notes or motivation..."></textarea>
    </div>

    <button class="btn-primary" onclick="saveHabit()">Save Habit 🌸</button>
    <button class="btn-secondary" id="deleteHabitBtn" onclick="deleteHabit()" style="display:none;border-color:var(--rose);color:var(--rose)">Delete Habit 🗑</button>
    <button class="btn-secondary" onclick="closeModal()">Cancel</button>
  </div>
</div>

<!-- HABIT DETAIL MODAL -->
<div class="modal-overlay" id="detailModal">
  <div class="modal">
    <div class="modal-handle"></div>
    <div id="detailContent"></div>
    <button class="btn-secondary" onclick="closeDetailModal()">Close</button>
  </div>
</div>

<script>
// ===================== DATA =====================
const ICONS = ['📚','💪','🙏','💻','📖','💧','😴','🏃','🧘','🎸','✍️','🎨','🍎','🥗','🚴','🏊','🧹','💊','☕','🌿','🌅','🌙','🔔','🎯','🏋️','🧠','❤️','🌳','🎵','🍵','📝','🦷','🚶','🌞','🧹','💡','🎮','🤸','🛏️','🚿','🥤','🌺','📿','🧘‍♀️','🏄','🎭','🍳','🌻','📓','🎲'];
const COLORS = [
  {name:'Terracotta',bg:'var(--accent-light)',text:'var(--accent)',hex:'#f5d5c0'},
  {name:'Sage',bg:'var(--green-light)',text:'var(--green)',hex:'#d4ead9'},
  {name:'Sky',bg:'var(--blue-light)',text:'var(--blue)',hex:'#d4e4f0'},
  {name:'Lavender',bg:'var(--purple-light)',text:'var(--purple)',hex:'#e0d4f5'},
  {name:'Rose',bg:'var(--rose-light)',text:'var(--rose)',hex:'#f5d4dc'},
  {name:'Honey',bg:'var(--yellow-light)',text:'var(--yellow)',hex:'#f5eac0'},
  {name:'Teal',bg:'var(--teal-light)',text:'var(--teal)',hex:'#d0ecea'},
];

const ACHIEVEMENTS = [
  {id:'first_habit',name:'First Bloom',icon:'🌱',desc:'Add your first habit',check:s=>s.habitsCreated>=1},
  {id:'streak_7',name:'Week Warrior',icon:'🔥',desc:'7 day streak on any habit',check:s=>s.maxStreak>=7},
  {id:'streak_30',name:'Monthly Master',icon:'🏆',desc:'30 day streak on any habit',check:s=>s.maxStreak>=30},
  {id:'streak_100',name:'Century Legend',icon:'💎',desc:'100 day streak',check:s=>s.maxStreak>=100},
  {id:'perfect_day',name:'Perfect Day',icon:'⭐',desc:'Complete all habits in a day',check:s=>s.perfectDays>=1},
  {id:'perfect_week',name:'Perfect Week',icon:'🌟',desc:'7 perfect days',check:s=>s.perfectDays>=7},
  {id:'total_50',name:'Habit Builder',icon:'🧱',desc:'50 total completions',check:s=>s.totalCompletions>=50},
  {id:'total_500',name:'Habit Master',icon:'🎖',desc:'500 total completions',check:s=>s.totalCompletions>=500},
  {id:'habits_5',name:'Multi-tasker',icon:'🌸',desc:'Track 5+ habits',check:s=>s.habitsCreated>=5},
  {id:'habits_10',name:'Garden Full',icon:'🌺',desc:'Track 10+ habits',check:s=>s.habitsCreated>=10},
  {id:'early_bird',name:'Early Bird',icon:'🌅',desc:'Complete a morning habit',check:s=>s.morningDone>=1},
  {id:'consistency',name:'Consistent',icon:'📈',desc:'70%+ rate for 2 weeks',check:s=>s.consistencyScore>=70},
];

let state = {
  habits: [],
  history: {},  // { 'YYYY-MM-DD': { habitId: 'done'|'missed'|'skip' } }
  settings: { dark: false, reminders: true, sound: true, themeColor: 'default' },
  selectedDate: todayStr(),
  calMonth: new Date(),
  editingId: null,
  currentFilter: 'all',
  selectedIcon: '📚',
  selectedColor: 0,
};

function todayStr() {
  const d = new Date();
  return `${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}-${String(d.getDate()).padStart(2,'0')}`;
}
function dateStr(d) {
  return `${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}-${String(d.getDate()).padStart(2,'0')}`;
}
function parseDate(s) {
  const [y,m,d] = s.split('-').map(Number);
  return new Date(y,m-1,d);
}

// ===================== STORAGE =====================
function save() {
  localStorage.setItem('bloom_state', JSON.stringify(state));
}
function load() {
  try {
    const s = localStorage.getItem('bloom_state');
    if(s) {
      const parsed = JSON.parse(s);
      state = {...state, ...parsed};
      state.calMonth = new Date();
    }
  } catch(e) {}
}

// ===================== HABIT FREQUENCY =====================
function getFreqLabel(h) {
  switch(h.freq) {
    case 'daily': return 'Every day';
    case '2days': return 'Every 2 days';
    case '3days': return 'Every 3 days';
    case 'weekly': return 'Once a week';
    case '2week': return 'Twice a week';
    case '3week': return '3× a week';
    case 'monthly': return 'Once a month';
    case '2month': return '2× a month';
    case '3month': return '3× a month';
    case 'custom': return `Every ${h.freqDays} days`;
    default: return h.freq;
  }
}

function isDueToday(habit, dateStr) {
  const d = parseDate(dateStr);
  const created = parseDate(habit.created || dateStr);
  const dayDiff = Math.floor((d - created) / 86400000);

  switch(habit.freq) {
    case 'daily': return true;
    case '2days': return dayDiff % 2 === 0;
    case '3days': return dayDiff % 3 === 0;
    case 'weekly': return d.getDay() === (created.getDay());
    case '2week': return [0,3].includes(d.getDay()) || [1,4].includes(d.getDay());
    case '3week': return [1,3,5].includes(d.getDay());
    case 'monthly': return d.getDate() === created.getDate();
    case '2month': return d.getDate() === 1 || d.getDate() === 15;
    case '3month': return d.getDate() === 1 || d.getDate() === 10 || d.getDate() === 20;
    case 'custom': {
      const n = parseInt(habit.freqDays) || 1;
      return dayDiff % n === 0;
    }
    default: return true;
  }
}

// ===================== STATS =====================
function getHabitStats(habit) {
  const entries = Object.entries(state.history);
  let completions = 0, total = 0, currentStreak = 0, maxStreak = 0, streak = 0;
  const sorted = entries.filter(([d]) => {
    const rec = entries.find(([dd]) => dd === d);
    return isDueToday(habit, d);
  }).sort((a,b) => a[0] < b[0] ? -1 : 1);

  for(const [d, dayData] of sorted) {
    if(!isDueToday(habit, d)) continue;
    total++;
    if(dayData[habit.id] === 'done') {
      completions++;
      streak++;
      maxStreak = Math.max(maxStreak, streak);
    } else {
      streak = 0;
    }
  }
  currentStreak = streak;
  const pct = total > 0 ? Math.round(completions/total*100) : 0;
  return { completions, total, currentStreak, maxStreak, pct };
}

function getStatus(pct) {
  if(pct >= 80) return { label: 'Often done', cls: 'status-often', color: 'var(--green)' };
  if(pct >= 50) return { label: 'Medium', cls: 'status-medium', color: 'var(--yellow)' };
  if(pct >= 25) return { label: 'Rarely done', cls: 'status-rarely', color: 'var(--rose)' };
  if(pct === 0) return { label: 'Neglected', cls: 'status-neglected', color: 'var(--text-light)' };
  return { label: 'Improving', cls: 'status-improving', color: 'var(--blue)' };
}

function getGlobalStats() {
  let habitsCreated = state.habits.length;
  let maxStreak = 0, totalCompletions = 0, perfectDays = 0, morningDone = 0;

  for(const h of state.habits) {
    const s = getHabitStats(h);
    maxStreak = Math.max(maxStreak, s.maxStreak);
    totalCompletions += s.completions;
    if(h.category === 'morning' && s.completions > 0) morningDone = s.completions;
  }

  // Perfect days
  for(const [d, dayData] of Object.entries(state.history)) {
    const due = state.habits.filter(h => isDueToday(h, d));
    if(due.length > 0 && due.every(h => dayData[h.id] === 'done')) perfectDays++;
  }

  // Consistency score (last 14 days)
  const last14 = [];
  for(let i=0;i<14;i++) {
    const d = new Date(); d.setDate(d.getDate()-i);
    last14.push(dateStr(d));
  }
  let c14done=0, c14total=0;
  for(const d of last14) {
    const due = state.habits.filter(h => isDueToday(h,d));
    c14total += due.length;
    for(const h of due) {
      if(state.history[d]?.[h.id]==='done') c14done++;
    }
  }
  const consistencyScore = c14total > 0 ? Math.round(c14done/c14total*100) : 0;

  // Current global streak (consecutive days with all habits done)
  let globalStreak = 0;
  let checking = true;
  for(let i=0; checking && i<365; i++) {
    const d = new Date(); d.setDate(d.getDate()-i);
    const ds = dateStr(d);
    if(i===0 && ds === todayStr()) continue; // don't break on today unless done
    const due = state.habits.filter(h => isDueToday(h,ds));
    if(due.length === 0) { continue; }
    const allDone = due.every(h => state.history[ds]?.[h.id]==='done');
    if(allDone) globalStreak++;
    else checking = false;
  }

  return { habitsCreated, maxStreak, totalCompletions, perfectDays, morningDone, consistencyScore, globalStreak };
}

// ===================== RENDER =====================
function renderAll() {
  renderDateBar();
  renderWeekStrip();
  renderHabits();
  renderReminders();
  updateProgress();
  renderStats();
  renderCalendar();
  renderHistory();
  renderAchievements();
  updateSettingsUI();
}

function renderDateBar() {
  const d = parseDate(state.selectedDate);
  const opts = { weekday:'long', month:'long', day:'numeric' };
  document.getElementById('dateToday').textContent = d.toLocaleDateString('en-US', {month:'long',day:'numeric'});
  document.getElementById('dateSub').textContent = d.toLocaleDateString('en-US',{weekday:'long'}) + (state.selectedDate === todayStr() ? ' · Today' : '');
  const gs = getGlobalStats();
  document.getElementById('globalStreakNum').textContent = gs.globalStreak;
}

function renderWeekStrip() {
  const strip = document.getElementById('weekStrip');
  strip.innerHTML = '';
  const today = new Date();
  for(let i=-3; i<=3; i++) {
    const d = new Date(today);
    d.setDate(d.getDate()+i);
    const ds = dateStr(d);
    const el = document.createElement('div');
    el.className = 'week-day' + (ds===state.selectedDate?' selected':'') + (ds===todayStr()&&ds!==state.selectedDate?' today':'');
    const days=['S','M','T','W','T','F','S'];
    el.innerHTML = `<span class="wd-label">${days[d.getDay()]}</span><span class="wd-num">${d.getDate()}</span>`;
    const dayHistory = state.history[ds];
    if(dayHistory && Object.values(dayHistory).some(v=>v==='done')) el.classList.add('has-completions');
    el.onclick = () => { state.selectedDate = ds; renderAll(); };
    strip.appendChild(el);
  }
}

function getHabitsForDate(dateStr) {
  return state.habits.filter(h => isDueToday(h, dateStr));
}

function renderHabits() {
  const list = document.getElementById('habitList');
  const searchVal = (document.getElementById('searchInput')?.value || '').toLowerCase();
  const sortVal = document.getElementById('sortSelect')?.value || 'default';
  const statusFilter = document.getElementById('statusFilter')?.value || 'all';
  const filter = state.currentFilter;

  let habits = getHabitsForDate(state.selectedDate);

  if(filter !== 'all') habits = habits.filter(h => h.category === filter);
  if(searchVal) habits = habits.filter(h => h.name.toLowerCase().includes(searchVal) || h.notes?.toLowerCase().includes(searchVal));

  // Status filter
  if(statusFilter !== 'all') {
    habits = habits.filter(h => {
      const s = state.history[state.selectedDate]?.[h.id];
      if(statusFilter==='done') return s==='done';
      if(statusFilter==='missed') return s==='missed';
      if(statusFilter==='pending') return !s || s==='skip';
      return true;
    });
  }

  // Sort
  if(sortVal==='name') habits.sort((a,b)=>a.name.localeCompare(b.name));
  else if(sortVal==='streak') habits.sort((a,b)=>getHabitStats(b).currentStreak - getHabitStats(a).currentStreak);
  else if(sortVal==='completion') habits.sort((a,b)=>getHabitStats(b).pct - getHabitStats(a).pct);
  else if(sortVal==='category') habits.sort((a,b)=>a.category.localeCompare(b.category));

  document.getElementById('habitCountLabel').textContent = `${habits.length} habit${habits.length!==1?'s':''}`;

  if(habits.length === 0) {
    list.innerHTML = state.habits.length === 0
      ? `<div class="empty-state"><span class="empty-icon">🌱</span><h3>Plant your first habit</h3><p>Tap the + button to add a habit and start your journey.</p></div>`
      : `<div class="empty-state"><span class="empty-icon">🌿</span><h3>Nothing to show</h3><p>No habits match your filter, or none are due today.</p></div>`;
    return;
  }

  list.innerHTML = '';
  habits.forEach(h => {
    const dayStatus = state.history[state.selectedDate]?.[h.id];
    const stats = getHabitStats(h);
    const status = getStatus(stats.pct);
    const color = COLORS[h.colorIdx] || COLORS[0];

    const card = document.createElement('div');
    card.className = `habit-card${dayStatus==='done'?' done':''}${dayStatus==='missed'?' missed':''}${dayStatus==='skip'?' skipped':''}`;
    card.dataset.id = h.id;

    const last30 = [];
    for(let i=29;i>=0;i--) { const d=new Date(); d.setDate(d.getDate()-i); last30.push(dateStr(d)); }
    const miniCells = last30.map(d => {
      const s = state.history[d]?.[h.id];
      return `<div class="mini-cell${s==='done'?' done':s==='missed'?' missed':''}" title="${d}"></div>`;
    }).join('');

    card.innerHTML = `
      <div class="habit-status-dot ${status.cls}"></div>
      <div class="habit-icon-wrap" style="background:${color.bg};color:${color.text}">${h.icon}</div>
      <div class="habit-info">
        <div class="habit-name">${h.name}</div>
        <div class="habit-meta">
          <span class="habit-freq">${getFreqLabel(h)}</span>
          ${stats.currentStreak>0?`<span class="habit-streak">🔥 ${stats.currentStreak}</span>`:''}
          <span class="habit-category-tag" style="background:${color.bg};color:${color.text}">${getCatLabel(h.category)}</span>
        </div>
        <div class="habit-progress-bar">
          <div class="habit-progress-fill" style="width:${stats.pct}%;background:${color.text}"></div>
        </div>
        <div class="habit-detail">
          <div style="display:flex;justify-content:space-between;margin-top:6px;font-size:12px;color:var(--text-muted)">
            <span>Completion: <strong style="color:${color.text}">${stats.pct}%</strong></span>
            <span>Best: 🔥${stats.maxStreak}</span>
            <span>Total: ${stats.completions}</span>
          </div>
          <div class="habit-mini-heatmap">${miniCells}</div>
          ${h.notes?`<div class="habit-notes-text">💬 ${h.notes}</div>`:''}
        </div>
      </div>
      <div class="habit-actions">
        <button class="habit-action-btn btn-done${dayStatus==='done'?' active':''}" onclick="event.stopPropagation();setHabitStatus('${h.id}','${dayStatus==='done'?'':'done'}')" title="Done">✓</button>
        <button class="habit-action-btn btn-missed${dayStatus==='missed'?' active':''}" onclick="event.stopPropagation();setHabitStatus('${h.id}','${dayStatus==='missed'?'':'missed'}')" title="Missed">✗</button>
        <button class="habit-action-btn btn-skip${dayStatus==='skip'?' active':''}" onclick="event.stopPropagation();setHabitStatus('${h.id}','${dayStatus==='skip'?'':'skip'}')" title="Skip">→</button>
        <button class="habit-action-btn btn-edit" onclick="event.stopPropagation();openEditHabit('${h.id}')" title="Edit">✎</button>
      </div>
    `;

    card.onclick = () => { card.classList.toggle('expanded'); };
    list.appendChild(card);
  });

  updateProgress();
}

function getCatLabel(cat) {
  const map = {morning:'🌅',afternoon:'☀️',evening:'🌙',health:'💪',mind:'🧠',social:'💬',creative:'🎨',other:'📦'};
  return map[cat] || cat;
}

function updateProgress() {
  const habits = getHabitsForDate(state.selectedDate);
  const done = habits.filter(h => state.history[state.selectedDate]?.[h.id]==='done').length;
  const pct = habits.length > 0 ? Math.round(done/habits.length*100) : 0;
  const circ = document.getElementById('progressCircleFill');
  const circumference = 2 * Math.PI * 30; // 188.5
  const offset = circumference - (pct/100)*circumference;
  circ.style.strokeDasharray = circumference;
  circ.style.strokeDashoffset = offset;
  document.getElementById('progressPct').textContent = pct+'%';

  const gs = getGlobalStats();
  document.getElementById('pstatDone').textContent = done;
  document.getElementById('pstatLeft').textContent = habits.length - done;
  document.getElementById('pstatStreak').textContent = gs.maxStreak;

  const titles = ['Start your day! 🌸','Great start! 🌿','Halfway there! ⭐','Almost done! 🔥','Perfect day! 🎉'];
  const ti = pct===100?4:pct>=75?3:pct>=50?2:pct>=25?1:0;
  document.getElementById('progressTitle').textContent = titles[ti];
  document.getElementById('progressDesc').textContent = pct===100 ? 'You crushed it today!' : `${habits.length-done} habit${habits.length-done!==1?'s':''} remaining for today.`;
}

// ===================== HABIT STATUS =====================
function setHabitStatus(habitId, status) {
  if(!state.history[state.selectedDate]) state.history[state.selectedDate] = {};
  if(!status) {
    delete state.history[state.selectedDate][habitId];
  } else {
    state.history[state.selectedDate][habitId] = status;
  }
  save();

  if(status==='done') {
    const h = state.habits.find(x=>x.id===habitId);
    showToast(`✓ ${h?.name||'Habit'} done!`);
    if(state.settings.sound) playClick();
    checkAchievements();
  }

  renderHabits();
  renderStats();
  renderCalendar();
}

function playClick() {
  try {
    const ctx = new (window.AudioContext||window.webkitAudioContext)();
    const o = ctx.createOscillator();
    const g = ctx.createGain();
    o.connect(g); g.connect(ctx.destination);
    o.frequency.value = 600;
    o.type = 'sine';
    g.gain.setValueAtTime(0.3, ctx.currentTime);
    g.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime+0.2);
    o.start(); o.stop(ctx.currentTime+0.2);
  } catch(e){}
}

// ===================== REMINDERS =====================
function renderReminders() {
  if(!state.settings.reminders) { document.getElementById('reminderArea').innerHTML=''; return; }
  const reminders = [];
  const today = new Date();

  for(const h of state.habits) {
    // Check days since last completion
    let daysSince = 0;
    for(let i=1; i<=60; i++) {
      const d = new Date(today); d.setDate(d.getDate()-i);
      const ds = dateStr(d);
      if(state.history[ds]?.[h.id]==='done') break;
      daysSince++;
    }
    if(daysSince >= 3) {
      reminders.push({ icon:'⚠️', title:`${h.icon} ${h.name}`, msg:`You haven't done this for ${daysSince} days.` });
    }
  }

  const area = document.getElementById('reminderArea');
  if(reminders.length === 0) { area.innerHTML=''; return; }
  const r = reminders[0]; // show top reminder
  area.innerHTML = `<div class="reminder-card">
    <span class="reminder-icon">${r.icon}</span>
    <div class="reminder-text"><strong>${r.title}</strong><p>${r.msg}</p></div>
    <button class="reminder-close" onclick="document.getElementById('reminderArea').innerHTML=''">×</button>
  </div>`;
}

// ===================== STATS =====================
function renderStats() {
  const gs = getGlobalStats();

  document.getElementById('bigStats').innerHTML = `
    <div class="big-stat"><span class="big-stat-num">${state.habits.length}</span><span class="big-stat-label">Active Habits</span></div>
    <div class="big-stat"><span class="big-stat-num">${gs.maxStreak}</span><span class="big-stat-label">Best Streak 🔥</span></div>
    <div class="big-stat"><span class="big-stat-num">${gs.totalCompletions}</span><span class="big-stat-label">Total Done ✓</span></div>
    <div class="big-stat"><span class="big-stat-num">${gs.perfectDays}</span><span class="big-stat-label">Perfect Days ⭐</span></div>
    <div class="big-stat"><span class="big-stat-num">${gs.consistencyScore}%</span><span class="big-stat-label">14-day Rate</span></div>
    <div class="big-stat"><span class="big-stat-num">${gs.globalStreak}</span><span class="big-stat-label">Day Streak 🔥</span></div>
  `;

  // Weekly bars
  const bars = document.getElementById('weeklyBars');
  bars.innerHTML = '';
  const dayNames = ['S','M','T','W','T','F','S'];
  for(let i=6;i>=0;i--) {
    const d = new Date(); d.setDate(d.getDate()-i);
    const ds = dateStr(d);
    const due = state.habits.filter(h=>isDueToday(h,ds));
    const done = due.filter(h=>state.history[ds]?.[h.id]==='done').length;
    const pct = due.length > 0 ? (done/due.length)*100 : 0;
    bars.innerHTML += `<div class="weekly-bar-wrap">
      <div class="weekly-bar-bg"><div class="weekly-bar-fill" style="height:${pct}%"></div></div>
      <span class="weekly-bar-label">${dayNames[d.getDay()]}</span>
    </div>`;
  }

  // Heatmap (52 weeks)
  const hw = document.getElementById('heatmapWrap');
  hw.innerHTML = '';
  const grid = document.createElement('div');
  grid.className = 'heatmap-grid';
  const startDate = new Date(); startDate.setDate(startDate.getDate() - 363);
  // Align to Sunday
  startDate.setDate(startDate.getDate() - startDate.getDay());

  for(let w=0; w<53; w++) {
    const week = document.createElement('div');
    week.className = 'heatmap-week';
    for(let d=0; d<7; d++) {
      const cur = new Date(startDate);
      cur.setDate(cur.getDate() + w*7 + d);
      if(cur > new Date()) { week.innerHTML += `<div class="heatmap-cell"></div>`; continue; }
      const ds = dateStr(cur);
      const due = state.habits.filter(h=>isDueToday(h,ds));
      const done = due.filter(h=>state.history[ds]?.[h.id]==='done').length;
      const pct = due.length > 0 ? done/due.length : 0;
      const level = pct===0?0:pct<0.34?1:pct<0.67?2:pct<1?3:4;
      week.innerHTML += `<div class="heatmap-cell${level>0?' level-'+level:''}" title="${ds}: ${done}/${due.length}"></div>`;
    }
    grid.appendChild(week);
  }
  hw.appendChild(grid);

  // Per-habit performance
  const perf = document.getElementById('habitPerfList');
  perf.innerHTML = '';
  state.habits.forEach(h => {
    const s = getHabitStats(h);
    const color = COLORS[h.colorIdx]||COLORS[0];
    const status = getStatus(s.pct);
    perf.innerHTML += `<div style="display:flex;align-items:center;gap:10px;padding:10px 0;border-bottom:1px solid var(--sand)">
      <span style="font-size:20px">${h.icon}</span>
      <div style="flex:1;min-width:0">
        <div style="font-size:14px;font-weight:600;color:var(--text);white-space:nowrap;overflow:hidden;text-overflow:ellipsis">${h.name}</div>
        <div style="height:4px;background:var(--sand);border-radius:2px;margin-top:5px;overflow:hidden">
          <div style="height:100%;width:${s.pct}%;background:${color.text};border-radius:2px;transition:width 0.8s"></div>
        </div>
      </div>
      <div style="text-align:right;flex:0 0 auto">
        <div style="font-size:14px;font-weight:700;color:${color.text}">${s.pct}%</div>
        <div style="font-size:11px;color:var(--text-muted)">🔥${s.currentStreak}</div>
      </div>
    </div>`;
  });
  if(state.habits.length===0) perf.innerHTML = '<p style="font-size:13px;color:var(--text-muted)">No habits yet.</p>';
}

// ===================== CALENDAR =====================
let calSelectedDay = null;

function renderCalendar() {
  const cm = state.calMonth;
  document.getElementById('calTitle').textContent = cm.toLocaleDateString('en-US',{month:'long',year:'numeric'});
  const grid = document.getElementById('calendarGrid');
  const firstDay = new Date(cm.getFullYear(), cm.getMonth(), 1);
  const lastDay = new Date(cm.getFullYear(), cm.getMonth()+1, 0);
  const startOffset = firstDay.getDay();

  let html = '<div class="cal-weekdays">';
  ['Su','Mo','Tu','We','Th','Fr','Sa'].forEach(d => { html += `<div class="cal-weekday">${d}</div>`; });
  html += '</div><div class="cal-days-grid">';

  // Blanks
  for(let i=0;i<startOffset;i++) {
    const prev = new Date(cm.getFullYear(),cm.getMonth(),1-startOffset+i);
    html += `<div class="cal-day other-month" onclick="calSelectDay('${dateStr(prev)}')">${prev.getDate()}</div>`;
  }

  const today = todayStr();
  for(let day=1; day<=lastDay.getDate(); day++) {
    const d = new Date(cm.getFullYear(),cm.getMonth(),day);
    const ds = dateStr(d);
    const due = state.habits.filter(h=>isDueToday(h,ds));
    const done = due.filter(h=>state.history[ds]?.[h.id]==='done').length;
    let cls = '';
    if(ds===today) cls='today';
    else if(ds===calSelectedDay) cls='selected';
    let dotCls = '';
    if(due.length>0) { dotCls = done===due.length?'full':done>0?'partial':'none'; }
    html += `<div class="cal-day ${cls} ${dotCls}" onclick="calSelectDay('${ds}')">${day}<span class="cal-day-dot"></span></div>`;
  }

  const remaining = 42 - startOffset - lastDay.getDate();
  for(let i=1; i<=remaining; i++) {
    const next = new Date(cm.getFullYear(),cm.getMonth()+1,i);
    html += `<div class="cal-day other-month" onclick="calSelectDay('${dateStr(next)}')">${next.getDate()}</div>`;
  }

  html += '</div>';
  grid.innerHTML = html;

  // Monthly stats
  let mDue=0, mDone=0;
  for(let day=1; day<=lastDay.getDate(); day++) {
    const d = new Date(cm.getFullYear(),cm.getMonth(),day);
    const ds = dateStr(d);
    const due = state.habits.filter(h=>isDueToday(h,ds));
    mDue += due.length;
    mDone += due.filter(h=>state.history[ds]?.[h.id]==='done').length;
  }
  const mPct = mDue > 0 ? Math.round(mDone/mDue*100) : 0;
  document.getElementById('monthlyStats').innerHTML = `
    <div style="display:flex;gap:20px;padding:8px 0">
      <div class="pstat"><span class="pstat-num" style="color:var(--accent)">${mDone}</span><span class="pstat-label">Completed</span></div>
      <div class="pstat"><span class="pstat-num">${mDue-mDone}</span><span class="pstat-label">Missed</span></div>
      <div class="pstat"><span class="pstat-num" style="color:var(--green)">${mPct}%</span><span class="pstat-label">Rate</span></div>
    </div>
    <div style="height:6px;background:var(--sand);border-radius:3px;overflow:hidden;margin-top:4px">
      <div style="height:100%;width:${mPct}%;background:var(--green);border-radius:3px;transition:width 0.8s"></div>
    </div>`;
}

function calNav(dir) {
  state.calMonth = new Date(state.calMonth.getFullYear(), state.calMonth.getMonth()+dir, 1);
  renderCalendar();
}

function calSelectDay(ds) {
  calSelectedDay = ds;
  state.selectedDate = ds;
  const due = state.habits.filter(h=>isDueToday(h,ds));
  const dayData = state.history[ds]||{};
  let html = `<p style="font-size:13px;color:var(--text-muted);margin-bottom:10px">${parseDate(ds).toLocaleDateString('en-US',{weekday:'long',month:'long',day:'numeric'})}</p>`;
  if(due.length===0) html += '<p style="font-size:13px;color:var(--text-muted)">No habits due.</p>';
  else due.forEach(h => {
    const s = dayData[h.id];
    const color = COLORS[h.colorIdx]||COLORS[0];
    html += `<div style="display:flex;align-items:center;gap:10px;padding:8px 0;border-bottom:1px solid var(--sand)">
      <span style="font-size:18px">${h.icon}</span>
      <span style="flex:1;font-size:14px">${h.name}</span>
      <span style="padding:3px 12px;border-radius:50px;font-size:12px;background:${s==='done'?'var(--green-light)':s==='missed'?'var(--rose-light)':'var(--sand)'};color:${s==='done'?'var(--green)':s==='missed'?'var(--rose)':'var(--text-muted)'}">${s||'pending'}</span>
    </div>`;
  });
  document.getElementById('calDaySummary').innerHTML = html;
  renderCalendar();
}

// ===================== HISTORY =====================
function renderHistory() {
  const hc = document.getElementById('historyContent');
  const allDates = Object.keys(state.history).sort().reverse();

  if(allDates.length===0) {
    hc.innerHTML = `<div class="empty-state"><span class="empty-icon">📖</span><h3>No history yet</h3><p>Start tracking habits to see your history here.</p></div>`;
    return;
  }

  // Group by month
  const months = {};
  for(const ds of allDates) {
    const d = parseDate(ds);
    const key = `${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}`;
    if(!months[key]) months[key] = [];
    months[key].push(ds);
  }

  let html = '';
  for(const [mk, dates] of Object.entries(months).sort().reverse()) {
    const [y,m] = mk.split('-');
    const monthName = new Date(y,m-1,1).toLocaleDateString('en-US',{month:'long',year:'numeric'});
    html += `<div class="history-month"><div class="history-month-title">${monthName}<span style="font-size:13px;font-weight:400;color:var(--text-muted)">${dates.length} days</span></div>`;
    for(const ds of dates.slice(0,30)) {
      const dayData = state.history[ds]||{};
      const due = state.habits.filter(h=>isDueToday(h,ds));
      if(due.length===0 && Object.keys(dayData).length===0) continue;
      const done = due.filter(h=>dayData[h.id]==='done');
      const pct = due.length>0?Math.round(done.length/due.length*100):0;
      const d = parseDate(ds);
      html += `<div class="history-day">
        <span class="history-day-date">${d.toLocaleDateString('en-US',{month:'short',day:'numeric'})}</span>
        <div class="history-day-habits">
          ${done.slice(0,6).map(h=>`<span class="history-habit-chip" style="background:${(COLORS[h.colorIdx]||COLORS[0]).bg};color:${(COLORS[h.colorIdx]||COLORS[0]).text}">${h.icon} ${h.name}</span>`).join('')}
          ${done.length>6?`<span class="history-habit-chip" style="background:var(--sand);color:var(--text-muted)">+${done.length-6} more</span>`:''}
          ${done.length===0?`<span style="font-size:12px;color:var(--text-muted)">No completions</span>`:''}
        </div>
        <span class="history-day-rate" style="color:${pct>=70?'var(--green)':pct>=40?'var(--yellow)':'var(--rose)'}">${pct}%</span>
      </div>`;
    }
    html += '</div>';
  }
  hc.innerHTML = html;
}

// ===================== ACHIEVEMENTS =====================
function checkAchievements() {
  const gs = getGlobalStats();
  ACHIEVEMENTS.forEach(a => {
    if(a.check(gs)) {
      const key = 'bloom_ach_'+a.id;
      if(!localStorage.getItem(key)) {
        localStorage.setItem(key,'1');
        showToast(`🏆 Achievement: ${a.name}!`);
      }
    }
  });
}

function renderAchievements() {
  const gs = getGlobalStats();
  const grid = document.getElementById('achievementsGrid');
  grid.innerHTML = ACHIEVEMENTS.map(a => {
    const unlocked = a.check(gs);
    return `<div class="achievement-card${unlocked?'':' locked'}">
      <div class="achievement-icon">${a.icon}</div>
      <div class="achievement-name">${a.name}</div>
      <div class="achievement-desc">${a.desc}</div>
      ${unlocked?'<span style="font-size:11px;color:var(--green);font-weight:600">✓ Unlocked</span>':'<span style="font-size:11px;color:var(--text-light)">Locked</span>'}
    </div>`;
  }).join('');
}

// ===================== MODAL =====================
function openAddHabit() {
  state.editingId = null;
  document.getElementById('modalTitle').textContent = 'Add New Habit 🌱';
  document.getElementById('habitName').value = '';
  document.getElementById('habitFreq').value = 'daily';
  document.getElementById('habitCategory').value = 'morning';
  document.getElementById('habitNotes').value = '';
  document.getElementById('deleteHabitBtn').style.display = 'none';
  document.getElementById('customFreqGroup').style.display = 'none';
  state.selectedIcon = '📚';
  state.selectedColor = 0;
  buildIconPicker();
  buildColorPicker();
  openModal('habitModal');
}

function openEditHabit(id) {
  const h = state.habits.find(x=>x.id===id);
  if(!h) return;
  state.editingId = id;
  document.getElementById('modalTitle').textContent = 'Edit Habit ✎';
  document.getElementById('habitName').value = h.name;
  document.getElementById('habitFreq').value = h.freq;
  document.getElementById('habitCategory').value = h.category;
  document.getElementById('habitNotes').value = h.notes||'';
  document.getElementById('deleteHabitBtn').style.display = '';
  document.getElementById('customFreqGroup').style.display = h.freq==='custom'?'':'none';
  if(h.freq==='custom') document.getElementById('customFreqDays').value = h.freqDays||5;
  state.selectedIcon = h.icon;
  state.selectedColor = h.colorIdx||0;
  buildIconPicker();
  buildColorPicker();
  openModal('habitModal');
}

function buildIconPicker() {
  document.getElementById('iconPicker').innerHTML = ICONS.map(i=>
    `<div class="icon-option${i===state.selectedIcon?' selected':''}" onclick="selectIcon('${i}')">${i}</div>`
  ).join('');
}

function buildColorPicker() {
  document.getElementById('colorPicker').innerHTML = COLORS.map((c,i)=>
    `<div class="color-option${i===state.selectedColor?' selected':''}" style="background:${c.text}" onclick="selectColor(${i})"></div>`
  ).join('');
}

function selectIcon(icon) {
  state.selectedIcon = icon;
  buildIconPicker();
}
function selectColor(idx) {
  state.selectedColor = idx;
  buildColorPicker();
}

function updateFreqCustom() {
  const v = document.getElementById('habitFreq').value;
  document.getElementById('customFreqGroup').style.display = v==='custom'?'':'none';
}

function saveHabit() {
  const name = document.getElementById('habitName').value.trim();
  if(!name) { showToast('Please enter a habit name 🌸'); return; }

  const freq = document.getElementById('habitFreq').value;
  const freqDays = document.getElementById('customFreqDays').value;

  if(state.editingId) {
    const h = state.habits.find(x=>x.id===state.editingId);
    if(h) {
      h.name = name;
      h.icon = state.selectedIcon;
      h.colorIdx = state.selectedColor;
      h.category = document.getElementById('habitCategory').value;
      h.freq = freq;
      h.freqDays = freqDays;
      h.notes = document.getElementById('habitNotes').value.trim();
    }
    showToast('Habit updated ✓');
  } else {
    const h = {
      id: 'h' + Date.now(),
      name,
      icon: state.selectedIcon,
      colorIdx: state.selectedColor,
      category: document.getElementById('habitCategory').value,
      freq,
      freqDays,
      notes: document.getElementById('habitNotes').value.trim(),
      created: todayStr(),
    };
    state.habits.push(h);
    showToast('Habit added 🌱');
    checkAchievements();
  }

  save();
  closeModal();
  renderAll();
}

function deleteHabit() {
  if(!confirm('Delete this habit and all its history?')) return;
  state.habits = state.habits.filter(h=>h.id!==state.editingId);
  // clean history
  for(const d of Object.keys(state.history)) {
    delete state.history[d][state.editingId];
  }
  save();
  closeModal();
  showToast('Habit deleted.');
  renderAll();
}

function openModal(id) {
  document.getElementById(id).classList.add('open');
  document.body.style.overflow = 'hidden';
}
function closeModal() {
  document.getElementById('habitModal').classList.remove('open');
  document.body.style.overflow = '';
}
function closeDetailModal() {
  document.getElementById('detailModal').classList.remove('open');
  document.body.style.overflow = '';
}

document.getElementById('habitModal').addEventListener('click', e => {
  if(e.target === document.getElementById('habitModal')) closeModal();
});
document.getElementById('detailModal').addEventListener('click', e => {
  if(e.target === document.getElementById('detailModal')) closeDetailModal();
});

// ===================== NAVIGATION =====================
function switchNav(view, btn) {
  document.querySelectorAll('.nav-item').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  document.querySelectorAll('.view').forEach(v=>v.classList.remove('active'));
  document.getElementById('view'+view.charAt(0).toUpperCase()+view.slice(1)).classList.add('active');

  const showTabs = view==='today';
  document.getElementById('mainTabs').style.display = showTabs ? 'flex' : 'none';
  document.getElementById('weekStrip').style.display = view==='today'||view==='calendar' ? 'flex' : 'none';

  if(view==='stats') renderStats();
  if(view==='calendar') renderCalendar();
  if(view==='history') renderHistory();
  if(view==='achievements') renderAchievements();
}

function setFilter(f) {
  state.currentFilter = f;
  document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));
  event.target.classList.add('active');
  renderHabits();
}

// ===================== THEME =====================
function toggleTheme() {
  state.settings.dark = !state.settings.dark;
  applyTheme();
  save();
  updateSettingsUI();
}
function applyTheme() {
  document.documentElement.setAttribute('data-theme', state.settings.dark ? 'dark' : 'light');
  document.getElementById('themeToggleBtn').textContent = state.settings.dark ? '☀️' : '🌙';
}

function setThemeColor(tc) {
  state.settings.themeColor = tc;
  document.querySelectorAll('.theme-option').forEach(o=>{
    o.classList.toggle('selected', o.dataset.themeColor===tc);
  });
  applyThemeColor(tc);
  save();
}

function applyThemeColor(tc) {
  const themes = {
    default: { '--accent':'#c47c5a', '--accent2':'#e8a87c', '--accent-light':'#f5d5c0' },
    sage: { '--accent':'#6aad7a', '--accent2':'#90c890', '--accent-light':'#d4ead9' },
    lavender: { '--accent':'#9e86c0', '--accent2':'#c0aadd', '--accent-light':'#e0d4f5' },
    ocean: { '--accent':'#4a8abf', '--accent2':'#7ab0d8', '--accent-light':'#d4e4f0' },
    rose: { '--accent':'#c4788a', '--accent2':'#e0a0b0', '--accent-light':'#f5d4dc' },
    mocha: { '--accent':'#a07850', '--accent2':'#c4a070', '--accent-light':'#f0e0c8' },
  };
  const t = themes[tc] || themes.default;
  const root = document.documentElement;
  for(const [k,v] of Object.entries(t)) root.style.setProperty(k,v);
}

function toggleReminders() {
  state.settings.reminders = !state.settings.reminders;
  save();
  updateSettingsUI();
  renderReminders();
}
function toggleSound() {
  state.settings.sound = !state.settings.sound;
  save();
  updateSettingsUI();
}

function updateSettingsUI() {
  const dt = document.getElementById('darkToggle');
  const rt = document.getElementById('reminderToggle');
  const st = document.getElementById('soundToggle');
  if(dt) dt.className = 'toggle'+(state.settings.dark?' on':'');
  if(rt) rt.className = 'toggle'+(state.settings.reminders?' on':'');
  if(st) st.className = 'toggle'+(state.settings.sound?' on':'');
  document.querySelectorAll('.theme-option').forEach(o=>{
    o.classList.toggle('selected', o.dataset.themeColor===(state.settings.themeColor||'default'));
  });
}

// ===================== SEARCH =====================
function toggleSearch() {
  const wrap = document.getElementById('searchBarWrap');
  const visible = wrap.style.display !== 'none';
  wrap.style.display = visible ? 'none' : 'block';
  if(!visible) document.getElementById('searchInput').focus();
}

// ===================== EXPORT / IMPORT =====================
function exportData() {
  const data = JSON.stringify(state, null, 2);
  const blob = new Blob([data], {type:'application/json'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = `bloom-habits-${todayStr()}.json`;
  a.click();
  showToast('Data exported! 📤');
}

function importData() {
  const input = document.createElement('input');
  input.type = 'file'; input.accept = '.json';
  input.onchange = e => {
    const file = e.target.files[0];
    if(!file) return;
    const reader = new FileReader();
    reader.onload = ev => {
      try {
        const parsed = JSON.parse(ev.target.result);
        state = {...state, ...parsed};
        save();
        renderAll();
        showToast('Data imported! 📥');
      } catch(err) { showToast('Invalid file 😕'); }
    };
    reader.readAsText(file);
  };
  input.click();
}

function confirmClear() {
  if(confirm('Are you sure? This will delete ALL habits and history.')) {
    state.habits = [];
    state.history = {};
    save();
    renderAll();
    showToast('Data cleared.');
  }
}

// ===================== TOAST =====================
function showToast(msg) {
  const tc = document.getElementById('toastContainer');
  const t = document.createElement('div');
  t.className = 'toast';
  t.textContent = msg;
  tc.appendChild(t);
  setTimeout(() => t.remove(), 2600);
}

// ===================== SEED DATA =====================
function seedDefaultHabits() {
  if(state.habits.length > 0) return;
  const defaults = [
    {name:'Morning Meditation',icon:'🧘',colorIdx:0,category:'morning',freq:'daily',notes:'5-10 mins of calm'},
    {name:'Drink Water',icon:'💧',colorIdx:1,category:'health',freq:'daily',notes:'8 glasses a day'},
    {name:'Read 20 mins',icon:'📚',colorIdx:2,category:'mind',freq:'daily',notes:''},
    {name:'Exercise',icon:'💪',colorIdx:3,category:'health',freq:'3week',notes:'Any movement counts'},
    {name:'Journal',icon:'✍️',colorIdx:4,category:'mind',freq:'daily',notes:'Gratitude & reflection'},
    {name:'Sleep by 11pm',icon:'😴',colorIdx:5,category:'evening',freq:'daily',notes:''},
  ];
  defaults.forEach(d => {
    state.habits.push({...d, id:'h'+Date.now()+Math.random(), created:todayStr()});
  });
  save();
}

// ===================== INIT =====================
load();
applyTheme();
applyThemeColor(state.settings.themeColor||'default');
seedDefaultHabits();
renderAll();
document.getElementById('mainTabs').style.display = 'flex';

// Periodic reminder check
setInterval(renderReminders, 60000);
</script>
</body>
</html>
