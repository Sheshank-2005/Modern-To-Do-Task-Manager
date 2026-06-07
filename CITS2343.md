<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>TaskFlow — Modern To-Do Manager</title>
<link rel="preconnect" href="https://fonts.googleapis.com"/>
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin/>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet"/>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@latest/tabler-icons.min.css"/>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #F7F5F0;
    --surface: #FFFFFF;
    --surface2: #F0EDE6;
    --border: rgba(0,0,0,0.08);
    --border-strong: rgba(0,0,0,0.15);
    --text: #1A1916;
    --text-muted: #7A7670;
    --text-hint: #B0ADA7;
    --accent: #2D5BE3;
    --accent-bg: #EEF2FD;
    --accent-text: #1A3A9F;
    --danger: #C0392B;
    --danger-bg: #FDEEEC;
    --success: #1D7A4F;
    --success-bg: #EEFAF3;
    --warning: #A0600A;
    --warning-bg: #FEF6E8;
    --radius: 12px;
    --radius-sm: 7px;
  }

  @media (prefers-color-scheme: dark) {
    :root {
      --bg: #161613;
      --surface: #1F1E1B;
      --surface2: #2A2925;
      --border: rgba(255,255,255,0.07);
      --border-strong: rgba(255,255,255,0.14);
      --text: #EAE7E0;
      --text-muted: #9A9790;
      --text-hint: #5A5855;
      --accent: #5B82F5;
      --accent-bg: #1A2456;
      --accent-text: #9BB5FF;
      --danger: #E05A50;
      --danger-bg: #3A1510;
      --success: #4BB87A;
      --success-bg: #0D2D1F;
      --warning: #E8A040;
      --warning-bg: #2D1F08;
    }
  }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    padding: 0;
  }

  .app {
    max-width: 680px;
    margin: 0 auto;
    padding: 2.5rem 1.5rem 4rem;
  }

  .header {
    display: flex;
    align-items: flex-end;
    justify-content: space-between;
    margin-bottom: 2rem;
  }

  .header-left h1 {
    font-family: 'DM Serif Display', serif;
    font-size: 2.6rem;
    font-weight: 400;
    letter-spacing: -0.02em;
    color: var(--text);
    line-height: 1;
  }

  .header-left .subtitle {
    font-size: 13px;
    color: var(--text-muted);
    margin-top: 6px;
    font-weight: 300;
  }

  .date-badge {
    font-size: 12px;
    color: var(--text-muted);
    background: var(--surface2);
    border: 0.5px solid var(--border-strong);
    padding: 6px 14px;
    border-radius: 20px;
    font-weight: 500;
  }

  /* Stats */
  .stats-row {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    margin-bottom: 1.5rem;
  }

  .stat-card {
    background: var(--surface);
    border: 0.5px solid var(--border);
    border-radius: var(--radius);
    padding: 16px 18px;
    display: flex;
    flex-direction: column;
    gap: 4px;
  }

  .stat-card .stat-num {
    font-size: 26px;
    font-weight: 600;
    color: var(--text);
    line-height: 1;
  }

  .stat-card .stat-label {
    font-size: 12px;
    color: var(--text-muted);
    font-weight: 400;
  }

  .stat-card.accent {
    border-color: var(--accent);
    background: var(--accent-bg);
  }
  .stat-card.accent .stat-num { color: var(--accent-text); }

  /* Add Section */
  .add-section {
    background: var(--surface);
    border: 0.5px solid var(--border);
    border-radius: var(--radius);
    padding: 18px;
    margin-bottom: 1.25rem;
  }

  .add-row {
    display: flex;
    gap: 8px;
    align-items: center;
  }

  .add-input {
    flex: 1;
    background: var(--surface2);
    border: 0.5px solid var(--border-strong);
    border-radius: var(--radius-sm);
    padding: 11px 14px;
    font-size: 14px;
    font-family: 'DM Sans', sans-serif;
    color: var(--text);
    outline: none;
    transition: border-color 0.15s, box-shadow 0.15s;
  }

  .add-input:focus {
    border-color: var(--accent);
    box-shadow: 0 0 0 3px rgba(45, 91, 227, 0.12);
  }

  .add-input::placeholder { color: var(--text-hint); }

  .add-options {
    display: flex;
    gap: 8px;
    margin-top: 10px;
    flex-wrap: wrap;
    align-items: center;
  }

  .priority-select, .category-select {
    background: var(--surface2);
    border: 0.5px solid var(--border-strong);
    border-radius: var(--radius-sm);
    padding: 7px 12px;
    font-size: 12px;
    font-family: 'DM Sans', sans-serif;
    color: var(--text);
    cursor: pointer;
    outline: none;
  }

  .btn-add {
    background: var(--accent);
    color: white;
    border: none;
    border-radius: var(--radius-sm);
    padding: 11px 20px;
    font-size: 13px;
    font-weight: 500;
    font-family: 'DM Sans', sans-serif;
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: 6px;
    transition: opacity 0.15s, transform 0.1s;
    white-space: nowrap;
  }

  .btn-add:hover { opacity: 0.88; }
  .btn-add:active { transform: scale(0.97); }

  /* Progress */
  .progress-wrap {
    margin-top: 14px;
  }

  .progress-meta {
    display: flex;
    justify-content: space-between;
    font-size: 12px;
    color: var(--text-muted);
    margin-bottom: 5px;
  }

  .progress-bar-wrap {
    background: var(--surface2);
    border-radius: 4px;
    height: 5px;
    overflow: hidden;
  }

  .progress-bar-fill {
    height: 100%;
    background: var(--success);
    border-radius: 4px;
    transition: width 0.5s cubic-bezier(0.4, 0, 0.2, 1);
  }

  /* Filters */
  .filters {
    display: flex;
    gap: 6px;
    margin-bottom: 1.25rem;
    flex-wrap: wrap;
  }

  .filter-btn {
    background: var(--surface);
    border: 0.5px solid var(--border);
    border-radius: 20px;
    padding: 6px 15px;
    font-size: 12px;
    font-weight: 500;
    font-family: 'DM Sans', sans-serif;
    color: var(--text-muted);
    cursor: pointer;
    transition: all 0.15s;
  }

  .filter-btn:hover {
    border-color: var(--border-strong);
    color: var(--text);
  }

  .filter-btn.active {
    background: var(--text);
    color: var(--bg);
    border-color: var(--text);
  }

  /* Task List */
  .section-label {
    font-size: 11px;
    font-weight: 600;
    color: var(--text-hint);
    letter-spacing: 0.08em;
    text-transform: uppercase;
    margin-bottom: 8px;
    padding-left: 2px;
  }

  .tasks-list {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .task-item {
    background: var(--surface);
    border: 0.5px solid var(--border);
    border-radius: var(--radius);
    padding: 14px 16px;
    display: flex;
    align-items: center;
    gap: 12px;
    transition: border-color 0.15s, opacity 0.2s;
    animation: slideIn 0.2s ease;
  }

  @keyframes slideIn {
    from { opacity: 0; transform: translateY(-6px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .task-item:hover { border-color: var(--border-strong); }

  .task-item.done { opacity: 0.5; }
  .task-item.done .task-title {
    text-decoration: line-through;
    color: var(--text-muted);
  }

  /* Checkbox */
  .task-check {
    width: 22px;
    height: 22px;
    border-radius: 50%;
    border: 1.5px solid var(--border-strong);
    background: transparent;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
    transition: all 0.15s;
  }

  .task-check:hover {
    border-color: var(--accent);
    background: var(--accent-bg);
  }

  .task-check.checked {
    background: var(--success);
    border-color: var(--success);
  }

  .task-check.checked::after {
    content: '';
    display: block;
    width: 11px;
    height: 11px;
    background: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='white' stroke-width='3' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpolyline points='20 6 9 17 4 12'/%3E%3C/svg%3E") center/contain no-repeat;
  }

  .task-body { flex: 1; min-width: 0; }

  .task-title {
    font-size: 14px;
    font-weight: 500;
    color: var(--text);
    margin-bottom: 5px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .task-meta {
    display: flex;
    gap: 6px;
    align-items: center;
    flex-wrap: wrap;
  }

  .tag {
    font-size: 11px;
    font-weight: 500;
    padding: 2px 9px;
    border-radius: 20px;
    line-height: 1.6;
  }

  .tag.priority-high  { background: var(--danger-bg);  color: var(--danger);  }
  .tag.priority-medium { background: var(--warning-bg); color: var(--warning); }
  .tag.priority-low   { background: var(--success-bg); color: var(--success); }

  .tag.cat-work     { background: var(--accent-bg);  color: var(--accent-text); }
  .tag.cat-personal { background: #F5EEFF; color: #6930C3; }
  .tag.cat-health   { background: var(--success-bg); color: var(--success); }
  .tag.cat-shopping { background: var(--warning-bg); color: var(--warning); }

  @media (prefers-color-scheme: dark) {
    .tag.cat-personal { background: #2D1654; color: #C4A0FF; }
  }

  /* Task Actions */
  .task-actions {
    display: flex;
    gap: 4px;
    flex-shrink: 0;
    opacity: 0;
    transition: opacity 0.15s;
  }

  .task-item:hover .task-actions { opacity: 1; }

  .icon-btn {
    width: 30px;
    height: 30px;
    border-radius: var(--radius-sm);
    border: 0.5px solid var(--border);
    background: var(--surface2);
    color: var(--text-muted);
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 16px;
    transition: all 0.15s;
  }

  .icon-btn:hover {
    background: var(--danger-bg);
    color: var(--danger);
    border-color: var(--danger);
  }

  /* Empty State */
  .empty-state {
    text-align: center;
    padding: 3.5rem 1rem;
    color: var(--text-hint);
  }

  .empty-state i {
    font-size: 40px;
    margin-bottom: 14px;
    display: block;
  }

  .empty-state p { font-size: 14px; }

  /* Responsive */
  @media (max-width: 480px) {
    .header { flex-direction: column; align-items: flex-start; gap: 10px; }
    .stats-row { grid-template-columns: repeat(3, 1fr); }
    .add-row { flex-wrap: wrap; }
    .btn-add { width: 100%; justify-content: center; }
  }
</style>
</head>
<body>
<div class="app">

  <div class="header">
    <div class="header-left">
      <h1>TaskFlow</h1>
      <p class="subtitle">Stay intentional. Get things done.</p>
    </div>
    <div class="date-badge" id="date-badge"></div>
  </div>

  <!-- Stats -->
  <div class="stats-row">
    <div class="stat-card accent">
      <span class="stat-num" id="stat-total">0</span>
      <span class="stat-label">Total tasks</span>
    </div>
    <div class="stat-card">
      <span class="stat-num" id="stat-pending">0</span>
      <span class="stat-label">Pending</span>
    </div>
    <div class="stat-card">
      <span class="stat-num" id="stat-done">0</span>
      <span class="stat-label">Completed</span>
    </div>
  </div>

  <!-- Add Task -->
  <div class="add-section">
    <div class="add-row">
      <input class="add-input" id="task-input" type="text" placeholder="What needs to be done?" maxlength="120" aria-label="New task title" />
      <button class="btn-add" id="add-btn" aria-label="Add task">
        <i class="ti ti-plus" aria-hidden="true"></i> Add Task
      </button>
    </div>
    <div class="add-options">
      <select class="priority-select" id="priority-sel" aria-label="Priority">
        <option value="medium">⚡ Medium priority</option>
        <option value="high">🔴 High priority</option>
        <option value="low">🟢 Low priority</option>
      </select>
      <select class="category-select" id="category-sel" aria-label="Category">
        <option value="work">💼 Work</option>
        <option value="personal">🎯 Personal</option>
        <option value="health">🌿 Health</option>
        <option value="shopping">🛒 Shopping</option>
      </select>
    </div>
    <div class="progress-wrap">
      <div class="progress-meta">
        <span>Progress</span>
        <span id="progress-pct">0%</span>
      </div>
      <div class="progress-bar-wrap">
        <div class="progress-bar-fill" id="progress-fill" style="width: 0%"></div>
      </div>
    </div>
  </div>

  <!-- Filters -->
  <div class="filters" role="group" aria-label="Filter tasks">
    <button class="filter-btn active" data-filter="all">All</button>
    <button class="filter-btn" data-filter="pending">Pending</button>
    <button class="filter-btn" data-filter="done">Done</button>
    <button class="filter-btn" data-filter="high">🔴 High</button>
    <button class="filter-btn" data-filter="work">💼 Work</button>
    <button class="filter-btn" data-filter="personal">🎯 Personal</button>
    <button class="filter-btn" data-filter="health">🌿 Health</button>
    <button class="filter-btn" data-filter="shopping">🛒 Shopping</button>
  </div>

  <!-- Task List -->
  <div class="section-label">Tasks</div>
  <div class="tasks-list" id="tasks-list" aria-live="polite" aria-label="Task list"></div>

</div>

<script>
  const SAMPLE_TASKS = [
    { id: 1, title: 'Review Q2 project proposal',         priority: 'high',   category: 'work',     done: false },
    { id: 2, title: 'Morning run — 5km target',           priority: 'medium', category: 'health',   done: true  },
    { id: 3, title: 'Pick up groceries for the week',     priority: 'low',    category: 'shopping', done: false },
    { id: 4, title: 'Update portfolio website',           priority: 'medium', category: 'personal', done: false },
    { id: 5, title: 'Send invoice to client',             priority: 'high',   category: 'work',     done: true  },
    { id: 6, title: 'Schedule annual health checkup',     priority: 'medium', category: 'health',   done: false },
  ];

  let tasks = [...SAMPLE_TASKS];
  let nextId = tasks.length + 1;
  let activeFilter = 'all';

  /* Date badge */
  const now = new Date();
  document.getElementById('date-badge').textContent =
    now.toLocaleDateString('en-GB', { weekday: 'short', day: 'numeric', month: 'short', year: 'numeric' });

  /* Helpers */
  function esc(s) {
    return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
  }

  function priorityLabel(p) {
    return { high: 'High', medium: 'Medium', low: 'Low' }[p] || p;
  }

  function categoryLabel(c) {
    return c.charAt(0).toUpperCase() + c.slice(1);
  }

  function getFiltered() {
    return tasks.filter(t => {
      switch (activeFilter) {
        case 'pending':  return !t.done;
        case 'done':     return t.done;
        case 'high':     return t.priority === 'high' && !t.done;
        case 'work':     return t.category === 'work';
        case 'personal': return t.category === 'personal';
        case 'health':   return t.category === 'health';
        case 'shopping': return t.category === 'shopping';
        default:         return true;
      }
    });
  }

  function updateStats() {
    const total = tasks.length;
    const done  = tasks.filter(t => t.done).length;
    document.getElementById('stat-total').textContent   = total;
    document.getElementById('stat-pending').textContent = total - done;
    document.getElementById('stat-done').textContent    = done;
    const pct = total === 0 ? 0 : Math.round((done / total) * 100);
    document.getElementById('progress-fill').style.width = pct + '%';
    document.getElementById('progress-pct').textContent  = pct + '%';
  }

  function render() {
    updateStats();
    const list     = document.getElementById('tasks-list');
    const filtered = getFiltered();

    if (filtered.length === 0) {
      list.innerHTML = `
        <div class="empty-state">
          <i class="ti ti-checklist" aria-hidden="true"></i>
          <p>No tasks here. Add one above!</p>
        </div>`;
      return;
    }

    list.innerHTML = filtered.map(t => `
      <div class="task-item ${t.done ? 'done' : ''}" data-id="${t.id}">
        <button
          class="task-check ${t.done ? 'checked' : ''}"
          onclick="toggleTask(${t.id})"
          aria-label="${t.done ? 'Mark incomplete' : 'Mark complete'}"
          title="${t.done ? 'Mark incomplete' : 'Mark complete'}"
        ></button>
        <div class="task-body">
          <div class="task-title" title="${esc(t.title)}">${esc(t.title)}</div>
          <div class="task-meta">
            <span class="tag priority-${t.priority}">${priorityLabel(t.priority)}</span>
            <span class="tag cat-${t.category}">${categoryLabel(t.category)}</span>
          </div>
        </div>
        <div class="task-actions">
          <button class="icon-btn" onclick="deleteTask(${t.id})" aria-label="Delete task" title="Delete">
            <i class="ti ti-trash" aria-hidden="true"></i>
          </button>
        </div>
      </div>
    `).join('');
  }

  function toggleTask(id) {
    const t = tasks.find(t => t.id === id);
    if (t) { t.done = !t.done; render(); }
  }

  function deleteTask(id) {
    tasks = tasks.filter(t => t.id !== id);
    render();
  }

  function addTask() {
    const input    = document.getElementById('task-input');
    const title    = input.value.trim();
    if (!title) { input.focus(); return; }
    const priority = document.getElementById('priority-sel').value;
    const category = document.getElementById('category-sel').value;
    tasks.unshift({ id: nextId++, title, priority, category, done: false });
    input.value = '';
    setFilter('all');
    render();
    input.focus();
  }

  function setFilter(f) {
    activeFilter = f;
    document.querySelectorAll('.filter-btn').forEach(b => {
      b.classList.toggle('active', b.dataset.filter === f);
    });
  }

  /* Event listeners */
  document.getElementById('add-btn').addEventListener('click', addTask);

  document.getElementById('task-input').addEventListener('keydown', e => {
    if (e.key === 'Enter') addTask();
  });

  document.querySelectorAll('.filter-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      setFilter(btn.dataset.filter);
      render();
    });
  });

  /* Boot */
  render();
</script>
</body>
</html>
