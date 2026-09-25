<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Glory's Business Dashboard</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,500;9..144,600;9..144,700&family=IBM+Plex+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#F3EAD9; --bg2:#ECE0C7;
    --card:#FFFFFF; --card-alt:#FFFDF8;
    --ink:#3D2A1B; --ink-soft:#7A6650;
    --line:#E6D8BE;
    --brown-deep:#3D2817;
    --gold:#DDAA45; --gold-dark:#A9782F;
    --green:#5B7F4A; --green-tint:#E7EEE1;
    --maroon:#7A3B2E; --maroon-tint:#F2E1DC;
    --tan:#C9915C;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{background:linear-gradient(180deg, var(--bg), var(--bg2) 400px, var(--bg) 900px); background-attachment:fixed; color:var(--ink); font-family:'IBM Plex Sans', sans-serif; font-variant-numeric:tabular-nums; -webkit-font-smoothing:antialiased;}
  h1,h2,h3,.serif{font-family:'Fraunces', serif;}
  button{font-family:'IBM Plex Sans', sans-serif;}
  .app{display:flex; min-height:100vh;}

  .sidebar{width:224px; flex-shrink:0; background:var(--card-alt); border-right:1px solid var(--line); display:flex; flex-direction:column; padding:24px 0; position:sticky; top:0; height:100vh;}
  .brand{padding:0 22px 18px 22px; border-bottom:1px solid var(--line); margin-bottom:16px; display:flex; align-items:center; gap:10px;}
  .brand-logo{width:36px; height:36px; border-radius:50%; background:var(--bg); flex-shrink:0; object-fit:cover; display:flex; align-items:center; justify-content:center; font-family:'Fraunces',serif; font-weight:600; color:var(--brown-deep); font-size:1.05rem; border:1px solid var(--line);}
  .brand-text .mark{font-family:'Fraunces', serif; font-weight:700; font-size:1.15rem; color:var(--brown-deep); line-height:1.1;}
  .brand-text .sub{font-size:0.68rem; color:var(--ink-soft); margin-top:2px;}
  nav{flex:1; padding:8px 12px;}
  nav a{display:flex; align-items:center; gap:10px; padding:10px 12px; border-radius:8px; color:var(--ink-soft); text-decoration:none; font-size:0.9rem; margin-bottom:2px; cursor:pointer; transition:background 0.15s ease;}
  nav a:hover{background:var(--bg);}
  nav a.active{background:var(--gold); color:#fff; font-weight:600;}
  nav a .dot{width:5px;height:5px;border-radius:50%;background:currentColor;opacity:0.6;flex-shrink:0;}
  .sidebar-foot{padding:12px; display:flex; flex-direction:column; gap:10px;}
  .add-expense-btn{width:100%; background:var(--brown-deep); color:#fff; border:none; padding:12px 14px; border-radius:24px; font-weight:600; font-size:0.9rem; cursor:pointer; transition:background 0.15s ease;}
  .add-expense-btn:hover{background:#2c1c10;}
  .lang-select{width:100%;}
  .lang-select select{width:100%; appearance:none; background:var(--card); border:1px solid var(--line); border-radius:8px; padding:8px 10px; font-size:0.82rem; color:var(--ink-soft); cursor:pointer;}

  main{flex:1; padding:32px 40px 60px; max-width:1180px;}
  .topbar{display:flex; justify-content:space-between; align-items:flex-end; margin-bottom:26px; flex-wrap:wrap; gap:16px;}
  .greeting h1{font-size:1.7rem; margin:0 0 4px 0; font-weight:700; color:var(--brown-deep);}
  .greeting p{margin:0; color:var(--ink-soft); font-size:0.9rem;}
  .topbar-actions{display:flex; gap:10px; align-items:center;}
  .period-select select{appearance:none; background:var(--card); border:1px solid var(--line); padding:9px 34px 9px 14px; border-radius:20px; font-size:0.86rem; color:var(--ink); cursor:pointer;}
  .btn{border:none; border-radius:20px; padding:9px 18px; font-size:0.85rem; font-weight:600; cursor:pointer;}
  .btn-primary{background:var(--brown-deep); color:#fff;}
  .btn-primary:hover{background:#2c1c10;}
  .btn-ghost{background:none; border:1px solid var(--line); color:var(--ink-soft); border-radius:20px;}
  .btn-ghost:hover{border-color:var(--gold-dark); color:var(--gold-dark);}
  .btn-gold{background:var(--gold); color:#3D2817;}
  .btn-gold:hover{background:#c99934;}

  .summary-row{display:grid; grid-template-columns:1fr 1fr; gap:14px; margin-bottom:30px;}
  .summary-cell{padding:18px 20px; background:var(--card); border:1px solid var(--line); border-radius:14px;}
  .summary-cell .label{font-size:0.7rem; letter-spacing:0.06em; text-transform:uppercase; color:var(--ink-soft); margin-bottom:8px; font-weight:600;}
  .summary-cell .value{font-size:1.5rem; font-weight:700; font-family:'Fraunces', serif; color:var(--ink);}
  .summary-cell.collected .value, .summary-cell.net .value{color:var(--green);}
  .summary-cell.outstanding .value{color:var(--gold-dark);}
  .summary-cell .delta{font-size:0.74rem; margin-top:6px; color:var(--ink-soft);}
  @media (min-width:640px){ .summary-row{grid-template-columns:repeat(4,1fr);} }

  .eyebrow{font-size:0.72rem; letter-spacing:0.08em; text-transform:uppercase; color:var(--gold-dark); font-weight:700; margin:0 0 6px 0;}
  .section-title{font-size:1.1rem; font-weight:700; color:var(--brown-deep); margin:0 0 4px 0;}
  .section-sub{font-size:0.82rem; color:var(--ink-soft); margin:0 0 16px 0;}

  .chart-grid{display:grid; grid-template-columns:1fr 1.3fr; gap:22px; margin-bottom:36px;}
  .panel{background:var(--card); border:1px solid var(--line); border-radius:14px; padding:22px 24px;}
  .donut-wrap{display:flex; align-items:center; gap:22px;}
  .legend{list-style:none; margin:0; padding:0; font-size:0.84rem; flex:1;}
  .legend li{display:flex; align-items:center; justify-content:space-between; padding:6px 0; border-bottom:1px dotted var(--line); cursor:pointer;}
  .legend li:last-child{border-bottom:none;}
  .legend li:hover{color:var(--gold-dark);}
  .legend .sw{width:9px; height:9px; border-radius:50%; display:inline-block; margin-right:8px;}
  .legend .name{display:flex; align-items:center; flex:1;}
  .legend .pct{font-weight:600; color:var(--ink);}
  .trend-legend{display:flex; gap:16px; font-size:0.78rem; color:var(--ink-soft); margin-bottom:6px;}
  .trend-legend span{display:flex; align-items:center; gap:6px;}
  .trend-legend .sw{width:9px;height:9px;border-radius:2px;display:inline-block;}
  #trendChart{width:100%; height:190px; display:block;}

  .insight-list{display:grid; grid-template-columns:repeat(3,1fr); gap:14px; margin-bottom:36px;}
  .insight-card{background:var(--green-tint); border-left:3px solid var(--green); padding:14px 16px; border-radius:10px; font-size:0.85rem; line-height:1.45;}
  .insight-card.warn{background:#FBF1DE; border-left-color:var(--gold-dark);}
  .insight-card .tag{display:block; font-size:0.68rem; text-transform:uppercase; letter-spacing:0.04em; color:var(--ink-soft); margin-bottom:4px;}

  .recent-head{display:flex; justify-content:space-between; align-items:baseline; margin-bottom:12px;}
  .view-all{font-size:0.84rem; color:var(--gold-dark); text-decoration:none; font-weight:600; cursor:pointer;}
  table{width:100%; border-collapse:collapse; font-size:0.87rem;}
  thead th{text-align:left; font-weight:600; color:var(--ink-soft); font-size:0.72rem; text-transform:uppercase; letter-spacing:0.04em; padding:0 4px 10px 4px; border-bottom:1px solid var(--line);}
  tbody td{padding:13px 4px; border-bottom:1px solid var(--line); vertical-align:middle;}
  tbody tr:last-child td{border-bottom:none;}
  .cat-pill{display:inline-block; padding:3px 10px; border-radius:20px; font-size:0.72rem; font-weight:600;}
  .amt{text-align:right; font-weight:700; font-family:'Fraunces',serif;}
  .desc-main{font-weight:600; color:var(--ink);}
  .desc-sub{display:block; font-size:0.75rem; color:var(--ink-soft); margin-top:2px;}
  .row-actions{display:flex; gap:8px; justify-content:flex-end; align-items:center;}
  .icon-btn{background:none; border:none; cursor:pointer; color:var(--ink-soft); font-size:0.78rem; padding:2px 6px; border-radius:4px;}
  .icon-btn:hover{color:var(--gold-dark); background:var(--bg);}
  .icon-btn.danger:hover{color:var(--maroon); background:var(--maroon-tint);}
  .receipt-chip{font-size:0.72rem; color:var(--gold-dark); cursor:pointer; text-decoration:underline;}
  .status-pill{display:inline-block; padding:2px 9px; border-radius:20px; font-size:0.7rem; font-weight:700;}
  .status-paid{background:var(--green-tint); color:var(--green);}
  .status-outstanding{background:#FBF1DE; color:var(--gold-dark);}

  .view{display:none;}
  .view.active{display:block;}

  .filter-bar{display:flex; gap:10px; margin-bottom:18px; flex-wrap:wrap;}
  .filter-bar input, .filter-bar select{border:1px solid var(--line); background:var(--card); border-radius:20px; padding:8px 14px; font-size:0.85rem; font-family:'IBM Plex Sans';}
  .filter-bar input[type="text"]{flex:1; min-width:160px;}

  .card-grid{display:grid; grid-template-columns:repeat(auto-fill, minmax(240px,1fr)); gap:16px;}
  .entity-card{background:var(--card); border:1px solid var(--line); border-radius:14px; padding:18px;}
  .entity-card h3{margin:0 0 6px 0; font-size:1.02rem; color:var(--brown-deep);}
  .entity-card .meta{font-size:0.8rem; color:var(--ink-soft); margin-bottom:10px; line-height:1.5;}
  .entity-card .stat-row{display:flex; justify-content:space-between; font-size:0.82rem; padding:6px 0; border-top:1px dotted var(--line);}
  .entity-card .stat-row span:last-child{font-weight:700; color:var(--ink);}
  .entity-card .stat-row.profit span:last-child{color:var(--green);}
  .stock-dot{display:inline-block; width:8px; height:8px; border-radius:50%; margin-right:6px;}
  .stock-green{background:var(--green);} .stock-amber{background:var(--gold-dark);} .stock-red{background:var(--maroon);}

  .empty-state{text-align:center; padding:50px 20px; color:var(--ink-soft);}
  .empty-state .big{font-family:'Fraunces', serif; font-size:1.2rem; color:var(--brown-deep); margin-bottom:6px;}

  .overlay{position:fixed; inset:0; background:rgba(61,40,23,0.45); display:none; align-items:flex-start; justify-content:center; padding:60px 20px; z-index:50; overflow-y:auto;}
  .overlay.active{display:flex;}
  .modal{background:var(--card); border-radius:16px; width:100%; max-width:480px; padding:28px 28px 24px; box-shadow:0 20px 50px rgba(61,40,23,0.25);}
  .modal.wide{max-width:640px;}
  .modal h2{font-size:1.25rem; margin:0 0 4px 0; color:var(--brown-deep);}
  .modal .modal-sub{font-size:0.82rem; color:var(--ink-soft); margin:0 0 20px 0;}
  .field{margin-bottom:14px;}
  .field label{display:block; font-size:0.8rem; font-weight:600; color:var(--ink); margin-bottom:5px;}
  .field .req{color:var(--maroon); font-weight:400;}
  .field input, .field select, .field textarea{width:100%; border:1px solid var(--line); background:var(--bg); border-radius:8px; padding:9px 12px; font-size:0.88rem; font-family:'IBM Plex Sans'; color:var(--ink);}
  .field textarea{resize:vertical; min-height:56px;}
  .field-row{display:grid; grid-template-columns:1fr 1fr; gap:12px;}
  .modal-actions{display:flex; justify-content:flex-end; gap:10px; margin-top:22px;}
  .form-error{color:var(--maroon); font-size:0.78rem; margin-top:-8px; margin-bottom:12px; display:none;}
  .form-error.active{display:block;}

  .toast{position:fixed; bottom:24px; right:24px; background:var(--brown-deep); color:#fff; padding:12px 18px; border-radius:10px; font-size:0.86rem; box-shadow:0 8px 24px rgba(0,0,0,0.25); opacity:0; transform:translateY(8px); transition:all 0.25s ease; z-index:60;}
  .toast.show{opacity:1; transform:translateY(0);}

  .receipt-upload{border:1.5px dashed var(--line); border-radius:10px; padding:14px; text-align:center; background:var(--bg);}
  .receipt-upload input[type="file"]{width:100%; font-size:0.8rem;}
  .receipt-status{font-size:0.78rem; color:var(--ink-soft); margin-top:8px; min-height:1.1em;}
  .receipt-status.scanning{color:var(--gold-dark);}
  .receipt-status.done{color:var(--green);}
  .receipt-preview-img{max-width:100%; max-height:160px; border-radius:8px; margin-top:10px; display:none; border:1px solid var(--line);}
  .ocr-banner{background:#FBF1DE; border-left:3px solid var(--gold-dark); padding:10px 12px; border-radius:8px; font-size:0.8rem; margin-bottom:14px; display:none;}
  .ocr-banner.active{display:block;}

  .settings-grid{display:grid; grid-template-columns:1fr 1fr; gap:28px;}
  .settings-panel{background:var(--card); border:1px solid var(--line); border-radius:14px; padding:22px 24px; margin-bottom:22px;}
  .settings-panel h3{margin:0 0 4px 0; font-size:1rem; color:var(--brown-deep);}
  .settings-panel .sub{font-size:0.8rem; color:var(--ink-soft); margin:0 0 16px 0;}
  .logo-row{display:flex; align-items:center; gap:14px; margin-bottom:16px;}
  .logo-preview{width:56px; height:56px; border-radius:50%; background:var(--bg); display:flex; align-items:center; justify-content:center; overflow:hidden; flex-shrink:0; font-family:'Fraunces',serif; font-weight:700; color:var(--brown-deep); font-size:1.3rem; border:1px solid var(--line);}
  .logo-preview img{width:100%; height:100%; object-fit:cover;}
  .category-manager{display:flex; flex-direction:column; gap:8px; margin-bottom:14px;}
  .category-row{display:flex; align-items:center; justify-content:space-between; padding:8px 10px; border:1px solid var(--line); border-radius:8px; font-size:0.85rem;}
  .category-row .name{display:flex; align-items:center; gap:8px;}
  .category-add-row{display:flex; gap:8px;}
  .category-add-row input{flex:1;}

  .pl-panel{background:var(--card); border:1px solid var(--line); border-radius:14px; padding:28px 32px; max-width:560px;}
  .pl-block{margin-bottom:22px;}
  .pl-block h4{margin:0 0 10px 0; font-size:0.8rem; text-transform:uppercase; letter-spacing:0.04em; color:var(--ink-soft); font-weight:700;}
  .pl-row{display:flex; justify-content:space-between; padding:7px 0; font-size:0.92rem; border-bottom:1px dotted var(--line);}
  .pl-row.total{border-bottom:none; border-top:1px solid var(--line); margin-top:6px; padding-top:12px; font-weight:700; font-family:'Fraunces',serif; font-size:1.1rem;}
  .pl-result{background:var(--green-tint); border-radius:12px; padding:18px 20px; margin-top:10px;}
  .pl-result .pl-row{border:none; font-size:1rem;}
  .pl-result .pl-row.profit{font-family:'Fraunces',serif; font-weight:700; font-size:1.35rem; color:var(--green); padding-top:10px;}

  @media (max-width: 900px){
    .app{flex-direction:column;}
    .sidebar{width:100%; height:auto; position:relative; flex-direction:row; align-items:center; padding:14px 16px; overflow-x:auto;}
    .brand{border-bottom:none; padding:0 16px 0 0; margin-bottom:0;}
    nav{display:flex; padding:0; gap:2px;}
    nav a{white-space:nowrap; padding:8px 10px;}
    .sidebar-foot{padding:0 0 0 8px; flex-direction:row;}
    .add-expense-btn{white-space:nowrap; padding:10px 14px;}
    .lang-select{width:140px;}
    main{padding:22px 16px 40px;}
    .chart-grid{grid-template-columns:1fr;}
    .insight-list{grid-template-columns:1fr;}
    table{font-size:0.8rem;}
    thead th:nth-child(3), tbody td:nth-child(3){display:none;}
    .field-row{grid-template-columns:1fr;}
    .settings-grid{grid-template-columns:1fr;}
  }
</style>
</head>
<body>
<div id="bootLoader" style="position:fixed;inset:0;background:#F3EAD9;display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:999;transition:opacity 0.25s ease;">
  <div style="width:34px;height:34px;border:3px solid #E6D8BE;border-top-color:#DDAA45;border-radius:50%;animation:spin 0.8s linear infinite;"></div>
  <div style="margin-top:14px;font-family:sans-serif;font-size:0.85rem;color:#7A6650;">Loading Glory's dashboard…</div>
</div>
<style>@keyframes spin{to{transform:rotate(360deg);}}</style>
<div class="app">

  <aside class="sidebar">
    <div class="brand">
      <div class="brand-logo" id="brandLogo">G</div>
      <div class="brand-text">
        <div class="mark" id="brandName">Glory's</div>
        <div class="sub" data-i18n="sidebar_sub">Business Dashboard</div>
      </div>
    </div>
    <nav id="mainNav">
      <a data-view="dashboard" class="active"><span class="dot"></span><span data-i18n="nav_dashboard">Dashboard</span></a>
      <a data-view="expenses"><span class="dot"></span><span data-i18n="nav_expenses">Expenses</span></a>
      <a data-view="revenue"><span class="dot"></span><span data-i18n="nav_income">Income</span></a>
      <a data-view="jobs"><span class="dot"></span><span data-i18n="nav_jobs">Jobs</span></a>
      <a data-view="suppliers"><span class="dot"></span><span data-i18n="nav_suppliers">Suppliers</span></a>
      <a data-view="inventory"><span class="dot"></span><span data-i18n="nav_inventory">Inventory</span></a>
      <a data-view="reports"><span class="dot"></span><span data-i18n="nav_reports">Reports</span></a>
      <a data-view="settings"><span class="dot"></span><span data-i18n="nav_settings">Settings</span></a>
    </nav>
    <div class="sidebar-foot">
      <div class="lang-select">
        <select id="langPicker">
          <option value="en">English</option>
          <option value="nl">Nederlands</option>
          <option value="de">Deutsch</option>
          <option value="es">Español</option>
          <option value="fr">Français</option>
          <option value="pt">Português</option>
          <option value="ko">한국어</option>
          <option value="zh">中文</option>
        </select>
      </div>
      <button class="add-expense-btn" id="quickAddExpense" data-i18n="btn_add_expense">+ Add Expense</button>
    </div>
  </aside>

  <main>
    <!-- DASHBOARD -->
    <section class="view active" id="view-dashboard">
      <div class="topbar">
        <div class="greeting">
          <h1 id="dashGreeting">Good morning, Glory's</h1>
          <p id="periodLabel" data-i18n="period_label_all">Here's how the business is doing across all time.</p>
        </div>
        <div class="topbar-actions">
          <div class="period-select">
            <select id="quickCurrencyPicker"></select>
          </div>
          <div class="period-select">
            <select id="periodPicker">
              <option value="today" data-i18n="period_today">Today</option>
              <option value="week" data-i18n="period_week">This week</option>
              <option value="month" data-i18n="period_month">This month</option>
              <option value="all" selected data-i18n="period_all">All time</option>
            </select>
          </div>
        </div>
      </div>

      <div class="summary-row" id="summaryRow"></div>

      <div class="chart-grid">
        <div class="panel">
          <div class="section-title" data-i18n="section_spending_category">Spending by category</div>
          <div class="donut-wrap">
            <svg id="donut" width="140" height="140" viewBox="0 0 150 150"></svg>
            <ul class="legend" id="legend"></ul>
          </div>
        </div>
        <div class="panel">
          <div class="section-title" data-i18n="section_last_six_months">Last six months</div>
          <div class="trend-legend">
            <span><span class="sw" style="background:var(--maroon);"></span><span data-i18n="legend_spent">Spent</span></span>
            <span><span class="sw" style="background:var(--green);"></span><span data-i18n="legend_collected">Collected</span></span>
          </div>
          <svg id="trendChart" viewBox="0 0 560 190" preserveAspectRatio="none"></svg>
        </div>
      </div>

      <div class="insights">
        <div class="section-title" data-i18n="section_insights">Financial insights</div>
        <div class="section-sub" data-i18n="section_insights_sub">Based on what's been recorded so far</div>
        <div class="insight-list" id="insightList"></div>
      </div>

      <div class="recent">
        <div class="recent-head">
          <div class="section-title" style="margin-bottom:0;" data-i18n="section_recent_expenses">Recent expenses</div>
          <a class="view-all"
