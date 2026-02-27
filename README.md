# justysherin-j-7981.github.io

<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Restaurant POS</title>
  <style>
    :root{
      --bg:#0b1220;
      --muted:#aab3c5;
      --text:#e9eefc;
      --border:rgba(255,255,255,.10);
      --good:#5dd6c0;
      --warn:#ffd166;
      --bad:#ff6b6b;
    }

    .pos-layout{display:grid;grid-template-columns:240px 1fr;gap:12px;align-items:start;margin-top:12px}
    @media (max-width:900px){.pos-layout{grid-template-columns:1fr}}

    .catbox{position:sticky;top:86px;max-height:calc(100vh - 120px);overflow:auto;padding:10px;border-radius:12px;border:1px solid var(--border);background:rgba(0,0,0,.18)}
    .catbar{display:flex;flex-direction:column;gap:8px}
    @media (max-width:900px){
      .catbox{position:static;max-height:none}
      .catbar{flex-direction:row;flex-wrap:nowrap;overflow:auto;gap:10px;padding-bottom:6px;scroll-snap-type:x mandatory}
      .catbtn{flex:0 0 auto;min-width:160px;scroll-snap-align:start}
    }
    .catbtn{text-align:left;border:1px solid var(--border);background:rgba(255,255,255,.04);color:var(--text);padding:10px 10px;border-radius:12px;cursor:pointer;font-weight:900;font-size:12px}
    .catbtn.active{background:rgba(93,214,192,.18);border-color:rgba(93,214,192,.35)}
    .catbtn .sub{display:block;color:var(--muted);font-weight:700;font-size:11px;margin-top:2px}

    *{box-sizing:border-box}
    body{margin:0;font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif;background:linear-gradient(180deg,#070b14,var(--bg));color:var(--text)}
    header{
      padding:14px 16px;border-bottom:1px solid var(--border);
      position:sticky;top:0;background:rgba(7,11,20,.9);backdrop-filter:blur(8px);
      z-index:10;display:flex;align-items:center;justify-content:space-between;gap:10px;flex-wrap:wrap;
    }
    header h1{margin:0;font-size:16px}
    header .sub{color:var(--muted);font-size:12px;margin-top:4px}
    .tabs{display:flex;gap:8px;flex-wrap:wrap}
    .tabbtn{border:1px solid var(--border);background:rgba(255,255,255,.04);color:var(--text);padding:9px 12px;border-radius:999px;cursor:pointer;font-weight:800;font-size:12px}
    .tabbtn.active{background:rgba(93,214,192,.18);border-color:rgba(93,214,192,.35)}
    main{max-width:1280px;margin:0 auto;padding:16px}
    .grid{display:grid;gap:16px;grid-template-columns:1.25fr .75fr}
    @media (max-width:980px){.grid{grid-template-columns:1fr}}
    .card{background:linear-gradient(180deg, rgba(255,255,255,.04), rgba(255,255,255,.02));border:1px solid var(--border);border-radius:14px;padding:14px}
    .card h2{margin:0 0 10px;font-size:14px;color:var(--muted);font-weight:700}
    .row{display:flex;gap:10px;flex-wrap:wrap}
    .row > *{flex:1;min-width:170px}
    label{display:block;font-size:12px;color:var(--muted);margin:6px 0}
    input,select,textarea{width:100%;border-radius:10px;border:1px solid var(--border);background:rgba(0,0,0,.25);color:var(--text);padding:10px 10px;outline:none}
    textarea{min-height:92px;resize:vertical}
    .pill{display:inline-block;padding:6px 10px;border:1px solid var(--border);border-radius:999px;color:var(--muted);font-size:12px}
    .actions{display:flex;gap:10px;flex-wrap:wrap;margin-top:12px}
    .btn{border:0;border-radius:12px;padding:12px 12px;font-weight:900;cursor:pointer;flex:1;min-width:160px}
    .btn.primary{background:rgba(93,214,192,.25);color:var(--text)}
    .btn.secondary{background:rgba(255,255,255,.06);color:var(--text)}
    .btn.danger{background:rgba(255,107,107,.18);color:var(--text)}
    details{border:1px solid var(--border);border-radius:12px;padding:10px;background:rgba(255,255,255,.02)}
    details > summary{cursor:pointer;color:var(--text);font-weight:900}
    .hint{margin-top:8px;color:var(--muted);font-size:12px;line-height:1.4}
    pre{white-space:pre-wrap;background:rgba(0,0,0,.25);padding:10px;border-radius:10px;border:1px solid var(--border);overflow:auto;max-height:340px}

    .menu-grid{display:grid;gap:10px;grid-template-columns:repeat(4, minmax(0,1fr))}
    @media (max-width:980px){.menu-grid{grid-template-columns:repeat(3, minmax(0,1fr))}}
    @media (max-width:720px){.menu-grid{grid-template-columns:repeat(2, minmax(0,1fr))}}
    @media (max-width:420px){.menu-grid{grid-template-columns:1fr}}
    .item{padding:10px;border-radius:12px;border:1px solid var(--border);background:rgba(255,255,255,.03);display:flex;flex-direction:column;gap:8px;overflow:hidden;opacity:1;transition:opacity .15s ease}
    .item.disabled{opacity:.45;filter:grayscale(.35)}
    .thumb{width:100%;aspect-ratio:4/3;border-radius:10px;border:1px solid var(--border);overflow:hidden;background:rgba(255,255,255,.04);cursor:pointer;display:flex;align-items:center;justify-content:center;position:relative}
    .thumb img{width:100%;height:100%;object-fit:cover;display:block;user-select:none;-webkit-user-drag:none}

    /* Removed IMAGE/IMG overlay label */
    .thumb .editBadge{display:none !important;}

    .badge{display:inline-block;padding:4px 8px;border-radius:999px;border:1px solid var(--border);font-size:11px;color:var(--muted);background:rgba(0,0,0,.20);font-weight:900}
    .badge.good{border-color:rgba(93,214,192,.35);color:var(--text);background:rgba(93,214,192,.12)}
    .badge.bad{border-color:rgba(255,107,107,.35);color:var(--text);background:rgba(255,107,107,.12)}
    .item .name{font-weight:900;line-height:1.2}
    .item .meta{display:flex;justify-content:space-between;align-items:center;color:var(--muted);font-size:12px;gap:8px}
    .item .addBtn{border:0;border-radius:10px;padding:10px;background:rgba(93,214,192,.16);color:var(--text);font-weight:900;cursor:pointer}
    .item.disabled .addBtn{cursor:not-allowed;opacity:.5}
    .item .addBtn[disabled]{cursor:not-allowed;opacity:.5}

    table{width:100%;border-collapse:collapse}
    th,td{padding:10px 6px;border-bottom:1px solid var(--border);text-align:left}
    th{color:var(--muted);font-size:12px;font-weight:700}
    .right{text-align:right}

    /* Qty in horizontal: - [box] + */
    .qtyCtl{display:flex;align-items:center;gap:6px;flex-wrap:nowrap}
    .qtyCtl button{width:30px;height:30px;border-radius:8px;border:1px solid var(--border);background:rgba(255,255,255,.04);color:var(--text);cursor:pointer;font-weight:900}
    .qtyCtl input{width:64px;min-width:auto;padding:8px 8px;border-radius:10px;text-align:center}

    /* BILL summary: total on right + print under it */
    .billBottom{
      margin-top:10px;
      display:grid;
      grid-template-columns: 1fr auto;
      gap:12px;
      align-items:end;
    }
    .billTotalBox{
      border:1px solid rgba(93,214,192,.35);
      background:rgba(93,214,192,.08);
      border-radius:14px;
      padding:12px;
      min-width:220px;
      justify-self:end;
    }
    .billTotalLabel{color:var(--muted);font-size:12px;font-weight:800}
    .billTotalValue{font-size:22px;font-weight:1000;margin-top:6px;text-align:right}
    .billTotalActions{margin-top:10px;display:grid;gap:10px}
    .btn.block{min-width:auto;width:100%}

    /* dashboard/report charts */
    .kpis{display:grid;gap:10px;grid-template-columns:repeat(4, minmax(0,1fr))}
    @media (max-width:980px){.kpis{grid-template-columns:repeat(2, minmax(0,1fr))}}
    .kpi{border:1px solid var(--border);border-radius:14px;padding:12px;background:rgba(255,255,255,.03)}
    .kpi .label{color:var(--muted);font-size:12px}
    .kpi .value{font-weight:1000;font-size:18px;margin-top:6px}
    .kpi .sub{color:var(--muted);font-size:12px;margin-top:6px}
    .chart{height:160px;border:1px solid var(--border);border-radius:14px;background:rgba(0,0,0,.18);padding:10px;display:flex;align-items:flex-end;gap:6px;overflow:hidden}
    .bar{flex:1;min-width:8px;background:rgba(93,214,192,.35);border:1px solid rgba(93,214,192,.35);border-radius:10px 10px 6px 6px;position:relative}
    .bar.bad{background:rgba(255,107,107,.25);border-color:rgba(255,107,107,.35)}
    .bar.neutral{background:rgba(255,255,255,.10);border-color:rgba(255,255,255,.16)}
    .bar .tip{
      position:absolute;left:50%;transform:translateX(-50%);bottom:100%;margin-bottom:6px;
      background:rgba(0,0,0,.65);border:1px solid rgba(255,255,255,.18);
      padding:4px 6px;border-radius:10px;font-size:11px;color:var(--text);white-space:nowrap;
      opacity:0;pointer-events:none
    }
    .bar:hover .tip{opacity:1}
    .ranklist{display:grid;gap:8px;margin-top:8px}
    .rankrow{display:flex;justify-content:space-between;gap:10px;padding:10px;border:1px solid var(--border);border-radius:12px;background:rgba(255,255,255,.03)}
    .rankrow .name{font-weight:900}
    .rankrow .meta{color:var(--muted);font-size:12px;margin-top:4px}
    .rankrow .amt{font-weight:1000}

    .hidden{display:none !important}

    .sticky-save{
      position:fixed;left:12px;right:12px;bottom:12px;
      background:rgba(7,11,20,.92);
      border:1px solid var(--border);
      border-radius:14px;
      padding:10px;
      display:flex;gap:10px;align-items:center;justify-content:space-between;
      backdrop-filter:blur(8px);
      z-index:99;
    }
    .sticky-save .msg{color:var(--muted);font-size:12px}

    .split{display:grid;gap:12px;grid-template-columns:1fr 1fr;align-items:start}
    @media (max-width:980px){.split{grid-template-columns:1fr}}

    @media print{
      header, .no-print { display:none !important; }
      body{background:#fff;color:#000}
      .card{border:none;background:#fff}
      .print-receipt{display:block !important}
      .print-receipt *{color:#000}
    }
    .print-receipt{display:none}
    .receipt{max-width:360px;margin:0 auto;font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace;font-size:12px}
    .receipt .center{text-align:center}
    .receipt hr{border:0;border-top:1px dashed #999;margin:8px 0}
    .receipt table{width:100%;border-collapse:collapse}
    .receipt td{padding:2px 0;vertical-align:top}
    .receipt .bold{font-weight:700}
  </style>
</head>
<body>
<header class="no-print">
  <div>
    <h1>Restaurant POS</h1>
    <div class="sub">Full Settings restored + Reports + Stock + Dashboard</div>
  </div>
  <div class="tabs" role="tablist" aria-label="tabs">
    <button class="tabbtn active" data-tab="pos">POS</button>
    <button class="tabbtn" data-tab="dashboard">Dashboard</button>
    <button class="tabbtn" data-tab="stock">Stock</button>
    <button class="tabbtn" data-tab="reports">Reports</button>
    <button class="tabbtn" data-tab="settings">Settings</button>
  </div>
</header>

<main>
  <!-- POS TAB -->
  <section id="tab-pos" class="tab no-print">
    <div class="grid">
      <section class="card">
        <h2>MENU</h2>

        <div class="row">
          <div>
            <label for="restaurantName">Restaurant Name</label>
            <input id="restaurantName" value="My Restaurant" />
          </div>
          <div>
            <label for="sessionSelect">Session</label>
            <select id="sessionSelect">
              <option value="TIFFIN">Tiffen</option>
              <option value="LUNCH">Lunch</option>
              <option value="DINNER">Dinner</option>
            </select>
          </div>
          <div>
            <label for="searchBox">Search</label>
            <input id="searchBox" placeholder="Search items..." />
          </div>
        </div>

        <div class="row" style="margin-top:8px">
          <div class="pill">Tap item to add</div>
          <div class="pill">Qty can be typed</div>
        </div>

        <div class="pos-layout">
          <aside class="catbox">
            <div class="pill" style="margin-bottom:10px">Categories</div>
            <nav class="catbar" id="categorySidebar"></nav>
          </aside>

          <section>
            <div class="menu-grid" id="menuGrid"></div>
          </section>
        </div>
      </section>

      <aside class="card">
        <h2>BILL</h2>

        <div class="row">
          <div>
            <label for="customerNote">Customer / Note</label>
            <input id="customerNote" placeholder="Table no / name" />
          </div>
          <div>
            <label for="discountAmount">Discount (₹)</label>
            <input id="discountAmount" type="number" min="0" step="0.01" value="0" />
          </div>
        </div>

        <!-- Tax % and Service % moved to Settings; hidden from BILL -->

        <div class="row">
          <div>
            <label for="paymentMethod">Payment Method</label>
            <select id="paymentMethod">
              <option value="CASH">Cash</option>
              <option value="UPI">UPI</option>
              <option value="SPLIT">Cash + UPI</option>
            </select>
          </div>
          <div>
            <label for="upiRef">UPI Ref / Txn ID (optional)</label>
            <input id="upiRef" placeholder="Only if needed" />
          </div>
        </div>

        <div style="margin-top:10px; overflow:auto">
          <table>
            <thead>
              <tr>
                <th style="width:44%">Item</th>
                <th style="width:30%">Qty</th>
                <th style="width:13%">Rate</th>
                <th style="width:13%" class="right">Amt</th>
              </tr>
            </thead>
            <tbody id="cartBody"></tbody>
          </table>
        </div>

        <div class="billBottom">
          <div style="display:grid;gap:8px">
            <div class="pill" style="display:flex;justify-content:space-between"><span style="color:var(--muted)">Subtotal</span><span id="subtotalText">₹0.00</span></div>
            <div class="pill" style="display:flex;justify-content:space-between"><span style="color:var(--muted)">Service</span><span id="serviceText">₹0.00</span></div>
            <div class="pill" style="display:flex;justify-content:space-between"><span style="color:var(--muted)">Tax</span><span id="taxText">₹0.00</span></div>
            <div class="pill" style="display:flex;justify-content:space-between"><span style="color:var(--muted)">Discount</span><span id="discountText">- ₹0.00</span></div>
          </div>

          <div class="billTotalBox">
            <div class="billTotalLabel">TOTAL</div>
            <div class="billTotalValue" id="grandText">₹0.00</div>
            <div class="billTotalActions">
              <button class="btn primary block" id="printBtn" type="button">Save & Print</button>
              <button class="btn secondary block" id="newBillBtn" type="button">New Bill</button>
              <button class="btn danger block" id="clearBtn" type="button">Clear Cart</button>
            </div>
          </div>
        </div>
      </aside>
    </div>
  </section>

  <!-- DASHBOARD TAB -->
  <section id="tab-dashboard" class="tab no-print hidden">
    <div class="card">
      <h2>Sales Dashboard</h2>

      <div class="row">
        <div>
          <label for="dashRange">Range</label>
          <select id="dashRange">
            <option value="TODAY">Today</option>
            <option value="LAST_7">Last 7 days</option>
            <option value="THIS_MONTH">This month</option>
            <option value="ALL">All time</option>
          </select>
        </div>
        <div>
          <label>&nbsp;</label>
          <button class="btn secondary" id="dashRefreshBtn" type="button">Refresh</button>
        </div>
        <div>
          <label>&nbsp;</label>
          <button class="btn danger" id="dashClearBtn" type="button">Clear Sales Data</button>
        </div>
      </div>

      <div class="kpis" style="margin-top:12px">
        <div class="kpi"><div class="label">Gross Sales</div><div class="value" id="kpiGross">₹0.00</div><div class="sub" id="kpiBills">Bills: 0</div></div>
        <div class="kpi"><div class="label">Cost (Expense)</div><div class="value" id="kpiCost">₹0.00</div><div class="sub">Item cost total</div></div>
        <div class="kpi"><div class="label">Profit</div><div class="value" id="kpiProfit">₹0.00</div><div class="sub">Sales - Cost</div></div>
        <div class="kpi"><div class="label">Avg Bill</div><div class="value" id="kpiAvg">₹0.00</div><div class="sub" id="kpiRangeLabel">Range</div></div>
      </div>

      <div class="row" style="margin-top:12px">
        <div class="card" style="flex:1;min-width:280px">
          <h2>Sales chart (by day)</h2>
          <div class="chart" id="salesChart"></div>
        </div>
        <div class="card" style="flex:1;min-width:280px">
          <h2>Profit chart (by day)</h2>
          <div class="chart" id="profitChart"></div>
        </div>
      </div>

      <div class="row" style="margin-top:12px">
        <div class="card" style="flex:1;min-width:280px">
          <h2>By Category (Sales/Cost/Profit)</h2>
          <div class="hint">Top 12 categories by profit</div>
          <div class="chart" id="categoryProfitChart"></div>
          <div class="ranklist" id="categoryRank"></div>
        </div>
        <div class="card" style="flex:1;min-width:280px">
          <h2>By Item (Top Profit)</h2>
          <div class="hint">Top 12 items by profit</div>
          <div class="chart" id="itemProfitChart"></div>
          <div class="ranklist" id="itemRank"></div>
        </div>
      </div>

      <details style="margin-top:12px">
        <summary>Raw dashboard data (JSON)</summary>
        <pre id="dashRaw"></pre>
      </details>
    </div>
  </section>

  <!-- STOCK TAB -->
  <section id="tab-stock" class="tab no-print hidden">
    <div class="card">
      <h2>Stock & Daily Expenses (Grocery inventory)</h2>
      <div class="hint">
        Add grocery items, record purchases (expenses). Menu items will be enabled/disabled based on recipe + available stock.
        Stock is reduced on <b>Save & Print</b>.
      </div>

      <div class="split" style="margin-top:12px">
        <div class="card">
          <h2>Add Stock Item</h2>
          <div class="row">
            <div>
              <label for="stockName">Item name</label>
              <input id="stockName" placeholder="e.g., Egg / Rice / Oil" />
            </div>
            <div>
              <label for="stockUnit">Unit</label>
              <select id="stockUnit">
                <option value="pcs">pcs</option>
                <option value="kg">kg</option>
                <option value="g">g</option>
                <option value="ltr">ltr</option>
                <option value="ml">ml</option>
                <option value="pack">pack</option>
              </select>
            </div>
          </div>
          <div class="row">
            <div>
              <label for="stockMin">Min level (optional)</label>
              <input id="stockMin" type="number" step="0.01" placeholder="e.g., 5" />
            </div>
            <div>
              <label>&nbsp;</label>
              <button class="btn secondary" id="addStockItemBtn" type="button">Add Stock Item</button>
            </div>
          </div>
          <div class="hint" id="stockAddStatus"></div>
        </div>

        <div class="card">
          <h2>Record Purchase (Daily Expense)</h2>
          <div class="row">
            <div>
              <label for="purchaseItem">Stock item</label>
              <select id="purchaseItem"></select>
            </div>
            <div>
              <label for="purchaseQty">Qty added</label>
              <input id="purchaseQty" type="number" step="0.01" />
            </div>
          </div>
          <div class="row">
            <div>
              <label for="purchaseCost">Cost (₹)</label>
              <input id="purchaseCost" type="number" step="0.01" />
            </div>
            <div>
              <label for="purchaseNote">Note</label>
              <input id="purchaseNote" placeholder="Supplier / remark" />
            </div>
          </div>
          <div class="actions">
            <button class="btn secondary" id="addPurchaseBtn" type="button">Add Purchase</button>
            <button class="btn danger" id="clearStockMovesBtn" type="button">Clear Stock History</button>
          </div>
          <div class="hint" id="purchaseStatus"></div>
        </div>
      </div>

      <div class="card" style="margin-top:12px">
        <h2>On-hand Stock</h2>
        <div class="hint">Shows current on-hand quantity (Purchases - Usage).</div>
        <div style="overflow:auto">
          <table>
            <thead>
              <tr>
                <th>Item</th>
                <th>Unit</th>
                <th class="right">On hand</th>
                <th class="right">Min</th>
              </tr>
            </thead>
            <tbody id="stockTableBody"></tbody>
          </table>
        </div>
      </div>

      <div class="card" style="margin-top:12px">
        <h2>Today Expenses</h2>
        <div class="pill" id="todayExpenseText">₹0.00</div>
        <div class="hint">Based on purchases recorded today.</div>
      </div>
    </div>
  </section>

  <!-- REPORTS TAB -->
  <section id="tab-reports" class="tab no-print hidden">
    <div class="card">
      <h2>Business Analyst Reports</h2>
      <div class="hint">
        Generates full stats for <b>Sales</b>, <b>Items</b>, <b>Categories</b>, and <b>Stock + Expenses</b> for Daily/Weekly/Monthly/Custom ranges.
      </div>

      <div class="row" style="margin-top:12px">
        <div>
          <label for="repPreset">Report</label>
          <select id="repPreset">
            <option value="DAILY">Daily (Today)</option>
            <option value="WEEKLY">Weekly (Last 7 days)</option>
            <option value="MONTHLY">Monthly (This month)</option>
            <option value="CUSTOM">Custom Range</option>
            <option value="ALL">All time</option>
          </select>
        </div>
        <div>
          <label for="repFrom">From</label>
          <input id="repFrom" type="date" />
        </div>
        <div>
          <label for="repTo">To</label>
          <input id="repTo" type="date" />
        </div>
        <div>
          <label>&nbsp;</label>
          <button class="btn secondary" id="repRunBtn" type="button">Generate</button>
        </div>
      </div>

      <div class="actions">
        <button class="btn secondary" id="repExportSalesCsvBtn" type="button">Export Sales Summary CSV</button>
        <button class="btn secondary" id="repExportItemsCsvBtn" type="button">Export Item Stats CSV</button>
        <button class="btn secondary" id="repExportStockCsvBtn" type="button">Export Stock/Expense CSV</button>
      </div>

      <div class="kpis" style="margin-top:12px">
        <div class="kpi"><div class="label">Gross Sales</div><div class="value" id="repGross">₹0.00</div><div class="sub" id="repBills">Bills: 0</div></div>
        <div class="kpi"><div class="label">COGS (Item Cost)</div><div class="value" id="repCost">₹0.00</div><div class="sub">Sum of item costs sold</div></div>
        <div class="kpi"><div class="label">Profit</div><div class="value" id="repProfit">₹0.00</div><div class="sub">Gross - COGS</div></div>
        <div class="kpi"><div class="label">Purchases / Expenses</div><div class="value" id="repExpense">₹0.00</div><div class="sub">Stock purchases in range</div></div>
      </div>

      <div class="row" style="margin-top:12px">
        <div class="card" style="flex:1;min-width:280px">
          <h2>Sales by day</h2>
          <div class="chart" id="repSalesByDayChart"></div>
        </div>
        <div class="card" style="flex:1;min-width:280px">
          <h2>Expenses by day</h2>
          <div class="chart" id="repExpenseByDayChart"></div>
        </div>
      </div>

      <div class="row" style="margin-top:12px">
        <div class="card" style="flex:1;min-width:280px">
          <h2>Top Items (Qty / Sales / Profit)</h2>
          <div class="ranklist" id="repItemRank"></div>
        </div>
        <div class="card" style="flex:1;min-width:280px">
          <h2>Top Categories (Profit)</h2>
          <div class="ranklist" id="repCategoryRank"></div>
        </div>
      </div>

      <details style="margin-top:12px">
        <summary>Full report JSON</summary>
        <pre id="repRaw"></pre>
      </details>
    </div>
  </section>

  <!-- SETTINGS TAB -->
  <section id="tab-settings" class="tab no-print hidden">
    <div class="card">
      <h2>Settings (Edit then Save All)</h2>
      <div class="hint">
        Changes are applied in the app immediately, but will be saved permanently only when you click <b>Save All Changes</b>.
      </div>

      <details open>
        <summary>Categories (create separately)</summary>
        <div class="hint">Add categories for each session. These categories will be used in “Add New Item” dropdown.</div>

        <div class="row" style="margin-top:10px">
          <div>
            <label for="catSession">Session</label>
            <select id="catSession">
              <option value="TIFFIN">Tiffen</option>
              <option value="LUNCH">Lunch</option>
              <option value="DINNER">Dinner</option>
            </select>
          </div>
          <div>
            <label for="catName">New Category Name</label>
            <input id="catName" placeholder="e.g., South Indian" />
          </div>
          <div>
            <label>&nbsp;</label>
            <button class="btn secondary" id="addCategoryBtn" type="button">Add Category (Apply)</button>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="catListSession">View Categories (Session)</label>
            <select id="catListSession">
              <option value="TIFFIN">Tiffen</option>
              <option value="LUNCH">Lunch</option>
              <option value="DINNER">Dinner</option>
            </select>
          </div>
          <div>
            <label for="catList">Categories</label>
            <select id="catList" size="6"></select>
          </div>
          <div>
            <label>&nbsp;</label>
            <button class="btn danger" id="deleteCategoryBtn" type="button">Delete Category (Apply)</button>
          </div>
        </div>

        <div class="hint" id="catStatus"></div>
      </details>

      <details style="margin-top:12px">
        <summary>Recipes (Ingredient consumption per menu item)</summary>
        <div class="hint">
          Connect grocery stock items to each menu item. Stock is reduced on Save & Print.
          If recipe is empty, POS will assume <b>unlimited</b> stock for that menu item.
        </div>

        <div class="row" style="margin-top:10px">
          <div>
            <label for="recipeSession">Session</label>
            <select id="recipeSession">
              <option value="TIFFIN">Tiffen</option>
              <option value="LUNCH">Lunch</option>
              <option value="DINNER">Dinner</option>
            </select>
          </div>
          <div>
            <label for="recipeCategory">Category</label>
            <select id="recipeCategory"></select>
          </div>
          <div>
            <label for="recipeItem">Menu Item</label>
            <select id="recipeItem"></select>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="recipeStockItem">Stock Item</label>
            <select id="recipeStockItem"></select>
          </div>
          <div>
            <label for="recipeQtyPerUnit">Qty per 1 menu item</label>
            <input id="recipeQtyPerUnit" type="number" step="0.01" placeholder="e.g., Egg=1, Oil=0.01" />
          </div>
          <div>
            <label>&nbsp;</label>
            <button class="btn secondary" id="addRecipeLineBtn" type="button">Add Ingredient</button>
          </div>
        </div>

        <div style="overflow:auto;margin-top:10px">
          <table>
            <thead>
              <tr><th>Stock item</th><th class="right">Qty / item</th><th></th></tr>
            </thead>
            <tbody id="recipeLinesBody"></tbody>
          </table>
        </div>

        <div class="actions">
          <button class="btn danger" id="clearRecipeBtn" type="button">Clear Recipe (Apply)</button>
        </div>

        <div class="hint" id="recipeStatus"></div>
      </details>

      <!-- NEW: BILL CHARGES (Tax/Service) -->
      <details style="margin-top:12px" open>
        <summary>Bill Charges (Tax / Service)</summary>
        <div class="hint">These values are used in BILL automatically.</div>
        <div class="row" style="margin-top:10px">
          <div>
            <label for="settingsTaxPercent">Tax %</label>
            <input id="settingsTaxPercent" type="number" min="0" step="0.01" value="0" />
          </div>
          <div>
            <label for="settingsServicePercent">Service %</label>
            <input id="settingsServicePercent" type="number" min="0" step="0.01" value="0" />
          </div>
        </div>
        <div class="actions">
          <button class="btn secondary" id="saveChargesBtn" type="button">Save Charges</button>
        </div>
        <div class="hint" id="chargesStatus"></div>
      </details>

      <details style="margin-top:12px">
        <summary>Price & Cost (Dropdown)</summary>

        <div class="row" style="margin-top:10px">
          <div>
            <label for="priceSession">Session</label>
            <select id="priceSession">
              <option value="TIFFIN">Tiffen</option>
              <option value="LUNCH">Lunch</option>
              <option value="DINNER">Dinner</option>
            </select>
          </div>
          <div>
            <label for="priceCategory">Category</label>
            <select id="priceCategory"></select>
          </div>
          <div>
            <label for="priceItem">Item</label>
            <select id="priceItem"></select>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="priceValue">Selling Price (₹)</label>
            <input id="priceValue" type="number" min="0" step="0.01" />
          </div>
          <div>
            <label for="costValue">Item Cost/Expense (₹)</label>
            <input id="costValue" type="number" min="0" step="0.01" />
          </div>
          <div>
            <label>&nbsp;</label>
            <button class="btn secondary" id="applyPriceBtn" type="button">Apply</button>
          </div>
        </div>

        <div class="hint" id="priceStatus">Apply updates, then Save All.</div>
      </details>

      <details style="margin-top:12px">
        <summary>Edit Item Name</summary>
        <div class="hint">Rename an item (keeps same ID).</div>

        <div class="row" style="margin-top:10px">
          <div>
            <label for="editSession">Session</label>
            <select id="editSession">
              <option value="TIFFIN">Tiffen</option>
              <option value="LUNCH">Lunch</option>
              <option value="DINNER">Dinner</option>
            </select>
          </div>
          <div>
            <label for="editCategory">Category</label>
            <select id="editCategory"></select>
          </div>
          <div>
            <label for="editItem">Item</label>
            <select id="editItem"></select>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="editNewName">New Item Name</label>
            <input id="editNewName" placeholder="Enter new name" />
          </div>
          <div>
            <label>&nbsp;</label>
            <button class="btn secondary" id="applyRenameBtn" type="button">Rename (Apply)</button>
          </div>
        </div>

        <div class="hint" id="renameStatus"></div>
      </details>

      <details style="margin-top:12px">
        <summary>Update Item Image (Gallery/Camera / URL)</summary>
        <div class="hint">
          Upload = offline. URL Apply = online. Download URL = offline (may fail if site blocks download).
        </div>

        <div class="row" style="margin-top:10px">
          <div>
            <label for="imgSession">Session</label>
            <select id="imgSession">
              <option value="TIFFIN">Tiffen</option>
              <option value="LUNCH">Lunch</option>
              <option value="DINNER">Dinner</option>
            </select>
          </div>
          <div>
            <label for="imgCategory">Category</label>
            <select id="imgCategory"></select>
          </div>
          <div>
            <label for="imgItem">Item</label>
            <select id="imgItem"></select>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="imgFile">Choose image (offline)</label>
            <input id="imgFile" type="file" accept="image/*" capture="environment" />
          </div>
          <div>
            <label for="imgUrl">Image URL</label>
            <input id="imgUrl" placeholder="https://...jpg / png / webp" />
            <div class="hint">Tip: Use a direct image URL (ends with .jpg/.png/.webp). Many Google links are not direct.</div>
          </div>
        </div>

        <div class="actions">
          <button class="btn secondary" id="applyImgUrlBtn" type="button">Apply URL (online)</button>
          <button class="btn secondary" id="downloadImgUrlBtn" type="button">Download URL (offline)</button>
          <button class="btn secondary" id="clearImgBtn" type="button">Remove Image</button>
        </div>

        <div class="hint" id="imgStatus"></div>
      </details>

      <details style="margin-top:12px">
        <summary>Delete Items (Permanent)</summary>

        <div class="row" style="margin-top:10px">
          <div>
            <label for="delSession">Session</label>
            <select id="delSession">
              <option value="TIFFIN">Tiffen</option>
              <option value="LUNCH">Lunch</option>
              <option value="DINNER">Dinner</option>
            </select>
          </div>
          <div>
            <label for="delCategory">Category</label>
            <select id="delCategory"></select>
          </div>
          <div>
            <label for="delItem">Item</label>
            <select id="delItem"></select>
          </div>
        </div>

        <div class="actions">
          <button class="btn danger" id="deleteItemBtn" type="button">Delete Permanently (Apply)</button>
        </div>

        <div class="hint" id="deleteStatus"></div>
      </details>

      <details style="margin-top:12px">
        <summary>Daily Items</summary>
        <div class="hint">Daily items selection + mode are saved immediately.</div>

        <div class="row" style="margin-top:10px">
          <div>
            <label for="dailySession">Session</label>
            <select id="dailySession">
              <option value="TIFFIN">Tiffen</option>
              <option value="LUNCH">Lunch</option>
              <option value="DINNER">Dinner</option>
            </select>
          </div>
          <div>
            <label for="dailyItems">Select Items</label>
            <select id="dailyItems" multiple size="12"></select>
          </div>
          <div>
            <label for="dailyMode">Daily mode</label>
            <select id="dailyMode">
              <option value="OFF">Show all items</option>
              <option value="ON">Show only daily items</option>
            </select>

            <div class="actions">
              <button class="btn secondary" id="saveDailyBtn" type="button">Save Daily Items</button>
            </div>

            <div class="hint" id="dailyStatus"></div>
          </div>
        </div>
      </details>

      <details style="margin-top:12px">
        <summary>Add New Menu Item</summary>

        <div class="row" style="margin-top:10px">
          <div>
            <label for="newSession">Session</label>
            <select id="newSession">
              <option value="TIFFIN">Tiffen</option>
              <option value="LUNCH">Lunch</option>
              <option value="DINNER">Dinner</option>
            </select>
          </div>
          <div>
            <label for="newCategorySel">Category</label>
            <select id="newCategorySel"></select>
          </div>
          <div>
            <label for="newName">Item Name</label>
            <input id="newName" placeholder="e.g., Idly" />
          </div>
        </div>

        <div class="row">
          <div>
            <label for="newPrice">Selling Price (₹)</label>
            <input id="newPrice" type="number" min="0" step="0.01" />
          </div>
          <div>
            <label for="newCost">Item Cost/Expense (₹)</label>
            <input id="newCost" type="number" min="0" step="0.01" />
          </div>
          <div>
            <label for="newImageFile">Image (optional)</label>
            <input id="newImageFile" type="file" accept="image/*" />
          </div>
        </div>

        <div class="actions">
          <button class="btn secondary" id="addItemBtn" type="button">Add Item (Apply)</button>
        </div>

        <div class="hint" id="addItemStatus"></div>
      </details>

      <details style="margin-top:12px">
        <summary>Export CSV (Excel)</summary>
        <div class="actions">
          <button class="btn secondary" id="exportMenuCsvBtn" type="button">Export Menu CSV</button>
        </div>
      </details>
    </div>
  </section>

  <section class="print-receipt">
    <div class="receipt" id="receipt"></div>
  </section>
</main>

<div class="sticky-save no-print hidden" id="saveBar">
  <div class="msg" id="saveMsg">You have unsaved menu changes.</div>
  <div style="display:flex;gap:10px">
    <button class="btn secondary" id="discardBtn" type="button" style="min-width:auto">Discard</button>
    <button class="btn primary" id="saveAllBtn" type="button" style="min-width:auto">Save All Changes</button>
  </div>
</div>

<script>
/* =======================
   STORAGE + BASE DATA
======================= */
const KEYS = {
  sales: "rb_sales_v6",
  menu: "rb_menu_v6",
  daily: "rb_daily_v1",
  categories: "rb_categories_v1",

  stockItems: "rb_stock_items_v1",
  stockMoves: "rb_stock_moves_v1",
  recipes: "rb_recipes_v1",

  charges: "rb_charges_v1" // tax/service defaults
};

function loadCharges(){
  try{
    const raw = JSON.parse(localStorage.getItem(KEYS.charges) || "{}");
    return {
      taxPercent: Math.max(0, Number(raw?.taxPercent || 0)),
      servicePercent: Math.max(0, Number(raw?.servicePercent || 0))
    };
  }catch{
    return { taxPercent: 0, servicePercent: 0 };
  }
}
function saveCharges(obj){
  localStorage.setItem(KEYS.charges, JSON.stringify({
    taxPercent: Math.max(0, Number(obj?.taxPercent || 0)),
    servicePercent: Math.max(0, Number(obj?.servicePercent || 0))
  }));
}
let CHARGES = loadCharges();

const DEFAULT_MENU = [
  ...["Idly","Poori","Pongal","Kall Dhosa","Nice Dosa","Masala Dosa","Onion Dosa","Egg Dosa","Parota","Chapathi","Vada"]
    .map(n => ({ session:"TIFFIN", category:"South Indian", name:n, price:0, cost:0, image:"" })),
  ...["Veg Meals","Non Veg Meals","Chicken Biriyani","Egg Briyani","Kuska","Parota"]
    .map(n => ({ session:"LUNCH", category:"South Indian", name:n, price:0, cost:0, image:"" })),
  ...["Fish Fry","Fish Curry"]
    .map(n => ({ session:"LUNCH", category:"Fish", name:n, price:0, cost:0, image:"" })),
  ...["Veg Rice","Chicken Rice","Egg Rice","Panner Rice","Mashrom Rice","Gobi Rice","Chilly Chicken","Gobi Manjurian"]
    .map(n => ({ session:"LUNCH", category:"Chinees", name:n, price:0, cost:0, image:"" })),
  ...["Idly","Chapathi","Kall Dhosa","Nice Dosa","Masala Dosa","Onion Dosa","Egg Dosa","Parota","Chicken Kothu parota","Egg Kothu Parota"]
    .map(n => ({ session:"DINNER", category:"South Indian", name:n, price:0, cost:0, image:"" })),
  ...["Chicken Chettinadu","Veg Rice","Chicken Rice","Egg Rice","Panner Rice","Mashrom Rice","Gobi Rice"]
    .map(n => ({ session:"DINNER", category:"Chinees", name:n, price:0, cost:0, image:"" })),
];

function uid(prefix="id"){ return prefix + "_" + Math.random().toString(16).slice(2) + "_" + Date.now().toString(16); }
function slug(s){ return String(s).toLowerCase().replaceAll(/[^a-z0-9]+/g,"-").replaceAll(/^-+|-+$/g,""); }
function itemId(it){ return `${it.session}__${slug(it.category)}__${slug(it.name)}`; }

function normalizeMenuItem(x){
  return {
    id: x.id || itemId(x),
    session: String(x.session||"TIFFIN").toUpperCase(),
    category: String(x.category||"General").trim(),
    name: String(x.name||"").trim(),
    price: Number(x.price||0),
    cost: Number(x.cost||0),
    image: String(x.image||"").trim(),
    active: x.active === false ? false : true
  };
}
function loadMenu(){
  try{
    const saved = localStorage.getItem(KEYS.menu);
    const raw = saved ? JSON.parse(saved) : structuredClone(DEFAULT_MENU);
    const cleaned = (Array.isArray(raw)?raw:[]).map(normalizeMenuItem).filter(x=>x.name);
    return cleaned.length ? cleaned : structuredClone(DEFAULT_MENU).map(normalizeMenuItem);
  }catch{
    return structuredClone(DEFAULT_MENU).map(normalizeMenuItem);
  }
}
function saveMenu(menu){ localStorage.setItem(KEYS.menu, JSON.stringify(menu)); }
let MENU_ITEMS = loadMenu();
let MENU_DRAFT = structuredClone(MENU_ITEMS);

let activeCategory = "ALL";
let HAS_UNSAVED = false;

function loadDaily(){ try{ return JSON.parse(localStorage.getItem(KEYS.daily) || "{}") || {}; } catch{ return {}; } }
function saveDaily(obj){ localStorage.setItem(KEYS.daily, JSON.stringify(obj)); }
let DAILY = loadDaily();

/* Categories */
function inferCategoriesFromMenu(menu){
  const out = { TIFFIN:[], LUNCH:[], DINNER:[] };
  for (const it of menu){
    out[it.session] = out[it.session] || [];
    if (!out[it.session].includes(it.category)) out[it.session].push(it.category);
  }
  for (const s of Object.keys(out)) out[s].sort((a,b)=>a.localeCompare(b));
  return out;
}
function loadCategories(){
  try{
    const saved = localStorage.getItem(KEYS.categories);
    if (saved){
      const parsed = JSON.parse(saved);
      if (parsed && typeof parsed === "object") return parsed;
    }
  }catch{}
  const inferred = inferCategoriesFromMenu(MENU_DRAFT);
  localStorage.setItem(KEYS.categories, JSON.stringify(inferred));
  return inferred;
}
function saveCategories(obj){ localStorage.setItem(KEYS.categories, JSON.stringify(obj)); }
let CATEGORIES = loadCategories();

/* =======================
   STOCK + RECIPES
======================= */
function normalizeStockItem(x){
  return { id: String(x.id || uid("stk")), name: String(x.name||"").trim(), unit: String(x.unit||"pcs"), minLevel: Number(x.minLevel||0) };
}
function loadStockItems(){
  try{ return (JSON.parse(localStorage.getItem(KEYS.stockItems)||"[]")||[]).map(normalizeStockItem).filter(x=>x.name); }catch{ return []; }
}
function saveStockItems(items){ localStorage.setItem(KEYS.stockItems, JSON.stringify(items)); }
let STOCK_ITEMS = loadStockItems();

function normalizeStockMove(x){
  return {
    id: String(x.id || uid("mov")),
    whenISO: String(x.whenISO || new Date().toISOString()),
    itemId: String(x.itemId||""),
    type: String(x.type||"PURCHASE"),
    qty: Number(x.qty||0),
    cost: Number(x.cost||0),
    note: String(x.note||"")
  };
}
function loadStockMoves(){
  try{ return (JSON.parse(localStorage.getItem(KEYS.stockMoves)||"[]")||[]).map(normalizeStockMove).filter(x=>x.itemId); }catch{ return []; }
}
function saveStockMoves(moves){ localStorage.setItem(KEYS.stockMoves, JSON.stringify(moves)); }
let STOCK_MOVES = loadStockMoves();

function loadRecipes(){
  try{
    const raw = JSON.parse(localStorage.getItem(KEYS.recipes) || "{}");
    if (!raw || typeof raw !== "object") return {};
    const out = {};
    for (const [k,v] of Object.entries(raw)){
      out[k] = (Array.isArray(v)?v:[])
        .map(x=>({ stockItemId:String(x.stockItemId||""), qtyPerUnit:Number(x.qtyPerUnit||0) }))
        .filter(x=>x.stockItemId && Number.isFinite(x.qtyPerUnit) && x.qtyPerUnit>0);
    }
    return out;
  }catch{ return {}; }
}
function saveRecipes(obj){ localStorage.setItem(KEYS.recipes, JSON.stringify(obj)); }
let RECIPES = loadRecipes();

function onHand(stockItemId){
  let sum=0;
  for (const m of STOCK_MOVES){
    if (m.itemId !== stockItemId) continue;
    if (m.type === "PURCHASE" || m.type === "ADJUST_ADD") sum += Number(m.qty||0);
    if (m.type === "USAGE" || m.type === "ADJUST_SUB") sum -= Number(m.qty||0);
  }
  return sum;
}
function menuItemAvailability(menuItemId){
  const lines = RECIPES[menuItemId] || [];
  if (!lines.length) return { available: Infinity };

  let available = Infinity;
  for (const ln of lines){
    const have = onHand(ln.stockItemId);
    const canMake = Math.floor(have / Number(ln.qtyPerUnit||1));
    available = Math.min(available, canMake);
  }
  return { available: Math.max(0, Number.isFinite(available)?available:0) };
}
function validateCartStockOrThrow(cart){
  const required = new Map();
  for (const cartIt of cart.values()){
    const recipe = RECIPES[cartIt.id] || [];
    for (const ln of recipe){
      const need = Number(ln.qtyPerUnit||0) * Number(cartIt.qty||0);
      required.set(ln.stockItemId, (required.get(ln.stockItemId)||0) + need);
    }
  }
  const errors=[];
  for (const [stockItemId, need] of required.entries()){
    const have = onHand(stockItemId);
    if (need > have + 1e-9){
      const si = STOCK_ITEMS.find(x=>x.id===stockItemId);
      errors.push(`${si?.name || stockItemId}: need ${need.toFixed(2)} but have ${have.toFixed(2)}`);
    }
  }
  if (errors.length) throw new Error("Stock not enough:\n" + errors.join("\n"));
}
function applyCartStockUsageMoves({cart, invoiceNo, whenISO}){
  const required = new Map();
  for (const cartIt of cart.values()){
    const recipe = RECIPES[cartIt.id] || [];
    for (const ln of recipe){
      const need = Number(ln.qtyPerUnit||0) * Number(cartIt.qty||0);
      required.set(ln.stockItemId, (required.get(ln.stockItemId)||0) + need);
    }
  }
  for (const [stockItemId, need] of required.entries()){
    if (!need) continue;
    STOCK_MOVES.push(normalizeStockMove({ whenISO, itemId: stockItemId, type:"USAGE", qty: need, cost: 0, note:`Used for invoice ${invoiceNo}` }));
  }
  saveStockMoves(STOCK_MOVES);
}

/* =======================
   DOM
======================= */
const $$ = (id)=>document.getElementById(id);
const els = {
  tabButtons: Array.from(document.querySelectorAll(".tabbtn")),
  tabs: {
    pos: $$("tab-pos"),
    dashboard: $$("tab-dashboard"),
    stock: $$("tab-stock"),
    reports: $$("tab-reports"),
    settings: $$("tab-settings")
  },

  restaurantName: $$("restaurantName"),
  sessionSelect: $$("sessionSelect"),
  searchBox: $$("searchBox"),
  categorySidebar: $$("categorySidebar"),
  menuGrid: $$("menuGrid"),

  cartBody: $$("cartBody"),
  customerNote: $$("customerNote"),
  discountAmount: $$("discountAmount"),

  paymentMethod: $$("paymentMethod"),
  upiRef: $$("upiRef"),

  subtotalText: $$("subtotalText"),
  serviceText: $$("serviceText"),
  taxText: $$("taxText"),
  discountText: $$("discountText"),
  grandText: $$("grandText"),

  printBtn: $$("printBtn"),
  newBillBtn: $$("newBillBtn"),
  clearBtn: $$("clearBtn"),

  receipt: $$("receipt"),

  // settings: charges
  settingsTaxPercent: $$("settingsTaxPercent"),
  settingsServicePercent: $$("settingsServicePercent"),
  saveChargesBtn: $$("saveChargesBtn"),
  chargesStatus: $$("chargesStatus"),

  // dashboard
  dashRange: $$("dashRange"),
  dashRefreshBtn: $$("dashRefreshBtn"),
  dashClearBtn: $$("dashClearBtn"),
  kpiGross: $$("kpiGross"),
  kpiCost: $$("kpiCost"),
  kpiProfit: $$("kpiProfit"),
  kpiAvg: $$("kpiAvg"),
  kpiBills: $$("kpiBills"),
  kpiRangeLabel: $$("kpiRangeLabel"),
  salesChart: $$("salesChart"),
  profitChart: $$("profitChart"),
  categoryProfitChart: $$("categoryProfitChart"),
  itemProfitChart: $$("itemProfitChart"),
  categoryRank: $$("categoryRank"),
  itemRank: $$("itemRank"),
  dashRaw: $$("dashRaw"),

  // stock
  stockName: $$("stockName"),
  stockUnit: $$("stockUnit"),
  stockMin: $$("stockMin"),
  addStockItemBtn: $$("addStockItemBtn"),
  stockAddStatus: $$("stockAddStatus"),

  purchaseItem: $$("purchaseItem"),
  purchaseQty: $$("purchaseQty"),
  purchaseCost: $$("purchaseCost"),
  purchaseNote: $$("purchaseNote"),
  addPurchaseBtn: $$("addPurchaseBtn"),
  clearStockMovesBtn: $$("clearStockMovesBtn"),
  purchaseStatus: $$("purchaseStatus"),

  stockTableBody: $$("stockTableBody"),
  todayExpenseText: $$("todayExpenseText"),

  // reports
  repPreset: $$("repPreset"),
  repFrom: $$("repFrom"),
  repTo: $$("repTo"),
  repRunBtn: $$("repRunBtn"),
  repExportSalesCsvBtn: $$("repExportSalesCsvBtn"),
  repExportItemsCsvBtn: $$("repExportItemsCsvBtn"),
  repExportStockCsvBtn: $$("repExportStockCsvBtn"),
  repGross: $$("repGross"),
  repBills: $$("repBills"),
  repCost: $$("repCost"),
  repProfit: $$("repProfit"),
  repExpense: $$("repExpense"),
  repSalesByDayChart: $$("repSalesByDayChart"),
  repExpenseByDayChart: $$("repExpenseByDayChart"),
  repItemRank: $$("repItemRank"),
  repCategoryRank: $$("repCategoryRank"),
  repRaw: $$("repRaw"),

  // settings: categories
  catSession: $$("catSession"),
  catName: $$("catName"),
  addCategoryBtn: $$("addCategoryBtn"),
  catListSession: $$("catListSession"),
  catList: $$("catList"),
  deleteCategoryBtn: $$("deleteCategoryBtn"),
  catStatus: $$("catStatus"),

  // settings: recipe
  recipeSession: $$("recipeSession"),
  recipeCategory: $$("recipeCategory"),
  recipeItem: $$("recipeItem"),
  recipeStockItem: $$("recipeStockItem"),
  recipeQtyPerUnit: $$("recipeQtyPerUnit"),
  addRecipeLineBtn: $$("addRecipeLineBtn"),
  recipeLinesBody: $$("recipeLinesBody"),
  clearRecipeBtn: $$("clearRecipeBtn"),
  recipeStatus: $$("recipeStatus"),

  // settings: price/cost
  priceSession: $$("priceSession"),
  priceCategory: $$("priceCategory"),
  priceItem: $$("priceItem"),
  priceValue: $$("priceValue"),
  costValue: $$("costValue"),
  applyPriceBtn: $$("applyPriceBtn"),
  priceStatus: $$("priceStatus"),

  // settings: rename
  editSession: $$("editSession"),
  editCategory: $$("editCategory"),
  editItem: $$("editItem"),
  editNewName: $$("editNewName"),
  applyRenameBtn: $$("applyRenameBtn"),
  renameStatus: $$("renameStatus"),

  // settings: image
  imgSession: $$("imgSession"),
  imgCategory: $$("imgCategory"),
  imgItem: $$("imgItem"),
  imgFile: $$("imgFile"),
  imgUrl: $$("imgUrl"),
  applyImgUrlBtn: $$("applyImgUrlBtn"),
  downloadImgUrlBtn: $$("downloadImgUrlBtn"),
  clearImgBtn: $$("clearImgBtn"),
  imgStatus: $$("imgStatus"),

  // settings: delete
  delSession: $$("delSession"),
  delCategory: $$("delCategory"),
  delItem: $$("delItem"),
  deleteItemBtn: $$("deleteItemBtn"),
  deleteStatus: $$("deleteStatus"),

  // settings: daily items
  dailySession: $$("dailySession"),
  dailyItems: $$("dailyItems"),
  dailyMode: $$("dailyMode"),
  saveDailyBtn: $$("saveDailyBtn"),
  dailyStatus: $$("dailyStatus"),

  // settings: add item
  newSession: $$("newSession"),
  newCategorySel: $$("newCategorySel"),
  newName: $$("newName"),
  newPrice: $$("newPrice"),
  newCost: $$("newCost"),
  newImageFile: $$("newImageFile"),
  addItemBtn: $$("addItemBtn"),
  addItemStatus: $$("addItemStatus"),

  // settings: export
  exportMenuCsvBtn: $$("exportMenuCsvBtn"),

  // save bar
  saveBar: $$("saveBar"),
  saveMsg: $$("saveMsg"),
  saveAllBtn: $$("saveAllBtn"),
  discardBtn: $$("discardBtn"),
};

const cart = new Map();

/* =======================
   UTIL
======================= */
const money = (n) => "₹" + (Number(n || 0)).toFixed(2);
function escapeHtml(s){
  return String(s).replaceAll("&","&amp;").replaceAll("<","&lt;").replaceAll(">","&gt;").replaceAll('"',"&quot;").replaceAll("'","&#039;");
}
function startOfDay(d){ const x=new Date(d); x.setHours(0,0,0,0); return x; }
function endOfDay(d){ const x=new Date(d); x.setHours(23,59,59,999); return x; }
function isoDate(d){
  const y=d.getFullYear();
  const m=String(d.getMonth()+1).padStart(2,"0");
  const day=String(d.getDate()).padStart(2,"0");
  return `${y}-${m}-${day}`;
}
function parseDateInput(val){
  if (!val) return null;
  const [y,m,d] = val.split("-").map(Number);
  if (!y||!m||!d) return null;
  return new Date(y, m-1, d, 0,0,0,0);
}
function toCsv(rows, header){
  const esc = (v)=>{
    const s=String(v??"");
    return /[",\n]/.test(s) ? `"${s.replaceAll('"','""')}"` : s;
  };
  const lines=[header.map(esc).join(",")];
  for(const r of rows) lines.push(header.map(h=>esc(r[h])).join(","));
  return lines.join("\n");
}
function downloadText(filename, content, mime){
  const blob = new Blob([content], { type: mime || "text/plain" });
  const a = document.createElement("a");
  a.href = URL.createObjectURL(blob);
  a.download = filename;
  a.click();
  URL.revokeObjectURL(a.href);
}
function fileToDataUrl(file, maxSize=900, quality=0.85){
  return new Promise((resolve, reject) => {
    const fr = new FileReader();
    fr.onload = () => {
      const img = new Image();
      img.onload = () => {
        const scale = Math.min(1, maxSize / Math.max(img.width, img.height));
        const w = Math.round(img.width * scale);
        const h = Math.round(img.height * scale);
        const canvas = document.createElement("canvas");
        canvas.width = w; canvas.height = h;
        canvas.getContext("2d").drawImage(img, 0, 0, w, h);
        resolve(canvas.toDataURL("image/jpeg", quality));
      };
      img.onerror = reject;
      img.src = fr.result;
    };
    fr.onerror = reject;
    fr.readAsDataURL(file);
  });
}

/* =======================
   SAVE BAR
======================= */
function markDirty(message){
  HAS_UNSAVED = true;
  els.saveMsg.textContent = message || "You have unsaved menu changes.";
  els.saveBar.classList.remove("hidden");
}
function clearDirty(){
  HAS_UNSAVED = false;
  els.saveBar.classList.add("hidden");
}
function saveAllChanges(){
  MENU_ITEMS = structuredClone(MENU_DRAFT);
  saveMenu(MENU_ITEMS);
  saveCategories(CATEGORIES);
  saveRecipes(RECIPES);

  clearDirty();
  alert("Saved all menu/category/recipe changes.");
  activeCategory="ALL";
  rebuildCategorySidebar();
  renderMenu();
  refreshSettingsDropdownsAll();
}
function discardChanges(){
  if (!confirm("Discard all unsaved menu changes?")) return;
  MENU_DRAFT = structuredClone(MENU_ITEMS);
  CATEGORIES = loadCategories();
  RECIPES = loadRecipes();
  clearDirty();
  activeCategory="ALL";
  rebuildCategorySidebar();
  renderMenu();
  refreshSettingsDropdownsAll();
}

/* =======================
   BILL CHARGES (Tax/Service)
======================= */
function applyChargesToBillingUI(){
  // No visible inputs in BILL, but we refresh totals.
  recalc();
}
function refreshChargesSettingsUI(){
  els.settingsTaxPercent.value = String(CHARGES.taxPercent ?? 0);
  els.settingsServicePercent.value = String(CHARGES.servicePercent ?? 0);
  els.chargesStatus.textContent = `Current: Tax ${Number(CHARGES.taxPercent||0).toFixed(2)}%, Service ${Number(CHARGES.servicePercent||0).toFixed(2)}%`;
}
function saveChargesFromSettings(){
  const tax = Math.max(0, Number(els.settingsTaxPercent.value||0));
  const service = Math.max(0, Number(els.settingsServicePercent.value||0));
  CHARGES = { taxPercent: tax, servicePercent: service };
  saveCharges(CHARGES);
  els.chargesStatus.textContent = "Saved. Billing totals updated.";
  applyChargesToBillingUI();
}

/* =======================
   MENU FILTER + DAILY
======================= */
function currentSession(){ return els.sessionSelect.value; }
function shouldShowOnlyDaily(session){ return (DAILY?.mode || {})[session] === "ON"; }
function dailySetFor(session){ return new Set((DAILY?.items || {})[session] || []); }

function baseVisibleMenuItems(){
  const session = currentSession();
  let items = MENU_DRAFT.filter(x => x.active !== false && x.session === session);
  if (shouldShowOnlyDaily(session)){
    const set = dailySetFor(session);
    items = items.filter(x => set.has(x.id));
  }
  return items;
}

function rebuildCategorySidebar(){
  const items = baseVisibleMenuItems();
  const cats = Array.from(new Set(items.map(x => x.category || "General"))).sort();
  if (activeCategory !== "ALL" && !cats.includes(activeCategory)) activeCategory="ALL";

  const allCats = ["ALL", ...cats];
  els.categorySidebar.innerHTML = allCats.map(c => {
    const label = (c === "ALL") ? "All Categories" : c;
    const count = (c === "ALL") ? items.length : items.filter(x=>x.category===c).length;
    const active = (c === activeCategory) ? "active" : "";
    return `
      <button class="catbtn ${active}" type="button" data-cat="${escapeHtml(c)}">
        ${escapeHtml(label)}
        <span class="sub">${count} items</span>
      </button>
    `;
  }).join("");

  els.categorySidebar.querySelectorAll("[data-cat]").forEach(btn=>{
    btn.addEventListener("click", ()=>{
      activeCategory = btn.getAttribute("data-cat");
      rebuildCategorySidebar();
      renderMenu();
    });
  });
}

function renderMenu(){
  const q = (els.searchBox.value || "").trim().toLowerCase();
  let items = baseVisibleMenuItems();
  if (activeCategory !== "ALL") items = items.filter(x=>x.category===activeCategory);
  if (q) items = items.filter(x=>x.name.toLowerCase().includes(q));

  els.menuGrid.innerHTML = items.map(it=>{
    const avail = menuItemAvailability(it.id).available;
    const isUnlimited = avail === Infinity;
    const isOut = !isUnlimited && avail <= 0;

    const stockBadge = isUnlimited
      ? `<span class="badge">Stock: ∞</span>`
      : (isOut ? `<span class="badge bad">Out of stock</span>` : `<span class="badge good">Stock: ${avail}</span>`);

    const img = it.image
      ? `<img src="${escapeHtml(it.image)}" alt="${escapeHtml(it.name)}" loading="lazy" />`
      : `<div style="color:var(--muted);font-size:12px;padding:10px;text-align:center">No Image</div>`;

    return `
      <div class="item ${isOut ? "disabled" : ""}">
        <div class="thumb" ${isOut ? "" : `data-add="${escapeHtml(it.id)}"`}>${img}</div>
        <div class="name">${escapeHtml(it.name)}</div>
        <div class="meta"><span>${escapeHtml(it.category)}</span>${stockBadge}</div>
        <div class="meta"><span>${money(it.price)}</span><span></span></div>
        <button class="addBtn" ${isOut ? "disabled" : ""} data-add="${escapeHtml(it.id)}" type="button">Add</button>
      </div>
    `;
  }).join("");

  document.querySelectorAll("[data-add]").forEach(el=>{
    el.addEventListener("click", ()=>{
      const id = el.getAttribute("data-add");
      const it = MENU_DRAFT.find(x=>x.id===id);
      if (it) addToCart(it);
    });
  });
}

/* =======================
   CART
======================= */
function addToCart(item){
  const avail = menuItemAvailability(item.id).available;
  if (avail !== Infinity){
    const inCart = cart.get(item.id)?.qty || 0;
    if (inCart + 1 > avail){
      alert(`Stock not enough for "${item.name}". Available: ${avail}`);
      return;
    }
  }

  const ex = cart.get(item.id);
  if (ex) ex.qty += 1;
  else cart.set(item.id, { id:item.id, name:item.name, price:Number(item.price||0), cost:Number(item.cost||0), qty:1 });
  renderCart();
}
function setQty(id, qty){
  const n=Number(qty);
  if (!Number.isFinite(n) || n<=0) cart.delete(id);
  else{
    const avail = menuItemAvailability(id).available;
    if (avail !== Infinity && n > avail){
      alert(`Stock not enough. Available: ${avail}`);
      cart.get(id).qty = avail;
    }else{
      cart.get(id).qty=n;
    }
  }
  renderCart();
}
function renderCart(){
  els.cartBody.innerHTML="";
  if (cart.size===0){
    els.cartBody.innerHTML=`<tr><td colspan="4" style="color:var(--muted);padding:14px 6px;">No items yet.</td></tr>`;
    recalc(); return;
  }
  for (const it of cart.values()){
    const tr=document.createElement("tr");
    tr.innerHTML=`
      <td><div style="font-weight:900">${escapeHtml(it.name)}</div></td>
      <td>
        <div class="qtyCtl">
          <button type="button" data-dec="${escapeHtml(it.id)}">-</button>
          <input type="number" min="1" step="1" value="${it.qty}" data-qty="${escapeHtml(it.id)}" />
          <button type="button" data-inc="${escapeHtml(it.id)}">+</button>
        </div>
      </td>
      <td>${money(it.price)}</td>
      <td class="right">${money(it.price*it.qty)}</td>
    `;
    els.cartBody.appendChild(tr);
  }
  document.querySelectorAll("[data-dec]").forEach(b=>b.addEventListener("click",()=>{
    const id=b.getAttribute("data-dec"); const it=cart.get(id); if(it) setQty(id,it.qty-1);
  }));
  document.querySelectorAll("[data-inc]").forEach(b=>b.addEventListener("click",()=>{
    const id=b.getAttribute("data-inc"); const it=cart.get(id); if(it) setQty(id,it.qty+1);
  }));
  document.querySelectorAll("[data-qty]").forEach(inp=>inp.addEventListener("change",()=>{
    setQty(inp.getAttribute("data-qty"), inp.value);
  }));
  recalc();
}
function totals(){
  let subtotal=0, costTotal=0;
  for (const it of cart.values()){
    subtotal += it.price*it.qty;
    costTotal += it.cost*it.qty;
  }

  // Use saved CHARGES (Settings), not BILL inputs.
  const service = subtotal * (Number(CHARGES.servicePercent||0)/100);
  const tax = (subtotal + service) * (Number(CHARGES.taxPercent||0)/100);

  const discount=Math.max(0, Number(els.discountAmount.value||0));
  const grand=Math.max(0, subtotal+service+tax-discount);
  return { subtotal, service, tax, discount, grand, costTotal, profitEstimate: grand-costTotal };
}
function recalc(){
  const t=totals();
  els.subtotalText.textContent=money(t.subtotal);
  els.serviceText.textContent=money(t.service);
  els.taxText.textContent=money(t.tax);
  els.discountText.textContent="- "+money(t.discount);
  els.grandText.textContent=money(t.grand);
}

/* =======================
   SALES + RECEIPT
======================= */
function nextInvoiceNumber(){
  const key="rb_invoice_counter_A";
  const next=Number(localStorage.getItem(key)||"0")+1;
  localStorage.setItem(key, String(next));
  return `A-${String(next).padStart(6,"0")}`;
}
function loadSalesRaw(){ try{ return JSON.parse(localStorage.getItem(KEYS.sales)||"[]"); } catch { return []; } }
function saveSalesList(list){ localStorage.setItem(KEYS.sales, JSON.stringify(list)); }

function recordSale({invoiceNo, whenISO}){
  const list=loadSalesRaw();
  list.push({
    invoiceNo, whenISO,
    session: currentSession(),
    restaurantName:(els.restaurantName.value||"").trim(),
    note:(els.customerNote.value||"").trim(),
    items:Array.from(cart.values()),
    totals:totals(),
    payment:{ method: els.paymentMethod.value, upiRef:(els.upiRef.value||"").trim() }
  });
  saveSalesList(list);
}
function buildReceiptHTML({invoiceNo, whenISO}){
  const t=totals();
  const restaurant=(els.restaurantName.value||"Restaurant").trim();
  const note=(els.customerNote.value||"").trim();
  const items=Array.from(cart.values());
  return `
    <div class="center bold">${escapeHtml(restaurant)}</div>
    <div class="center">RECEIPT</div>
    <hr/>
    <table>
      <tr><td>Invoice</td><td class="right">${escapeHtml(invoiceNo)}</td></tr>
      <tr><td>Date</td><td class="right">${escapeHtml(new Date(whenISO).toLocaleString())}</td></tr>
      <tr><td>Session</td><td class="right">${escapeHtml(currentSession())}</td></tr>
      ${note ? `<tr><td>Note</td><td class="right">${escapeHtml(note)}</td></tr>` : ""}
      <tr><td>Pay</td><td class="right">${escapeHtml(els.paymentMethod.value)}</td></tr>
    </table>
    <hr/>
    <table>
      <tr class="bold"><td>Item</td><td class="right">Amt</td></tr>
      ${items.map(it=>`<tr><td>${escapeHtml(it.name)} x${it.qty}</td><td class="right">${money(it.price*it.qty)}</td></tr>`).join("")}
    </table>
    <hr/>
    <table>
      <tr><td>Subtotal</td><td class="right">${money(t.subtotal)}</td></tr>
      <tr><td>Service (${Number(CHARGES.servicePercent||0).toFixed(2)}%)</td><td class="right">${money(t.service)}</td></tr>
      <tr><td>Tax (${Number(CHARGES.taxPercent||0).toFixed(2)}%)</td><td class="right">${money(t.tax)}</td></tr>
      <tr><td>Discount</td><td class="right">- ${money(t.discount)}</td></tr>
      <tr class="bold"><td>Total</td><td class="right">${money(t.grand)}</td></tr>
    </table>
    <hr/>
    <div class="center">Thank you!</div>
  `;
}

/* =======================
   DASHBOARD + REPORTS shared helpers
======================= */
function rangeFor(type){
  const now = new Date();
  if (type === "ALL") return { from: new Date(0), to: new Date(8640000000000000), label:"All time" };
  if (type === "TODAY") return { from: startOfDay(now), to: endOfDay(now), label:"Today" };
  if (type === "LAST_7") return { from: startOfDay(new Date(now.getTime()-6*86400000)), to: endOfDay(now), label:"Last 7 days" };
  if (type === "THIS_MONTH"){
    const from = new Date(now.getFullYear(), now.getMonth(), 1, 0,0,0,0);
    const to = new Date(now.getFullYear(), now.getMonth()+1, 0, 23,59,59,999);
    return { from, to, label:"This month" };
  }
  return { from: new Date(0), to: new Date(), label:"All time" };
}
function renderBarChart(container, series, valueKey, labelFn, mode){
  container.innerHTML="";
  const max = Math.max(1, ...series.map(x=>Math.abs(Number(x[valueKey]||0))));
  for (const x of series){
    const v = Number(x[valueKey]||0);
    const pct = Math.max(0, Math.round((Math.abs(v)/max)*100));
    const bar=document.createElement("div");
    bar.className="bar";
    if (mode==="profit" && v<0) bar.classList.add("bad");
    if (mode==="neutral") bar.classList.add("neutral");
    bar.style.height = pct + "%";
    const tip=document.createElement("div");
    tip.className="tip";
    tip.textContent=labelFn(x);
    bar.appendChild(tip);
    container.appendChild(bar);
  }
}
function computeSalesStats({from, to}){
  const sales = loadSalesRaw().filter(s=>{
    const ms = new Date(s.whenISO).getTime();
    return ms >= from.getTime() && ms <= to.getTime();
  });

  let gross=0, cost=0;
  const byDay = new Map();
  const byCategory = new Map();
  const byItem = new Map();

  for (const s of sales){
    const g = Number(s?.totals?.grand||0);
    const c = Number(s?.totals?.costTotal||0);
    gross += g; cost += c;

    const day = isoDate(new Date(s.whenISO));
    const drow = byDay.get(day) || { key: day, gross:0, cost:0, profit:0, bills:0 };
    drow.gross += g; drow.cost += c; drow.profit += (g-c); drow.bills += 1;
    byDay.set(day, drow);

    for (const it of (s.items||[])){
      const menuIt = MENU_DRAFT.find(x=>x.id===it.id);
      const category = menuIt?.category || "(unknown)";
      const session = menuIt?.session || s.session || "(unknown)";
      const catKey = session + " | " + category;

      const cat = byCategory.get(catKey) || { key: catKey, session, category, gross:0, cost:0, profit:0, qty:0 };
      const ig = Number(it.price||0) * Number(it.qty||0);
      const ic = Number(it.cost||0) * Number(it.qty||0);
      cat.gross += ig; cat.cost += ic; cat.profit += (ig-ic); cat.qty += Number(it.qty||0);
      byCategory.set(catKey, cat);

      const row = byItem.get(it.id) || { key: it.id, name: it.name, session, category, qty:0, gross:0, cost:0, profit:0 };
      row.name = it.name;
      row.session = session; row.category = category;
      row.qty += Number(it.qty||0);
      row.gross += ig; row.cost += ic; row.profit += (ig-ic);
      byItem.set(it.id, row);
    }
  }

  return {
    sales,
    totals: { gross, cost, profit: gross-cost, bills: sales.length, avg: sales.length ? gross/sales.length : 0 },
    byDay: Array.from(byDay.values()).sort((a,b)=>a.key.localeCompare(b.key)),
    byCategory: Array.from(byCategory.values()).sort((a,b)=>b.profit-a.profit),
    byItem: Array.from(byItem.values()).sort((a,b)=>b.profit-a.profit)
  };
}
function computeExpenseStats({from, to}){
  const moves = STOCK_MOVES.filter(m=>{
    const ms = new Date(m.whenISO).getTime();
    return ms >= from.getTime() && ms <= to.getTime();
  });
  const purchases = moves.filter(m=>m.type==="PURCHASE");
  const expenseTotal = purchases.reduce((s,m)=>s+Number(m.cost||0),0);

  const byDay = new Map();
  for (const m of purchases){
    const day = isoDate(new Date(m.whenISO));
    byDay.set(day, (byDay.get(day)||0) + Number(m.cost||0));
  }
  const byDayRows = Array.from(byDay.entries()).map(([key, expense])=>({ key, expense }))
    .sort((a,b)=>a.key.localeCompare(b.key));

  return { moves, purchases, expenseTotal, byDay: byDayRows };
}

/* =======================
   DASHBOARD
======================= */
function refreshDashboard(){
  const r = rangeFor(els.dashRange.value);
  const stats = computeSalesStats(r);

  els.kpiGross.textContent = money(stats.totals.gross);
  els.kpiCost.textContent = money(stats.totals.cost);
  els.kpiProfit.textContent = money(stats.totals.profit);
  els.kpiAvg.textContent = money(stats.totals.avg);
  els.kpiBills.textContent = `Bills: ${stats.totals.bills}`;
  els.kpiRangeLabel.textContent = r.label;

  renderBarChart(els.salesChart, stats.byDay.length?stats.byDay:[{key:r.label,gross:0,bills:0}], "gross",
    x=>`${x.key} | Sales ${money(x.gross)} | Bills ${x.bills}`, "neutral");
  renderBarChart(els.profitChart, stats.byDay.length?stats.byDay:[{key:r.label,profit:0,cost:0}], "profit",
    x=>`${x.key} | Profit ${money(x.profit)} | Cost ${money(x.cost)}`, "profit");

  const catRows = stats.byCategory.slice(0,12);
  renderBarChart(els.categoryProfitChart, catRows.length?catRows:[{key:"-",profit:0,cost:0,gross:0,qty:0}], "profit",
    x=>`${x.key} | Profit ${money(x.profit)} | Sales ${money(x.gross)} | Cost ${money(x.cost)} | Qty ${x.qty}`, "profit");
  els.categoryRank.innerHTML = catRows.length ? catRows.map(x=>`
    <div class="rankrow">
      <div>
        <div class="name">${escapeHtml(x.key)}</div>
        <div class="meta">Sales ${money(x.gross)} | Cost ${money(x.cost)} | Qty ${x.qty}</div>
      </div>
      <div class="amt" style="color:${x.profit<0?'var(--bad)':'var(--good)'}">${money(x.profit)}</div>
    </div>
  `).join("") : `<div class="hint">No data</div>`;

  const itemRows = stats.byItem.slice(0,12);
  renderBarChart(els.itemProfitChart, itemRows.length?itemRows:[{key:"-",profit:0,cost:0,gross:0,qty:0,name:"-"}], "profit",
    x=>`${x.name} (${x.session}/${x.category}) | Profit ${money(x.profit)} | Sales ${money(x.gross)} | Cost ${money(x.cost)} | Qty ${x.qty}`, "profit");
  els.itemRank.innerHTML = itemRows.length ? itemRows.map(x=>`
    <div class="rankrow">
      <div>
        <div class="name">${escapeHtml(x.name)}</div>
        <div class="meta">${escapeHtml(x.session)} / ${escapeHtml(x.category)} | Sales ${money(x.gross)} | Cost ${money(x.cost)} | Qty ${x.qty}</div>
      </div>
      <div class="amt" style="color:${x.profit<0?'var(--bad)':'var(--good)'}">${money(x.profit)}</div>
    </div>
  `).join("") : `<div class="hint">No data</div>`;

  els.dashRaw.textContent = JSON.stringify({ range:r, ...stats }, null, 2);
}
function clearSalesData(){
  if (!confirm("Clear ALL sales data? This cannot be undone.")) return;
  localStorage.removeItem(KEYS.sales);
  refreshDashboard();
}

/* =======================
   REPORTS
======================= */
function setReportPreset(preset){
  const now = new Date();
  const to = endOfDay(now);
  let from = startOfDay(now);

  if (preset === "DAILY") from = startOfDay(now);
  else if (preset === "WEEKLY") from = startOfDay(new Date(now.getTime() - 6*86400000));
  else if (preset === "MONTHLY") from = new Date(now.getFullYear(), now.getMonth(), 1, 0,0,0,0);
  else if (preset === "ALL") from = new Date(0);
  else if (preset === "CUSTOM") return;

  els.repFrom.value = isoDate(from);
  els.repTo.value = isoDate(to);
}
function getReportRange(){
  const preset = els.repPreset.value;
  if (preset !== "CUSTOM") setReportPreset(preset);
  const from = parseDateInput(els.repFrom.value) || new Date(0);
  const to = endOfDay(parseDateInput(els.repTo.value) || new Date());
  return { from, to, label: `${isoDate(from)} to ${isoDate(to)}` };
}
let LAST_REPORT = null;

function runReport(){
  const r = getReportRange();
  const salesStats = computeSalesStats(r);
  const expenseStats = computeExpenseStats(r);

  const rep = {
    range: { fromISO: r.from.toISOString(), toISO: r.to.toISOString(), label: r.label },
    sales: salesStats,
    expenses: { total: expenseStats.expenseTotal, purchases_count: expenseStats.purchases.length, byDay: expenseStats.byDay },
    stock_on_hand: STOCK_ITEMS.map(si=>({
      stockItemId: si.id, name: si.name, unit: si.unit, on_hand: onHand(si.id), min_level: si.minLevel
    })).sort((a,b)=>a.name.localeCompare(b.name))
  };
  LAST_REPORT = rep;

  els.repGross.textContent = money(rep.sales.totals.gross);
  els.repBills.textContent = `Bills: ${rep.sales.totals.bills}`;
  els.repCost.textContent = money(rep.sales.totals.cost);
  els.repProfit.textContent = money(rep.sales.totals.profit);
  els.repExpense.textContent = money(rep.expenses.total);

  renderBarChart(els.repSalesByDayChart,
    rep.sales.byDay.length ? rep.sales.byDay : [{key:r.label,gross:0,bills:0}],
    "gross",
    x=>`${x.key} | Sales ${money(x.gross)} | Bills ${x.bills}`, "neutral"
  );
  renderBarChart(els.repExpenseByDayChart,
    rep.expenses.byDay.length ? rep.expenses.byDay : [{key:r.label,expense:0}],
    "expense",
    x=>`${x.key} | Expense ${money(x.expense)}`, "neutral"
  );

  const topItems = rep.sales.byItem.slice(0, 30);
  els.repItemRank.innerHTML = topItems.length ? topItems.map(x=>`
    <div class="rankrow">
      <div>
        <div class="name">${escapeHtml(x.name)}</div>
        <div class="meta">${escapeHtml(x.session)} / ${escapeHtml(x.category)} | Qty ${x.qty} | Sales ${money(x.gross)} | Cost ${money(x.cost)}</div>
      </div>
      <div class="amt" style="color:${x.profit<0?'var(--bad)':'var(--good)'}">${money(x.profit)}</div>
    </div>
  `).join("") : `<div class="hint">No item sales in this range</div>`;

  const topCats = rep.sales.byCategory.slice(0, 30);
  els.repCategoryRank.innerHTML = topCats.length ? topCats.map(x=>`
    <div class="rankrow">
      <div>
        <div class="name">${escapeHtml(x.key)}</div>
        <div class="meta">Qty ${x.qty} | Sales ${money(x.gross)} | Cost ${money(x.cost)}</div>
      </div>
      <div class="amt" style="color:${x.profit<0?'var(--bad)':'var(--good)'}">${money(x.profit)}</div>
    </div>
  `).join("") : `<div class="hint">No category sales in this range</div>`;

  els.repRaw.textContent = JSON.stringify(rep, null, 2);
}
function exportReportSalesCsv(){
  if (!LAST_REPORT) runReport();
  const rows = (LAST_REPORT?.sales?.byDay || []).map(d=>({ date:d.key, bills:d.bills, gross_sales:d.gross, cogs:d.cost, profit:d.profit }));
  downloadText("report_sales_by_day.csv", toCsv(rows, ["date","bills","gross_sales","cogs","profit"]), "text/csv");
}
function exportReportItemsCsv(){
  if (!LAST_REPORT) runReport();
  const rows = (LAST_REPORT?.sales?.byItem || []).map(x=>({
    item_id:x.key, name:x.name, session:x.session, category:x.category, qty:x.qty,
    gross_sales:x.gross, cogs:x.cost, profit:x.profit
  }));
  downloadText("report_items.csv", toCsv(rows, ["item_id","name","session","category","qty","gross_sales","cogs","profit"]), "text/csv");
}
function exportReportStockCsv(){
  if (!LAST_REPORT) runReport();
  const rows = (LAST_REPORT?.stock_on_hand || []).map(x=>({ stock_item_id:x.stockItemId, name:x.name, unit:x.unit, on_hand:x.on_hand, min_level:x.min_level }));
  downloadText("report_stock_on_hand.csv", toCsv(rows, ["stock_item_id","name","unit","on_hand","min_level"]), "text/csv");
}

/* =======================
   STOCK UI
======================= */
function refreshPurchaseItemDropdown(){
  const opts = STOCK_ITEMS.slice().sort((a,b)=>a.name.localeCompare(b.name));
  const html = opts.map(s=>`<option value="${escapeHtml(s.id)}">${escapeHtml(s.name)} (${escapeHtml(s.unit)})</option>`).join("");
  els.purchaseItem.innerHTML = html || `<option value="">(No stock items)</option>`;
  els.purchaseItem.value = opts[0]?.id || "";

  els.recipeStockItem.innerHTML = html || `<option value="">(No stock items)</option>`;
  els.recipeStockItem.value = opts[0]?.id || "";
}
function renderStockTable(){
  const rows = STOCK_ITEMS.slice().sort((a,b)=>a.name.localeCompare(b.name)).map(si=>{
    const have = onHand(si.id);
    const low = si.minLevel>0 && have < si.minLevel;
    return `
      <tr>
        <td>${escapeHtml(si.name)} ${low ? `<span class="badge bad" style="margin-left:8px">LOW</span>` : ""}</td>
        <td>${escapeHtml(si.unit)}</td>
        <td class="right">${have.toFixed(2)}</td>
        <td class="right">${Number(si.minLevel||0).toFixed(2)}</td>
      </tr>
    `;
  }).join("") || `<tr><td colspan="4" style="color:var(--muted);padding:10px 6px;">No stock items yet.</td></tr>`;
  els.stockTableBody.innerHTML = rows;

  const today = rangeFor("TODAY");
  const exp = computeExpenseStats(today);
  els.todayExpenseText.textContent = money(exp.expenseTotal);
}
function addStockItem(){
  const name = (els.stockName.value||"").trim();
  const unit = els.stockUnit.value;
  const minLevel = Number(els.stockMin.value||0);
  if (!name){ alert("Enter stock item name"); return; }

  STOCK_ITEMS.push(normalizeStockItem({ name, unit, minLevel: Number.isFinite(minLevel)?minLevel:0 }));
  saveStockItems(STOCK_ITEMS);

  els.stockName.value=""; els.stockMin.value="";
  els.stockAddStatus.textContent = "Added stock item.";
  refreshPurchaseItemDropdown();
  renderStockTable();
}
function addPurchase(){
  const itemId = els.purchaseItem.value;
  if (!itemId){ alert("Add stock items first"); return; }
  const qty = Number(els.purchaseQty.value||0);
  const cost = Number(els.purchaseCost.value||0);
  const note = (els.purchaseNote.value||"").trim();
  if (!Number.isFinite(qty) || qty<=0){ alert("Enter qty added"); return; }
  if (!Number.isFinite(cost) || cost<0){ alert("Enter valid cost"); return; }

  STOCK_MOVES.push(normalizeStockMove({ whenISO:new Date().toISOString(), itemId, type:"PURCHASE", qty, cost, note }));
  saveStockMoves(STOCK_MOVES);

  els.purchaseQty.value=""; els.purchaseCost.value=""; els.purchaseNote.value="";
  els.purchaseStatus.textContent = "Purchase added.";
  renderStockTable();
  renderMenu();
}
function clearStockHistory(){
  if (!confirm("Clear ALL stock history (purchases + usage)? This will reset on-hand to 0.")) return;
  localStorage.removeItem(KEYS.stockMoves);
  STOCK_MOVES = [];
  renderStockTable();
  renderMenu();
}

/* =======================
   SETTINGS HELPERS (existing)
======================= */
function categoriesFor(session){
  return (CATEGORIES[session] || []).slice().sort((a,b)=>a.localeCompare(b));
}
function refreshCategoryManagementUI(){
  const s = els.catListSession.value;
  const cats = categoriesFor(s);
  els.catList.innerHTML = cats.map(c=>`<option value="${escapeHtml(c)}">${escapeHtml(c)}</option>`).join("");
  els.catStatus.textContent = `${s}: ${cats.length} categories`;
}
function refreshCategoryDropdownForSession(selectEl, session){
  const cats = categoriesFor(session);
  selectEl.innerHTML = cats.length ? cats.map(c=>`<option value="${escapeHtml(c)}">${escapeHtml(c)}</option>`).join("") : `<option value="">(No categories - add first)</option>`;
  selectEl.value = cats[0] || "";
}
function itemsFor(session){ return MENU_DRAFT.filter(x=>x.session===session); }
function refreshCategorySelect(session, categoryEl){
  const cats = Array.from(new Set(itemsFor(session).map(x=>x.category))).sort();
  categoryEl.innerHTML = cats.map(c=>`<option value="${escapeHtml(c)}">${escapeHtml(c)}</option>`).join("") || `<option value="">(No categories)</option>`;
  categoryEl.value = cats[0] || "";
}
function refreshItemSelect(session, category, itemEl){
  const items = itemsFor(session).filter(x=>x.category===category).sort((a,b)=>a.name.localeCompare(b.name));
  itemEl.innerHTML = items.length
    ? items.map(it=>`<option value="${escapeHtml(it.id)}">${escapeHtml(it.name)}</option>`).join("")
    : `<option value="">(No items in this category)</option>`;
  itemEl.value = items[0]?.id || "";
  return items[0] || null;
}
function refreshCategorySelectFromCategories(session, categoryEl){
  refreshCategoryDropdownForSession(categoryEl, session);
}

/* Categories Apply */
function addCategoryApply(){
  const s = els.catSession.value;
  const name = (els.catName.value||"").trim();
  if (!name){ alert("Enter category name"); return; }
  CATEGORIES[s] = CATEGORIES[s] || [];
  if (CATEGORIES[s].includes(name)){ alert("Category already exists"); return; }
  CATEGORIES[s].push(name);
  CATEGORIES[s].sort((a,b)=>a.localeCompare(b));
  els.catName.value="";
  markDirty(`Category added (${s}: ${name}). Click Save All.`);
  refreshCategoryManagementUI();
  refreshSettingsDropdownsAll();
}
function deleteCategoryApply(){
  const s = els.catListSession.value;
  const cat = els.catList.value;
  if (!cat){ alert("Select category"); return; }
  if (!confirm(`Delete category "${cat}" from ${s}?\nItems will be moved to "(Uncategorized)".`)) return;

  CATEGORIES[s] = (CATEGORIES[s]||[]).filter(x=>x!==cat);
  for (const it of MENU_DRAFT){
    if (it.session===s && it.category===cat) it.category="(Uncategorized)";
  }
  if (!CATEGORIES[s].includes("(Uncategorized)")) CATEGORIES[s].push("(Uncategorized)");
  CATEGORIES[s].sort((a,b)=>a.localeCompare(b));

  markDirty(`Category deleted (${s}: ${cat}). Click Save All.`);
  refreshCategoryManagementUI();
  activeCategory="ALL";
  rebuildCategorySidebar();
  renderMenu();
  refreshSettingsDropdownsAll();
}

/* Recipes UI */
function refreshRecipeDropdowns(){
  const session = els.recipeSession.value;
  refreshCategorySelectFromCategories(session, els.recipeCategory);
  refreshRecipeItems();
}
function refreshRecipeItems(){
  const session = els.recipeSession.value;
  const cat = els.recipeCategory.value;
  refreshItemSelect(session, cat, els.recipeItem);
  renderRecipeLinesTable();
}
function renderRecipeLinesTable(){
  const menuItemId = els.recipeItem.value;
  const lines = RECIPES[menuItemId] || [];
  els.recipeLinesBody.innerHTML = lines.map((ln, idx)=>{
    const si = STOCK_ITEMS.find(x=>x.id===ln.stockItemId);
    return `
      <tr>
        <td>${escapeHtml(si?.name || ln.stockItemId)} <span style="color:var(--muted)">(${escapeHtml(si?.unit||"")})</span></td>
        <td class="right">${Number(ln.qtyPerUnit||0).toFixed(2)}</td>
        <td class="right"><button class="btn secondary" type="button" data-rm-line="${idx}" style="min-width:auto;padding:8px 10px">Remove</button></td>
      </tr>
    `;
  }).join("") || `<tr><td colspan="3" style="color:var(--muted);padding:10px 6px;">No recipe lines yet.</td></tr>`;

  els.recipeLinesBody.querySelectorAll("[data-rm-line]").forEach(btn=>{
    btn.addEventListener("click", ()=>{
      const i = Number(btn.getAttribute("data-rm-line"));
      RECIPES[menuItemId] = (RECIPES[menuItemId] || []).filter((_,idx)=>idx!==i);
      saveRecipes(RECIPES);
      markDirty("Recipe updated. Click Save All.");
      renderRecipeLinesTable();
      renderMenu();
    });
  });
}
function addRecipeLine(){
  const menuItemId = els.recipeItem.value;
  const stockItemId = els.recipeStockItem.value;
  const qty = Number(els.recipeQtyPerUnit.value||0);
  if (!menuItemId){ alert("Select menu item"); return; }
  if (!stockItemId){ alert("Add stock items first"); return; }
  if (!Number.isFinite(qty) || qty<=0){ alert("Enter qty per item"); return; }

  RECIPES[menuItemId] = RECIPES[menuItemId] || [];
  const ex = RECIPES[menuItemId].findIndex(x=>x.stockItemId===stockItemId);
  if (ex>=0) RECIPES[menuItemId][ex].qtyPerUnit = qty;
  else RECIPES[menuItemId].push({ stockItemId, qtyPerUnit: qty });

  saveRecipes(RECIPES);
  markDirty("Recipe updated. Click Save All.");
  els.recipeQtyPerUnit.value="";
  renderRecipeLinesTable();
  renderMenu();
}
function clearRecipeApply(){
  const menuItemId = els.recipeItem.value;
  if (!menuItemId) return;
  if (!confirm("Clear recipe for selected item?")) return;
  RECIPES[menuItemId] = [];
  saveRecipes(RECIPES);
  markDirty("Recipe cleared. Click Save All.");
  renderRecipeLinesTable();
  renderMenu();
}

/* Price/Cost */
function refreshPriceDropdowns(){
  const session = els.priceSession.value;
  refreshCategorySelectFromCategories(session, els.priceCategory);
  refreshPriceItems();
}
function refreshPriceItems(){
  const session = els.priceSession.value;
  const cat = els.priceCategory.value;
  const it = refreshItemSelect(session, cat, els.priceItem);
  els.priceValue.value = it ? String(it.price||0) : "";
  els.costValue.value = it ? String(it.cost||0) : "";
}
function applyPriceCost(){
  const id = els.priceItem.value;
  const it = MENU_DRAFT.find(x=>x.id===id);
  if (!it){ alert("Select item"); return; }
  it.price = Math.max(0, Number(els.priceValue.value||0));
  it.cost = Math.max(0, Number(els.costValue.value||0));
  markDirty(`Price/Cost changed for ${it.name}. Click Save All.`);
  rebuildCategorySidebar();
  renderMenu();
}

/* Rename */
function refreshRenameDropdowns(){
  const session = els.editSession.value;
  refreshCategorySelectFromCategories(session, els.editCategory);
  refreshRenameItems();
}
function refreshRenameItems(){
  const session = els.editSession.value;
  const cat = els.editCategory.value;
  const it = refreshItemSelect(session, cat, els.editItem);
  els.editNewName.value = it ? it.name : "";
}
function applyRename(){
  const id = els.editItem.value;
  const it = MENU_DRAFT.find(x=>x.id===id);
  if (!it){ alert("Select item"); return; }
  const newName = (els.editNewName.value||"").trim();
  if (!newName){ alert("Enter new name"); return; }
  it.name = newName;
  markDirty(`Renamed item to "${newName}". Click Save All.`);
  els.renameStatus.textContent = "Renamed (pending Save All).";
  rebuildCategorySidebar();
  renderMenu();
  refreshSettingsDropdownsAll();
}

/* Image */
function refreshImageDropdowns(){
  const session = els.imgSession.value;
  refreshCategorySelect(session, els.imgCategory);
  refreshImageItems();
}
function refreshImageItems(){
  const session = els.imgSession.value;
  const cat = els.imgCategory.value;
  refreshItemSelect(session, cat, els.imgItem);
  els.imgStatus.textContent = "";
}
function looksLikeUrl(u){ try{ new URL(u); return true; } catch { return false; } }
async function fetchAsDataUrl(url, maxSize=900, quality=0.85){
  const res = await fetch(url, { mode: "cors" });
  if (!res.ok) throw new Error("Download failed (HTTP " + res.status + ")");
  const blob = await res.blob();
  if (!blob.type.startsWith("image/")) throw new Error("URL did not return an image. Content-Type: " + blob.type);
  const file = new File([blob], "img", { type: blob.type });
  return await fileToDataUrl(file, maxSize, quality);
}
function applyImageUrlOnline(){
  const id = els.imgItem.value;
  const it = MENU_DRAFT.find(x=>x.id===id);
  if (!it){ alert("Select item"); return; }
  const url = (els.imgUrl.value||"").trim();
  if (!url){ alert("Paste image URL"); return; }
  if (!looksLikeUrl(url)){ alert("Invalid URL"); return; }
  it.image = url;
  markDirty(`Image URL set for ${it.name}. Click Save All.`);
  els.imgStatus.textContent = "Applied URL (online).";
  renderMenu();
}
async function downloadImageUrlOffline(){
  const id = els.imgItem.value;
  const it = MENU_DRAFT.find(x=>x.id===id);
  if (!it){ alert("Select item"); return; }
  const url = (els.imgUrl.value||"").trim();
  if (!url){ alert("Paste image URL"); return; }
  if (!looksLikeUrl(url)){ alert("Invalid URL"); return; }

  els.imgStatus.textContent = "Downloading image...";
  try{
    const dataUrl = await fetchAsDataUrl(url, 900, 0.85);
    it.image = dataUrl;
    markDirty(`Image downloaded & saved offline for ${it.name}. Click Save All.`);
    els.imgStatus.textContent = "Downloaded & applied (offline).";
    renderMenu();
  }catch(e){
    els.imgStatus.textContent = "Download failed: " + (e?.message || String(e));
    alert("Download failed.\n\nReason: " + (e?.message || String(e)));
  }
}
async function onImageChosen(file){
  if (!file) return;
  const id = els.imgItem.value;
  const it = MENU_DRAFT.find(x=>x.id===id);
  if (!it) return;
  els.imgStatus.textContent = "Processing image...";
  try{
    it.image = await fileToDataUrl(file);
    markDirty(`Image updated for ${it.name}. Click Save All.`);
    els.imgStatus.textContent = "Image applied (not saved yet).";
    renderMenu();
  }catch(e){
    els.imgStatus.textContent = "Failed: " + String(e);
  }
}
function clearImage(){
  const id = els.imgItem.value;
  const it = MENU_DRAFT.find(x=>x.id===id);
  if (!it) return;
  it.image = "";
  markDirty(`Image removed for ${it.name}. Click Save All.`);
  els.imgStatus.textContent = "Image removed (not saved yet).";
  renderMenu();
}

/* Delete items */
function refreshDeleteDropdowns(){
  const session = els.delSession.value;
  refreshCategorySelect(session, els.delCategory);
  refreshDeleteItems();
}
function refreshDeleteItems(){
  const session = els.delSession.value;
  const cat = els.delCategory.value;
  refreshItemSelect(session, cat, els.delItem);
  els.deleteStatus.textContent = "";
}
function deleteSelectedItemPermanent(){
  const id = els.delItem.value;
  const it = MENU_DRAFT.find(x=>x.id===id);
  if (!it) return;
  if (!confirm(`Permanently delete "${it.name}"? This removes item from menu and daily selections.`)) return;

  MENU_DRAFT = MENU_DRAFT.filter(x=>x.id!==id);

  DAILY.items = DAILY.items || {};
  for (const sess of ["TIFFIN","LUNCH","DINNER"]){
    DAILY.items[sess] = (DAILY.items[sess]||[]).filter(x=>x!==id);
  }
  saveDaily(DAILY);

  if (cart.has(id)){ cart.delete(id); renderCart(); }

  markDirty(`Item deleted: ${it.name}. Click Save All.`);
  els.deleteStatus.textContent = "Deleted (pending Save All).";
  activeCategory="ALL";
  rebuildCategorySidebar();
  renderMenu();
  refreshSettingsDropdownsAll();
}

/* Daily items */
function refreshDailyItemsUI(){
  const session = els.dailySession.value;
  els.dailyMode.value = ((DAILY.mode||{})[session] || "OFF");
  const items = itemsFor(session).sort((a,b)=>a.name.localeCompare(b.name));
  const selected = new Set((DAILY.items||{})[session] || []);
  els.dailyItems.innerHTML = items.map(it=>{
    const sel = selected.has(it.id) ? "selected" : "";
    return `<option value="${escapeHtml(it.id)}" ${sel}>${escapeHtml(it.name)} (${escapeHtml(it.category)})</option>`;
  }).join("");
}
function saveDailyItems(){
  const session = els.dailySession.value;
  const selected = Array.from(els.dailyItems.selectedOptions).map(o=>o.value);
  DAILY.items = DAILY.items || {};
  DAILY.mode = DAILY.mode || {};
  DAILY.items[session] = selected;
  DAILY.mode[session] = els.dailyMode.value;
  saveDaily(DAILY);
  els.dailyStatus.textContent = `Saved daily items for ${session}.`;
  activeCategory="ALL";
  rebuildCategorySidebar();
  renderMenu();
}

/* Add item */
async function addNewMenuItem(){
  const session = els.newSession.value;
  const category = els.newCategorySel.value;
  const name = (els.newName.value||"").trim();
  const price = Number(els.newPrice.value||0);
  const cost = Number(els.newCost.value||0);

  if (!category){ alert("Select category"); return; }
  if (!name){ alert("Enter item name"); return; }
  if (!Number.isFinite(price) || price<0){ alert("Invalid price"); return; }
  if (!Number.isFinite(cost) || cost<0){ alert("Invalid cost"); return; }

  const newItem = normalizeMenuItem({ session, category, name, price, cost, image:"" });
  newItem.id = itemId(newItem);

  if (MENU_DRAFT.some(x=>x.id===newItem.id)){ alert("Item already exists."); return; }

  const file = els.newImageFile.files?.[0];
  if (file){ try{ newItem.image = await fileToDataUrl(file); }catch{} }

  MENU_DRAFT.push(newItem);
  markDirty(`Added item: ${newItem.name}. Click Save All.`);
  els.addItemStatus.textContent = "Added (pending Save All).";
  els.newName.value=""; els.newPrice.value=""; els.newCost.value=""; els.newImageFile.value="";

  activeCategory="ALL";
  rebuildCategorySidebar();
  renderMenu();
  refreshSettingsDropdownsAll();
}

/* Export menu CSV */
function exportMenuCsv(){
  const header = ["session","category","name","price","cost","profit_per_item","has_image"];
  const rows = MENU_DRAFT.map(it => ({
    session: it.session,
    category: it.category,
    name: it.name,
    price: it.price,
    cost: it.cost,
    profit_per_item: Number(it.price||0)-Number(it.cost||0),
    has_image: it.image ? "TRUE" : "FALSE"
  }));
  downloadText("menu.csv", toCsv(rows, header), "text/csv");
}

/* =======================
   TAB HANDLING
======================= */
function showTab(key){
  Object.entries(els.tabs).forEach(([k,el]) => el.classList.toggle("hidden", k !== key));
  els.tabButtons.forEach(b => b.classList.toggle("active", b.dataset.tab === key));

  if (key === "dashboard") refreshDashboard();
  if (key === "stock") renderStockTable();
  if (key === "reports") runReport();
  if (key === "settings") refreshSettingsDropdownsAll();
}

/* Settings refresh (all) */
function refreshSettingsDropdownsAll(){
  refreshCategoryManagementUI();

  refreshCategoryDropdownForSession(els.newCategorySel, els.newSession.value);

  refreshRecipeDropdowns();
  refreshPriceDropdowns();
  refreshRenameDropdowns();
  refreshImageDropdowns();
  refreshDeleteDropdowns();
  refreshDailyItemsUI();

  refreshPurchaseItemDropdown();

  refreshChargesSettingsUI();
}

/* =======================
   POS new bill
======================= */
function newBill(){
  cart.clear();
  renderCart();
  els.discountAmount.value="0";
  els.customerNote.value="";
  els.paymentMethod.value="CASH";
  els.upiRef.value="";
  recalc();
}

/* =======================
   EVENTS
======================= */
els.tabButtons.forEach(btn => btn.addEventListener("click", () => showTab(btn.dataset.tab)));
els.sessionSelect.addEventListener("change", () => { activeCategory="ALL"; rebuildCategorySidebar(); renderMenu(); });
els.searchBox.addEventListener("input", renderMenu);

els.discountAmount.addEventListener("input", recalc);

els.clearBtn.addEventListener("click", () => { if (confirm("Clear the cart?")) { cart.clear(); renderCart(); } });
els.newBillBtn.addEventListener("click", () => { if (confirm("Start a new bill?")) newBill(); });

els.printBtn.addEventListener("click", () => {
  if (cart.size === 0){ alert("Add at least one item."); return; }
  try{ validateCartStockOrThrow(cart); } catch(e){ alert(String(e.message||e)); return; }

  const invoiceNo = nextInvoiceNumber();
  const whenISO = new Date().toISOString();

  recordSale({invoiceNo, whenISO});
  applyCartStockUsageMoves({cart, invoiceNo, whenISO});

  renderStockTable();
  renderMenu();
  refreshDashboard();
  runReport();

  els.receipt.innerHTML = buildReceiptHTML({invoiceNo, whenISO});
  window.print();
  setTimeout(() => newBill(), 250);
});

/* dashboard */
els.dashRefreshBtn.addEventListener("click", refreshDashboard);
els.dashClearBtn.addEventListener("click", clearSalesData);
els.dashRange.addEventListener("change", refreshDashboard);

/* stock */
els.addStockItemBtn.addEventListener("click", addStockItem);
els.addPurchaseBtn.addEventListener("click", addPurchase);
els.clearStockMovesBtn.addEventListener("click", clearStockHistory);

/* reports */
els.repPreset.addEventListener("change", ()=>{ if(els.repPreset.value!=="CUSTOM") setReportPreset(els.repPreset.value); });
els.repRunBtn.addEventListener("click", runReport);
els.repExportSalesCsvBtn.addEventListener("click", exportReportSalesCsv);
els.repExportItemsCsvBtn.addEventListener("click", exportReportItemsCsv);
els.repExportStockCsvBtn.addEventListener("click", exportReportStockCsv);

/* settings: charges */
els.saveChargesBtn.addEventListener("click", saveChargesFromSettings);

/* settings: categories */
els.addCategoryBtn.addEventListener("click", addCategoryApply);
els.catListSession.addEventListener("change", refreshCategoryManagementUI);
els.deleteCategoryBtn.addEventListener("click", deleteCategoryApply);

/* settings: recipes */
els.recipeSession.addEventListener("change", refreshRecipeDropdowns);
els.recipeCategory.addEventListener("change", refreshRecipeItems);
els.recipeItem.addEventListener("change", renderRecipeLinesTable);
els.addRecipeLineBtn.addEventListener("click", addRecipeLine);
els.clearRecipeBtn.addEventListener("click", clearRecipeApply);

/* settings: price/cost */
els.priceSession.addEventListener("change", refreshPriceDropdowns);
els.priceCategory.addEventListener("change", refreshPriceItems);
els.priceItem.addEventListener("change", refreshPriceItems);
els.applyPriceBtn.addEventListener("click", applyPriceCost);

/* rename */
els.editSession.addEventListener("change", refreshRenameDropdowns);
els.editCategory.addEventListener("change", refreshRenameItems);
els.editItem.addEventListener("change", refreshRenameItems);
els.applyRenameBtn.addEventListener("click", applyRename);

/* image */
els.imgSession.addEventListener("change", refreshImageDropdowns);
els.imgCategory.addEventListener("change", refreshImageItems);
els.imgFile.addEventListener("change", async (e)=>{ await onImageChosen(e.target.files?.[0]); els.imgFile.value=""; });
els.applyImgUrlBtn.addEventListener("click", applyImageUrlOnline);
els.downloadImgUrlBtn.addEventListener("click", downloadImageUrlOffline);
els.clearImgBtn.addEventListener("click", clearImage);

/* delete */
els.delSession.addEventListener("change", refreshDeleteDropdowns);
els.delCategory.addEventListener("change", refreshDeleteItems);
els.deleteItemBtn.addEventListener("click", deleteSelectedItemPermanent);

/* daily */
els.dailySession.addEventListener("change", refreshDailyItemsUI);
els.saveDailyBtn.addEventListener("click", saveDailyItems);

/* add item */
els.newSession.addEventListener("change", ()=>refreshCategoryDropdownForSession(els.newCategorySel, els.newSession.value));
els.addItemBtn.addEventListener("click", addNewMenuItem);

/* export */
els.exportMenuCsvBtn.addEventListener("click", exportMenuCsv);

/* save bar */
els.saveAllBtn.addEventListener("click", saveAllChanges);
els.discardBtn.addEventListener("click", discardChanges);

window.addEventListener("beforeunload", (e) => {
  if (!HAS_UNSAVED) return;
  e.preventDefault();
  e.returnValue = "";
});

/* =======================
   INIT
======================= */
(function init(){
  for (const s of ["TIFFIN","LUNCH","DINNER"]){
    if (!Array.isArray(CATEGORIES[s]) || !CATEGORIES[s].length){
      CATEGORIES[s] = inferCategoriesFromMenu(MENU_DRAFT)[s] || [];
    }
  }

  // default report dates
  setReportPreset("DAILY");

  // load charges into settings UI (and apply to bill totals)
  refreshChargesSettingsUI();
  applyChargesToBillingUI();

  activeCategory="ALL";
  rebuildCategorySidebar();
  renderMenu();
  renderCart();

  refreshSettingsDropdownsAll();
  renderStockTable();
  refreshDashboard();
  runReport();

  showTab("pos");
})();
</script>
</body>
</html>
