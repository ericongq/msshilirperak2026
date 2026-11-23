<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MSS Hilir Perak 2026 - Perempuan Bawah 16</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap');
    body { font-family: 'Inter', sans-serif; background-color: #f8fafc; color: #0f172a; }
    .tabular-nums { font-variant-numeric: tabular-nums; }
  </style>
</head>
<body class="min-h-screen pb-16">

  <!-- Top Header -->
  <header class="bg-slate-900 text-white shadow-md sticky top-0 z-30">
    <div class="max-w-5xl mx-auto px-4 py-3.5 flex flex-wrap items-center justify-between gap-3">
      <div>
        <div class="flex items-center gap-2">
          <span class="bg-amber-500 text-slate-950 text-xs font-extrabold px-2 py-0.5 rounded uppercase tracking-wider">Live</span>
          <h1 class="text-base sm:text-lg font-bold tracking-tight">MSS Hilir Perak 2026 — Perempuan Bawah 16</h1>
        </div>
        <p class="text-xs text-slate-300 mt-0.5">Tarikh: 24/11/2026 (Selasa) &bull; Gelanggang: Glg. SPEEDY &bull; Sistem Liga 1 Pusingan</p>
      </div>

      <div class="flex flex-wrap items-center gap-2">
        <!-- Sync Indicator -->
        <div id="syncBadge" class="flex items-center gap-1.5 text-xs font-medium px-2.5 py-1 rounded-full bg-slate-800 text-slate-300 border border-slate-700">
          <span id="syncDot" class="w-2 h-2 rounded-full bg-amber-400 animate-pulse"></span>
          <span id="syncText">Connecting...</span>
        </div>

        <!-- Mode Controls (Hidden in ?view=spectator) -->
        <div id="organizerToolbar" class="flex items-center gap-2">
          <button id="btnToggleMode" onclick="toggleMode()" class="text-xs font-semibold px-3 py-1.5 rounded-lg bg-indigo-600 hover:bg-indigo-500 text-white transition">
            Organizer Mode
          </button>
          <button id="btnSetPin" onclick="openPinSetup()" class="text-xs font-semibold px-2.5 py-1.5 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 transition" title="Set or Change Organizer PIN">
            PIN
          </button>
          <button onclick="copySpectatorLink()" class="text-xs font-semibold px-2.5 py-1.5 rounded-lg bg-emerald-600 hover:bg-emerald-500 text-white transition">
            Share Spectator Link
          </button>
        </div>
      </div>
    </div>
  </header>

  <main class="max-w-5xl mx-auto px-4 pt-6 space-y-8">

    <!-- Spectator Notice Banner -->
    <div id="spectatorBanner" class="hidden bg-indigo-50 border border-indigo-200 text-indigo-900 px-4 py-2.5 rounded-xl text-xs sm:text-sm flex items-center justify-between">
      <span><strong>Paparan Penonton (Read-Only):</strong> Keputusan dan kedudukan dikemas kini secara automatik.</span>
      <span class="text-xs font-semibold text-indigo-600">MSS Hilir Perak</span>
    </div>

    <!-- Live Standings Table -->
    <section class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
      <div class="px-5 py-4 border-b border-slate-100 flex flex-wrap items-center justify-between gap-2">
        <div>
          <h2 class="text-base font-bold text-slate-900">Kedudukan Semasa (Standings)</h2>
          <p class="text-xs text-slate-500">Mata FIBA: Menang = 2, Kalah = 1, Lucut Hak = 0 &bull; Pemecah Seri: Head-to-Head, Beza Mata</p>
        </div>
        <button id="btnResetAll" onclick="resetTournament()" class="hidden text-xs font-semibold text-rose-600 hover:text-rose-700 px-2.5 py-1 rounded border border-rose-200 hover:bg-rose-50 transition">
          Reset Semua Skor
        </button>
      </div>
      <div class="overflow-x-auto">
        <table class="w-full text-left border-collapse">
          <thead>
            <tr class="bg-slate-50 text-slate-600 text-xs uppercase tracking-wider border-b border-slate-200">
              <th class="py-3 px-4 w-12 text-center">Ked</th>
              <th class="py-3 px-4">Pasukan</th>
              <th class="py-3 px-3 text-center">P</th>
              <th class="py-3 px-3 text-center">M</th>
              <th class="py-3 px-3 text-center">K</th>
              <th class="py-3 px-3 text-center">MS</th>
              <th class="py-3 px-3 text-center">MK</th>
              <th class="py-3 px-3 text-center">Beza</th>
              <th class="py-3 px-4 text-center font-bold text-slate-900">Mata</th>
            </tr>
          </thead>
          <tbody id="standingsBody" class="divide-y divide-slate-100 text-sm tabular-nums"></tbody>
        </table>
      </div>
    </section>

    <!-- Schedule & Live Scoreboard -->
    <section class="space-y-4">
      <div class="flex items-center justify-between">
        <div>
          <h2 class="text-base font-bold text-slate-900">Jadual & Keputusan Perlawanan (24/11/2026)</h2>
          <p class="text-xs text-slate-500">Semua perlawanan berlangsung di Glg. SPEEDY</p>
        </div>
        <span id="modeIndicator" class="text-xs font-semibold px-2.5 py-1 rounded-md bg-slate-200 text-slate-700">View Only</span>
      </div>

      <div id="matchesContainer" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>
    </section>

  </main>

  <!-- Toast Notification -->
  <div id="toast" class="fixed bottom-5 right-5 bg-slate-900 text-white text-xs font-medium px-4 py-2.5 rounded-xl shadow-lg opacity-0 pointer-events-none transition-opacity duration-200 z-50"></div>

  <!-- PIN Modal -->
  <div id="pinModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-xs hidden items-center justify-center z-50 p-4">
    <div class="bg-white rounded-2xl max-w-sm w-full p-6 shadow-xl space-y-4">
      <h3 id="pinModalTitle" class="text-base font-bold text-slate-900">Masukkan PIN Penganjur</h3>
      <p id="pinModalDesc" class="text-xs text-slate-500">Sila masukkan 4-digit PIN untuk mengedit skor perlawanan.</p>
      <input id="pinInput" type="password" inputmode="numeric" maxlength="6" placeholder="••••" class="w-full text-center text-2xl tracking-widest font-bold border border-slate-300 rounded-xl py-2.5 focus:outline-none focus:ring-2 focus:ring-indigo-500" />
      <p id="pinError" class="text-xs text-rose-600 hidden"></p>
      <div class="flex justify-end gap-2 pt-2">
        <button onclick="closePinModal()" class="px-4 py-2 text-xs font-semibold text-slate-600 hover:bg-slate-100 rounded-lg">Batal</button>
        <button id="pinSubmitBtn" onclick="submitPin()" class="px-4 py-2 text-xs font-semibold bg-indigo-600 hover:bg-indigo-500 text-white rounded-lg">Sahkan</button>
      </div>
    </div>
  </div>

  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";
    import { getDatabase, ref, onValue, set } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-database.js";

    const FIREBASE_ROOM = "mss_hilir_perak_2026_perempuan_u16_fresh";
    const firebaseConfig = {
      databaseURL: "https://mss-hilir-perak-live-default-rtdb.asia-southeast1.firebasedatabase.app"
    };

    const INITIAL_TEAMS = [
      { id: "P1", name: "SUWA" },
      { id: "P2", name: "SMJK SAN MIN" },
      { id: "P3", name: "SMK CONVENT" },
      { id: "P4", name: "BAGAN" }
    ];

    const INITIAL_MATCHES = [
      { id: 1, time: "08:00", court: "Glg. SPEEDY", home: "P2", away: "P3", scoreH: 0, scoreA: 0, status: "upcoming" },
      { id: 2, time: "09:15", court: "Glg. SPEEDY", home: "P1", away: "P4", scoreH: 0, scoreA: 0, status: "upcoming" },
      { id: 3, time: "10:30", court: "Glg. SPEEDY", home: "P3", away: "P4", scoreH: 0, scoreA: 0, status: "upcoming" },
      { id: 4, time: "11:45", court: "Glg. SPEEDY", home: "P1", away: "P2", scoreH: 0, scoreA: 0, status: "upcoming" },
      { id: 5, time: "14:00", court: "Glg. SPEEDY", home: "P2", away: "P4", scoreH: 0, scoreA: 0, status: "upcoming" },
      { id: 6, time: "15:15", court: "Glg. SPEEDY", home: "P1", away: "P3", scoreH: 0, scoreA: 0, status: "upcoming" }
    ];

    const urlParams = new URLSearchParams(window.location.search);
    const isSpectatorForced = urlParams.get("view") === "spectator";

    let state = {
      teams: JSON.parse(JSON.stringify(INITIAL_TEAMS)),
      matches: JSON.parse(JSON.stringify(INITIAL_MATCHES)),
      pin: localStorage.getItem("mss_u16_p_pin") || ""
    };

    let isOrganizer = false;
    let db = null;
    let roomRef = null;
    let pinModalAction = "unlock";

    // Load local backup immediately
    const cached = localStorage.getItem("mss_cache_" + FIREBASE_ROOM);
    if (cached) {
      try {
        const parsed = JSON.parse(cached);
        if (parsed.teams && parsed.matches) {
          state.teams = parsed.teams;
          state.matches = parsed.matches;
          if (parsed.pin) state.pin = parsed.pin;
        }
      } catch (e) {}
    }

    // Initialize Firebase Realtime Database
    try {
      const app = initializeApp(firebaseConfig);
      db = getDatabase(app);
      roomRef = ref(db, "tournaments/" + FIREBASE_ROOM);

      onValue(roomRef, (snapshot) => {
        const data = snapshot.val();
        setSyncStatus(true);
        if (data && data.matches) {
          state.teams = data.teams || state.teams;
          state.matches = data.matches;
          if (data.pin !== undefined) state.pin = data.pin;
          localStorage.setItem("mss_cache_" + FIREBASE_ROOM, JSON.stringify(state));
          renderAll();
        } else {
          saveState();
        }
      }, (err) => {
        console.warn("Firebase offline:", err);
        setSyncStatus(false);
      });
    } catch (e) {
      setSyncStatus(false);
    }

    function setSyncStatus(online) {
      const dot = document.getElementById("syncDot");
      const text = document.getElementById("syncText");
      if (online) {
        dot.className = "w-2 h-2 rounded-full bg-emerald-400";
        text.textContent = "Live Synced";
      } else {
        dot.className = "w-2 h-2 rounded-full bg-amber-400";
        text.textContent = "Local Backup";
      }
    }

    function saveState() {
      localStorage.setItem("mss_cache_" + FIREBASE_ROOM, JSON.stringify(state));
      if (roomRef) {
        set(roomRef, {
          teams: state.teams,
          matches: state.matches,
          pin: state.pin,
          updatedAt: Date.now()
        }).catch(() => setSyncStatus(false));
      }
      renderAll();
    }

    function showToast(msg) {
      const t = document.getElementById("toast");
      t.textContent = msg;
      t.classList.remove("opacity-0");
      setTimeout(() => t.classList.add("opacity-0"), 2400);
    }

    function getTeamName(id) {
      const t = state.teams.find(x => x.id === id);
      return t ? t.name : id;
    }

    function computeStandings() {
      const stats = {};
      state.teams.forEach(t => {
        stats[t.id] = { id: t.id, name: t.name, p: 0, w: 0, l: 0, pf: 0, pa: 0, pts: 0 };
      });

      state.matches.forEach(m => {
        if (m.status === "upcoming") return;
        const h = stats[m.home];
        const a = stats[m.away];
        if (!h || !a) return;

        h.p += 1;
        a.p += 1;
        h.pf += Number(m.scoreH);
        h.pa += Number(m.scoreA);
        a.pf += Number(m.scoreA);
        a.pa += Number(m.scoreH);

        if (m.scoreH > m.scoreA) {
          h.w += 1; h.pts += 2;
          a.l += 1; a.pts += 1;
        } else if (m.scoreA > m.scoreH) {
          a.w += 1; a.pts += 2;
          h.l += 1; h.pts += 1;
        }
      });

      const list = Object.values(stats);
      list.sort((a, b) => {
        if (b.pts !== a.pts) return b.pts - a.pts;
        // 2-way Head-to-Head tiebreaker
        const h2h = state.matches.find(m =>
          m.status !== "upcoming" &&
          ((m.home === a.id && m.away === b.id) || (m.home === b.id && m.away === a.id))
        );
        if (h2h && h2h.scoreH !== h2h.scoreA) {
          const winner = h2h.scoreH > h2h.scoreA ? h2h.home : h2h.away;
          if (winner === a.id) return -1;
          if (winner === b.id) return 1;
        }
        const diffA = a.pf - a.pa;
        const diffB = b.pf - b.pa;
        if (diffB !== diffA) return diffB - diffA;
        return b.pf - a.pf;
      });

      return list;
    }

    function renderStandings() {
      const tbody = document.getElementById("standingsBody");
      const rows = computeStandings();
      tbody.innerHTML = rows.map((r, idx) => {
        const diff = r.pf - r.pa;
        const diffStr = diff > 0 ? `+${diff}` : `${diff}`;
        const badgeClass = idx === 0 ? "bg-amber-100 text-amber-800 font-bold" : idx === 1 ? "bg-slate-200 text-slate-800 font-semibold" : "text-slate-500";
        return `
          <tr class="hover:bg-slate-50/80 transition">
            <td class="py-3 px-4 text-center"><span class="inline-flex items-center justify-center w-6 h-6 rounded-full text-xs ${badgeClass}">${idx + 1}</span></td>
            <td class="py-3 px-4 font-semibold text-slate-900"><span class="text-xs font-bold text-indigo-600 mr-1.5">${r.id}</span>${r.name}</td>
            <td class="py-3 px-3 text-center text-slate-600">${r.p}</td>
            <td class="py-3 px-3 text-center text-emerald-600 font-medium">${r.w}</td>
            <td class="py-3 px-3 text-center text-rose-600 font-medium">${r.l}</td>
            <td class="py-3 px-3 text-center text-slate-600">${r.pf}</td>
            <td class="py-3 px-3 text-center text-slate-600">${r.pa}</td>
            <td class="py-3 px-3 text-center text-slate-600">${diffStr}</td>
            <td class="py-3 px-4 text-center font-extrabold text-slate-900 bg-slate-50/60">${r.pts}</td>
          </tr>
        `;
      }).join("");
    }

    function renderMatches() {
      const container = document.getElementById("matchesContainer");
      container.innerHTML = state.matches.map(m => {
        const statusBadge = m.status === "live"
          ? `<span class="bg-rose-100 text-rose-700 text-[11px] font-bold px-2 py-0.5 rounded-full uppercase">Sedang Berlangsung</span>`
          : m.status === "final"
          ? `<span class="bg-emerald-100 text-emerald-700 text-[11px] font-bold px-2 py-0.5 rounded-full uppercase">Tamat</span>`
          : `<span class="bg-slate-100 text-slate-600 text-[11px] font-semibold px-2 py-0.5 rounded-full uppercase">Akan Datang</span>`;

        const homeWin = m.status !== "upcoming" && m.scoreH > m.scoreA;
        const awayWin = m.status !== "upcoming" && m.scoreA > m.scoreH;

        const controlsHtml = isOrganizer ? `
          <div class="mt-3 pt-3 border-t border-slate-100 space-y-2.5">
            <div class="grid grid-cols-2 gap-3">
              <div class="flex items-center justify-between bg-slate-50 p-1.5 rounded-lg">
                <span class="text-[11px] font-bold text-slate-500 pl-1">${m.home}</span>
                <div class="flex gap-1">
                  <button onclick="addScore(${m.id}, 'H', -1)" class="w-6 h-6 rounded bg-white border border-slate-200 text-xs font-bold hover:bg-slate-100">-1</button>
                  <button onclick="addScore(${m.id}, 'H', 1)" class="w-6 h-6 rounded bg-indigo-600 text-white text-xs font-bold hover:bg-indigo-500">+1</button>
                  <button onclick="addScore(${m.id}, 'H', 2)" class="w-6 h-6 rounded bg-indigo-600 text-white text-xs font-bold hover:bg-indigo-500">+2</button>
                  <button onclick="addScore(${m.id}, 'H', 3)" class="w-6 h-6 rounded bg-indigo-600 text-white text-xs font-bold hover:bg-indigo-500">+3</button>
                </div>
              </div>
              <div class="flex items-center justify-between bg-slate-50 p-1.5 rounded-lg">
                <span class="text-[11px] font-bold text-slate-500 pl-1">${m.away}</span>
                <div class="flex gap-1">
                  <button onclick="addScore(${m.id}, 'A', -1)" class="w-6 h-6 rounded bg-white border border-slate-200 text-xs font-bold hover:bg-slate-100">-1</button>
                  <button onclick="addScore(${m.id}, 'A', 1)" class="w-6 h-6 rounded bg-indigo-600 text-white text-xs font-bold hover:bg-indigo-500">+1</button>
                  <button onclick="addScore(${m.id}, 'A', 2)" class="w-6 h-6 rounded bg-indigo-600 text-white text-xs font-bold hover:bg-indigo-500">+2</button>
                  <button onclick="addScore(${m.id}, 'A', 3)" class="w-6 h-6 rounded bg-indigo-600 text-white text-xs font-bold hover:bg-indigo-500">+3</button>
                </div>
              </div>
            </div>
            <div class="flex items-center justify-between gap-2">
              <div class="flex gap-1.5">
                <button onclick="setStatus(${m.id}, 'upcoming')" class="text-[11px] px-2 py-1 rounded border ${m.status==='upcoming'?'bg-slate-800 text-white border-slate-800':'bg-white text-slate-600 border-slate-200'}">Upcoming</button>
                <button onclick="setStatus(${m.id}, 'live')" class="text-[11px] px-2 py-1 rounded border ${m.status==='live'?'bg-rose-600 text-white border-rose-600':'bg-white text-slate-600 border-slate-200'}">Live</button>
                <button onclick="setStatus(${m.id}, 'final')" class="text-[11px] px-2 py-1 rounded border ${m.status==='final'?'bg-emerald-600 text-white border-emerald-600':'bg-white text-slate-600 border-slate-200'}">Tamat</button>
              </div>
              <button onclick="resetMatch(${m.id})" class="text-[11px] text-slate-400 hover:text-rose-600">Reset</button>
            </div>
          </div>
        ` : "";

        return `
          <div class="bg-white rounded-2xl p-4 shadow-sm border ${m.status === 'live' ? 'border-rose-300 ring-1 ring-rose-100' : 'border-slate-200'}">
            <div class="flex items-center justify-between text-xs text-slate-500 mb-3">
              <span class="font-bold text-slate-700">Bil. ${m.id} &bull; ${m.time}</span>
              ${statusBadge}
            </div>
            <div class="space-y-2 tabular-nums">
              <div class="flex items-center justify-between">
                <div class="flex items-center gap-2">
                  <span class="text-xs font-bold px-1.5 py-0.5 rounded bg-slate-100 text-slate-600">${m.home}</span>
                  <span class="text-sm ${homeWin ? 'font-extrabold text-slate-900' : 'font-medium text-slate-700'}">${getTeamName(m.home)}</span>
                </div>
                <span class="text-xl ${homeWin ? 'font-extrabold text-indigo-600' : 'font-bold text-slate-800'}">${m.scoreH}</span>
              </div>
              <div class="flex items-center justify-between">
                <div class="flex items-center gap-2">
                  <span class="text-xs font-bold px-1.5 py-0.5 rounded bg-slate-100 text-slate-600">${m.away}</span>
                  <span class="text-sm ${awayWin ? 'font-extrabold text-slate-900' : 'font-medium text-slate-700'}">${getTeamName(m.away)}</span>
                </div>
                <span class="text-xl ${awayWin ? 'font-extrabold text-indigo-600' : 'font-bold text-slate-800'}">${m.scoreA}</span>
              </div>
            </div>
            ${controlsHtml}
          </div>
        `;
      }).join("");
    }

    function renderAll() {
      if (isSpectatorForced) {
        document.getElementById("organizerToolbar").classList.add("hidden");
        document.getElementById("spectatorBanner").classList.remove("hidden");
        isOrganizer = false;
      }
      const modeBtn = document.getElementById("btnToggleMode");
      const modeInd = document.getElementById("modeIndicator");
      const resetAllBtn = document.getElementById("btnResetAll");

      if (isOrganizer) {
        modeBtn.textContent = "Exit Organizer";
        modeBtn.className = "text-xs font-semibold px-3 py-1.5 rounded-lg bg-rose-600 hover:bg-rose-500 text-white transition";
        modeInd.textContent = "Organizer Active";
        modeInd.className = "text-xs font-semibold px-2.5 py-1 rounded-md bg-indigo-100 text-indigo-700";
        resetAllBtn.classList.remove("hidden");
      } else {
        modeBtn.textContent = "Organizer Mode";
        modeBtn.className = "text-xs font-semibold px-3 py-1.5 rounded-lg bg-indigo-600 hover:bg-indigo-500 text-white transition";
        modeInd.textContent = "View Only";
        modeInd.className = "text-xs font-semibold px-2.5 py-1 rounded-md bg-slate-200 text-slate-700";
        resetAllBtn.classList.add("hidden");
      }

      renderStandings();
      renderMatches();
    }

    window.addScore = (id, side, delta) => {
      if (!isOrganizer) return;
      const m = state.matches.find(x => x.id === id);
      if (!m) return;
      if (side === "H") m.scoreH = Math.max(0, m.scoreH + delta);
      if (side === "A") m.scoreA = Math.max(0, m.scoreA + delta);
      if (m.status === "upcoming" && (m.scoreH > 0 || m.scoreA > 0)) m.status = "live";
      saveState();
    };

    window.setStatus = (id, status) => {
      if (!isOrganizer) return;
      const m = state.matches.find(x => x.id === id);
      if (!m) return;
      m.status = status;
      saveState();
    };

    window.resetMatch = (id) => {
      if (!isOrganizer) return;
      const m = state.matches.find(x => x.id === id);
      if (!m) return;
      m.scoreH = 0;
      m.scoreA = 0;
      m.status = "upcoming";
      saveState();
    };

    window.resetTournament = () => {
      if (!isOrganizer) return;
      if (!confirm("Reset semua 6 perlawanan kembali ke 0 - 0?")) return;
      state.matches = JSON.parse(JSON.stringify(INITIAL_MATCHES));
      saveState();
      showToast("Semua skor telah di-reset.");
    };

    window.toggleMode = () => {
      if (isOrganizer) {
        isOrganizer = false;
        renderAll();
        return;
      }
      if (!state.pin) {
        isOrganizer = true;
        renderAll();
        showToast("Organizer Mode aktif (Tiada PIN ditetapkan).");
        return;
      }
      pinModalAction = "unlock";
      document.getElementById("pinModalTitle").textContent = "Masukkan PIN Penganjur";
      document.getElementById("pinModalDesc").textContent = "Masukkan PIN untuk membuka kawalan skor.";
      openPinModal();
    };

    window.openPinSetup = () => {
      pinModalAction = "set";
      document.getElementById("pinModalTitle").textContent = "Tetapkan PIN Penganjur";
      document.getElementById("pinModalDesc").textContent = "Tetapkan 4–6 digit PIN untuk mengunci Organizer Mode.";
      openPinModal();
    };

    function openPinModal() {
      const modal = document.getElementById("pinModal");
      const input = document.getElementById("pinInput");
      const err = document.getElementById("pinError");
      input.value = "";
      err.classList.add("hidden");
      modal.classList.remove("hidden");
      modal.classList.add("flex");
      setTimeout(() => input.focus(), 50);
    }

    window.closePinModal = () => {
      const modal = document.getElementById("pinModal");
      modal.classList.add("hidden");
      modal.classList.remove("flex");
    };

    window.submitPin = () => {
      const val = document.getElementById("pinInput").value.trim();
      const err = document.getElementById("pinError");
      if (pinModalAction === "set") {
        if (val.length < 4) {
          err.textContent = "PIN mesti sekurang-kurangnya 4 digit.";
          err.classList.remove("hidden");
          return;
        }
        state.pin = val;
        localStorage.setItem("mss_u16_p_pin", val);
        isOrganizer = true;
        saveState();
        closePinModal();
        showToast("PIN berjaya disimpan!");
      } else {
        if (val === state.pin) {
          isOrganizer = true;
          closePinModal();
          renderAll();
          showToast("Organizer Mode dibuka.");
        } else {
          err.textContent = "PIN salah. Sila cuba lagi.";
          err.classList.remove("hidden");
        }
      }
    };

    window.copySpectatorLink = () => {
      const base = window.location.origin + window.location.pathname;
      const specUrl = base + "?view=spectator";
      navigator.clipboard.writeText(specUrl).then(() => {
        showToast("Link Penonton (?view=spectator) disalin!");
      }).catch(() => {
        prompt("Salin link penonton di bawah:", specUrl);
      });
    };

    renderAll();
  </script>
</body>
</html>
