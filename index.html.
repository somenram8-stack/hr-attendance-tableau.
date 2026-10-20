<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Tableau HR Operations & Attendance Intelligence Hub</title>

  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Chart.js -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <!-- SheetJS (xlsx) -->
  <script src="https://cdn.sheetjs.com/xlsx-0.20.1/package/dist/xlsx.full.min.js"></script>
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />

  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            tableau: {
              blue: '#1F538D',
              dark: '#112233',
              teal: '#2B83BA',
              lightBlue: '#E6F0F8',
              orange: '#E37222',
              green: '#2CA02C',
              red: '#D62728',
              purple: '#9467BD',
              surface: '#F8FAFC',
              sidebar: '#0F1E36',
              card: '#FFFFFF'
            }
          }
        }
      }
    };
  </script>

  <style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
    body { font-family: 'Inter', sans-serif; background-color: #F1F5F9; }
    
    /* Custom Scrollbars */
    ::-webkit-scrollbar { width: 6px; height: 6px; }
    ::-webkit-scrollbar-track { background: #F1F5F9; }
    ::-webkit-scrollbar-thumb { background: #CBD5E1; border-radius: 4px; }
    ::-webkit-scrollbar-thumb:hover { background: #94A3B8; }

    /* Sticky column styling for muster roll */
    .sticky-col-left {
      position: sticky;
      left: 0;
      background: white;
      z-index: 20;
    }
    .sticky-col-left-2 {
      position: sticky;
      left: 80px;
      background: white;
      z-index: 20;
    }
    .sticky-header {
      position: sticky;
      top: 0;
      z-index: 30;
    }
    .nav-active {
      background: linear-gradient(90deg, rgba(43,131,186,0.2) 0%, rgba(43,131,186,0.05) 100%);
      border-left: 4px solid #2B83BA;
      color: #FFFFFF !important;
    }
  </style>
</head>
<body class="text-slate-800 antialiased overflow-x-hidden flex h-screen">

  <!-- ======================== LEFT SIDEBAR ======================== -->
  <aside class="w-64 bg-tableau-sidebar text-slate-300 flex flex-col justify-between shrink-0 shadow-2xl z-40 transition-all duration-300">
    <div>
      <!-- Header / Logo -->
      <div class="h-20 flex items-center px-6 border-b border-slate-700/60 bg-[#0B1628]">
        <div class="w-10 h-10 rounded-lg bg-gradient-to-tr from-tableau-blue to-tableau-teal flex items-center justify-center text-white font-bold text-xl shadow-md">
          <i class="fa-solid fa-chart-line"></i>
        </div>
        <div class="ml-3">
          <h1 class="text-sm font-bold tracking-wider text-white uppercase">Tableau HR</h1>
          <p class="text-[10px] text-teal-400 font-semibold uppercase tracking-widest">Ops Intelligence</p>
        </div>
      </div>

      <!-- Navigation Tabs -->
      <nav class="mt-4 space-y-1 px-3">
        <a href="#overview" onclick="switchTab('overview')" id="tab-overview" class="nav-active group flex items-center px-4 py-3 text-xs font-medium rounded-lg text-slate-300 hover:text-white hover:bg-slate-800/60 transition-all">
          <i class="fa-solid fa-gauge-high w-6 text-teal-400 text-sm"></i>
          <span>HR Overview</span>
        </a>
        <a href="#daily" onclick="switchTab('daily')" id="tab-daily" class="group flex items-center px-4 py-3 text-xs font-medium rounded-lg text-slate-400 hover:text-white hover:bg-slate-800/60 transition-all">
          <i class="fa-solid fa-calendar-check w-6 text-teal-400 text-sm"></i>
          <span>Daily Attendance</span>
        </a>
        <a href="#directory" onclick="switchTab('directory')" id="tab-directory" class="group flex items-center px-4 py-3 text-xs font-medium rounded-lg text-slate-400 hover:text-white hover:bg-slate-800/60 transition-all">
          <i class="fa-solid fa-users w-6 text-teal-400 text-sm"></i>
          <span>Employee Directory</span>
        </a>
        <a href="#muster" onclick="switchTab('muster')" id="tab-muster" class="group flex items-center px-4 py-3 text-xs font-medium rounded-lg text-slate-400 hover:text-white hover:bg-slate-800/60 transition-all">
          <i class="fa-solid fa-table-cells w-6 text-teal-400 text-sm"></i>
          <span>Monthly Muster</span>
        </a>
        <a href="#analytics" onclick="switchTab('analytics')" id="tab-analytics" class="group flex items-center px-4 py-3 text-xs font-medium rounded-lg text-slate-400 hover:text-white hover:bg-slate-800/60 transition-all">
          <i class="fa-solid fa-chart-pie w-6 text-teal-400 text-sm"></i>
          <span>Dept Analytics</span>
        </a>
        <a href="#importexport" onclick="switchTab('importexport')" id="tab-importexport" class="group flex items-center px-4 py-3 text-xs font-medium rounded-lg text-slate-400 hover:text-white hover:bg-slate-800/60 transition-all">
          <i class="fa-solid fa-file-excel w-6 text-teal-400 text-sm"></i>
          <span>Data Import / Export</span>
        </a>
      </nav>
    </div>

    <!-- Quick Stats Footer in Sidebar -->
    <div class="p-4 border-t border-slate-800/80 bg-[#0B1628]/60 text-xs">
      <div class="flex items-center justify-between text-slate-400 mb-1">
        <span>Total Roster:</span>
        <span id="sidebar-total-count" class="font-bold text-white">204</span>
      </div>
      <div class="flex items-center justify-between text-slate-400 mb-2">
        <span>Active Departments:</span>
        <span id="sidebar-dept-count" class="font-bold text-teal-400">30</span>
      </div>
      <div class="w-full bg-slate-700/50 rounded-full h-1.5 overflow-hidden">
        <div id="sidebar-progress" class="bg-teal-400 h-1.5 rounded-full" style="width: 100%"></div>
      </div>
    </div>
  </aside>

  <!-- ======================== MAIN CONTAINER ======================== -->
  <main class="flex-1 flex flex-col h-screen overflow-hidden">
    
    <!-- Top Header -->
    <header class="h-20 bg-white border-b border-slate-200 px-8 flex items-center justify-between shrink-0 shadow-sm">
      <div class="flex items-center gap-4">
        <div>
          <h2 id="view-title" class="text-xl font-bold text-slate-800">Operational Overview</h2>
          <p class="text-xs text-slate-500 font-medium">Real-time attendance & workforce allocation metrics</p>
        </div>
      </div>

      <!-- Date selector & Global Actions -->
      <div class="flex items-center gap-4">
        <div class="flex items-center bg-slate-100 rounded-lg px-3 py-1.5 border border-slate-200 shadow-inner">
          <i class="fa-regular fa-calendar-days text-slate-500 mr-2 text-sm"></i>
          <span class="text-xs font-semibold text-slate-600 mr-2">Audit Date:</span>
          <input type="date" id="global-date-picker" onchange="handleDateChange(this.value)" class="bg-transparent text-xs font-bold text-slate-800 outline-none cursor-pointer" />
        </div>

        <button onclick="openAddEmployeeModal()" class="flex items-center gap-2 bg-tableau-blue hover:bg-blue-800 text-white px-3.5 py-2 rounded-lg text-xs font-semibold shadow-sm transition">
          <i class="fa-solid fa-user-plus"></i>
          <span>Add Employee</span>
        </button>

        <button onclick="openManageDeptModal()" class="flex items-center gap-2 bg-slate-100 hover:bg-slate-200 text-slate-700 px-3.5 py-2 rounded-lg text-xs font-semibold border border-slate-200 transition">
          <i class="fa-solid fa-sitemap"></i>
          <span>Departments</span>
        </button>
      </div>
    </header>

    <!-- Main Content Area -->
    <div class="flex-1 overflow-y-auto p-8 space-y-6">

      <!-- ==================== 1. TAB: OVERVIEW ==================== -->
      <section id="content-overview" class="space-y-6">
        <!-- Top KPI Cards -->
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-5 gap-4">
          <div class="bg-white rounded-xl p-5 border border-slate-200/80 shadow-sm hover:shadow-md transition">
            <div class="flex justify-between items-start">
              <div>
                <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">Total Strength</p>
                <h3 id="kpi-total-strength" class="text-2xl font-black text-slate-800 mt-1">204</h3>
              </div>
              <span class="p-2.5 bg-blue-50 text-tableau-blue rounded-lg text-lg"><i class="fa-solid fa-users"></i></span>
            </div>
            <div class="mt-3 flex items-center text-xs text-slate-500">
              <span class="text-emerald-600 font-bold mr-1"><i class="fa-solid fa-check"></i> 100%</span> Roster active
            </div>
          </div>

          <div class="bg-white rounded-xl p-5 border border-slate-200/80 shadow-sm hover:shadow-md transition">
            <div class="flex justify-between items-start">
              <div>
                <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">Present Today</p>
                <h3 id="kpi-present-count" class="text-2xl font-black text-emerald-600 mt-1">0</h3>
              </div>
              <span class="p-2.5 bg-emerald-50 text-emerald-600 rounded-lg text-lg"><i class="fa-solid fa-user-check"></i></span>
            </div>
            <div class="mt-3 flex items-center text-xs text-slate-500">
              <span id="kpi-present-pct" class="text-emerald-600 font-bold mr-1">0%</span> of active workforce
            </div>
          </div>

          <div class="bg-white rounded-xl p-5 border border-slate-200/80 shadow-sm hover:shadow-md transition">
            <div class="flex justify-between items-start">
              <div>
                <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">Absent Today</p>
                <h3 id="kpi-absent-count" class="text-2xl font-black text-rose-600 mt-1">0</h3>
              </div>
              <span class="p-2.5 bg-rose-50 text-rose-600 rounded-lg text-lg"><i class="fa-solid fa-user-xmark"></i></span>
            </div>
            <div class="mt-3 flex items-center text-xs text-slate-500">
              <span id="kpi-absent-pct" class="text-rose-600 font-bold mr-1">0%</span> unexcused/absent
            </div>
          </div>

          <div class="bg-white rounded-xl p-5 border border-slate-200/80 shadow-sm hover:shadow-md transition">
            <div class="flex justify-between items-start">
              <div>
                <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">Half Day / Leaves</p>
                <h3 id="kpi-leave-count" class="text-2xl font-black text-amber-500 mt-1">0</h3>
              </div>
              <span class="p-2.5 bg-amber-50 text-amber-500 rounded-lg text-lg"><i class="fa-solid fa-clock-rotate-left"></i></span>
            </div>
            <div class="mt-3 flex items-center text-xs text-slate-500">
              <span id="kpi-leave-pct" class="text-amber-600 font-bold mr-1">0%</span> on authorized leave/HD
            </div>
          </div>

          <div class="bg-white rounded-xl p-5 border border-slate-200/80 shadow-sm hover:shadow-md transition">
            <div class="flex justify-between items-start">
              <div>
                <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">Week Off (WO)</p>
                <h3 id="kpi-wo-count" class="text-2xl font-black text-indigo-500 mt-1">0</h3>
              </div>
              <span class="p-2.5 bg-indigo-50 text-indigo-500 rounded-lg text-lg"><i class="fa-solid fa-mug-hot"></i></span>
            </div>
            <div class="mt-3 flex items-center text-xs text-slate-500">
              <span id="kpi-wo-pct" class="text-indigo-600 font-bold mr-1">0%</span> scheduled weekly off
            </div>
          </div>
        </div>

        <!-- Charts Grid (Tableau Style) -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
          <!-- Donut chart -->
          <div class="bg-white p-6 rounded-xl border border-slate-200/80 shadow-sm">
            <div class="flex justify-between items-center mb-4">
              <h4 class="text-sm font-bold text-slate-700 tracking-wide uppercase">Daily Attendance Status</h4>
              <span class="text-xs text-slate-400"><i class="fa-solid fa-circle-info mr-1"></i> Live</span>
            </div>
            <div class="h-64 relative flex items-center justify-center">
              <canvas id="attendanceDonutChart"></canvas>
            </div>
          </div>

          <!-- Bar Chart for Department Breakdown -->
          <div class="bg-white p-6 rounded-xl border border-slate-200/80 shadow-sm lg:col-span-2">
            <div class="flex justify-between items-center mb-4">
              <h4 class="text-sm font-bold text-slate-700 tracking-wide uppercase">Department Attendance Distribution</h4>
              <span class="text-xs text-slate-400">Present vs Absent by Unit</span>
            </div>
            <div class="h-64 relative">
              <canvas id="deptBarChart"></canvas>
            </div>
          </div>
        </div>

        <!-- Quick Top Departments Table -->
        <div class="bg-white rounded-xl border border-slate-200/80 shadow-sm overflow-hidden">
          <div class="px-6 py-4 border-b border-slate-100 flex items-center justify-between bg-slate-50/50">
            <h4 class="text-sm font-bold text-slate-700 uppercase">Operational Unit Summary</h4>
            <button onclick="switchTab('analytics')" class="text-xs text-tableau-blue hover:underline font-semibold">View Full Analytics &rarr;</button>
          </div>
          <div class="overflow-x-auto">
            <table class="w-full text-left text-xs text-slate-600">
              <thead class="bg-slate-50 text-slate-500 uppercase font-semibold border-b border-slate-200">
                <tr>
                  <th class="py-3 px-6">Department</th>
                  <th class="py-3 px-4 text-center">Headcount</th>
                  <th class="py-3 px-4 text-center">Present</th>
                  <th class="py-3 px-4 text-center">Absent</th>
                  <th class="py-3 px-4 text-center">Leaves / WO</th>
                  <th class="py-3 px-6 text-center">Operational Health</th>
                </tr>
              </thead>
              <tbody id="overview-dept-tbody" class="divide-y divide-slate-100">
                <!-- Populated Dynamically -->
              </tbody>
            </table>
          </div>
        </div>
      </section>


      <!-- ==================== 2. TAB: DAILY ATTENDANCE ENTRY ==================== -->
      <section id="content-daily" class="space-y-6 hidden">
        <!-- Controls & Filters Bar -->
        <div class="bg-white rounded-xl p-5 border border-slate-200 shadow-sm flex flex-wrap items-center justify-between gap-4">
          <div class="flex flex-wrap items-center gap-3 flex-1">
            <!-- Search -->
            <div class="relative w-64">
              <i class="fa-solid fa-magnifying-glass absolute left-3 top-2.5 text-slate-400 text-xs"></i>
              <input type="text" id="daily-search" oninput="renderDailyTable()" placeholder="Search Paycode or Name..." class="w-full pl-9 pr-3 py-1.5 text-xs bg-slate-50 border border-slate-200 rounded-lg focus:outline-none focus:ring-1 focus:ring-tableau-teal" />
            </div>

            <!-- Dept Filter -->
            <select id="daily-dept-filter" onchange="renderDailyTable()" class="text-xs bg-slate-50 border border-slate-200 rounded-lg px-3 py-2 font-medium text-slate-700 focus:outline-none">
              <option value="ALL">All Departments</option>
            </select>

            <!-- Status Filter -->
            <select id="daily-status-filter" onchange="renderDailyTable()" class="text-xs bg-slate-50 border border-slate-200 rounded-lg px-3 py-2 font-medium text-slate-700 focus:outline-none">
              <option value="ALL">All Statuses</option>
              <option value="P">Present (P)</option>
              <option value="A">Absent (A)</option>
              <option value="HD">Half Day (HD)</option>
              <option value="L">Leave (L)</option>
              <option value="WO">Week Off (WO)</option>
              <option value="UNMARKED">Unmarked</option>
            </select>

            <!-- Realtime Dept Attendance Badge -->
            <div id="dept-rate-badge" class="px-3 py-1.5 rounded-lg bg-teal-50 border border-teal-200 text-teal-800 text-xs font-bold flex items-center gap-2">
              <i class="fa-solid fa-chart-simple"></i>
              <span>Dept Attendance: <strong id="badge-pct">0%</strong> (<span id="badge-ratio">0/0</span>)</span>
            </div>
          </div>

          <!-- Fast Bulk Action Buttons -->
          <div class="flex items-center gap-2">
            <span class="text-xs font-semibold text-slate-400 uppercase mr-1">Bulk Apply:</span>
            <button onclick="bulkMarkStatus('P')" class="px-3 py-1.5 bg-emerald-600 hover:bg-emerald-700 text-white rounded text-xs font-bold transition shadow-sm">Mark All P</button>
            <button onclick="bulkMarkStatus('A')" class="px-3 py-1.5 bg-rose-600 hover:bg-rose-700 text-white rounded text-xs font-bold transition shadow-sm">Mark All A</button>
            <button onclick="bulkMarkStatus('WO')" class="px-3 py-1.5 bg-indigo-600 hover:bg-indigo-700 text-white rounded text-xs font-bold transition shadow-sm">Mark All WO</button>
            <button onclick="bulkMarkStatus('')" class="px-2.5 py-1.5 bg-slate-200 hover:bg-slate-300 text-slate-700 rounded text-xs font-bold transition">Clear</button>
          </div>
        </div>

        <!-- Daily Register Table -->
        <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
          <div class="max-h-[620px] overflow-y-auto">
            <table class="w-full text-left text-xs text-slate-700">
              <thead class="sticky top-0 bg-slate-100/95 backdrop-blur-sm text-slate-600 font-semibold uppercase border-b border-slate-200 z-10">
                <tr>
                  <th class="py-3 px-4 w-16">#</th>
                  <th class="py-3 px-4 w-28">Paycode</th>
                  <th class="py-3 px-6">Employee Name</th>
                  <th class="py-3 px-6">Department</th>
                  <th class="py-3 px-6 text-center">Current Status</th>
                  <th class="py-3 px-6 text-center w-72">Fast-Mark Actions</th>
                </tr>
              </thead>
              <tbody id="daily-attendance-tbody" class="divide-y divide-slate-100">
                <!-- Dynamically Rendered -->
              </tbody>
            </table>
          </div>
        </div>
      </section>


      <!-- ==================== 3. TAB: EMPLOYEE DIRECTORY ==================== -->
      <section id="content-directory" class="space-y-6 hidden">
        <div class="bg-white rounded-xl p-5 border border-slate-200 shadow-sm flex items-center justify-between gap-4">
          <div class="flex items-center gap-3 flex-1">
            <div class="relative w-80">
              <i class="fa-solid fa-magnifying-glass absolute left-3 top-2.5 text-slate-400 text-xs"></i>
              <input type="text" id="dir-search" oninput="renderDirectoryTable()" placeholder="Search directory by name or paycode..." class="w-full pl-9 pr-3 py-2 text-xs bg-slate-50 border border-slate-200 rounded-lg focus:outline-none" />
            </div>
            <select id="dir-dept-filter" onchange="renderDirectoryTable()" class="text-xs bg-slate-50 border border-slate-200 rounded-lg px-3 py-2 font-medium text-slate-700 focus:outline-none">
              <option value="ALL">All Departments</option>
            </select>
          </div>
          <div class="flex items-center gap-2">
            <button onclick="openAddEmployeeModal()" class="px-4 py-2 bg-tableau-blue hover:bg-blue-800 text-white text-xs font-bold rounded-lg shadow-sm">
              <i class="fa-solid fa-user-plus mr-1"></i> Add Record
            </button>
          </div>
        </div>

        <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
          <div class="max-h-[620px] overflow-y-auto">
            <table class="w-full text-left text-xs text-slate-700">
              <thead class="sticky top-0 bg-slate-100/95 backdrop-blur-sm text-slate-600 font-semibold uppercase border-b border-slate-200 z-10">
                <tr>
                  <th class="py-3.5 px-6">Paycode</th>
                  <th class="py-3.5 px-6">Full Name</th>
                  <th class="py-3.5 px-6">Department</th>
                  <th class="py-3.5 px-6">Designation</th>
                  <th class="py-3.5 px-6">Status</th>
                  <th class="py-3.5 px-6 text-right">Actions</th>
                </tr>
              </thead>
              <tbody id="directory-tbody" class="divide-y divide-slate-100">
                <!-- Dynamically populated -->
              </tbody>
            </table>
          </div>
        </div>
      </section>


      <!-- ==================== 4. TAB: MONTHLY MUSTER REGISTER ==================== -->
      <section id="content-muster" class="space-y-6 hidden">
        <div class="bg-white rounded-xl p-5 border border-slate-200 shadow-sm flex flex-wrap items-center justify-between gap-4">
          <div class="flex flex-wrap items-center gap-3">
            <label class="text-xs font-bold text-slate-600 uppercase">Muster Period:</label>
            <select id="muster-year" onchange="renderMusterGrid()" class="text-xs bg-slate-50 border border-slate-200 rounded-lg px-3 py-1.5 font-bold text-slate-700">
              <option value="2025">2025</option>
              <option value="2026" selected>2026</option>
              <option value="2027">2027</option>
            </select>
            <select id="muster-month" onchange="renderMusterGrid()" class="text-xs bg-slate-50 border border-slate-200 rounded-lg px-3 py-1.5 font-bold text-slate-700">
              <option value="0">January</option>
              <option value="1">February</option>
              <option value="2">March</option>
              <option value="3">April</option>
              <option value="4">May</option>
              <option value="5">June</option>
              <option value="6">July</option>
              <option value="7">August</option>
              <option value="8">September</option>
              <option value="9">October</option>
              <option value="10">November</option>
              <option value="11">December</option>
            </select>

            <select id="muster-dept-filter" onchange="renderMusterGrid()" class="text-xs bg-slate-50 border border-slate-200 rounded-lg px-3 py-1.5 font-medium text-slate-700">
              <option value="ALL">All Departments</option>
            </select>
          </div>

          <div class="flex items-center gap-2">
            <button onclick="exportMusterToExcel()" class="px-4 py-2 bg-emerald-600 hover:bg-emerald-700 text-white rounded-lg text-xs font-bold flex items-center gap-1.5 shadow-sm transition">
              <i class="fa-solid fa-file-excel"></i>
              <span>Export Muster Sheet</span>
            </button>
          </div>
        </div>

        <!-- 31 Days Matrix Grid -->
        <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
          <div class="overflow-x-auto max-h-[640px]">
            <table class="w-full text-center text-[11px] text-slate-700 border-collapse">
              <thead id="muster-thead" class="sticky-header bg-slate-100 text-slate-600 uppercase font-bold border-b border-slate-200">
                <!-- Dynamically generated days 1..31 -->
              </tbody>
              <tbody id="muster-tbody" class="divide-y divide-slate-100">
                <!-- Dynamically generated employee row -->
              </tbody>
            </table>
          </div>
        </div>
      </section>


      <!-- ==================== 5. TAB: DEPARTMENT ANALYTICS ==================== -->
      <section id="content-analytics" class="space-y-6 hidden">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm">
            <h4 class="text-sm font-bold text-slate-700 uppercase mb-4">Department Attendance Rate (%)</h4>
            <div class="h-80">
              <canvas id="deptRateBarChart"></canvas>
            </div>
          </div>
          <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm">
            <h4 class="text-sm font-bold text-slate-700 uppercase mb-4">Headcount Distribution by Unit</h4>
            <div class="h-80 flex items-center justify-center">
              <canvas id="deptHeadcountPieChart"></canvas>
            </div>
          </div>
        </div>

        <!-- Full Analytics Table -->
        <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
          <div class="p-4 bg-slate-50 border-b border-slate-200">
            <h4 class="text-xs font-bold text-slate-700 uppercase">Department Performance Scorecard</h4>
          </div>
          <div class="overflow-x-auto">
            <table class="w-full text-left text-xs text-slate-700">
              <thead class="bg-slate-100 text-slate-600 uppercase font-semibold">
                <tr>
                  <th class="py-3 px-6">Department Name</th>
                  <th class="py-3 px-4 text-center">Roster Size</th>
                  <th class="py-3 px-4 text-center">Present</th>
                  <th class="py-3 px-4 text-center">Absent</th>
                  <th class="py-3 px-4 text-center">Leaves</th>
                  <th class="py-3 px-4 text-center">WO</th>
                  <th class="py-3 px-6 text-center">Attendance Efficiency</th>
                </tr>
              </thead>
              <tbody id="analytics-tbody" class="divide-y divide-slate-100">
                <!-- Injected via JS -->
              </tbody>
            </table>
          </div>
        </div>
      </section>


      <!-- ==================== 6. TAB: IMPORT / EXPORT ==================== -->
      <section id="content-importexport" class="space-y-6 hidden">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <!-- SheetJS File Upload Card -->
          <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm space-y-4">
            <div class="flex items-center gap-3">
              <span class="w-10 h-10 rounded-lg bg-emerald-50 text-emerald-600 flex items-center justify-center text-xl">
                <i class="fa-solid fa-file-excel"></i>
              </span>
              <div>
                <h4 class="text-sm font-bold text-slate-800 uppercase">Import Roster via Excel / CSV</h4>
                <p class="text-xs text-slate-500">Supports .xlsx, .xls, and .csv with columns: Paycode, Name, Department</p>
              </div>
            </div>

            <div class="border-2 border-dashed border-slate-300 rounded-xl p-6 text-center hover:bg-slate-50 transition cursor-pointer" onclick="document.getElementById('excelFileInput').click()">
              <input type="file" id="excelFileInput" accept=".xlsx, .xls, .csv" class="hidden" onchange="handleExcelUpload(event)" />
              <i class="fa-solid fa-cloud-arrow-up text-3xl text-slate-400 mb-2"></i>
              <p class="text-xs font-semibold text-slate-700">Click to browse or drag & drop attendance roster</p>
              <p class="text-[10px] text-slate-400 mt-1">Processed client-side with SheetJS</p>
            </div>

            <div class="text-[11px] bg-slate-50 p-3 rounded-lg text-slate-600 border border-slate-200">
              <span class="font-bold">Format requirement:</span> The uploaded file must contain column headers named <code>Paycode</code>, <code>Name</code>, and <code>Department</code>.
            </div>
          </div>

          <!-- Backup & Recovery Card -->
          <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm space-y-4">
            <div class="flex items-center gap-3">
              <span class="w-10 h-10 rounded-lg bg-blue-50 text-tableau-blue flex items-center justify-center text-xl">
                <i class="fa-solid fa-database"></i>
              </span>
              <div>
                <h4 class="text-sm font-bold text-slate-800 uppercase">Backup, Restore & Reset</h4>
                <p class="text-xs text-slate-500">Client-side localStorage state manager</p>
              </div>
            </div>

            <div class="flex flex-col gap-3 pt-2">
              <button onclick="downloadJsonBackup()" class="w-full py-2.5 px-4 bg-tableau-blue hover:bg-blue-800 text-white rounded-lg text-xs font-bold flex items-center justify-center gap-2 shadow-sm transition">
                <i class="fa-solid fa-download"></i>
                <span>Download Complete JSON Backup</span>
              </button>

              <label class="w-full py-2.5 px-4 bg-slate-100 hover:bg-slate-200 text-slate-700 rounded-lg text-xs font-bold flex items-center justify-center gap-2 border border-slate-300 cursor-pointer transition">
                <i class="fa-solid fa-upload"></i>
                <span>Restore JSON Backup</span>
                <input type="file" id="jsonFileInput" accept=".json" class="hidden" onchange="handleJsonRestore(event)" />
              </label>

              <button onclick="resetToFactoryDefault()" class="w-full py-2.5 px-4 bg-rose-50 hover:bg-rose-100 text-rose-700 rounded-lg text-xs font-bold flex items-center justify-center gap-2 border border-rose-200 transition">
                <i class="fa-solid fa-rotate-left"></i>
                <span>Reset to Preloaded 204 Records</span>
              </button>
            </div>
          </div>
        </div>

        <!-- Quick Excel Exports -->
        <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm">
          <h4 class="text-sm font-bold text-slate-800 uppercase mb-4">Direct Excel Export Reports</h4>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <button onclick="exportDailyToExcel()" class="p-4 border border-slate-200 rounded-lg text-left hover:border-tableau-teal hover:bg-slate-50 transition flex items-center justify-between">
              <div>
                <p class="text-xs font-bold text-slate-800">Export Today's Attendance Sheet</p>
                <p class="text-[11px] text-slate-500">Includes current selected date logs, status, and time stamps</p>
              </div>
              <i class="fa-solid fa-file-arrow-down text-emerald-600 text-lg"></i>
            </button>
            <button onclick="exportMusterToExcel()" class="p-4 border border-slate-200 rounded-lg text-left hover:border-tableau-teal hover:bg-slate-50 transition flex items-center justify-between">
              <div>
                <p class="text-xs font-bold text-slate-800">Export Full Monthly Muster Roll</p>
                <p class="text-[11px] text-slate-500">Includes day-by-day matrix and employee attendance % totals</p>
              </div>
              <i class="fa-solid fa-file-excel text-emerald-600 text-lg"></i>
            </button>
          </div>
        </div>
      </section>

    </div>
  </main>


  <!-- ======================== MODALS ======================== -->

  <!-- Modal: Add / Edit Employee -->
  <div id="employeeModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
    <div class="bg-white rounded-2xl shadow-2xl max-w-md w-full overflow-hidden border border-slate-200 animate-fadeIn">
      <div class="px-6 py-4 bg-tableau-sidebar text-white flex justify-between items-center">
        <h3 id="modalEmpTitle" class="text-sm font-bold tracking-wide uppercase">Add New Employee</h3>
        <button onclick="closeEmployeeModal()" class="text-slate-400 hover:text-white"><i class="fa-solid fa-xmark text-lg"></i></button>
      </div>
      <form id="employeeForm" onsubmit="handleEmployeeFormSubmit(event)" class="p-6 space-y-4">
        <input type="hidden" id="emp-edit-mode" value="ADD" />
        <input type="hidden" id="emp-original-paycode" value="" />

        <div>
          <label class="block text-xs font-bold text-slate-600 uppercase mb-1">Paycode / Emp ID *</label>
          <input type="text" id="emp-paycode" required class="w-full text-xs bg-slate-50 border border-slate-300 rounded-lg p-2.5 focus:ring-1 focus:ring-tableau-teal outline-none font-semibold text-slate-800" placeholder="e.g. 2481" />
        </div>

        <div>
          <label class="block text-xs font-bold text-slate-600 uppercase mb-1">Full Employee Name *</label>
          <input type="text" id="emp-name" required class="w-full text-xs bg-slate-50 border border-slate-300 rounded-lg p-2.5 focus:ring-1 focus:ring-tableau-teal outline-none font-semibold text-slate-800 uppercase" placeholder="e.g. PRADEEP KUAMR TIWARI" />
        </div>

        <div>
          <label class="block text-xs font-bold text-slate-600 uppercase mb-1">Department *</label>
          <select id="emp-department" required class="w-full text-xs bg-slate-50 border border-slate-300 rounded-lg p-2.5 focus:ring-1 focus:ring-tableau-teal outline-none font-semibold text-slate-800">
            <!-- Dynamically populated -->
          </select>
        </div>

        <div>
          <label class="block text-xs font-bold text-slate-600 uppercase mb-1">Designation / Role</label>
          <input type="text" id="emp-designation" class="w-full text-xs bg-slate-50 border border-slate-300 rounded-lg p-2.5 focus:ring-1 focus:ring-tableau-teal outline-none font-semibold text-slate-800" placeholder="e.g. Technician / Operator / Staff" value="Operator" />
        </div>

        <div class="pt-2 flex items-center justify-end gap-2">
          <button type="button" onclick="closeEmployeeModal()" class="px-4 py-2 text-xs font-bold text-slate-600 hover:bg-slate-100 rounded-lg">Cancel</button>
          <button type="submit" class="px-5 py-2 text-xs font-bold bg-tableau-blue hover:bg-blue-800 text-white rounded-lg shadow-sm">Save Record</button>
        </div>
      </form>
    </div>
  </div>

  <!-- Modal: Manage Departments -->
  <div id="deptModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
    <div class="bg-white rounded-2xl shadow-2xl max-w-lg w-full overflow-hidden border border-slate-200">
      <div class="px-6 py-4 bg-tableau-sidebar text-white flex justify-between items-center">
        <h3 class="text-sm font-bold tracking-wide uppercase">Department Hierarchy Manager</h3>
        <button onclick="closeDeptModal()" class="text-slate-400 hover:text-white"><i class="fa-solid fa-xmark text-lg"></i></button>
      </div>
      <div class="p-6 space-y-4">
        <!-- Add department form -->
        <div class="flex items-center gap-2">
          <input type="text" id="new-dept-name" placeholder="New Department Name..." class="flex-1 text-xs bg-slate-50 border border-slate-300 rounded-lg p-2.5 outline-none uppercase font-semibold text-slate-800" />
          <button onclick="addNewDepartment()" class="px-4 py-2.5 bg-tableau-teal hover:bg-teal-700 text-white text-xs font-bold rounded-lg shrink-0">
            <i class="fa-solid fa-plus mr-1"></i> Add Unit
          </button>
        </div>

        <!-- Department pill list -->
        <div>
          <label class="block text-xs font-bold text-slate-500 uppercase mb-2">Active Departments List (<span id="modal-dept-count">0</span>)</label>
          <div id="dept-pills-container" class="max-h-60 overflow-y-auto flex flex-wrap gap-2 p-3 bg-slate-50 rounded-xl border border-slate-200">
            <!-- Badges populated dynamically -->
          </div>
        </div>

        <div class="pt-2 flex justify-end">
          <button type="button" onclick="closeDeptModal()" class="px-4 py-2 text-xs font-bold text-slate-600 bg-slate-100 hover:bg-slate-200 rounded-lg">Done</button>
        </div>
      </div>
    </div>
  </div>


  <!-- ======================== APPLICATION LOGIC ======================== -->
  <script>
    /**
     * CORE STATE MANAGEMENT
     */
    const STORAGE_KEY_EMPLOYEES = 'tableau_hr_employees_v1';
    const STORAGE_KEY_ATTENDANCE = 'tableau_hr_attendance_v1';
    const STORAGE_KEY_DEPTS = 'tableau_hr_depts_v1';

    // Standard preloaded departments requested
    const DEFAULT_DEPARTMENTS = [
      "CHARGING", "ELECTRICAL", "ETP", "FINISHER", "LAB", "MAINTENANCE",
      "OFFICE", "STORE", "UTILITY", "GENERAL", "FORKLIFT", "WL GROUP A",
      "BALER", "BL GROUP B", "CAN", "CREEL", "CRIMPER", "CUTTER",
      "DRYER", "SPINNING", "WASHING LINE HELPER", "WL GROUP B", "WL GROUP C",
      "WL GROUP D", "BURNOUT", "TAKEUP", "HEAD", "BL GROUP A", "FIBRELINE",
      "SWEEPER", "SECURITY", "LADIES", "FIBERLINE GROUP C"
    ];

    // Seed data generator creating the exact requested employees and expanding to full 204 active roster
    function generateInitialEmployees() {
      // 1. Mandatory specified records
      const exactList = [
        { paycode: '2481', name: 'PRADEEP KUAMR TIWARI', department: 'CHARGING', designation: 'Senior Operator' },
        { paycode: '2287', name: 'JAYANTA BAGARTI', department: 'ELECTRICAL', designation: 'Electrician' },
        { paycode: '12', name: 'ROHANKUMAR BHARATBHAI PRAJAPATI', department: 'ELECTRICAL', designation: 'Electrical Engineer' },
        { paycode: '11', name: 'VANSIYA RAHULKUMAR', department: 'ELECTRICAL', designation: 'Assistant Electrician' },
        { paycode: '1963', name: 'DEVENDRA SINGH', department: 'ETP', designation: 'Plant Specialist' },
        { paycode: '2363', name: 'NRUSINHA GOUDA', department: 'FINISHER', designation: 'Finishing Technician' },
        { paycode: '1351', name: 'BABBU PRASAD YADAV', department: 'LAB', designation: 'Lab Analyst' },
        { paycode: '2063', name: 'KAMLESH KUMAR YADAV', department: 'LAB', designation: 'Quality Inspector' },
        { paycode: '836', name: 'KISUN KUMAR YADAV', department: 'LAB', designation: 'Senior Chemist' },
        { paycode: '2457', name: 'PATEL PRIYANSHEEKUMARI BHUPATBHAI', department: 'LAB', designation: 'Lab Incharge' },
        { paycode: '2473', name: 'VIKAS DUBE', department: 'LAB', designation: 'QC Technician' },
        { paycode: '18', name: 'BANWARI LAL', department: 'MAINTENANCE', designation: 'Maintenance Supervisor' }
      ];

      // Pool of realistic factory roster names for remaining records to complete 204
      const firstNames = [
        "RAMESH", "SURESH", "MAHESH", "ANIL", "SUNIL", "MANOJ", "SANJAY", "RAJESH", "VIJAY", "AJAY",
        "MUKESH", "DINESH", "VINOD", "ASHOK", "RAKESH", "PRAVEEN", "AMIT", "SUMIT", "PANKAJ", "DEEPAK",
        "ARUN", "DHARMENDRA", "JITENDRA", "NARENDRA", "SATISH", "KRISHNA", "SHIV", "HARI", "GANESH", "MOHAN"
      ];
      const lastNames = [
        "SHARMA", "VERMA", "GUPTA", "SINGH", "YADAV", "PATEL", "KUMAR", "MISHRA", "PANDEY", "TIWARI",
        "DAS", "BAG", "SAHU", "PRASAD", "CHAUHAN", "PASWAN", "THAKUR", "GOUDA", "MALIK", "JENA"
      ];

      const records = [...exactList];
      let currentId = 101;
      let deptIndex = 0;

      while (records.length < 204) {
        const paycodeStr = String(currentId++);
        // Skip if already in exactList
        if (records.some(r => r.paycode === paycodeStr)) continue;

        const dept = DEFAULT_DEPARTMENTS[deptIndex % DEFAULT_DEPARTMENTS.length];
        deptIndex++;

        const fName = firstNames[(records.length * 7) % firstNames.length];
        const lName = lastNames[(records.length * 11) % lastNames.length];
        const fullName = `${fName} ${lName}`;

        records.push({
          paycode: paycodeStr,
          name: fullName,
          department: dept,
          designation: dept.includes('OFFICE') ? 'Executive' : (dept.includes('SECURITY') ? 'Guard' : 'Operator')
        });
      }

      return records;
    }

    // App State
    let employees = [];
    let departments = [];
    let attendanceLogs = {}; // Key format: YYYY-MM-DD -> { [paycode]: 'P'|'A'|'HD'|'L'|'WO' }
    let currentDateStr = '';
    let charts = {};

    // Initialize application
    window.addEventListener('DOMContentLoaded', () => {
      initDate();
      loadStorageData();
      populateDepartmentDropdowns();
      initCharts();
      renderAllViews();
    });

    /**
     * DATE HANDLING
     */
    function initDate() {
      const today = new Date();
      currentDateStr = formatDateKey(today);
      const datePicker = document.getElementById('global-date-picker');
      datePicker.value = currentDateStr;

      // Seed monthly muster selector with today's month/year
      document.getElementById('muster-year').value = today.getFullYear();
      document.getElementById('muster-month').value = today.getMonth();
    }

    function formatDateKey(date) {
      const d = new Date(date);
      let month = '' + (d.getMonth() + 1);
      let day = '' + d.getDate();
      const year = d.getFullYear();

      if (month.length < 2) month = '0' + month;
      if (day.length < 2) day = '0' + day;
      return [year, month, day].join('-');
    }

    function handleDateChange(newVal) {
      if (!newVal) return;
      currentDateStr = newVal;
      renderAllViews();
    }

    /**
     * LOCALSTORAGE & INITIAL DATA
     */
    function loadStorageData() {
      // 1. Departments
      const savedDepts = localStorage.getItem(STORAGE_KEY_DEPTS);
      if (savedDepts) {
        try { departments = JSON.parse(savedDepts); } catch (e) { departments = [...DEFAULT_DEPARTMENTS]; }
      } else {
        departments = [...DEFAULT_DEPARTMENTS];
        saveDepartments();
      }

      // 2. Employees
      const savedEmps = localStorage.getItem(STORAGE_KEY_EMPLOYEES);
      if (savedEmps) {
        try { employees = JSON.parse(savedEmps); } catch (e) { employees = generateInitialEmployees(); }
      } else {
        employees = generateInitialEmployees();
        saveEmployees();
      }

      // 3. Attendance Logs
      const savedAtt = localStorage.getItem(STORAGE_KEY_ATTENDANCE);
      if (savedAtt) {
        try { attendanceLogs = JSON.parse(savedAtt); } catch (e) { attendanceLogs = {}; }
      } else {
        attendanceLogs = {};
        // Auto-seed sample attendance for today for immediate demonstration
        seedInitialDailyAttendance(currentDateStr);
      }
    }

    function seedInitialDailyAttendance(dateKey) {
      if (!attendanceLogs[dateKey]) attendanceLogs[dateKey] = {};
      employees.forEach((emp, i) => {
        // High operational present rate (~85%)
        const rand = (i * 13) % 100;
        if (rand < 82) attendanceLogs[dateKey][emp.paycode] = 'P';
        else if (rand < 90) attendanceLogs[dateKey][emp.paycode] = 'A';
        else if (rand < 94) attendanceLogs[dateKey][emp.paycode] = 'HD';
        else if (rand < 97) attendanceLogs[dateKey][emp.paycode] = 'L';
        else attendanceLogs[dateKey][emp.paycode] = 'WO';
      });
      saveAttendance();
    }

    function saveEmployees() {
      localStorage.setItem(STORAGE_KEY_EMPLOYEES, JSON.stringify(employees));
      document.getElementById('sidebar-total-count').textContent = employees.length;
    }

    function saveDepartments() {
      localStorage.setItem(STORAGE_KEY_DEPTS, JSON.stringify(departments));
      document.getElementById('sidebar-dept-count').textContent = departments.length;
    }

    function saveAttendance() {
      localStorage.setItem(STORAGE_KEY_ATTENDANCE, JSON.stringify(attendanceLogs));
    }

    /**
     * NAVIGATION TAB SWITCHING
     */
    function switchTab(tabId) {
      const tabs = ['overview', 'daily', 'directory', 'muster', 'analytics', 'importexport'];
      tabs.forEach(t => {
        const sec = document.getElementById(`content-${t}`);
        const nav = document.getElementById(`tab-${t}`);
        if (t === tabId) {
          sec.classList.remove('hidden');
          nav.classList.add('nav-active');
          nav.classList.remove('text-slate-400');
        } else {
          sec.classList.add('hidden');
          nav.classList.remove('nav-active');
          nav.classList.add('text-slate-400');
        }
      });

      // Update header title
      const titles = {
        overview: 'Operational Overview & Live Metrics',
        daily: 'Daily Fast Attendance Register',
        directory: 'Employee Master Directory',
        muster: 'Monthly Muster Attendance Matrix',
        analytics: 'Department-Wise Analytics & Scorecards',
        importexport: 'Data Import / Export & SheetJS Portal'
      };
      document.getElementById('view-title').textContent = titles[tabId];

      // Refresh view-specific rendering
      if (tabId === 'overview') updateOverviewMetrics();
      if (tabId === 'daily') renderDailyTable();
      if (tabId === 'directory') renderDirectoryTable();
      if (tabId === 'muster') renderMusterGrid();
      if (tabId === 'analytics') updateAnalyticsTab();
    }

    /**
     * POPULATE DROPDOWNS
     */
    function populateDepartmentDropdowns() {
      const filterSelects = ['daily-dept-filter', 'dir-dept-filter', 'muster-dept-filter'];
      filterSelects.forEach(selId => {
        const el = document.getElementById(selId);
        if (!el) return;
        const currentVal = el.value || 'ALL';
        el.innerHTML = '<option value="ALL">All Departments</option>';
        departments.sort().forEach(d => {
          const opt = document.createElement('option');
          opt.value = d;
          opt.textContent = d;
          el.appendChild(opt);
        });
        el.value = currentVal;
      });

      // Modal select
      const empDeptSel = document.getElementById('emp-department');
      if (empDeptSel) {
        empDeptSel.innerHTML = '';
        departments.sort().forEach(d => {
          const opt = document.createElement('option');
          opt.value = d;
          opt.textContent = d;
          empDeptSel.appendChild(opt);
        });
      }

      renderDeptPills();
      document.getElementById('sidebar-dept-count').textContent = departments.length;
    }

    /**
     * RENDER ALL VIEWS
     */
    function renderAllViews() {
      updateOverviewMetrics();
      renderDailyTable();
      renderDirectoryTable();
      renderMusterGrid();
      updateAnalyticsTab();
      document.getElementById('sidebar-total-count').textContent = employees.length;
    }

    /**
     * 1. TAB: OVERVIEW & CHARTS
     */
    function updateOverviewMetrics() {
      const todayLogs = attendanceLogs[currentDateStr] || {};
      let p = 0, a = 0, hd = 0, l = 0, wo = 0, unmarked = 0;
      const total = employees.length;

      employees.forEach(emp => {
        const st = todayLogs[emp.paycode];
        if (st === 'P') p++;
        else if (st === 'A') a++;
        else if (st === 'HD') hd++;
        else if (st === 'L') l++;
        else if (st === 'WO') wo++;
        else unmarked++;
      });

      // Update KPI Cards
      document.getElementById('kpi-total-strength').textContent = total;
      document.getElementById('kpi-present-count').textContent = p;
      document.getElementById('kpi-absent-count').textContent = a;
      document.getElementById('kpi-leave-count').textContent = hd + l;
      document.getElementById('kpi-wo-count').textContent = wo;

      document.getElementById('kpi-present-pct').textContent = total ? `${Math.round((p / total) * 100)}%` : '0%';
      document.getElementById('kpi-absent-pct').textContent = total ? `${Math.round((a / total) * 100)}%` : '0%';
      document.getElementById('kpi-leave-pct').textContent = total ? `${Math.round(((hd + l) / total) * 100)}%` : '0%';
      document.getElementById('kpi-wo-pct').textContent = total ? `${Math.round((wo / total) * 100)}%` : '0%';

      // Update Donut Chart
      if (charts.donut) {
        charts.donut.data.datasets[0].data = [p, a, hd, l, wo, unmarked];
        charts.donut.update();
      }

      // Department bar breakdown
      const deptStats = {};
      departments.forEach(d => { deptStats[d] = { total: 0, present: 0, absent: 0, other: 0 }; });
      employees.forEach(emp => {
        if (!deptStats[emp.department]) deptStats[emp.department] = { total: 0, present: 0, absent: 0, other: 0 };
        deptStats[emp.department].total++;
        const st = todayLogs[emp.paycode];
        if (st === 'P') deptStats[emp.department].present++;
        else if (st === 'A') deptStats[emp.department].absent++;
        else if (st) deptStats[emp.department].other++;
      });

      // Top 8 departments for clean overview bar chart
      const sortedDepts = Object.keys(deptStats).sort((a,b) => deptStats[b].total - deptStats[a].total).slice(0, 9);
      if (charts.bar) {
        charts.bar.data.labels = sortedDepts;
        charts.bar.data.datasets[0].data = sortedDepts.map(d => deptStats[d].present);
        charts.bar.data.datasets[1].data = sortedDepts.map(d => deptStats[d].absent);
        charts.bar.update();
      }

      // Populate Overview Dept table
      const tbody = document.getElementById('overview-dept-tbody');
      tbody.innerHTML = '';
      sortedDepts.forEach(dept => {
        const s = deptStats[dept];
        const pct = s.total > 0 ? Math.round((s.present / s.total) * 100) : 0;
        const tr = document.createElement('tr');
        tr.className = "hover:bg-slate-50/80 transition";
        tr.innerHTML = `
          <td class="py-3 px-6 font-bold text-slate-800">${dept}</td>
          <td class="py-3 px-4 text-center font-semibold">${s.total}</td>
          <td class="py-3 px-4 text-center text-emerald-600 font-bold">${s.present}</td>
          <td class="py-3 px-4 text-center text-rose-600 font-bold">${s.absent}</td>
          <td class="py-3 px-4 text-center text-amber-600 font-medium">${s.other}</td>
          <td class="py-3 px-6 text-center">
            <div class="flex items-center justify-center gap-2">
              <div class="w-24 bg-slate-200 rounded-full h-2 overflow-hidden">
                <div class="bg-teal-600 h-2 rounded-full" style="width: ${pct}%"></div>
              </div>
              <span class="font-bold text-xs ${pct >= 80 ? 'text-emerald-600' : 'text-amber-600'}">${pct}%</span>
            </div>
          </td>
        `;
        tbody.appendChild(tr);
      });
    }

    function initCharts() {
      // Donut Chart
      const donutCtx = document.getElementById('attendanceDonutChart').getContext('2d');
      charts.donut = new Chart(donutCtx, {
        type: 'doughnut',
        data: {
          labels: ['Present', 'Absent', 'Half Day', 'Leave', 'Week Off', 'Unmarked'],
          datasets: [{
            data: [0, 0, 0, 0, 0, 0],
            backgroundColor: ['#10B981', '#EF4444', '#F59E0B', '#8B5CF6', '#6366F1', '#CBD5E1'],
            borderWidth: 2,
            borderColor: '#FFFFFF'
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: { position: 'bottom', labels: { boxWidth: 12, font: { size: 10, family: 'Inter' } } }
          },
          cutout: '68%'
        }
      });

      // Overview Bar Chart
      const barCtx = document.getElementById('deptBarChart').getContext('2d');
      charts.bar = new Chart(barCtx, {
        type: 'bar',
        data: {
          labels: [],
          datasets: [
            { label: 'Present', data: [], backgroundColor: '#2B83BA', borderRadius: 4 },
            { label: 'Absent', data: [], backgroundColor: '#EF4444', borderRadius: 4 }
          ]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          scales: {
            x: { grid: { display: false }, ticks: { font: { size: 10 } } },
            y: { grid: { color: '#F1F5F9' }, ticks: { stepSize: 5, font: { size: 10 } } }
          },
          plugins: {
            legend: { position: 'top', labels: { boxWidth: 12, font: { size: 11 } } }
          }
        }
      });

      // Department Analytics Tab Bar Chart
      const deptRateCtx = document.getElementById('deptRateBarChart').getContext('2d');
      charts.deptRate = new Chart(deptRateCtx, {
        type: 'bar',
        data: {
          labels: [],
          datasets: [{
            label: 'Attendance %',
            data: [],
            backgroundColor: '#1F538D',
            borderRadius: 4
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          indexAxis: 'y',
          scales: {
            x: { max: 100, ticks: { callback: v => v + '%' } },
            y: { ticks: { font: { size: 9 } } }
          },
          plugins: { legend: { display: false } }
        }
      });

      // Department Headcount Pie
      const deptPieCtx = document.getElementById('deptHeadcountPieChart').getContext('2d');
      charts.deptPie = new Chart(deptPieCtx, {
        type: 'pie',
        data: {
          labels: [],
          datasets: [{
            data: [],
            backgroundColor: [
              '#1F538D', '#2B83BA', '#2CA02C', '#E37222', '#9467BD',
              '#17BECF', '#BCBD22', '#7F7F7F', '#E377C2', '#8C564B'
            ]
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: { position: 'right', labels: { boxWidth: 10, font: { size: 9 } } }
          }
        }
      });
    }

    /**
     * 2. TAB: DAILY ATTENDANCE ENTRY
     */
    function renderDailyTable() {
      const search = (document.getElementById('daily-search').value || '').trim().toLowerCase();
      const deptFilter = document.getElementById('daily-dept-filter').value;
      const statusFilter = document.getElementById('daily-status-filter').value;
      const todayLogs = attendanceLogs[currentDateStr] || {};

      const tbody = document.getElementById('daily-attendance-tbody');
      tbody.innerHTML = '';

      let filtered = employees.filter(emp => {
        const matchSearch = emp.name.toLowerCase().includes(search) || emp.paycode.toLowerCase().includes(search);
        const matchDept = deptFilter === 'ALL' || emp.department === deptFilter;
        const curStatus = todayLogs[emp.paycode] || '';
        let matchStatus = true;
        if (statusFilter === 'UNMARKED') matchStatus = !curStatus;
        else if (statusFilter !== 'ALL') matchStatus = curStatus === statusFilter;
        return matchSearch && matchDept && matchStatus;
      });

      // Update Department summary badge
      const activeDeptPool = deptFilter === 'ALL' ? employees : employees.filter(e => e.department === deptFilter);
      const activeDeptPresent = activeDeptPool.filter(e => (todayLogs[e.paycode] === 'P')).length;
      const badgePct = activeDeptPool.length ? Math.round((activeDeptPresent / activeDeptPool.length) * 100) : 0;
      document.getElementById('badge-pct').textContent = `${badgePct}%`;
      document.getElementById('badge-ratio').textContent = `${activeDeptPresent}/${activeDeptPool.length}`;

      if (filtered.length === 0) {
        tbody.innerHTML = `<tr><td colspan="6" class="text-center py-8 text-slate-400 font-medium text-xs">No employee records match the filter criteria.</td></tr>`;
        return;
      }

      filtered.forEach((emp, index) => {
        const curStatus = todayLogs[emp.paycode] || '';
        const tr = document.createElement('tr');
        tr.className = "hover:bg-slate-50 transition border-b border-slate-100";

        let badgeHtml = '<span class="px-2 py-0.5 rounded text-[10px] font-bold bg-slate-100 text-slate-400 border border-slate-200">UNMARKED</span>';
        if (curStatus === 'P') badgeHtml = '<span class="px-2.5 py-0.5 rounded-full text-[10px] font-bold bg-emerald-100 text-emerald-700 border border-emerald-300"><i class="fa-solid fa-check mr-1"></i>PRESENT</span>';
        else if (curStatus === 'A') badgeHtml = '<span class="px-2.5 py-0.5 rounded-full text-[10px] font-bold bg-rose-100 text-rose-700 border border-rose-300"><i class="fa-solid fa-xmark mr-1"></i>ABSENT</span>';
        else if (curStatus === 'HD') badgeHtml = '<span class="px-2.5 py-0.5 rounded-full text-[10px] font-bold bg-amber-100 text-amber-700 border border-amber-300">HALF DAY</span>';
        else if (curStatus === 'L') badgeHtml = '<span class="px-2.5 py-0.5 rounded-full text-[10px] font-bold bg-purple-100 text-purple-700 border border-purple-300">LEAVE</span>';
        else if (curStatus === 'WO') badgeHtml = '<span class="px-2.5 py-0.5 rounded-full text-[10px] font-bold bg-indigo-100 text-indigo-700 border border-indigo-300">WEEK OFF</span>';

        tr.innerHTML = `
          <td class="py-3 px-4 text-slate-400 font-mono text-[11px]">${index + 1}</td>
          <td class="py-3 px-4 font-mono font-bold text-slate-800">${emp.paycode}</td>
          <td class="py-3 px-6 font-semibold text-slate-900">${emp.name}</td>
          <td class="py-3 px-6">
            <span class="px-2 py-0.5 bg-slate-100 text-slate-700 rounded text-[10px] font-semibold">${emp.department}</span>
          </td>
          <td class="py-3 px-6 text-center">${badgeHtml}</td>
          <td class="py-3 px-6 text-center">
            <div class="inline-flex rounded-md shadow-sm gap-1" role="group">
              <button onclick="setEmpStatus('${emp.paycode}', 'P')" class="px-2.5 py-1 text-[11px] font-bold rounded ${curStatus === 'P' ? 'bg-emerald-600 text-white' : 'bg-slate-100 hover:bg-emerald-50 text-emerald-700 border border-slate-200'}">P</button>
              <button onclick="setEmpStatus('${emp.paycode}', 'A')" class="px-2.5 py-1 text-[11px] font-bold rounded ${curStatus === 'A' ? 'bg-rose-600 text-white' : 'bg-slate-100 hover:bg-rose-50 text-rose-700 border border-slate-200'}">A</button>
              <button onclick="setEmpStatus('${emp.paycode}', 'HD')" class="px-2.5 py-1 text-[11px] font-bold rounded ${curStatus === 'HD' ? 'bg-amber-500 text-white' : 'bg-slate-100 hover:bg-amber-50 text-amber-700 border border-slate-200'}">HD</button>
              <button onclick="setEmpStatus('${emp.paycode}', 'L')" class="px-2.5 py-1 text-[11px] font-bold rounded ${curStatus === 'L' ? 'bg-purple-600 text-white' : 'bg-slate-100 hover:bg-purple-50 text-purple-700 border border-slate-200'}">L</button>
              <button onclick="setEmpStatus('${emp.paycode}', 'WO')" class="px-2.5 py-1 text-[11px] font-bold rounded ${curStatus === 'WO' ? 'bg-indigo-600 text-white' : 'bg-slate-100 hover:bg-indigo-50 text-indigo-700 border border-slate-200'}">WO</button>
            </div>
          </td>
        `;
        tbody.appendChild(tr);
      });
    }

    function setEmpStatus(paycode, status) {
      if (!attendanceLogs[currentDateStr]) attendanceLogs[currentDateStr] = {};
      attendanceLogs[currentDateStr][paycode] = status;
      saveAttendance();
      renderDailyTable();
      updateOverviewMetrics();
    }

    function bulkMarkStatus(status) {
      const deptFilter = document.getElementById('daily-dept-filter').value;
      if (!attendanceLogs[currentDateStr]) attendanceLogs[currentDateStr] = {};

      employees.forEach(emp => {
        if (deptFilter === 'ALL' || emp.department === deptFilter) {
          if (status === '') {
            delete attendanceLogs[currentDateStr][emp.paycode];
          } else {
            attendanceLogs[currentDateStr][emp.paycode] = status;
          }
        }
      });

      saveAttendance();
      renderDailyTable();
      updateOverviewMetrics();
    }

    /**
     * 3. TAB: EMPLOYEE DIRECTORY & CRUD
     */
    function renderDirectoryTable() {
      const search = (document.getElementById('dir-search').value || '').trim().toLowerCase();
      const deptFilter = document.getElementById('dir-dept-filter').value;
      const tbody = document.getElementById('directory-tbody');
      tbody.innerHTML = '';

      const filtered = employees.filter(emp => {
        const mSearch = emp.name.toLowerCase().includes(search) || emp.paycode.toLowerCase().includes(search);
        const mDept = deptFilter === 'ALL' || emp.department === deptFilter;
        return mSearch && mDept;
      });

      filtered.forEach(emp => {
        const tr = document.createElement('tr');
        tr.className = "hover:bg-slate-50 transition";
        tr.innerHTML = `
          <td class="py-3.5 px-6 font-mono font-bold text-slate-800">${emp.paycode}</td>
          <td class="py-3.5 px-6 font-bold text-slate-900">${emp.name}</td>
          <td class="py-3.5 px-6">
            <span class="px-2.5 py-1 bg-slate-100 border border-slate-200 rounded-md font-semibold text-slate-700 text-[11px]">${emp.department}</span>
          </td>
          <td class="py-3.5 px-6 text-slate-500 font-medium">${emp.designation || 'Staff'}</td>
          <td class="py-3.5 px-6">
            <span class="inline-flex items-center gap-1.5 px-2 py-0.5 rounded-full text-[10px] font-bold bg-emerald-50 text-emerald-700 border border-emerald-200">
              <span class="w-1.5 h-1.5 rounded-full bg-emerald-500"></span> Active
            </span>
          </td>
          <td class="py-3.5 px-6 text-right space-x-2">
            <button onclick="editEmployee('${emp.paycode}')" class="px-2.5 py-1 text-slate-600 hover:text-tableau-blue hover:bg-blue-50 rounded text-xs transition"><i class="fa-solid fa-pen-to-square mr-1"></i>Edit</button>
            <button onclick="deleteEmployee('${emp.paycode}')" class="px-2.5 py-1 text-rose-600 hover:bg-rose-50 rounded text-xs transition"><i class="fa-solid fa-trash-can mr-1"></i>Delete</button>
          </td>
        `;
        tbody.appendChild(tr);
      });
    }

    function openAddEmployeeModal() {
      document.getElementById('modalEmpTitle').textContent = "Add New Employee";
      document.getElementById('emp-edit-mode').value = "ADD";
      document.getElementById('emp-paycode').value = '';
      document.getElementById('emp-paycode').readOnly = false;
      document.getElementById('emp-name').value = '';
      document.getElementById('emp-designation').value = 'Operator';
      document.getElementById('employeeModal').classList.remove('hidden');
    }

    function editEmployee(paycode) {
      const emp = employees.find(e => e.paycode === paycode);
      if (!emp) return;
      document.getElementById('modalEmpTitle').textContent = "Edit Employee Details";
      document.getElementById('emp-edit-mode').value = "EDIT";
      document.getElementById('emp-original-paycode').value = paycode;
      document.getElementById('emp-paycode').value = emp.paycode;
      document.getElementById('emp-paycode').readOnly = true;
      document.getElementById('emp-name').value = emp.name;
      document.getElementById('emp-department').value = emp.department;
      document.getElementById('emp-designation').value = emp.designation || 'Operator';
      document.getElementById('employeeModal').classList.remove('hidden');
    }

    function closeEmployeeModal() {
      document.getElementById('employeeModal').classList.add('hidden');
    }

    function handleEmployeeFormSubmit(e) {
      e.preventDefault();
      const mode = document.getElementById('emp-edit-mode').value;
      const paycode = document.getElementById('emp-paycode').value.trim();
      const name = document.getElementById('emp-name').value.trim().toUpperCase();
      const department = document.getElementById('emp-department').value;
      const designation = document.getElementById('emp-designation').value.trim();

      if (mode === 'ADD') {
        if (employees.some(emp => emp.paycode === paycode)) {
          alert('Paycode already exists! Please use a unique ID.');
          return;
        }
        employees.unshift({ paycode, name, department, designation });
      } else {
        const orig = document.getElementById('emp-original-paycode').value;
        const target = employees.find(emp => emp.paycode === orig);
        if (target) {
          target.name = name;
          target.department = department;
          target.designation = designation;
        }
      }

      saveEmployees();
      closeEmployeeModal();
      renderAllViews();
    }

    function deleteEmployee(paycode) {
      if (!confirm(`Are you sure you want to remove employee paycode: ${paycode}?`)) return;
      employees = employees.filter(e => e.paycode !== paycode);
      saveEmployees();
      renderAllViews();
    }

    /**
     * DEPARTMENT MANAGEMENT MODAL
     */
    function openManageDeptModal() {
      renderDeptPills();
      document.getElementById('deptModal').classList.remove('hidden');
    }

    function closeDeptModal() {
      document.getElementById('deptModal').classList.add('hidden');
      populateDepartmentDropdowns();
    }

    function renderDeptPills() {
      const container = document.getElementById('dept-pills-container');
      if (!container) return;
      container.innerHTML = '';
      document.getElementById('modal-dept-count').textContent = departments.length;

      departments.sort().forEach(dept => {
        const count = employees.filter(e => e.department === dept).length;
        const pill = document.createElement('div');
        pill.className = "inline-flex items-center gap-1.5 px-3 py-1 bg-white border border-slate-300 rounded-lg text-xs font-semibold text-slate-700 shadow-2xs";
        pill.innerHTML = `
          <span>${dept}</span>
          <span class="text-[10px] bg-slate-100 text-slate-500 rounded px-1.5 py-0.2 font-mono">${count}</span>
          <button onclick="removeDepartment('${dept}')" class="text-slate-400 hover:text-rose-600 ml-1"><i class="fa-solid fa-xmark text-xs"></i></button>
        `;
        container.appendChild(pill);
      });
    }

    function addNewDepartment() {
      const input = document.getElementById('new-dept-name');
      const val = input.value.trim().toUpperCase();
      if (!val) return;
      if (departments.includes(val)) {
        alert('Department already exists.');
        return;
      }
      departments.push(val);
      saveDepartments();
      input.value = '';
      renderDeptPills();
      populateDepartmentDropdowns();
    }

    function removeDepartment(dept) {
      if (employees.some(e => e.department === dept)) {
        alert(`Cannot remove "${dept}" because employees are currently assigned to it.`);
        return;
      }
      departments = departments.filter(d => d !== dept);
      saveDepartments();
      renderDeptPills();
      populateDepartmentDropdowns();
    }

    /**
     * 4. TAB: MONTHLY MUSTER REGISTER
     */
    function renderMusterGrid() {
      const year = parseInt(document.getElementById('muster-year').value);
      const month = parseInt(document.getElementById('muster-month').value);
      const deptFilter = document.getElementById('muster-dept-filter').value;
      const daysInMonth = new Date(year, month + 1, 0).getDate();

      const thead = document.getElementById('muster-thead');
      const tbody = document.getElementById('muster-tbody');

      // Build Table Header
      let headerHtml = `<tr>
        <th class="py-3 px-3 sticky-col-left w-16 text-left bg-slate-100">Code</th>
        <th class="py-3 px-4 sticky-col-left-2 w-48 text-left bg-slate-100">Name</th>
        <th class="py-3 px-3 w-32 text-left bg-slate-100">Dept</th>`;

      for (let day = 1; day <= daysInMonth; day++) {
        headerHtml += `<th class="py-2 px-1 w-7 text-center font-mono font-bold">${day}</th>`;
      }

      headerHtml += `
        <th class="py-3 px-2 text-center text-emerald-700 bg-emerald-50/50 font-bold">P</th>
        <th class="py-3 px-2 text-center text-rose-700 bg-rose-50/50 font-bold">A</th>
        <th class="py-3 px-2 text-center text-amber-700 bg-amber-50/50 font-bold">HD</th>
        <th class="py-3 px-2 text-center text-purple-700 bg-purple-50/50 font-bold">L</th>
        <th class="py-3 px-2 text-center text-indigo-700 bg-indigo-50/50 font-bold">WO</th>
        <th class="py-3 px-3 text-center bg-teal-50 text-teal-800 font-bold">Att %</th>
      </tr>`;
      thead.innerHTML = headerHtml;

      // Filter employees
      const filtered = employees.filter(e => deptFilter === 'ALL' || e.department === deptFilter);
      tbody.innerHTML = '';

      filtered.forEach(emp => {
        let pCount = 0, aCount = 0, hdCount = 0, lCount = 0, woCount = 0;
        let dayCells = '';

        for (let day = 1; day <= daysInMonth; day++) {
          const dStr = [
            year,
            String(month + 1).padStart(2, '0'),
            String(day).padStart(2, '0')
          ].join('-');

          const status = (attendanceLogs[dStr] && attendanceLogs[dStr][emp.paycode]) || '-';

          let cellColor = "text-slate-300";
          if (status === 'P') { pCount++; cellColor = "text-emerald-700 bg-emerald-50 font-bold"; }
          else if (status === 'A') { aCount++; cellColor = "text-rose-700 bg-rose-50 font-bold"; }
          else if (status === 'HD') { hdCount++; cellColor = "text-amber-700 bg-amber-50 font-bold"; }
          else if (status === 'L') { lCount++; cellColor = "text-purple-700 bg-purple-50 font-bold"; }
          else if (status === 'WO') { woCount++; cellColor = "text-indigo-700 bg-indigo-50 font-bold"; }

          dayCells += `<td class="py-2 px-1 border-r border-slate-100 ${cellColor}">${status}</td>`;
        }

        // Attendance Percentage Calculation (P + 0.5*HD / (Total Days - WO))
        const effectiveWorkingDays = daysInMonth - woCount;
        const attendedValue = pCount + (hdCount * 0.5);
        const attPct = effectiveWorkingDays > 0 ? Math.round((attendedValue / effectiveWorkingDays) * 100) : 0;

        const tr = document.createElement('tr');
        tr.className = "hover:bg-slate-50/60 transition border-b border-slate-100 text-[11px]";
        tr.innerHTML = `
          <td class="py-2.5 px-3 font-mono font-bold text-slate-800 sticky-col-left text-left border-r border-slate-100">${emp.paycode}</td>
          <td class="py-2.5 px-4 font-semibold text-slate-900 sticky-col-left-2 text-left truncate max-w-[190px] border-r border-slate-100" title="${emp.name}">${emp.name}</td>
          <td class="py-2.5 px-3 text-left text-slate-500 truncate max-w-[130px] border-r border-slate-100">${emp.department}</td>
          ${dayCells}
          <td class="py-2.5 px-2 font-bold text-emerald-700 bg-emerald-50/30">${pCount}</td>
          <td class="py-2.5 px-2 font-bold text-rose-700 bg-rose-50/30">${aCount}</td>
          <td class="py-2.5 px-2 font-bold text-amber-700 bg-amber-50/30">${hdCount}</td>
          <td class="py-2.5 px-2 font-bold text-purple-700 bg-purple-50/30">${lCount}</td>
          <td class="py-2.5 px-2 font-bold text-indigo-700 bg-indigo-50/30">${woCount}</td>
          <td class="py-2.5 px-3 font-bold bg-teal-50 text-teal-800">${attPct}%</td>
        `;
        tbody.appendChild(tr);
      });
    }

    /**
     * 5. TAB: DEPARTMENT ANALYTICS
     */
    function updateAnalyticsTab() {
      const todayLogs = attendanceLogs[currentDateStr] || {};
      const stats = {};

      departments.forEach(d => {
        stats[d] = { total: 0, p: 0, a: 0, l: 0, wo: 0 };
      });

      employees.forEach(e => {
        if (!stats[e.department]) stats[e.department] = { total: 0, p: 0, a: 0, l: 0, wo: 0 };
        stats[e.department].total++;
        const st = todayLogs[e.paycode];
        if (st === 'P') stats[e.department].p++;
        else if (st === 'A') stats[e.department].a++;
        else if (st === 'HD' || st === 'L') stats[e.department].l++;
        else if (st === 'WO') stats[e.department].wo++;
      });

      const deptsList = Object.keys(stats).filter(d => stats[d].total > 0).sort();
      const rates = deptsList.map(d => {
        const s = stats[d];
        return s.total > 0 ? Math.round((s.p / s.total) * 100) : 0;
      });

      if (charts.deptRate) {
        charts.deptRate.data.labels = deptsList;
        charts.deptRate.data.datasets[0].data = rates;
        charts.deptRate.update();
      }

      if (charts.deptPie) {
        charts.deptPie.data.labels = deptsList.slice(0, 10);
        charts.deptPie.data.datasets[0].data = deptsList.slice(0, 10).map(d => stats[d].total);
        charts.deptPie.update();
      }

      // Populate Analytics Scorecard
      const tbody = document.getElementById('analytics-tbody');
      tbody.innerHTML = '';
      deptsList.forEach(dept => {
        const s = stats[dept];
        const pct = s.total > 0 ? Math.round((s.p / s.total) * 100) : 0;
        const tr = document.createElement('tr');
        tr.className = "hover:bg-slate-50 transition border-b border-slate-100";
        tr.innerHTML = `
          <td class="py-3 px-6 font-bold text-slate-800">${dept}</td>
          <td class="py-3 px-4 text-center font-bold">${s.total}</td>
          <td class="py-3 px-4 text-center font-bold text-emerald-600">${s.p}</td>
          <td class="py-3 px-4 text-center font-bold text-rose-600">${s.a}</td>
          <td class="py-3 px-4 text-center font-medium text-amber-600">${s.l}</td>
          <td class="py-3 px-4 text-center font-medium text-indigo-600">${s.wo}</td>
          <td class="py-3 px-6 text-center">
            <span class="px-2.5 py-1 rounded-full text-xs font-bold ${pct >= 85 ? 'bg-emerald-100 text-emerald-700' : (pct >= 70 ? 'bg-amber-100 text-amber-700' : 'bg-rose-100 text-rose-700')}">
              ${pct}%
            </span>
          </td>
        `;
        tbody.appendChild(tr);
      });
    }

    /**
     * 6. TAB: IMPORT / EXPORT (SheetJS & JSON)
     */
    function handleExcelUpload(event) {
      const file = event.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = function(e) {
        try {
          const data = new Uint8Array(e.target.result);
          const workbook = XLSX.read(data, { type: 'array' });
          const firstSheet = workbook.Sheets[workbook.SheetNames[0]];
          const json = XLSX.utils.sheet_to_json(firstSheet);

          if (!json || json.length === 0) {
            alert('Uploaded file contains no data.');
            return;
          }

          let added = 0;
          let updated = 0;

          json.forEach(row => {
            // Flexible column header matching
            const paycode = String(row.Paycode || row.PAYCODE || row.paycode || row['Emp Code'] || '').trim();
            const name = String(row.Name || row.NAME || row.name || row['Employee Name'] || '').trim().toUpperCase();
            const department = String(row.Department || row.DEPARTMENT || row.department || row.Dept || 'GENERAL').trim().toUpperCase();
            const designation = String(row.Designation || row.DESIGNATION || row.designation || 'Operator').trim();

            if (!paycode || !name) return;

            // Register department if new
            if (!departments.includes(department)) departments.push(department);

            const existingIndex = employees.findIndex(emp => emp.paycode === paycode);
            if (existingIndex >= 0) {
              employees[existingIndex] = { paycode, name, department, designation };
              updated++;
            } else {
              employees.push({ paycode, name, department, designation });
              added++;
            }
          });

          saveDepartments();
          saveEmployees();
          populateDepartmentDropdowns();
          renderAllViews();
          alert(`Import successful! ${added} new employees added, ${updated} records updated.`);
        } catch (err) {
          alert('Error parsing Excel file. Please ensure it is a valid format.');
          console.error(err);
        }
      };
      reader.readAsArrayBuffer(file);
    }

    function exportDailyToExcel() {
      const todayLogs = attendanceLogs[currentDateStr] || {};
      const exportData = employees.map((emp, i) => ({
        "Sr No": i + 1,
        "Paycode": emp.paycode,
        "Employee Name": emp.name,
        "Department": emp.department,
        "Designation": emp.designation || 'Staff',
        "Date": currentDateStr,
        "Status": todayLogs[emp.paycode] || 'UNMARKED'
      }));

      const worksheet = XLSX.utils.json_to_sheet(exportData);
      const workbook = XLSX.utils.book_new();
      XLSX.utils.book_append_sheet(workbook, worksheet, `Daily_${currentDateStr}`);
      XLSX.writeFile(workbook, `Attendance_${currentDateStr}.xlsx`);
    }

    function exportMusterToExcel() {
      const year = parseInt(document.getElementById('muster-year').value);
      const month = parseInt(document.getElementById('muster-month').value);
      const daysInMonth = new Date(year, month + 1, 0).getDate();

      const exportRows = employees.map(emp => {
        const row = {
          "Paycode": emp.paycode,
          "Employee Name": emp.name,
          "Department": emp.department
        };

        let pCount = 0, aCount = 0, hdCount = 0, lCount = 0, woCount = 0;

        for (let d = 1; d <= daysInMonth; d++) {
          const dStr = [year, String(month + 1).padStart(2, '0'), String(d).padStart(2, '0')].join('-');
          const status = (attendanceLogs[dStr] && attendanceLogs[dStr][emp.paycode]) || '-';
          row[`Day ${d}`] = status;

          if (status === 'P') pCount++;
          else if (status === 'A') aCount++;
          else if (status === 'HD') hdCount++;
          else if (status === 'L') lCount++;
          else if (status === 'WO') woCount++;
        }

        const effectiveDays = daysInMonth - woCount;
        const attPct = effectiveDays > 0 ? Math.round(((pCount + hdCount * 0.5) / effectiveDays) * 100) : 0;

        row['Present (P)'] = pCount;
        row['Absent (A)'] = aCount;
        row['Half Day (HD)'] = hdCount;
        row['Leave (L)'] = lCount;
        row['Week Off (WO)'] = woCount;
        row['Attendance %'] = `${attPct}%`;

        return row;
      });

      const ws = XLSX.utils.json_to_sheet(exportRows);
      const wb = XLSX.utils.book_new();
      XLSX.utils.book_append_sheet(wb, ws, `Muster_${year}_${month + 1}`);
      XLSX.writeFile(wb, `Monthly_Muster_${year}_${month + 1}.xlsx`);
    }

    function downloadJsonBackup() {
      const backup = {
        departments,
        employees,
        attendanceLogs,
        exportTimestamp: new Date().toISOString()
      };
      const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(backup, null, 2));
      const downloadAnchor = document.createElement('a');
      downloadAnchor.setAttribute("href", dataStr);
      downloadAnchor.setAttribute("download", `HR_Tableau_Backup_${currentDateStr}.json`);
      document.body.appendChild(downloadAnchor);
      downloadAnchor.click();
      downloadAnchor.remove();
    }

    function handleJsonRestore(event) {
      const file = event.target.files[0];
      if (!file) return;
      const reader = new FileReader();
      reader.onload = function(e) {
        try {
          const parsed = JSON.parse(e.target.result);
          if (parsed.employees && parsed.departments) {
            employees = parsed.employees;
            departments = parsed.departments;
            attendanceLogs = parsed.attendanceLogs || {};
            saveEmployees();
            saveDepartments();
            saveAttendance();
            populateDepartmentDropdowns();
            renderAllViews();
            alert('JSON Backup successfully restored!');
          } else {
            alert('Invalid backup file schema.');
          }
        } catch (err) {
          alert('Failed to parse backup JSON.');
        }
      };
      reader.readAsText(file);
    }

    function resetToFactoryDefault() {
      if (!confirm('This will restore the original 204 roster records and reset local modifications. Continue?')) return;
      localStorage.removeItem(STORAGE_KEY_EMPLOYEES);
      localStorage.removeItem(STORAGE_KEY_DEPTS);
      localStorage.removeItem(STORAGE_KEY_ATTENDANCE);
      loadStorageData();
      populateDepartmentDropdowns();
      renderAllViews();
      alert('System reset to default 204 records.');
    }
  </script>
</body>
</html>
