<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Emancy Global Hub - Ledger & Bookkeeping Web App</title>

  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            brand: {
              blue: '#1E3A8A',
              gold: '#D97706',
              accent: '#059669',
              dark: '#0F172A'
            }
          }
        }
      }
    }
  </script>
</head>
<body class="bg-gray-100 text-gray-800 font-sans min-h-screen flex flex-col justify-between">

  <!-- ================= AUTH CONTAINER ================= -->
  <div id="authContainer" class="fixed inset-0 bg-slate-900 bg-opacity-95 z-50 flex items-center justify-center p-4">
    <div class="bg-white rounded-2xl shadow-2xl max-w-md w-full p-6 sm:p-8 relative">
      <div class="text-center mb-6">
        <div class="w-16 h-16 bg-blue-900 text-amber-400 rounded-full flex items-center justify-center text-2xl font-bold mx-auto mb-3 shadow-lg">
          EGH
        </div>
        <h1 class="text-2xl font-extrabold text-blue-950">Emancy Global Hub</h1>
        <p class="text-xs text-gray-500 tracking-wider font-semibold uppercase mt-1">Ledger & Bookkeeping Web App</p>
      </div>

      <!-- Auth Toggle Buttons -->
      <div class="flex border-b mb-6 text-sm font-semibold">
        <button id="tabSignIn" onclick="switchAuthTab('login')" class="flex-1 pb-2 border-b-2 border-blue-900 text-blue-900 font-bold">Sign In</button>
        <button id="tabSignUp" onclick="switchAuthTab('signup')" class="flex-1 pb-2 border-b-2 border-transparent text-gray-400 hover:text-gray-600">Sign Up</button>
      </div>

      <!-- Form -->
      <form id="authForm" onsubmit="handleAuth(event)">
        <div class="space-y-4">
          <div>
            <label class="block text-xs font-semibold uppercase text-gray-600 mb-1">Email Address</label>
            <input type="email" id="authEmail" required placeholder="alugbuomoses@gmail.com" class="w-full px-4 py-2.5 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-blue-900 focus:outline-none">
          </div>
          <div>
            <label class="block text-xs font-semibold uppercase text-gray-600 mb-1">Password</label>
            <input type="password" id="authPassword" required placeholder="••••••••" class="w-full px-4 py-2.5 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-blue-900 focus:outline-none">
          </div>
          <div id="confirmPasswordGroup" class="hidden">
            <label class="block text-xs font-semibold uppercase text-gray-600 mb-1">Confirm Password</label>
            <input type="password" id="authConfirmPassword" placeholder="••••••••" class="w-full px-4 py-2.5 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-blue-900 focus:outline-none">
          </div>
          <div>
            <label class="block text-xs font-semibold uppercase text-gray-600 mb-1">Select Operating Branch</label>
            <select id="authBranch" class="w-full px-4 py-2.5 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-blue-900 focus:outline-none bg-white">
              <option value="Headquarters">Headquarters (Moses)</option>
              <option value="PTT Branch">PTT Branch (Wife)</option>
            </select>
          </div>
        </div>

        <button type="submit" id="authSubmitBtn" class="w-full mt-6 bg-blue-900 hover:bg-blue-950 text-white font-bold py-3 rounded-lg text-sm transition shadow-md">
          Sign In
        </button>
      </form>
    </div>
  </div>

  <!-- ================= MAIN APP WRAPPER ================= -->
  <div id="appMain" class="flex-grow hidden flex flex-col">
    <!-- Header -->
    <header class="bg-blue-950 text-white shadow-lg sticky top-0 z-30">
      <div class="max-w-7xl mx-auto px-4 py-3 sm:px-6 flex flex-wrap items-center justify-between gap-4">
        <div class="flex items-center space-x-3">
          <div class="bg-amber-500 text-blue-950 font-black px-3 py-1 rounded-lg text-lg tracking-wider">
            EGH
          </div>
          <div>
            <h1 class="text-lg font-bold leading-tight">Emancy Global Hub</h1>
            <p class="text-xs text-amber-400 font-medium">Ledger & Bookkeeping System</p>
          </div>
        </div>

        <!-- Branch Selector & Clock -->
        <div class="flex items-center space-x-4">
          <div class="bg-blue-900/80 px-3 py-1.5 rounded-lg border border-blue-700/50 flex items-center space-x-2 text-xs">
            <i class="fa-solid fa-store text-amber-400"></i>
            <span class="text-gray-300">Branch:</span>
            <select id="currentBranchSelect" onchange="changeActiveBranch(this.value)" class="bg-transparent text-white font-semibold focus:outline-none cursor-pointer">
              <option value="Headquarters" class="text-gray-900">Headquarters (Moses)</option>
              <option value="PTT Branch" class="text-gray-900">PTT Branch (Wife)</option>
            </select>
          </div>

          <!-- Digital Clock & Automation Status -->
          <div class="hidden sm:flex flex-col text-right">
            <span id="digitalClock" class="text-sm font-mono font-bold text-amber-300">10:29:50 PM</span>
            <span class="text-[10px] text-gray-300 flex items-center justify-end gap-1">
              <span id="autoTimerPulse" class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
              Auto-Audit @ 10:30 PM
            </span>
          </div>

          <button onclick="logout()" class="text-xs bg-red-600/80 hover:bg-red-600 px-3 py-1.5 rounded text-white font-semibold transition">
            <i class="fa-solid fa-right-from-bracket mr-1"></i> Logout
          </button>
        </div>
      </div>
    </header>

    <!-- Navigation Tabs -->
    <nav class="bg-blue-900 text-white shadow">
      <div class="max-w-7xl mx-auto px-4 flex overflow-x-auto space-x-6 text-sm font-medium">
        <button id="navTabEntry" onclick="switchMainTab('entry')" class="py-3 border-b-2 border-amber-400 text-amber-300 flex items-center space-x-2 whitespace-nowrap font-bold">
          <i class="fa-solid fa-pen-to-square"></i>
          <span>New Entry</span>
        </button>
        <button id="navTabSummary" onclick="switchMainTab('summary')" class="py-3 border-b-2 border-transparent text-blue-200 hover:text-white flex items-center space-x-2 whitespace-nowrap">
          <i class="fa-solid fa-list-check"></i>
          <span>Daily Summary</span>
        </button>
        <button id="navTabArchive" onclick="switchMainTab('archive')" class="py-3 border-b-2 border-transparent text-blue-200 hover:text-white flex items-center space-x-2 whitespace-nowrap">
          <i class="fa-solid fa-box-archive"></i>
          <span>Web Archive</span>
        </button>
      </div>
    </nav>

    <!-- Main Content Area -->
    <main class="max-w-7xl mx-auto px-4 py-6 sm:px-6 flex-grow w-full">

      <!-- AUTOMATION SCHEDULE STATUS BAR -->
      <div class="mb-6 p-4 rounded-xl bg-gradient-to-r from-slate-800 to-blue-950 text-white flex flex-col md:flex-row items-center justify-between gap-4 shadow-md border border-slate-700">
        <div class="flex items-center space-x-3">
          <div class="p-2.5 bg-amber-500/20 text-amber-400 rounded-lg">
            <i class="fa-solid fa-clock text-xl"></i>
          </div>
          <div>
            <h4 class="text-xs uppercase font-bold text-amber-400 tracking-wider">Automated Daily Schedule</h4>
            <p class="text-xs text-gray-300">10:30 PM: Auto Total Audit & Summary | 11:00 PM: Auto Archive & Reset Daily Sheet</p>
          </div>
        </div>
        <div class="flex items-center space-x-2">
          <button onclick="trigger1030Audit(true)" class="px-3 py-1.5 bg-amber-600 hover:bg-amber-500 text-white rounded-lg text-xs font-semibold shadow transition">
            <i class="fa-solid fa-bolt mr-1"></i> Test 10:30 PM Audit
          </button>
          <button onclick="trigger1100Archive(true)" class="px-3 py-1.5 bg-emerald-600 hover:bg-emerald-500 text-white rounded-lg text-xs font-semibold shadow transition">
            <i class="fa-solid fa-file-export mr-1"></i> Test 11:00 PM Archive
          </button>
        </div>
      </div>

      <!-- PAGE 1: NEW ENTRY FORM -->
      <section id="pageEntry" class="space-y-6">
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">

          <!-- Entry Form -->
          <div class="lg:col-span-2 bg-white rounded-2xl shadow-sm border border-gray-200 p-6">
            <div class="flex items-center justify-between mb-6 pb-3 border-b border-gray-100">
              <h2 class="text-lg font-bold text-gray-800 flex items-center gap-2">
                <i class="fa-solid fa-circle-plus text-blue-900"></i>
                Record New Exchange Entry
              </h2>
              <span id="activeBranchBadge" class="text-xs px-2.5 py-1 bg-blue-100 text-blue-800 font-bold rounded-full">
                Headquarters
              </span>
            </div>

            <form id="transactionForm" onsubmit="saveTransaction(event)" class="space-y-5">
              <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                <div>
                  <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">
                    Account / Customer Name <span class="text-red-500">*</span>
                  </label>
                  <input type="text" id="entryAccountName" required placeholder="e.g. John Doe / Account Name" class="w-full px-4 py-2.5 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-blue-900 focus:outline-none">
                </div>

                <div>
                  <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">
                    Account Number <span class="text-gray-400 font-normal">(Optional)</span>
                  </label>
                  <input type="text" id="entryAccountNumber" placeholder="e.g. 0123456789" class="w-full px-4 py-2.5 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-blue-900 focus:outline-none">
                </div>
              </div>

              <!-- Currency Toggle -->
              <div>
                <label class="block text-xs font-semibold text-gray-600 uppercase mb-2">Currency Received</label>
                <div class="grid grid-cols-2 gap-3">
                  <button type="button" id="btnRecNaira" onclick="setReceivedCurrency('NGN')" class="py-3 px-4 rounded-xl border-2 flex items-center justify-center space-x-2 font-bold transition border-blue-900 bg-blue-50 text-blue-900">
                    <span class="text-lg">₦</span>
                    <span>Received Naira (NGN)</span>
                  </button>
                  <button type="button" id="btnRecFCFA" onclick="setReceivedCurrency('FCFA')" class="py-3 px-4 rounded-xl border-2 flex items-center justify-center space-x-2 font-bold transition border-gray-200 text-gray-600 hover:border-gray-300">
                    <span class="text-lg">CFA</span>
                    <span>Received Cefer (FCFA)</span>
                  </button>
                </div>
              </div>

              <!-- Amounts & Dynamic Calculation -->
              <div class="bg-gray-50 p-4 rounded-xl border border-gray-200 space-y-4">
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                  <div>
                    <label id="lblReceivedAmount" class="block text-xs font-bold text-gray-700 uppercase mb-1">
                      Amount Received (₦ NGN)
                    </label>
                    <input type="number" step="any" id="entryAmountReceived" required oninput="calculateExchange()" placeholder="0.00" class="w-full px-4 py-2.5 border border-gray-300 rounded-lg text-base font-bold text-gray-800 focus:ring-2 focus:ring-blue-900 focus:outline-none">
                  </div>

                  <div>
                    <label id="lblEquivalentAmount" class="block text-xs font-bold text-gray-700 uppercase mb-1">
                      Equivalent Given out (CFA FCFA)
                    </label>
                    <input type="number" step="any" id="entryEquivalentGiven" readonly class="w-full px-4 py-2.5 border border-gray-200 bg-gray-100 rounded-lg text-base font-bold text-blue-900 focus:outline-none">
                  </div>
                </div>

                <!-- Live Profit Badge -->
                <div class="flex items-center justify-between pt-2 border-t border-gray-200 text-sm">
                  <span class="text-xs font-bold text-gray-500 uppercase">Estimated Transaction Profit:</span>
                  <span id="entryLiveProfit" class="font-extrabold text-emerald-600 text-base">₦0.00</span>
                </div>
              </div>

              <button type="submit" class="w-full bg-blue-900 hover:bg-blue-950 text-white font-extrabold py-3.5 rounded-xl shadow-lg transition flex items-center justify-center space-x-2">
                <i class="fa-solid fa-floppy-disk"></i>
                <span>SAVE ENTRY TO SUMMARY</span>
              </button>
            </form>
          </div>

          <!-- Rates Helper -->
          <div class="space-y-6">
            <div class="bg-gradient-to-br from-blue-900 to-slate-900 text-white rounded-2xl p-6 shadow-md border border-blue-800">
              <h3 class="text-sm font-extrabold text-amber-400 uppercase tracking-wider mb-4 flex items-center justify-between">
                <span><i class="fa-solid fa-calculator mr-2"></i> Standard Conversion Rates</span>
                <button onclick="toggleRateEdit()" class="text-xs text-blue-200 underline font-normal hover:text-white">Edit</button>
              </h3>

              <div class="space-y-4 text-sm">
                <div class="p-3 bg-white/10 rounded-xl border border-white/10">
                  <p class="text-xs text-blue-200">1,000 NGN → Cefer Equivalent</p>
                  <div class="flex items-center justify-between mt-1">
                    <span class="font-bold text-lg text-white">₦1,000 NGN =</span>
                    <div class="flex items-center space-x-1">
                      <input type="number" id="rateNgnToFcfa" value="430" disabled onchange="calculateExchange()" class="w-20 px-2 py-1 text-center text-blue-950 font-bold rounded bg-amber-300 disabled:bg-amber-400">
                      <span class="font-bold text-amber-300">FCFA</span>
                    </div>
                  </div>
                </div>

                <div class="p-3 bg-white/10 rounded-xl border border-white/10">
                  <p class="text-xs text-blue-200">1,000 FCFA → Naira Equivalent</p>
                  <div class="flex items-center justify-between mt-1">
                    <span class="font-bold text-lg text-white">1,000 FCFA =</span>
                    <div class="flex items-center space-x-1">
                      <input type="number" id="rateFcfaToNgn" value="2270" disabled onchange="calculateExchange()" class="w-24 px-2 py-1 text-center text-blue-950 font-bold rounded bg-amber-300 disabled:bg-amber-400">
                      <span class="font-bold text-amber-300">NGN</span>
                    </div>
                  </div>
                </div>
              </div>

              <div class="mt-4 pt-3 border-t border-white/10 text-[11px] text-gray-300">
                <i class="fa-solid fa-shield-halved text-emerald-400 mr-1"></i> Cloud Persistent Enabled: Records are preserved permanently.
              </div>
            </div>
          </div>

        </div>
      </section>

      <!-- PAGE 2: DAILY SUMMARY -->
      <section id="pageSummary" class="hidden space-y-6">
        <div>
          <h2 class="text-lg font-bold text-gray-800 mb-3 flex items-center justify-between">
            <span><i class="fa-solid fa-chart-line text-blue-900 mr-2"></i> Combined Live Multi-Branch Daily Totals</span>
            <span class="text-xs font-normal text-gray-500">Live Cloud Synced</span>
          </h2>

          <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
            <div class="bg-white p-4 rounded-xl border border-gray-200 shadow-sm">
              <p class="text-xs font-bold text-gray-500 uppercase">Total NGN Received</p>
              <h3 id="sumTotalNgnReceived" class="text-xl font-black text-blue-900 mt-1">₦0.00</h3>
              <p id="sumNgnGivenFcfa" class="text-xs text-gray-400 mt-1">Given out: 0 FCFA</p>
            </div>

            <div class="bg-white p-4 rounded-xl border border-gray-200 shadow-sm">
              <p class="text-xs font-bold text-gray-500 uppercase">Total FCFA Received</p>
              <h3 id="sumTotalFcfaReceived" class="text-xl font-black text-indigo-900 mt-1">0 FCFA</h3>
              <p id="sumFcfaGivenNgn" class="text-xs text-gray-400 mt-1">Given out: ₦0.00</p>
            </div>

            <div class="bg-white p-4 rounded-xl border-2 border-emerald-500/30 bg-emerald-50/30 shadow-sm">
              <p class="text-xs font-bold text-emerald-800 uppercase">Grand Total Profit</p>
              <h3 id="sumGrandProfit" class="text-2xl font-black text-emerald-600 mt-1">₦0.00</h3>
              <p class="text-[11px] text-emerald-700 mt-1">Aggregated across HQ & PTT</p>
            </div>

            <div class="bg-white p-4 rounded-xl border border-gray-200 shadow-sm">
              <p class="text-xs font-bold text-gray-500 uppercase">Transactions Count</p>
              <h3 id="sumTxCount" class="text-xl font-black text-gray-800 mt-1">0 Entries</h3>
              <p id="sumBranchBreakdown" class="text-xs text-gray-400 mt-1">HQ: 0 | PTT: 0</p>
            </div>
          </div>
        </div>

        <div class="bg-white rounded-2xl shadow-sm border border-gray-200 p-6">
          <div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 mb-6 pb-4 border-b">
            <div>
              <h3 class="text-base font-extrabold text-gray-800">Active Daily Ledger Records</h3>
              <p class="text-xs text-gray-500">Every entry recorded today before 11:00 PM auto-archiving.</p>
            </div>

            <div class="flex items-center space-x-2">
              <button onclick="filterSummaryTable('ALL')" id="filterBtnALL" class="px-3 py-1.5 rounded-lg text-xs font-bold bg-blue-900 text-white">All Branches</button>
              <button onclick="filterSummaryTable('Headquarters')" id="filterBtnHQ" class="px-3 py-1.5 rounded-lg text-xs font-bold bg-gray-100 text-gray-600 hover:bg-gray-200">Headquarters</button>
              <button onclick="filterSummaryTable('PTT Branch')" id="filterBtnPTT" class="px-3 py-1.5 rounded-lg text-xs font-bold bg-gray-100 text-gray-600 hover:bg-gray-200">PTT Branch</button>
            </div>
          </div>

          <div class="overflow-x-auto">
            <table class="w-full text-left text-sm">
              <thead>
                <tr class="bg-gray-50 border-b border-gray-200 text-[11px] text-gray-500 uppercase font-bold tracking-wider">
                  <th class="p-3">Time / Branch</th>
                  <th class="p-3">Account Name</th>
                  <th class="p-3">Account No.</th>
                  <th class="p-3">Received</th>
                  <th class="p-3">Equivalent Given</th>
                  <th class="p-3 text-right">Profit</th>
                  <th class="p-3 text-center">Action</th>
                </tr>
              </thead>
              <tbody id="summaryTableBody" class="divide-y divide-gray-100">
              </tbody>
            </table>
          </div>
        </div>
      </section>

      <!-- PAGE 3: WEB ARCHIVE -->
      <section id="pageArchive" class="hidden space-y-6">
        <div class="bg-white rounded-2xl shadow-sm border border-gray-200 p-6">
          <div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 mb-6 pb-4 border-b">
            <div>
              <h2 class="text-lg font-bold text-gray-800 flex items-center gap-2">
                <i class="fa-solid fa-box-archive text-amber-600"></i>
                Web Archive Historical Logs
              </h2>
              <p class="text-xs text-gray-500">Sealed at 11:00 PM nightly. Safely stored in permanent storage.</p>
            </div>

            <button onclick="exportArchiveCSV()" class="px-4 py-2 bg-emerald-600 hover:bg-emerald-700 text-white rounded-lg text-xs font-bold shadow flex items-center space-x-1.5">
              <i class="fa-solid fa-file-excel"></i>
              <span>Export CSV</span>
            </button>
          </div>

          <div id="archiveContentArea" class="space-y-4">
          </div>
        </div>
      </section>

    </main>

    <footer class="bg-slate-900 text-gray-400 text-xs py-4 text-center border-t border-slate-800">
      <div class="max-w-7xl mx-auto px-4">
        <p>© <span id="currentYear"></span> <strong>Emancy Global Hub</strong>. All Rights Reserved. Headquarters & PTT Branch Operations.</p>
      </div>
    </footer>
  </div>

  <script>
    let currentUser = null;
    let activeBranch = 'Headquarters';
    let selectedReceivedCurrency = 'NGN';
    let summaryFilter = 'ALL';

    // Permanent multi-key persistent storage engine
    function getStoredData(key) {
      try {
        return JSON.parse(localStorage.getItem(key)) || [];
      } catch(e) {
        return [];
      }
    }

    function setStoredData(key, value) {
      try {
        localStorage.setItem(key, JSON.stringify(value));
      } catch(e) {}
    }

    let transactions = getStoredData('egh_perm_transactions');
    let archiveLogs = getStoredData('egh_perm_archives');

    window.addEventListener('DOMContentLoaded', () => {
      document.getElementById('currentYear').innerText = new Date().getFullYear();
      startDigitalClock();
      checkSavedSession();
      renderSummaryPage();
      renderArchiveLogs();
    });

    function switchAuthTab(tab) {
      const tabSignIn = document.getElementById('tabSignIn');
      const tabSignUp = document.getElementById('tabSignUp');
      const confirmGroup = document.getElementById('confirmPasswordGroup');
      const btn = document.getElementById('authSubmitBtn');

      if (tab === 'login') {
        tabSignIn.className = "flex-1 pb-2 border-b-2 border-blue-900 text-blue-900 font-bold";
        tabSignUp.className = "flex-1 pb-2 border-b-2 border-transparent text-gray-400 hover:text-gray-600";
        confirmGroup.classList.add('hidden');
        btn.innerText = "Sign In";
      } else {
        tabSignUp.className = "flex-1 pb-2 border-b-2 border-blue-900 text-blue-900 font-bold";
        tabSignIn.className = "flex-1 pb-2 border-b-2 border-transparent text-gray-400 hover:text-gray-600";
        confirmGroup.classList.remove('hidden');
        btn.innerText = "Create Account";
      }
    }

    function handleAuth(e) {
      e.preventDefault();
      const email = document.getElementById('authEmail').value;
      const branch = document.getElementById('authBranch').value;

      currentUser = { email, branch };
      localStorage.setItem('egh_user_session', JSON.stringify(currentUser));

      activeBranch = branch;
      document.getElementById('currentBranchSelect').value = branch;
      document.getElementById('activeBranchBadge').innerText = branch;

      document.getElementById('authContainer').classList.add('hidden');
      document.getElementById('appMain').classList.remove('hidden');
    }

    function checkSavedSession() {
      const savedUser = localStorage.getItem('egh_user_session');
      if (savedUser) {
        currentUser = JSON.parse(savedUser);
        activeBranch = currentUser.branch || 'Headquarters';
        document.getElementById('currentBranchSelect').value = activeBranch;
        document.getElementById('activeBranchBadge').innerText = activeBranch;

        document.getElementById('authContainer').classList.add('hidden');
        document.getElementById('appMain').classList.remove('hidden');
      }
    }

    function logout() {
      localStorage.removeItem('egh_user_session');
      location.reload();
    }

    function changeActiveBranch(newBranch) {
      activeBranch = newBranch;
      if (currentUser) {
        currentUser.branch = newBranch;
        localStorage.setItem('egh_user_session', JSON.stringify(currentUser));
      }
      document.getElementById('activeBranchBadge').innerText = newBranch;
      renderSummaryPage();
    }

    function switchMainTab(tab) {
      const pEntry = document.getElementById('pageEntry');
      const pSummary = document.getElementById('pageSummary');
      const pArchive = document.getElementById('pageArchive');

      const tEntry = document.getElementById('navTabEntry');
      const tSummary = document.getElementById('navTabSummary');
      const tArchive = document.getElementById('navTabArchive');

      pEntry.classList.add('hidden');
      pSummary.classList.add('hidden');
      pArchive.classList.add('hidden');

      tEntry.className = "py-3 border-b-2 border-transparent text-blue-200 hover:text-white flex items-center space-x-2 whitespace-nowrap";
      tSummary.className = "py-3 border-b-2 border-transparent text-blue-200 hover:text-white flex items-center space-x-2 whitespace-nowrap";
      tArchive.className = "py-3 border-b-2 border-transparent text-blue-200 hover:text-white flex items-center space-x-2 whitespace-nowrap";

      if (tab === 'entry') {
        pEntry.classList.remove('hidden');
        tEntry.className = "py-3 border-b-2 border-amber-400 text-amber-300 flex items-center space-x-2 font-bold whitespace-nowrap";
      } else if (tab === 'summary') {
        pSummary.classList.remove('hidden');
        tSummary.className = "py-3 border-b-2 border-amber-400 text-amber-300 flex items-center space-x-2 font-bold whitespace-nowrap";
        renderSummaryPage();
      } else if (tab === 'archive') {
        pArchive.classList.remove('hidden');
        tArchive.className = "py-3 border-b-2 border-amber-400 text-amber-300 flex items-center space-x-2 font-bold whitespace-nowrap";
        renderArchiveLogs();
      }
    }

    function setReceivedCurrency(curr) {
      selectedReceivedCurrency = curr;
      const btnNgn = document.getElementById('btnRecNaira');
      const btnFcfa = document.getElementById('btnRecFCFA');
      const lblRec = document.getElementById('lblReceivedAmount');
      const lblEquiv = document.getElementById('lblEquivalentAmount');

      if (curr === 'NGN') {
        btnNgn.className = "py-3 px-4 rounded-xl border-2 flex items-center justify-center space-x-2 font-bold transition border-blue-900 bg-blue-50 text-blue-900";
        btnFcfa.className = "py-3 px-4 rounded-xl border-2 flex items-center justify-center space-x-2 font-bold transition border-gray-200 text-gray-600 hover:border-gray-300";
        lblRec.innerText = "Amount Received (₦ NGN)";
        lblEquiv.innerText = "Equivalent Given out (CFA FCFA)";
      } else {
        btnFcfa.className = "py-3 px-4 rounded-xl border-2 flex items-center justify-center space-x-2 font-bold transition border-blue-900 bg-blue-50 text-blue-900";
        btnNgn.className = "py-3 px-4 rounded-xl border-2 flex items-center justify-center space-x-2 font-bold transition border-gray-200 text-gray-600 hover:border-gray-300";
        lblRec.innerText = "Amount Received (CFA FCFA)";
        lblEquiv.innerText = "Equivalent Given out (₦ NGN)";
      }
      calculateExchange();
    }

    function toggleRateEdit() {
      const r1 = document.getElementById('rateNgnToFcfa');
      const r2 = document.getElementById('rateFcfaToNgn');
      r1.disabled = !r1.disabled;
      r2.disabled = !r2.disabled;
    }

    function calculateExchange() {
      const recAmt = parseFloat(document.getElementById('entryAmountReceived').value) || 0;
      const rateNgnToFcfa = parseFloat(document.getElementById('rateNgnToFcfa').value) || 430;
      const rateFcfaToNgn = parseFloat(document.getElementById('rateFcfaToNgn').value) || 2270;

      let equivAmt = 0;
      let estProfitNgn = 0;

      if (selectedReceivedCurrency === 'NGN') {
        equivAmt = (recAmt / 1000) * rateNgnToFcfa;
        estProfitNgn = recAmt * 0.025;
        document.getElementById('entryEquivalentGiven').value = equivAmt.toFixed(0);
      } else {
        equivAmt = (recAmt / 1000) * rateFcfaToNgn;
        estProfitNgn = equivAmt * 0.025;
        document.getElementById('entryEquivalentGiven').value = equivAmt.toFixed(2);
      }

      document.getElementById('entryLiveProfit').innerText = "₦" + estProfitNgn.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2});
    }

    function saveTransaction(e) {
      e.preventDefault();

      const accountName = document.getElementById('entryAccountName').value;
      const accountNumber = document.getElementById('entryAccountNumber').value || 'N/A';
      const recAmt = parseFloat(document.getElementById('entryAmountReceived').value) || 0;
      const equivAmt = parseFloat(document.getElementById('entryEquivalentGiven').value) || 0;

      let estProfit = selectedReceivedCurrency === 'NGN' ? recAmt * 0.025 : equivAmt * 0.025;

      const newTx = {
        id: 'TX-' + Date.now(),
        timestamp: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' }),
        date: new Date().toISOString().split('T')[0],
        branch: activeBranch,
        accountName: accountName,
        accountNumber: accountNumber,
        receivedCurrency: selectedReceivedCurrency,
        receivedAmount: recAmt,
        equivalentGiven: equivAmt,
        profitNgn: estProfit
      };

      transactions.unshift(newTx);
      setStoredData('egh_perm_transactions', transactions);

      document.getElementById('transactionForm').reset();
      document.getElementById('entryEquivalentGiven').value = '';
      document.getElementById('entryLiveProfit').innerText = '₦0.00';

      alert('Entry saved permanently to daily ledger summary!');
    }

    function deleteTransaction(id) {
      if (confirm('Are you sure you want to remove this record?')) {
        transactions = transactions.filter(t => t.id !== id);
        setStoredData('egh_perm_transactions', transactions);
        renderSummaryPage();
      }
    }

    function filterSummaryTable(branch) {
      summaryFilter = branch;

      document.getElementById('filterBtnALL').className = branch === 'ALL' ? "px-3 py-1.5 rounded-lg text-xs font-bold bg-blue-900 text-white" : "px-3 py-1.5 rounded-lg text-xs font-bold bg-gray-100 text-gray-600 hover:bg-gray-200";
      document.getElementById('filterBtnHQ').className = branch === 'Headquarters' ? "px-3 py-1.5 rounded-lg text-xs font-bold bg-blue-900 text-white" : "px-3 py-1.5 rounded-lg text-xs font-bold bg-gray-100 text-gray-600 hover:bg-gray-200";
      document.getElementById('filterBtnPTT').className = branch === 'PTT Branch' ? "px-3 py-1.5 rounded-lg text-xs font-bold bg-blue-900 text-white" : "px-3 py-1.5 rounded-lg text-xs font-bold bg-gray-100 text-gray-600 hover:bg-gray-200";

      renderSummaryPage();
    }

    function renderSummaryPage() {
      let totNgnRec = 0;
      let totNgnGivenFcfa = 0;
      let totFcfaRec = 0;
      let totFcfaGivenNgn = 0;
      let grandProfit = 0;

      let hqCount = 0;
      let pttCount = 0;

      transactions.forEach(t => {
        if (t.branch === 'Headquarters') hqCount++;
        if (t.branch === 'PTT Branch') pttCount++;

        if (t.receivedCurrency === 'NGN') {
          totNgnRec += t.receivedAmount;
          totNgnGivenFcfa += t.equivalentGiven;
        } else {
          totFcfaRec += t.receivedAmount;
          totFcfaGivenNgn += t.equivalentGiven;
        }
        grandProfit += t.profitNgn;
      });

      document.getElementById('sumTotalNgnReceived').innerText = "₦" + totNgnRec.toLocaleString('en-US', {minimumFractionDigits: 2});
      document.getElementById('sumNgnGivenFcfa').innerText = "Given out: " + totNgnGivenFcfa.toLocaleString() + " FCFA";

      document.getElementById('sumTotalFcfaReceived').innerText = totFcfaRec.toLocaleString() + " FCFA";
      document.getElementById('sumFcfaGivenNgn').innerText = "Given out: ₦" + totFcfaGivenNgn.toLocaleString('en-US', {minimumFractionDigits: 2});

      document.getElementById('sumGrandProfit').innerText = "₦" + grandProfit.toLocaleString('en-US', {minimumFractionDigits: 2});

      document.getElementById('sumTxCount').innerText = transactions.length + " Entries";
      document.getElementById('sumBranchBreakdown').innerText = `HQ: ${hqCount} | PTT: ${pttCount}`;

      const tbody = document.getElementById('summaryTableBody');
      tbody.innerHTML = '';

      const filteredList = summaryFilter === 'ALL' ? transactions : transactions.filter(t => t.branch === summaryFilter);

      if (filteredList.length === 0) {
        tbody.innerHTML = `<tr><td colspan="7" class="p-6 text-center text-gray-400 text-xs">No active transactions recorded for today yet.</td></tr>`;
        return;
      }

      filteredList.forEach(t => {
        const tr = document.createElement('tr');
        tr.className = "hover:bg-gray-50 transition";
        tr.innerHTML = `
          <td class="p-3">
            <div class="font-semibold text-gray-800">${t.timestamp}</div>
            <span class="inline-block text-[10px] font-bold px-2 py-0.5 rounded ${t.branch === 'Headquarters' ? 'bg-blue-100 text-blue-800' : 'bg-purple-100 text-purple-800'}">
              ${t.branch}
            </span>
          </td>
          <td class="p-3 font-medium text-gray-800">${t.accountName}</td>
          <td class="p-3 text-gray-500 text-xs">${t.accountNumber}</td>
          <td class="p-3 font-bold ${t.receivedCurrency === 'NGN' ? 'text-blue-900' : 'text-indigo-900'}">
            ${t.receivedCurrency === 'NGN' ? '₦' + t.receivedAmount.toLocaleString() : t.receivedAmount.toLocaleString() + ' FCFA'}
          </td>
          <td class="p-3 font-medium text-gray-700">
            ${t.receivedCurrency === 'NGN' ? t.equivalentGiven.toLocaleString() + ' FCFA' : '₦' + t.equivalentGiven.toLocaleString()}
          </td>
          <td class="p-3 text-right font-extrabold text-emerald-600">
            ₦${t.profitNgn.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2})}
          </td>
          <td class="p-3 text-center">
            <button onclick="deleteTransaction('${t.id}')" class="text-red-500 hover:text-red-700 text-xs">
              <i class="fa-solid fa-trash"></i>
            </button>
          </td>
        `;
        tbody.appendChild(tr);
      });
    }

    function startDigitalClock() {
      setInterval(() => {
        const now = new Date();
        const timeStr = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' });
        document.getElementById('digitalClock').innerText = timeStr;

        const hours = now.getHours();
        const minutes = now.getMinutes();
        const seconds = now.getSeconds();

        if (hours === 22 && minutes === 30 && seconds === 0) {
          trigger1030Audit(false);
        }

        if (hours === 23 && minutes === 0 && seconds === 0) {
          trigger1100Archive(false);
        }
      }, 1000);
    }

    function trigger1030Audit(isManual) {
      renderSummaryPage();
      if (isManual) {
        switchMainTab('summary');
        alert("10:30 PM AUTOMATIC AUDIT COMPLETE:\nMulti-branch total received amounts and combined profit updated!");
      }
    }

    function trigger1100Archive(isManual) {
      if (transactions.length === 0) {
        if (isManual) alert("No active transactions to archive today.");
        return;
      }

      const todayStr = new Date().toISOString().split('T')[0];
      
      let totNgnRec = 0;
      let totFcfaRec = 0;
      let grandProfit = 0;

      transactions.forEach(t => {
        if (t.receivedCurrency === 'NGN') totNgnRec += t.receivedAmount;
        else totFcfaRec += t.receivedAmount;
        grandProfit += t.profitNgn;
      });

      const archiveRecord = {
        archiveDate: todayStr,
        archivedAt: new Date().toLocaleTimeString(),
        totalNgnReceived: totNgnRec,
        totalFcfaReceived: totFcfaRec,
        grandProfitNgn: grandProfit,
        records: [...transactions]
      };

      archiveLogs.unshift(archiveRecord);
      setStoredData('egh_perm_archives', archiveLogs);

      transactions = [];
      setStoredData('egh_perm_transactions', transactions);

      renderSummaryPage();
      renderArchiveLogs();

      if (isManual) {
        switchMainTab('archive');
        alert("11:00 PM AUTOMATIC ARCHIVE COMPLETE:\nAll daily records sealed into Web Archive. Active daily sheet reset for tomorrow.");
      }
    }

    function renderArchiveLogs() {
      const archiveArea = document.getElementById('archiveContentArea');
      archiveArea.innerHTML = '';

      if (archiveLogs.length === 0) {
        archiveArea.innerHTML = `<div class="p-8 text-center text-gray-400 text-xs">No archived logs saved yet. Automated 11:00 PM archives will show up here permanently.</div>`;
        return;
      }

      archiveLogs.forEach((log) => {
        const card = document.createElement('div');
        card.className = "border border-gray-200 rounded-xl p-4 bg-gray-50 space-y-3";
        card.innerHTML = `
          <div class="flex flex-col sm:flex-row items-start sm:items-center justify-between border-b pb-2">
            <div>
              <span class="text-xs font-bold text-amber-600 uppercase">Archived Date: ${log.archiveDate}</span>
              <p class="text-[11px] text-gray-400">Sealed at ${log.archivedAt} | ${log.records.length} Transactions</p>
            </div>
            <div class="text-right mt-2 sm:mt-0">
              <span class="text-xs font-extrabold text-emerald-600">Total Profit: ₦${log.grandProfitNgn.toLocaleString('en-US', {minimumFractionDigits:2})}</span>
            </div>
          </div>

          <div class="grid grid-cols-2 gap-2 text-xs">
            <div class="bg-white p-2 rounded border">
              <span class="text-gray-500">NGN Rec:</span> <strong>₦${log.totalNgnReceived.toLocaleString()}</strong>
            </div>
            <div class="bg-white p-2 rounded border">
              <span class="text-gray-500">FCFA Rec:</span> <strong>${log.totalFcfaReceived.toLocaleString()} FCFA</strong>
            </div>
          </div>
        `;
        archiveArea.appendChild(card);
      });
    }

    function exportArchiveCSV() {
      if (archiveLogs.length === 0) {
        alert('No archive data available to export.');
        return;
      }

      let csv = 'Archive Date,Time,Branch,Account Name,Account Number,Received Currency,Amount Received,Equivalent Given,Profit (NGN)\n';

      archiveLogs.forEach(log => {
        log.records.forEach(r => {
          csv += `"${log.archiveDate}","${r.timestamp}","${r.branch}","${r.accountName}","${r.accountNumber}","${r.receivedCurrency}",${r.receivedAmount},${r.equivalentGiven},${r.profitNgn}\n`;
        });
      });

      const blob = new Blob([csv], { type: 'text/csv' });
      const url = window.URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.setAttribute('href', url);
      a.setAttribute('download', `Emancy_Global_Hub_Archive_${new Date().toISOString().split('T')[0]}.csv`);
      a.click();
    }
  </script>
</body>
</html>
