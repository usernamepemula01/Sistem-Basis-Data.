<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sistem Basis Data — ISH2G4 | Telkom University</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=DM+Serif+Display:ital@0;1&family=IBM+Plex+Sans:wght@300;400;500;600&family=Fira+Code:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0e1a;
    --surface: #111827;
    --surface2: #1c2535;
    --border: #2a3548;
    --accent: #00d4aa;
    --accent2: #4f9eff;
    --accent3: #ff6b6b;
    --accent4: #ffc948;
    --text: #e8edf5;
    --text-dim: #8899b4;
    --text-muted: #4d6080;
    --code-bg: #0d1117;
    --radius: 8px;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'IBM Plex Sans', sans-serif;
    font-size: 15px;
    line-height: 1.7;
    min-height: 100vh;
  }

  /* ── HEADER ── */
  header {
    background: linear-gradient(135deg, #0a0e1a 0%, #0f1d35 50%, #0a1520 100%);
    border-bottom: 1px solid var(--border);
    padding: 40px 24px 36px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  header::before {
    content: '';
    position: absolute; inset: 0;
    background: radial-gradient(ellipse 80% 60% at 50% 0%, rgba(0,212,170,.12) 0%, transparent 70%);
    pointer-events: none;
  }
  .header-badge {
    display: inline-block;
    background: rgba(0,212,170,.12);
    border: 1px solid rgba(0,212,170,.35);
    color: var(--accent);
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    letter-spacing: 2px;
    padding: 4px 14px;
    border-radius: 20px;
    margin-bottom: 16px;
    text-transform: uppercase;
  }
  header h1 {
    font-family: 'DM Serif Display', serif;
    font-size: clamp(28px, 5vw, 52px);
    font-weight: 400;
    color: #fff;
    letter-spacing: -0.5px;
    line-height: 1.1;
  }
  header h1 span { color: var(--accent); font-style: italic; }
  header p {
    color: var(--text-dim);
    font-size: 14px;
    margin-top: 10px;
    font-family: 'Space Mono', monospace;
  }
  .header-stats {
    display: flex;
    justify-content: center;
    gap: 32px;
    margin-top: 24px;
    flex-wrap: wrap;
  }
  .stat {
    text-align: center;
  }
  .stat-num {
    font-family: 'Space Mono', monospace;
    font-size: 24px;
    font-weight: 700;
    color: var(--accent);
  }
  .stat-label { font-size: 11px; color: var(--text-muted); letter-spacing: 1px; text-transform: uppercase; }

  /* ── LAYOUT ── */
  .container { max-width: 1100px; margin: 0 auto; padding: 0 20px; }
  .layout { display: flex; gap: 24px; padding: 24px 20px; max-width: 1100px; margin: 0 auto; }

  /* ── SIDEBAR / NAV ── */
  .sidebar {
    width: 260px;
    flex-shrink: 0;
    position: sticky;
    top: 20px;
    align-self: flex-start;
    max-height: calc(100vh - 40px);
    overflow-y: auto;
  }
  .sidebar::-webkit-scrollbar { width: 3px; }
  .sidebar::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }

  .nav-section { margin-bottom: 8px; }
  .nav-materi-btn {
    width: 100%;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    color: var(--text);
    cursor: pointer;
    padding: 10px 14px;
    display: flex;
    align-items: center;
    gap: 10px;
    font-family: 'IBM Plex Sans', sans-serif;
    font-size: 13px;
    font-weight: 500;
    text-align: left;
    transition: all .2s;
    position: relative;
  }
  .nav-materi-btn:hover { border-color: var(--accent2); background: var(--surface2); }
  .nav-materi-btn.active {
    background: rgba(0,212,170,.08);
    border-color: var(--accent);
    color: var(--accent);
  }
  .nav-num {
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    color: var(--text-muted);
    background: var(--bg);
    border-radius: 4px;
    padding: 2px 6px;
    flex-shrink: 0;
  }
  .nav-materi-btn.active .nav-num { background: rgba(0,212,170,.2); color: var(--accent); }
  .nav-sub {
    padding: 4px 0 4px 14px;
    display: none;
  }
  .nav-sub.open { display: block; }
  .nav-sub-link {
    display: block;
    padding: 5px 10px;
    font-size: 12px;
    color: var(--text-muted);
    text-decoration: none;
    border-left: 2px solid var(--border);
    transition: all .2s;
    cursor: pointer;
  }
  .nav-sub-link:hover { color: var(--text); border-color: var(--accent2); }
  .nav-sub-link.active { color: var(--accent); border-color: var(--accent); }
  .nav-arrow {
    margin-left: auto;
    transition: transform .2s;
    font-size: 10px;
  }
  .nav-materi-btn.active .nav-arrow { transform: rotate(180deg); }

  /* ── MAIN CONTENT ── */
  .main { flex: 1; min-width: 0; }

  .materi-section { display: none; }
  .materi-section.active { display: block; }

  .materi-header {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 28px 32px;
    margin-bottom: 20px;
    position: relative;
    overflow: hidden;
  }
  .materi-header::after {
    content: '';
    position: absolute;
    right: -20px; top: -20px;
    width: 140px; height: 140px;
    border-radius: 50%;
    background: var(--materi-color, var(--accent));
    opacity: .05;
    pointer-events: none;
  }
  .materi-tag {
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: var(--materi-color, var(--accent));
    margin-bottom: 8px;
  }
  .materi-title {
    font-family: 'DM Serif Display', serif;
    font-size: clamp(20px, 3vw, 30px);
    font-weight: 400;
    color: #fff;
    line-height: 1.2;
  }
  .materi-desc { color: var(--text-dim); font-size: 13px; margin-top: 8px; }

  /* ── SUB SECTION ── */
  .sub-section {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    margin-bottom: 16px;
    overflow: hidden;
    transition: border-color .2s;
  }
  .sub-section:hover { border-color: var(--border); }
  .sub-header {
    padding: 16px 20px;
    display: flex;
    align-items: center;
    gap: 12px;
    cursor: pointer;
    user-select: none;
    transition: background .2s;
  }
  .sub-header:hover { background: rgba(255,255,255,.02); }
  .sub-number {
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    color: var(--accent);
    background: rgba(0,212,170,.1);
    border: 1px solid rgba(0,212,170,.2);
    border-radius: 4px;
    padding: 3px 8px;
    flex-shrink: 0;
  }
  .sub-title {
    font-weight: 600;
    font-size: 14px;
    color: var(--text);
    flex: 1;
  }
  .sub-toggle {
    width: 20px; height: 20px;
    display: flex; align-items: center; justify-content: center;
    background: var(--surface2);
    border-radius: 4px;
    font-size: 10px;
    color: var(--text-muted);
    flex-shrink: 0;
    transition: transform .25s, background .2s;
  }
  .sub-section.open .sub-toggle { transform: rotate(45deg); background: rgba(0,212,170,.15); color: var(--accent); }

  .sub-body {
    display: none;
    padding: 0 20px 20px;
    border-top: 1px solid var(--border);
  }
  .sub-section.open .sub-body { display: block; }

  .content-text { color: var(--text-dim); font-size: 14px; line-height: 1.8; padding: 16px 0 8px; }
  .content-text p { margin-bottom: 12px; }
  .content-text strong { color: var(--text); font-weight: 600; }

  /* ── CALLOUT/QUOTE BOXES ── */
  .callout {
    background: var(--code-bg);
    border: 1px solid var(--border);
    border-left: 3px solid var(--accent);
    border-radius: 0 6px 6px 0;
    padding: 16px 20px;
    margin: 14px 0;
    font-size: 13px;
  }
  .callout-title {
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 10px;
  }

  /* ── CODE BLOCKS ── */
  pre {
    background: var(--code-bg);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 16px;
    overflow-x: auto;
    margin: 12px 0;
    position: relative;
  }
  pre code {
    font-family: 'Fira Code', monospace;
    font-size: 12.5px;
    color: #a8d8a0;
    line-height: 1.6;
    white-space: pre;
  }
  .code-label {
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-bottom: 6px;
  }

  /* ── TABLES ── */
  .table-wrap { overflow-x: auto; margin: 14px 0; }
  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 13px;
  }
  th {
    background: var(--surface2);
    color: var(--text);
    font-weight: 600;
    padding: 10px 14px;
    text-align: left;
    border: 1px solid var(--border);
    font-size: 12px;
    letter-spacing: .3px;
  }
  td {
    padding: 9px 14px;
    border: 1px solid var(--border);
    color: var(--text-dim);
    vertical-align: top;
  }
  tr:nth-child(even) td { background: rgba(255,255,255,.02); }
  tr:hover td { background: rgba(255,255,255,.04); }

  /* ── LISTS ── */
  .content-list {
    list-style: none;
    margin: 10px 0;
  }
  .content-list li {
    display: flex;
    gap: 10px;
    padding: 6px 0;
    font-size: 13.5px;
    color: var(--text-dim);
    border-bottom: 1px solid rgba(255,255,255,.04);
  }
  .content-list li:last-child { border: none; }
  .content-list li::before {
    content: '▸';
    color: var(--accent);
    flex-shrink: 0;
    margin-top: 2px;
  }

  /* ── HIGHLIGHT BOXES ── */
  .highlight-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 10px;
    margin: 14px 0;
  }
  .highlight-card {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 14px;
    font-size: 13px;
  }
  .highlight-card strong {
    display: block;
    color: var(--accent2);
    font-size: 12px;
    font-family: 'Space Mono', monospace;
    margin-bottom: 4px;
  }

  /* ── PROGRESS INDICATOR ── */
  .progress-bar-wrap {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 14px 20px;
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    gap: 16px;
  }
  .progress-label { font-family: 'Space Mono', monospace; font-size: 11px; color: var(--text-muted); flex-shrink: 0; }
  .progress-track { flex: 1; height: 4px; background: var(--border); border-radius: 4px; overflow: hidden; }
  .progress-fill { height: 100%; background: var(--accent); border-radius: 4px; transition: width .4s; }
  .progress-count { font-family: 'Space Mono', monospace; font-size: 12px; color: var(--accent); flex-shrink: 0; }

  /* ── MATERI COLORS ── */
  [data-materi="1"] { --materi-color: #00d4aa; }
  [data-materi="2"] { --materi-color: #4f9eff; }
  [data-materi="3"] { --materi-color: #ff9f4a; }
  [data-materi="4"] { --materi-color: #c77dff; }
  [data-materi="5"] { --materi-color: #ff6b6b; }
  [data-materi="6"] { --materi-color: #ffd166; }
  [data-materi="7"] { --materi-color: #06d6a0; }

  /* ── MOBILE ── */
  @media (max-width: 768px) {
    .layout { flex-direction: column; padding: 16px; }
    .sidebar { width: 100%; position: static; max-height: none; }
    .nav-sub { display: none !important; }
    .nav-sub.open { display: block !important; }
    .materi-header { padding: 20px; }
  }

  /* ── ANIMATIONS ── */
  .materi-section.active { animation: fadeIn .25s ease; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }

  /* keyword highlight */
  .kw { color: var(--accent4); font-weight: 600; }
  .kw-blue { color: var(--accent2); font-weight: 600; }
  .kw-red { color: var(--accent3); font-weight: 600; }
  .kw-green { color: var(--accent); font-weight: 600; }

  /* search box */
  .search-wrap { padding: 0 0 12px; }
  .search-input {
    width: 100%;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 9px 14px;
    color: var(--text);
    font-family: 'IBM Plex Sans', sans-serif;
    font-size: 13px;
    outline: none;
    transition: border-color .2s;
  }
  .search-input:focus { border-color: var(--accent); }
  .search-input::placeholder { color: var(--text-muted); }
</style>
</head>
<body>

<header>
  <div class="header-badge">ISH2G4 · Telkom University</div>
  <h1>Sistem <span>Basis Data</span></h1>
  <p>S1 Sistem Informasi · Fakultas Rekayasa Industri</p>
  <div class="header-stats">
    <div class="stat"><div class="stat-num">7</div><div class="stat-label">Materi Utama</div></div>
    <div class="stat"><div class="stat-num">32</div><div class="stat-label">Sub Materi</div></div>
    <div class="stat"><div class="stat-num">SQL</div><div class="stat-label">Bahasa Basis Data</div></div>
  </div>
</header>

<div class="layout">
  <!-- SIDEBAR -->
  <nav class="sidebar">
    <div class="search-wrap">
      <input class="search-input" type="text" placeholder="🔍  Cari sub materi..." id="searchInput" oninput="filterNav(this.value)">
    </div>

    <div id="navList">
      <!-- M1 -->
      <div class="nav-section">
        <button class="nav-materi-btn active" onclick="switchMateri(1, this)" data-materi="1">
          <span class="nav-num">M1</span> Konsep Dasar Basis Data
          <span class="nav-arrow">▾</span>
        </button>
        <div class="nav-sub open" id="sub-nav-1">
          <a class="nav-sub-link" onclick="scrollToSub('1-1')">1.1 Pentingnya Data</a>
          <a class="nav-sub-link" onclick="scrollToSub('1-2')">1.2 Definisi Basis Data</a>
          <a class="nav-sub-link" onclick="scrollToSub('1-3')">1.3 Basis Data vs File Tradisional</a>
          <a class="nav-sub-link" onclick="scrollToSub('1-4')">1.4 Redundansi Data</a>
          <a class="nav-sub-link" onclick="scrollToSub('1-5')">1.5 Keunggulan & Risiko</a>
        </div>
      </div>
      <!-- M2 -->
      <div class="nav-section">
        <button class="nav-materi-btn" onclick="switchMateri(2, this)" data-materi="2">
          <span class="nav-num">M2</span> Sistem Basis Data & Model
          <span class="nav-arrow">▾</span>
        </button>
        <div class="nav-sub" id="sub-nav-2">
          <a class="nav-sub-link" onclick="scrollToSub('2-1')">2.1 Sistem Basis Data & DBMS</a>
          <a class="nav-sub-link" onclick="scrollToSub('2-2')">2.2 Bahasa Basis Data (DDL & DML)</a>
          <a class="nav-sub-link" onclick="scrollToSub('2-3')">2.3 Model Data: Flat File & Hirarki</a>
          <a class="nav-sub-link" onclick="scrollToSub('2-4')">2.4 Model Data: Jaringan & Relasional</a>
          <a class="nav-sub-link" onclick="scrollToSub('2-5')">2.5 Model Berorientasi Objek</a>
        </div>
      </div>
      <!-- M3 -->
      <div class="nav-section">
        <button class="nav-materi-btn" onclick="switchMateri(3, this)" data-materi="3">
          <span class="nav-num">M3</span> Desain Basis Data & ER
          <span class="nav-arrow">▾</span>
        </button>
        <div class="nav-sub" id="sub-nav-3">
          <a class="nav-sub-link" onclick="scrollToSub('3-1')">3.1 Tahapan Desain</a>
          <a class="nav-sub-link" onclick="scrollToSub('3-2')">3.2 Entitas, Atribut, Himpunan</a>
          <a class="nav-sub-link" onclick="scrollToSub('3-3')">3.3 Relasi & Kardinalitas</a>
          <a class="nav-sub-link" onclick="scrollToSub('3-4')">3.4 Kunci (Key)</a>
          <a class="nav-sub-link" onclick="scrollToSub('3-5')">3.5 Diagram ERD</a>
          <a class="nav-sub-link" onclick="scrollToSub('3-6')">3.6 Entitas Lemah & Spesialisasi</a>
        </div>
      </div>
      <!-- M4 -->
      <div class="nav-section">
        <button class="nav-materi-btn" onclick="switchMateri(4, this)" data-materi="4">
          <span class="nav-num">M4</span> Normalisasi Database
          <span class="nav-arrow">▾</span>
        </button>
        <div class="nav-sub" id="sub-nav-4">
          <a class="nav-sub-link" onclick="scrollToSub('4-1')">4.1 Konsep Normalisasi & Anomali</a>
          <a class="nav-sub-link" onclick="scrollToSub('4-2')">4.2 Ketergantungan Fungsional</a>
          <a class="nav-sub-link" onclick="scrollToSub('4-3')">4.3 Tahapan UNF → 1NF → 2NF → 3NF</a>
        </div>
      </div>
      <!-- M5 -->
      <div class="nav-section">
        <button class="nav-materi-btn" onclick="switchMateri(5, this)" data-materi="5">
          <span class="nav-num">M5</span> DBMS & SQL (DDL & DCL)
          <span class="nav-arrow">▾</span>
        </button>
        <div class="nav-sub" id="sub-nav-5">
          <a class="nav-sub-link" onclick="scrollToSub('5-1')">5.1 DBMS & Komponen SQL</a>
          <a class="nav-sub-link" onclick="scrollToSub('5-2')">5.2 Tipe Data MySQL</a>
          <a class="nav-sub-link" onclick="scrollToSub('5-3')">5.3 DDL: CREATE dan USE</a>
          <a class="nav-sub-link" onclick="scrollToSub('5-4')">5.4 DDL: ALTER dan DROP</a>
          <a class="nav-sub-link" onclick="scrollToSub('5-5')">5.5 INDEX dan DCL</a>
        </div>
      </div>
      <!-- M6 -->
      <div class="nav-section">
        <button class="nav-materi-btn" onclick="switchMateri(6, this)" data-materi="6">
          <span class="nav-num">M6</span> DML 1: Manipulasi Data
          <span class="nav-arrow">▾</span>
        </button>
        <div class="nav-sub" id="sub-nav-6">
          <a class="nav-sub-link" onclick="scrollToSub('6-1')">6.1 INSERT — Menambah Data</a>
          <a class="nav-sub-link" onclick="scrollToSub('6-2')">6.2 UPDATE dan DELETE</a>
          <a class="nav-sub-link" onclick="scrollToSub('6-3')">6.3 SELECT Dasar & Kondisi</a>
          <a class="nav-sub-link" onclick="scrollToSub('6-4')">6.4 Fungsi Agregasi & GROUP BY</a>
        </div>
      </div>
      <!-- M7 -->
      <div class="nav-section">
        <button class="nav-materi-btn" onclick="switchMateri(7, this)" data-materi="7">
          <span class="nav-num">M7</span> DML 2: SELECT Lanjutan
          <span class="nav-arrow">▾</span>
        </button>
        <div class="nav-sub" id="sub-nav-7">
          <a class="nav-sub-link" onclick="scrollToSub('7-1')">7.1 Operasi Himpunan: UNION, INTERSECT</a>
          <a class="nav-sub-link" onclick="scrollToSub('7-2')">7.2 JOIN: Gabungan Banyak Tabel</a>
          <a class="nav-sub-link" onclick="scrollToSub('7-3')">7.3 SubQuery (Nested Query)</a>
        </div>
      </div>
    </div>
  </nav>

  <!-- MAIN -->
  <main class="main">

    <!-- ──────── MATERI 1 ──────── -->
    <section class="materi-section active" id="materi-1" data-materi="1">
      <div class="materi-header" data-materi="1">
        <div class="materi-tag">Materi 1 · Konsep Dasar</div>
        <h2 class="materi-title">Konsep Dasar Basis Data</h2>
        <p class="materi-desc">Definisi, pentingnya data, perbedaan dengan pemrosesan file tradisional, redundansi, serta keunggulan dan risiko penggunaan basis data.</p>
      </div>

      <div class="sub-section open" id="sub-1-1">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">1.1</span>
          <span class="sub-title">Pentingnya Data dalam Sistem Informasi</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Sistem Informasi didefinisikan sebagai pengelolaan <strong>Data, Orang/Pengguna, Proses, dan Teknologi Informasi</strong> yang saling berinteraksi untuk mengumpulkan, memproses, menyimpan, dan menyediakan output informasi yang diperlukan dalam mendukung suatu organisasi (Jeffery L. Whitten dkk, 2004). Di dalam sistem ini, data merupakan bahan baku utama yang harus dikelola dengan baik agar dapat diolah menjadi informasi yang berguna dan akurat.</p>
            <p>Siklus pengolahan data digambarkan sebagai proses <strong>Input → Proses → Output</strong>, di mana data formulir atau data mentah dimasukkan sebagai input, kemudian diproses, dan menghasilkan informasi sebagai output. Tanpa pengelolaan data yang tepat, informasi yang dihasilkan akan tidak lengkap, tidak relevan, dan tidak dapat mendukung pengambilan keputusan secara optimal.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Ilustrasi Siklus Pengolahan Data</div>
            <pre><code>Data Formulir  →  [Input Data]  →  [Proses]  →  Informasi</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-1-2">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">1.2</span>
          <span class="sub-title">Definisi Basis Data</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Basis Data (Database) adalah mekanisme yang digunakan untuk menyimpan informasi atau data secara terorganisasi, sehingga pengguna dapat dengan mudah menyimpan, mengambil, memodifikasi, dan menghapus data berdasarkan berbagai kriteria (Stephens dan Plew, 2000). Cara data disimpan dalam basisdata sangat menentukan seberapa mudah informasi dapat ditemukan kembali.</p>
            <p>Para ahli mendefinisikan basisdata dari sudut pandang yang berbeda-beda, namun bermuara pada satu kesimpulan: basisdata adalah <strong>sekumpulan data yang saling berhubungan, disimpan dengan minimum redundansi untuk melayani banyak aplikasi secara optimal</strong>. Contohnya, basisdata universitas dapat berisi entitas mahasiswa, fakultas, matakuliah, dan ruang kuliah beserta hubungan antarentitas tersebut.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Ringkasan Definisi dari Berbagai Ahli</div>
            <div class="table-wrap">
              <table>
                <tr><th>Sumber</th><th>Definisi</th></tr>
                <tr><td>Stephens &amp; Plew (2000)</td><td>Menyimpan informasi dan data</td></tr>
                <tr><td>Silberschatz dkk (2002)</td><td>Kumpulan data berisi informasi yang sesuai untuk perusahaan</td></tr>
                <tr><td>Mc Leod dkk (2001)</td><td>Kumpulan seluruh sumber daya berbasis komputer milik organisasi</td></tr>
                <tr><td>Ramakrishnan &amp; Gehrke (2003)</td><td>Kumpulan data yang mendeskripsikan aktivitas satu atau lebih organisasi</td></tr>
              </table>
            </div>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-1-3">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">1.3</span>
          <span class="sub-title">Basis Data vs Pemrosesan File Tradisional</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Pemrosesan file tradisional adalah pendekatan konvensional di mana setiap aplikasi memiliki file datanya sendiri yang tidak terhubung dengan file data aplikasi lain. Akibatnya, antar file tidak ada hubungan, data didefinisikan dan disusun secara berbeda untuk setiap aplikasi, dan integrasi data menjadi sangat sulit dilakukan.</p>
            <p>Keterbatasan pendekatan tradisional ini meliputi: (1) data terpisah dan terisolasi; (2) redundansi data tidak terelakkan; (3) inkonsistensi data bila modifikasi tidak dilakukan di semua file yang menyimpan data yang sama; (4) <strong>program-data-dependence</strong> yaitu perubahan format data memaksa semua program yang menggunakannya dimodifikasi; dan (5) tidak mendukung <strong>data sharing</strong>. Untuk mengatasi semua keterbatasan tersebut, diperkenalkanlah konsep basis data.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Ilustrasi Keterbatasan File Tradisional</div>
            <pre><code>Aplikasi Akademik  →  File Data Akademik
Aplikasi Keuangan  →  File Data Keuangan      (tidak terhubung satu sama lain)
Aplikasi Alumni    →  File Data Alumni</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-1-4">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">1.4</span>
          <span class="sub-title">Redundansi Data</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Redundansi adalah penyimpanan data yang berlebihan, yang dapat terjadi dalam tiga bentuk: (1) penyimpanan data yang sama secara berulang (misalnya pasangan KODE_MK dan SKS ditulis berkali-kali di tabel nilai); (2) penyimpanan data yang dapat diperoleh dari data lain (tabel yang isinya bisa dihitung/dihasilkan dari tabel lain); dan (3) data yang sama disimpan di banyak tabel yang berbeda.</p>
            <p>Redundansi menimbulkan masalah serius dalam pengelolaan basis data, yaitu: pemborosan ruang penyimpanan, kesulitan melakukan update (harus diperbarui di banyak tempat sekaligus), dan potensi inkonsistensi data bila pembaruan tidak dilakukan secara menyeluruh. Oleh karena itu, desain basisdata yang baik harus meminimalkan redundansi.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Contoh Redundansi Tipe 1 — Data sama diulang</div>
            <div class="table-wrap">
              <table>
                <tr><th>NIM</th><th>KODE_MK</th><th>SKS</th><th>NILAI</th></tr>
                <tr><td>A10</td><td>MK_01</td><td>3</td><td>A</td></tr>
                <tr><td>A10</td><td>MK_02</td><td>2</td><td>B</td></tr>
                <tr><td>A11</td><td>MK_01</td><td style="color:#ff6b6b;font-weight:700">3</td><td>A</td></tr>
                <tr><td>A12</td><td>MK_01</td><td style="color:#ff6b6b;font-weight:700">3</td><td>A</td></tr>
              </table>
            </div>
            <p style="color:var(--accent3);font-size:12px;margin-top:8px">⚠ MK_01 dengan SKS=3 ditulis 3 kali → redundansi!</p>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-1-5">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">1.5</span>
          <span class="sub-title">Keunggulan dan Risiko Penggunaan Basis Data</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Penggunaan basis data memberikan sejumlah keunggulan potensial dibandingkan pemrosesan file tradisional, antara lain: kecepatan dan kemudahan akses, efisiensi ruang penyimpanan, redundansi minimum, konsistensi dan integrasi data, pemakaian data bersama (<strong>data sharing</strong>), pembakuan data, kemudahan pengembangan aplikasi, antarmuka banyak pengguna, penggambaran relasi kompleks, penegakan batasan integritas, serta penyediaan backup dan recovery.</p>
            <p>Di sisi lain, terdapat pula risiko yang harus diperhitungkan dalam pendekatan basis data, yaitu: kebutuhan spesialisasi baru (DBA), biaya awal (<strong>start-up cost</strong>) yang tinggi, perlunya konversi data lama, kebutuhan backup rutin, peningkatan kompleksitas pengelolaan, data lebih rentan diserang (<strong>vulnerable</strong>), potensi gangguan akibat berbagi data, dan kemungkinan konflik organisasi antar bagian pengguna.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Keunggulan Basis Data</div>
            <ul class="content-list">
              <li>Kecepatan, kemudahan, &amp; efisiensi ruang</li>
              <li>Redundansi data minimum</li>
              <li>Konsistensi &amp; integrasi data</li>
              <li>Pemakaian data bersama (data sharing)</li>
              <li>Pembakuan data</li>
              <li>Memudahkan pengembangan aplikasi</li>
              <li>Menyediakan antarmuka banyak pengguna</li>
              <li>Menggambarkan relasi kompleks</li>
              <li>Penegakan batasan integritas</li>
              <li>Backup dan recovery</li>
            </ul>
          </div>
        </div>
      </div>
    </section>

    <!-- ──────── MATERI 2 ──────── -->
    <section class="materi-section" id="materi-2" data-materi="2">
      <div class="materi-header" data-materi="2">
        <div class="materi-tag">Materi 2 · Sistem & Model</div>
        <h2 class="materi-title">Sistem Basis Data &amp; Model Data</h2>
        <p class="materi-desc">Komponen sistem basis data, DBMS, bahasa basis data DDL &amp; DML, serta berbagai model data dari flat file hingga relasional-objek.</p>
      </div>

      <div class="sub-section open" id="sub-2-1">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">2.1</span>
          <span class="sub-title">Pengertian Sistem Basis Data dan DBMS</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Sistem Basis Data adalah tatanan terpadu yang terdiri dari basis data dan sekumpulan program (DBMS) untuk mengakses serta memanipulasi data tersebut (Silberschatz dkk, 2002). Basis data sendiri bersifat pasif — ia hanya berguna jika ada pengelola/penggeraknya, yaitu DBMS dan pengguna. Komponen sistem basis data meliputi: perangkat keras, sistem operasi, basis data itu sendiri, DBMS, pemakai (pemrogram aplikasi, pengguna akhir, DBA), dan program aplikasi.</p>
            <p><strong>DBMS (Database Management System)</strong> adalah perangkat lunak yang terdiri dari sekumpulan program untuk mengelola dan memelihara data dalam suatu struktur yang digunakan banyak aplikasi, bersifat <strong>general-purposed</strong>, dan bebas (<strong>independence</strong>) terhadap media penyimpanan serta metoda akses. Contoh DBMS: MySQL, Oracle, MS SQL Server, PostgreSQL, MS Access, Firebird.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Struktur Hierarki Sistem Basis Data</div>
            <pre><code>Sistem Basis Data
  └── Basis Data
        └── File / Tabel
              └── Record
                    └── Aggregate Data
                          └── Field
                                └── Byte
                                      └── Bit</code></pre>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Penyederhanaan Sistem Basis Data</div>
            <pre><code>Program Aplikasi  ←→  DBMS  ←→  Basis Data  ←→  Pengguna</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-2-2">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">2.2</span>
          <span class="sub-title">Bahasa Basis Data (DDL dan DML)</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Bahasa Basis Data adalah bahasa khusus yang ditetapkan oleh pembuat DBMS untuk mengatur interaksi/komunikasi antara pemakai dan basis data. Bahasa ini mencakup <strong>DDL (Data Definition Language)</strong> yang digunakan untuk mendefinisikan struktur data (membuat tabel baru, indeks, mengubah tabel, menetapkan struktur penyimpanan), dan <strong>DML (Data Manipulation Language)</strong> yang digunakan untuk memanipulasi data (penyisipan, penghapusan, pengubahan, dan pengambilan data).</p>
            <p>DML terbagi menjadi dua jenis: (1) <strong>Prosedural</strong>, yang mensyaratkan pemakai menentukan data apa yang diinginkan beserta cara mendapatkannya; dan (2) <strong>Nonprosedural</strong>, yang memungkinkan pemakai menentukan data apa yang diinginkan tanpa harus menyebutkan cara mendapatkannya. Contoh bahasa basis data yang umum digunakan adalah SQL, QBE, dan QUEL.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Diagram Komponen Bahasa Basis Data</div>
            <pre><code>             Database Language
            /                 \
          DDL                 DML
  (Data Definition)   (Data Manipulation)
        |                     |
 CREATE, ALTER, DROP   SELECT, INSERT,
                        UPDATE, DELETE</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-2-3">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">2.3</span>
          <span class="sub-title">Model Data: Flat File &amp; Hirarki</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p><strong>Flat-file</strong> adalah bentuk basis data paling sederhana yang terdiri dari satu atau lebih file teks berformat, di mana informasi disimpan sebagai <em>fields</em> berpanjang konstan atau bervariasi yang dipisahkan oleh <em>delimiter</em>. Kekurangannya adalah tidak menggunakan struktur data yang mudah direlasikan, sulit mengatur data secara efisien, lokasi fisik <em>fields</em> harus diketahui, dan program harus dikembangkan secara khusus untuk mengakses data.</p>
            <p><strong>Model Hirarki</strong> berada satu tingkat di atas flat-file karena mampu menemukan dan memelihara relasi antar kelompok data berdasarkan konsep hubungan <strong>parent/child</strong> (satu <em>root table</em> terhubung ke beberapa <em>child table</em>). Kelebihannya: data dapat di-<em>retrieve</em> dengan cepat dan integritas data mudah diatur. Kekurangannya: pengguna harus sangat familiar dengan struktur basis data dan masih terjadi redundansi data.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Struktur Hirarki</div>
            <pre><code>Publishers  ← Root Table (Parent)
  ├── Authors    ← Child Table
  ├── Titles     ← Child Table
  └── BookStores ← Child Table
        └── Orders    ← Child dari Child
        └── Inventory ← Child dari Child</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-2-4">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">2.4</span>
          <span class="sub-title">Model Data: Jaringan &amp; Relasional</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p><strong>Model Jaringan</strong> merupakan perbaikan dari model hirarki dengan menambahkan kemampuan <em>root table</em> untuk berbagi (<em>share</em>) <em>relationships</em> dengan <em>child tables</em>. Relasi antar tabel disebut <strong>set structure</strong> (owner → member), merepresentasikan relasi <em>one-to-many</em>. Kelebihannya: data lebih cepat diakses, user dapat memulai akses dari beberapa tabel, dan query kompleks mudah dibentuk. Kekurangannya: struktur sulit dimodifikasi dan user harus memahami struktur basis data.</p>
            <p><strong>Model Relasional</strong> adalah model basis data yang paling populer saat ini. Data diorganisasikan dalam bentuk <strong>tabel</strong> (baris = tuple/record, kolom = field), dan antar-tabel dapat dihubungkan menggunakan <strong>kunci</strong> (<em>key</em>). Kelebihannya sangat banyak: akses cepat, struktur mudah diubah, representasi data bersifat logis, query kompleks mudah dibentuk, SQL telah dikembangkan. Kekurangannya: pengguna perlu memahami relasi antar tabel dan belajar SQL.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Perbandingan Model Data</div>
            <div class="table-wrap">
              <table>
                <tr><th>Model</th><th>Struktur</th><th>Kelebihan Utama</th><th>Kelemahan Utama</th></tr>
                <tr><td>Flat File</td><td>File teks</td><td>Sederhana</td><td>Tidak ada relasi</td></tr>
                <tr><td>Hirarki</td><td>Parent-Child tree</td><td>Akses cepat</td><td>Redundansi, kaku</td></tr>
                <tr><td>Jaringan</td><td>Set structure</td><td>Query kompleks mudah</td><td>Struktur sulit dimodifikasi</td></tr>
                <tr><td>Relasional</td><td>Tabel 2 dimensi</td><td>Fleksibel, SQL</td><td>Perlu JOIN</td></tr>
                <tr><td>OO</td><td>Objek &amp; method</td><td>Kompatibel OOP</td><td>Perlu paham OOP</td></tr>
                <tr><td>Relasional-Objek</td><td>Tabel + Objek</td><td>Tipe bentukan bisa dibuat</td><td>Kompleks</td></tr>
              </table>
            </div>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-2-5">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">2.5</span>
          <span class="sub-title">Model Data Berorientasi Objek (OO) &amp; Relasional-Objek</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p><strong>Model Basis Data Berorientasi Objek (OO)</strong> adalah model di mana data didefinisikan, disimpan, dan diakses menggunakan pemrograman berorientasi objek (Java). Object Database Management System digunakan untuk membuat <em>link</em> antara basis data dan aplikasi. Kelebihan: programmer cukup memahami konsep OOP, objek dapat mewarisi sifat dari objek lain (<em>inheritance</em>). Kelemahan: tidak dapat bekerja dengan metoda pemrograman tradisional.</p>
            <p><strong>Model Relasional-Objek</strong> mengkombinasikan konsep model basis data relasional dengan gaya pemrograman berorientasi objek. Kelebihannya adalah tipe bentukan dapat dibuat sesuai kebutuhan. Namun kelemahannya adalah pengguna harus memahami dua konsep sekaligus (OOP + relasional), dan beberapa vendor tidak mendukung <em>inheritance</em>.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Komponen Class dalam Model OO</div>
            <pre><code>┌─────────────────────┐
│     Nama Class      │
├─────────────────────┤
│  Properties / Field │  ← Atribut data
├─────────────────────┤
│  Operasi / Method   │  ← Fungsi/perilaku
└─────────────────────┘</code></pre>
          </div>
        </div>
      </div>
    </section>

    <!-- ──────── MATERI 3 ──────── -->
    <section class="materi-section" id="materi-3" data-materi="3">
      <div class="materi-header" data-materi="3">
        <div class="materi-tag">Materi 3 · Desain &amp; ER</div>
        <h2 class="materi-title">Desain Basis Data &amp; Entity-Relationship</h2>
        <p class="materi-desc">Tahapan desain, konsep entitas-atribut-relasi, kardinalitas, jenis kunci, diagram ERD, entitas lemah, spesialisasi, dan generalisasi.</p>
      </div>

      <div class="sub-section open" id="sub-3-1">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">3.1</span>
          <span class="sub-title">Tahapan Desain Basis Data</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Desain basis data adalah proses perancangan yang sistematis untuk menghasilkan skema basis data yang optimal. Prosesnya terdiri dari enam tahapan berurutan: (1) <strong>Analisis Persyaratan</strong> — memahami data yang harus disimpan, aplikasi yang dibangun, dan kebutuhan pengguna; (2) <strong>Desain Konseptual</strong> — mengembangkan deskripsi data tingkat tinggi menggunakan model E-R; (3) <strong>Desain Logika</strong> — mengubah skema E-R menjadi skema database relasional; (4) <strong>Perbaikan Skema</strong> — mengidentifikasi dan memperbaiki masalah pada skema relasional; (5) <strong>Desain Fisik</strong> — pembuatan indeks dan pengelompokan tabel; (6) <strong>Desain Aplikasi dan Keamanan</strong> — mempertimbangkan aspek aplikasi di luar database seperti enkripsi dan digital signature.</p>
            <p>Tahap Analisis Persyaratan menjadi fondasi keseluruhan desain karena di sinilah semua kebutuhan pengguna diidentifikasi untuk menentukan tingkat kepuasan user dan mengurangi biaya pembuatan sistem. Tahap Desain Konseptual yang menggunakan model E-R bertujuan menciptakan gambaran sederhana tentang data yang mirip dengan pemikiran pengguna, sebelum diterjemahkan ke dalam skema relasional pada tahap berikutnya.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Alur Tahapan Desain Basis Data</div>
            <pre><code>Analisis Persyaratan
        ↓
Desain Konseptual (E-R Model)
        ↓
Desain Logika (Skema Relasional)
        ↓
Perbaikan Skema (Normalisasi)
        ↓
Desain Fisik (Indeks, Clustering)
        ↓
Desain Aplikasi &amp; Keamanan</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-3-2">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">3.2</span>
          <span class="sub-title">Entitas, Atribut, dan Himpunan Entitas</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p><strong>Entitas (entity)</strong> adalah objek dasar atau individu yang mewakili sesuatu yang nyata eksistensinya dan dapat dibedakan dari objek lain. Setiap entitas memiliki sekumpulan sifat (<em>atribut</em>) dan beberapa nilai atribut bersifat unik untuk mengidentifikasi entitas tersebut. Sekumpulan entitas yang sejenis dan berada dalam lingkup yang sama membentuk <strong>Himpunan Entitas</strong> (contoh: semua mahasiswa di suatu perguruan tinggi membentuk himpunan entitas Mahasiswa).</p>
            <p>Atribut dalam model E-R dikarakterisasikan menjadi beberapa tipe: (1) <strong>Atribut Sederhana</strong> — tidak dapat diuraikan lebih lanjut; (2) <strong>Atribut Komposit</strong> — dapat diuraikan menjadi beberapa sub-atribut (contoh: <code>alamat_mhs</code> → <code>alamat</code>, <code>nama_kota</code>, <code>kodepos</code>); (3) <strong>Atribut Bernilai Tunggal</strong> — paling banyak satu nilai per baris; (4) <strong>Atribut Bernilai Banyak</strong> — dapat berisi lebih dari satu nilai (contoh: <code>hobi</code>); (5) <strong>Atribut Null</strong> — tidak ada nilai untuk atribut tersebut; (6) <strong>Atribut Turunan (Derived)</strong> — nilainya diperoleh dari atribut lain (contoh: <code>angkatan</code> diturunkan dari <code>NIM</code>, <code>usia</code> dari <code>tgl_lahir</code>).</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Contoh Tabel Entitas Mahasiswa</div>
            <div class="table-wrap">
              <table>
                <tr><th>NIM</th><th>Nama_mhs</th><th>Alamat_mhs</th><th>Tgl_lahir</th></tr>
                <tr><td>1202170085</td><td>Pascal</td><td>Menjangan 9 Subang</td><td>25-02-1995</td></tr>
                <tr><td>1202170075</td><td>Rudi</td><td>Beruang 12 Jogjakarta</td><td>10-05-1998</td></tr>
                <tr><td>1202170055</td><td>Firdaus</td><td>Mliwis 7 Semarang</td><td>09-12-1998</td></tr>
              </table>
            </div>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-3-3">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">3.3</span>
          <span class="sub-title">Relasi dan Kardinalitas</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p><strong>Relasi</strong> menunjukkan adanya hubungan di antara sejumlah entitas yang berasal dari himpunan entitas yang berbeda. Misalnya, entitas mahasiswa dengan NIM tertentu berelasi dengan entitas mata kuliah tertentu mengandung arti bahwa mahasiswa tersebut sedang mengambil mata kuliah itu. <strong>Kardinalitas</strong> merupakan jumlah maksimum entitas yang dapat berelasi dengan entitas pada himpunan entitas lain.</p>
            <p>Terdapat empat tipe kardinalitas: <strong>(1) Satu-satu (1:1)</strong> — satu entitas A berhubungan dengan paling banyak satu entitas B dan sebaliknya; <strong>(2) Satu-Banyak (1:N)</strong> — satu entitas A berhubungan dengan lebih dari satu entitas B, tetapi setiap entitas B hanya berhubungan dengan satu entitas A; <strong>(3) Banyak-Satu (N:1)</strong> — kebalikan dari 1:N; <strong>(4) Banyak-Banyak (M:N)</strong> — satu entitas A dapat berhubungan dengan banyak entitas B dan sebaliknya. Relasi M:N akan menghasilkan tabel baru ketika diterjemahkan ke skema relasional.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Ilustrasi Kardinalitas</div>
            <div class="table-wrap">
              <table>
                <tr><th>Tipe</th><th>Notasi</th><th>Contoh</th></tr>
                <tr><td>One-to-One</td><td>1 ─── 1</td><td>Rektor memimpin 1 Universitas</td></tr>
                <tr><td>One-to-Many</td><td>1 ─── N</td><td>1 Dosen mengajar banyak MK</td></tr>
                <tr><td>Many-to-One</td><td>N ─── 1</td><td>Banyak Mahasiswa 1 Wali</td></tr>
                <tr><td>Many-to-Many</td><td>M ─── N</td><td>Mahasiswa ↔ Matakuliah</td></tr>
              </table>
            </div>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-3-4">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">3.4</span>
          <span class="sub-title">Kunci (Key)</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p><strong>Kunci (key)</strong> adalah satu atau gabungan dari beberapa atribut yang dapat membedakan semua baris data secara unik — artinya tidak boleh ada dua baris data dengan nilai kunci yang sama. Pemilihan kunci yang tepat sangat penting untuk menjamin integritas data dan kecepatan akses.</p>
            <p>Terdapat beberapa jenis kunci dalam model relasional: <strong>(1) Candidate Key</strong> — kunci yang berpotensi menjadi primary key; <strong>(2) Primary Key</strong> — kunci utama yang dipilih untuk mengidentifikasi setiap baris secara unik; <strong>(3) Secondary Key</strong> — kunci alternatif selain primary key; <strong>(4) Composite Key</strong> — kunci yang terdiri dari kombinasi lebih dari satu atribut; <strong>(5) Foreign Key</strong> — atribut di satu tabel yang mengacu pada primary key tabel lain, digunakan untuk membangun relasi antar tabel; <strong>(6) Surrogate Key</strong> — kunci buatan (bukan dari data alami) yang digunakan sebagai primary key.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Jenis-jenis Kunci (Key)</div>
            <pre><code>┌─────────────────────────────────────┐
│         Jenis-jenis Key             │
├──────────────┬──────────────────────┤
│ Candidate Key│ Semua kandidat PK    │
│ Primary Key  │ PK terpilih (unik)   │
│ Secondary Key│ Kunci alternatif     │
│ Composite Key│ Gabungan atribut     │
│ Foreign Key  │ Referensi ke PK lain │
│ Surrogate Key│ Kunci buatan (ID)    │
└──────────────┴──────────────────────┘</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-3-5">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">3.5</span>
          <span class="sub-title">Diagram Entity-Relationship (ERD)</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>ERD adalah diagram yang menggambarkan hubungan antar entitas di dalam database. Terdapat dua versi ERD yang umum digunakan: <strong>ERD Chen</strong> (menggunakan notasi angka 1, N, M untuk kardinalitas) dan <strong>ERD Martin</strong> (menggunakan simbol garis dengan tanda <em>crow's foot</em> untuk menunjukkan connectivity). ERD Chen lebih umum digunakan secara akademis, sementara ERD Martin lebih umum di industri.</p>
            <p>Notasi dasar ERD Chen terdiri dari: <strong>Persegi panjang</strong> = himpunan entitas; <strong>Elips</strong> = atribut (atribut kunci digarisbawahi); <strong>Elips ganda</strong> = atribut bernilai banyak; <strong>Elips putus-putus</strong> = atribut turunan; <strong>Belah ketupat</strong> = himpunan relasi; <strong>Garis</strong> = penghubung atribut ke entitas atau entitas ke relasi; <strong>Kotak ganda</strong> = entitas lemah (<em>weak entity</em>). Tahapan pembuatan ERD meliputi: identifikasi entitas → tentukan atribut key → identifikasi relasi dan FK → tentukan kardinalitas → lengkapi dengan atribut deskriptif.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Notasi Simbol ERD (Versi Chen)</div>
            <div class="table-wrap">
              <table>
                <tr><th>Simbol</th><th>Bentuk</th><th>Keterangan</th></tr>
                <tr><td>Entitas</td><td>Persegi panjang tunggal</td><td>Himpunan entitas kuat</td></tr>
                <tr><td>Entitas Lemah</td><td>Persegi panjang ganda</td><td>Bergantung pada entitas lain</td></tr>
                <tr><td>Relasi</td><td>Belah ketupat</td><td>Hubungan antar entitas</td></tr>
                <tr><td>Atribut</td><td>Elips</td><td>Sifat/properti entitas</td></tr>
                <tr><td>Atribut Key</td><td>Elips + garis bawah</td><td>Atribut pengidentifikasi unik</td></tr>
                <tr><td>Atribut Turunan</td><td>Elips putus-putus</td><td>Nilai dihitung dari atribut lain</td></tr>
                <tr><td>Atribut Multi-nilai</td><td>Elips ganda</td><td>Dapat memiliki banyak nilai</td></tr>
              </table>
            </div>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-3-6">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">3.6</span>
          <span class="sub-title">Himpunan Entitas Lemah &amp; Spesialisasi</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p><strong>Himpunan Entitas Lemah (Weak Entity Set)</strong> adalah himpunan entitas yang tidak memiliki atribut yang dapat berfungsi sebagai <em>primary key</em> secara mandiri. Keberadaan entitas lemah bergantung pada entitas lain (entitas kuat). Contoh: entitas <code>hobi</code> tidak bermakna tanpa ada entitas <code>mahasiswa</code> yang memiliki hobi tersebut. Dalam diagram E-R, entitas lemah digambarkan dengan kotak ganda dan relasinya dengan entitas kuat juga digambarkan dengan belah ketupat ganda.</p>
            <p><strong>Spesialisasi</strong> adalah proses <strong>top-down</strong> di mana suatu himpunan entitas dipisah menjadi sub-kelompok yang lebih spesifik (contoh: entitas <code>Dosen</code> → <code>Dosen Tetap</code> dan <code>Dosen Tidak Tetap</code>). Kebalikannya, <strong>Generalisasi</strong> adalah proses <em>bottom-up</em> yang menggabungkan beberapa entitas spesifik menjadi satu entitas yang lebih umum (contoh: <code>Mahasiswa S1-Regular</code> + <code>Mahasiswa S1-Ekstensi</code> → <code>Mahasiswa</code>). Keduanya digambarkan menggunakan hubungan <strong>ISA</strong> dalam diagram E-R.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Ilustrasi Spesialisasi &amp; Generalisasi</div>
            <pre><code>Spesialisasi (top-down):
        Dosen
          |
         ISA
        /   \
Dosen Tetap  Dosen Tidak Tetap

Generalisasi (bottom-up):
Mhs S1-Regular  +  Mhs S1-Ekstensi
               |
              ISA
               |
          Mahasiswa</code></pre>
          </div>
        </div>
      </div>
    </section>

    <!-- ──────── MATERI 4 ──────── -->
    <section class="materi-section" id="materi-4" data-materi="4">
      <div class="materi-header" data-materi="4">
        <div class="materi-tag">Materi 4 · Normalisasi</div>
        <h2 class="materi-title">Normalisasi Database</h2>
        <p class="materi-desc">Konsep normalisasi, jenis anomali, ketergantungan fungsional &amp; transitif, serta tahapan normalisasi UNF → 1NF → 2NF → 3NF → BCNF.</p>
      </div>

      <div class="sub-section open" id="sub-4-1">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">4.1</span>
          <span class="sub-title">Konsep Normalisasi dan Anomali</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p><strong>Normalisasi</strong> adalah teknik untuk mengorganisasikan data ke dalam tabel-tabel guna memenuhi kebutuhan pemakai di dalam suatu organisasi. Normalisasi juga berfungsi sebagai perangkat verifikasi terhadap tabel-tabel yang dihasilkan oleh metodologi lain seperti E-R. Keuntungan normalisasi antara lain: meminimalkan ukuran penyimpanan, meminimalkan risiko inkonsistensi, meminimalkan kemungkinan anomali pembaruan, dan memaksimalkan stabilitas struktur data.</p>
            <p><strong>Anomali</strong> adalah efek samping yang tidak diharapkan pada proses basis data akibat desain yang buruk. Terdapat tiga jenis anomali: <strong>(1) Update Anomali</strong> — inkonsistensi data akibat operasi update (contoh: mengubah kota pemasok hanya di sebagian baris); <strong>(2) Insert Anomali</strong> — tidak dapat menyisipkan data baru karena ketergantungan pada data yang belum ada (contoh: tidak bisa memasukkan kursus baru jika belum ada siswa yang mendaftar); <strong>(3) Delete Anomali</strong> — menghapus satu record menyebabkan hilangnya informasi lain yang penting (contoh: menghapus siswa terakhir yang mengambil kursus B.Jepang menyebabkan data kursus tersebut ikut hilang).</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Ilustrasi Delete Anomali</div>
            <div class="table-wrap">
              <table>
                <tr><th>NO_SISWA</th><th>NAMA_KURSUS</th><th>BIAYA</th></tr>
                <tr><td>10</td><td>B. Inggris</td><td>60.000</td></tr>
                <tr><td>10</td><td>B. Prancis</td><td>80.000</td></tr>
                <tr><td>20</td><td style="color:#ff6b6b;font-weight:700">B. Jepang</td><td>65.000</td></tr>
              </table>
            </div>
            <p style="color:var(--accent3);font-size:12px;margin-top:8px">⚠ Jika siswa 20 dihapus → data Kursus B. Jepang ikut hilang!</p>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-4-2">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">4.2</span>
          <span class="sub-title">Ketergantungan Fungsional dan Transitif</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Ketergantungan/dependensi menjelaskan hubungan antar atribut — secara khusus menjelaskan nilai suatu atribut yang menentukan nilai atribut lainnya. <strong>Ketergantungan Fungsional (FD)</strong> terjadi jika atribut Y bergantung secara fungsional pada atribut X, ditulis sebagai X → Y, artinya setiap nilai X selalu berhubungan dengan tepat satu nilai Y. Contoh: <code>NIM → NamaMhs</code> (setiap NIM menentukan tepat satu nama mahasiswa).</p>
            <p><strong>Ketergantungan Transitif (TD)</strong> terjadi bila: X → Y dan Y → Z, sehingga X → Y → Z. Atribut Z dikatakan memiliki ketergantungan transitif terhadap X. Contoh: <code>Kuliah → Tempat</code> dan <code>Tempat → Ruang</code>, maka <code>Kuliah → Tempat → Ruang</code>. Ketergantungan transitif inilah yang harus dihilangkan pada proses normalisasi ke bentuk 3NF, dengan cara mendekomposisi tabel.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Rumus Ketergantungan</div>
            <pre><code>Ketergantungan Fungsional:
  X → Y  (X menentukan Y)

Ketergantungan Transitif:
  X → Y  dan  Y → Z
  maka: X → Y → Z  (Z transitif terhadap X)
  Solusi: dekomposisi menjadi X → Y  dan  Y → Z</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-4-3">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">4.3</span>
          <span class="sub-title">Tahapan Normalisasi: Unnormal → 1NF → 2NF → 3NF</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Tabel <strong>Unnormal (UNF)</strong> adalah kumpulan data yang tidak mengikuti format tertentu, bisa tidak lengkap atau terduplikasi. <strong>Bentuk Normal Pertama (1NF)</strong> terpenuhi jika setiap atribut dalam tabel bernilai <em>atomic</em> (tidak dapat dibagi-bagi lagi) — tidak ada atribut bernilai banyak, atribut komposit, atau kombinasinya. Artinya setiap sel tabel hanya berisi satu nilai tunggal.</p>
            <p><strong>Bentuk Normal Kedua (2NF)</strong> mensyaratkan tabel sudah memenuhi 1NF <em>dan</em> setiap atribut bukan kunci bergantung secara fungsional penuh pada seluruh kunci primer (tidak ada ketergantungan parsial). <strong>Bentuk Normal Ketiga (3NF)</strong> mensyaratkan tabel sudah memenuhi 2NF <em>dan</em> tidak ada ketergantungan transitif di antara atribut bukan kunci — jika A → B dan B → C, maka tabel harus didekomposisi. Tabel dikatakan baik/normal jika memenuhi tiga kriteria: Lossless-Join Decomposition, Dependency Preservation, dan tidak melanggar BCNF (minimal tidak melanggar 3NF).</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Alur Normalisasi</div>
            <pre><code>UNF (Unnormal)
  ↓  hapus atribut multi-nilai &amp; komposit
1NF (semua atribut atomic)
  ↓  hapus ketergantungan parsial
2NF (semua non-key bergantung penuh pada PK)
  ↓  hapus ketergantungan transitif
3NF (tidak ada transitif dependency)
  ↓  (lanjut ke BCNF bila perlu)
BCNF</code></pre>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Contoh Dekomposisi ke 3NF</div>
            <p style="color:var(--text-dim);font-size:13px;margin-bottom:8px">Sebelum (masih ada ketergantungan transitif: KodeMK → KodeDosen → NamaDosen):</p>
            <div class="table-wrap">
              <table>
                <tr><th>KodeMK</th><th>NamaMK</th><th>KodeDosen</th><th>NamaDosen</th></tr>
                <tr><td>MI350</td><td>Manajemen DB</td><td>B104</td><td>Ati</td></tr>
              </table>
            </div>
            <p style="color:var(--accent);font-size:13px;margin:10px 0 8px">Sesudah (3NF — didekomposisi):</p>
            <div class="table-wrap">
              <table>
                <tr><th>KodeMK</th><th>NamaMK</th><th>KodeDosen</th><th style="background:var(--bg)"></th><th>KodeDosen</th><th>NamaDosen</th></tr>
                <tr><td>MI350</td><td>Manajemen DB</td><td>B104</td><td style="background:var(--bg)"></td><td>B104</td><td>Ati</td></tr>
              </table>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ──────── MATERI 5 ──────── -->
    <section class="materi-section" id="materi-5" data-materi="5">
      <div class="materi-header" data-materi="5">
        <div class="materi-tag">Materi 5 · DDL &amp; DCL</div>
        <h2 class="materi-title">DBMS &amp; SQL (DDL &amp; DCL)</h2>
        <p class="materi-desc">Komponen DBMS, tipe data MySQL, perintah DDL: CREATE, ALTER, DROP, serta pengelolaan INDEX dan kontrol akses dengan DCL GRANT &amp; REVOKE.</p>
      </div>

      <div class="sub-section open" id="sub-5-1">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">5.1</span>
          <span class="sub-title">DBMS dan Komponen SQL</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p><strong>DBMS (Database Management System)</strong> adalah basis data dan set perangkat lunak untuk pengelolaan basis data — suatu program komputer yang digunakan untuk memasukkan, mengubah, menghapus, memanipulasi, dan memperoleh informasi secara praktis dan efisien. Komponen utama DBMS meliputi: perangkat keras (komputer, prosesor, memori, harddisk), data (terpadu dan berbagi), perangkat lunak DBMS itu sendiri, serta pengguna (pengguna akhir, pemrogram aplikasi, administrator database).</p>
            <p><strong>SQL (Structured Query Language)</strong> adalah komponen bahasa <em>relational database system</em> yang merupakan bahasa baku (ANSI/SQL), non-prosedural, dan berorientasi himpunan (<em>set-oriented language</em>). SQL dapat digunakan secara interaktif maupun di-<em>embed</em> dalam program aplikasi. SQL terdiri dari tiga komponen: <strong>DDL</strong> (Data Definition Language — CREATE, DROP, ALTER), <strong>DML</strong> (Data Manipulation Language — SELECT, INSERT, UPDATE, DELETE), dan <strong>DCL</strong> (Data Control Language — GRANT, REVOKE).</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Komponen SQL</div>
            <pre><code>             SQL
          /   |    \
        DDL  DML   DCL
         |    |     |
      CREATE SELECT GRANT
      ALTER  INSERT REVOKE
      DROP   UPDATE
             DELETE</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-5-2">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">5.2</span>
          <span class="sub-title">Tipe Data MySQL</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Setiap kolom/field dalam tabel MySQL harus ditentukan tipe datanya yang menentukan jangkauan nilai yang bisa diisikan. Tipe data yang sering digunakan antara lain: <strong>CHAR(n)</strong> — string panjang tetap (1–255 karakter); <strong>VARCHAR(n)</strong> — string panjang variabel (1–255 karakter, lebih efisien dari CHAR); <strong>INT</strong> — bilangan bulat (-2.147.483.648 s/d 2.147.483.647); <strong>FLOAT</strong> — angka pecahan; <strong>DATE</strong> — tanggal (format YYYY-MM-DD); <strong>DATETIME</strong> — tanggal dan waktu; <strong>BLOB</strong> — teks panjang maks 65.535 karakter; <strong>LONGBLOB</strong> — teks panjang maks ~4 GB.</p>
            <p>Pemilihan tipe data yang tepat sangat penting untuk efisiensi penyimpanan dan kecepatan akses. Misalnya, untuk nama gunakan VARCHAR (bukan CHAR) karena panjang nama bervariasi; untuk kode yang panjangnya tetap gunakan CHAR; untuk tanggal gunakan DATE bukan VARCHAR. Masing-masing DBMS memiliki jenis dan nama tipe data tersendiri, sehingga portabilitas antar DBMS perlu diperhatikan.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Tabel Tipe Data MySQL</div>
            <div class="table-wrap">
              <table>
                <tr><th>Tipe Data</th><th>Keterangan</th></tr>
                <tr><td><code>INT</code></td><td>Angka bulat: -2.147.483.648 s/d 2.147.483.647</td></tr>
                <tr><td><code>FLOAT</code></td><td>Angka pecahan</td></tr>
                <tr><td><code>DATE</code></td><td>Tanggal format: YYYY-MM-DD</td></tr>
                <tr><td><code>DATETIME</code></td><td>Tanggal dan waktu</td></tr>
                <tr><td><code>CHAR(n)</code></td><td>String panjang tetap, 1–255 karakter</td></tr>
                <tr><td><code>VARCHAR(n)</code></td><td>String panjang variabel, 1–255 karakter</td></tr>
                <tr><td><code>BLOB</code></td><td>Teks maks 65.535 karakter</td></tr>
                <tr><td><code>LONGBLOB</code></td><td>Teks maks 4.294.967.295 karakter</td></tr>
              </table>
            </div>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-5-3">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">5.3</span>
          <span class="sub-title">DDL: CREATE dan USE</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Perintah DDL digunakan untuk mendefinisikan dan mengelola struktur objek basis data. Langkah pembuatan tabel secara berurutan adalah: <code>CREATE DATABASE</code> → <code>USE DATABASE</code> → <code>CREATE TABLE</code> → <code>ALTER TABLE</code> (opsional). Perintah <code>CREATE DATABASE</code> digunakan untuk membuat basis data baru, sedangkan <code>USE</code> digunakan untuk memilih/mengaktifkan basis data yang akan digunakan sebelum membuat tabel.</p>
            <p>Perintah <code>CREATE TABLE</code> digunakan untuk membuat tabel baru dengan mendefinisikan nama kolom, tipe data, dan constraint-nya. Constraint yang umum diterapkan antara lain: <code>NOT NULL</code> (kolom tidak boleh kosong), <code>PRIMARY KEY</code> (kunci unik pengidentifikasi baris), <code>FOREIGN KEY</code> (referensi ke tabel lain), <code>UNIQUE</code> (nilai tidak boleh duplikat), <code>DEFAULT</code> (nilai bawaan jika tidak diisi), dan <code>CHECK</code> (validasi nilai berdasarkan ekspresi logis).</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Sintaks dan Contoh CREATE TABLE</div>
            <pre><code>-- Membuat database
CREATE DATABASE PERPUSTAKAAN;
USE PERPUSTAKAAN;

-- Membuat tabel pengarang
CREATE TABLE IF NOT EXISTS pengarang (
    id_pengarang VARCHAR(5),
    nama_pengarang VARCHAR(50),
    alamat VARCHAR(50),
    no_telp VARCHAR(15),
    PRIMARY KEY (id_pengarang)
);

-- Membuat tabel buku dengan foreign key
CREATE TABLE IF NOT EXISTS buku (
    id_buku VARCHAR(5) NOT NULL PRIMARY KEY,
    judul_buku VARCHAR(50),
    id_pengarang VARCHAR(5),
    jumlah_buku INT(5),
    tanggal_pengadaan DATE,
    FOREIGN KEY (id_pengarang) REFERENCES pengarang(id_pengarang)
    ON DELETE CASCADE ON UPDATE CASCADE
);</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-5-4">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">5.4</span>
          <span class="sub-title">DDL: ALTER dan DROP</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Perintah <code>ALTER TABLE</code> digunakan untuk mengubah struktur tabel yang sudah ada tanpa harus menghapus dan membuat ulang tabel. Operasi yang dapat dilakukan: <code>ADD COLUMN</code> (menambah kolom baru), <code>ADD MULTI COLUMN</code> (menambah banyak kolom sekaligus), <code>MODIFY COLUMN</code> (mengubah tipe data kolom), <code>DROP COLUMN</code> (menghapus kolom), <code>CHANGE</code> (mengganti nama kolom), dan <code>RENAME TABLE</code> (mengganti nama tabel).</p>
            <p>Perintah <code>DROP</code> digunakan untuk menghapus objek basis data secara permanen beserta seluruh isinya. Perintah yang tersedia: <code>DROP DATABASE</code> (menghapus seluruh database beserta tabel, view, trigger, stored procedure di dalamnya), <code>DROP TABLE</code> (menghapus tabel dan seluruh record, index, view-nya), <code>DROP INDEX</code> (menghapus indeks), dan <code>DROP VIEW</code> (menghapus view). Penggunaan <code>DROP</code> harus sangat berhati-hati karena operasi ini tidak dapat dibatalkan (<strong>irreversible</strong>).</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Sintaks ALTER TABLE</div>
            <pre><code>-- Menambah kolom
ALTER TABLE mahasiswa ADD jurusan VARCHAR(50);
ALTER TABLE mahasiswa ADD kota VARCHAR(50) DEFAULT 'Bandung';

-- Mengubah tipe data kolom
ALTER TABLE MAHASISWA MODIFY NAMA VARCHAR(50) NOT NULL;

-- Menghapus kolom
ALTER TABLE mahasiswa DROP COLUMN jurusan2;

-- Mengganti nama kolom
ALTER TABLE mahasiswa CHANGE nama nama_mhs VARCHAR(50);

-- Mengganti nama tabel
RENAME TABLE mahasiswa TO tb_mahasiswa;

-- Menghapus tabel
DROP TABLE MAHASISWA;</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-5-5">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">5.5</span>
          <span class="sub-title">INDEX dan DCL (GRANT &amp; REVOKE)</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p><strong>Indeks (index)</strong> adalah struktur tambahan pada tabel yang memungkinkan pengaksesan data dengan urutan tertentu tanpa mengubah urutan fisik data, serta mempercepat pengaksesan data melalui nilai field tertentu (seperti daftar isi di buku). Perintah <code>CREATE UNIQUE INDEX</code> membuat indeks unik (tidak ada nilai duplikat), sedangkan <code>DROP INDEX</code> digunakan untuk menghapus indeks yang tidak diperlukan.</p>
            <p><strong>DCL (Data Control Language)</strong> digunakan untuk mengontrol hak akses pengguna terhadap basis data. Perintah <code>GRANT</code> memberikan hak akses kepada user (misalnya hak SELECT, INSERT, DELETE, UPDATE, CREATE, atau ALL PRIVILEGES), sedangkan perintah <code>REVOKE</code> mencabut hak akses yang telah diberikan. Hak akses ini dikelola oleh administrator basis data (DBA) untuk menjamin keamanan data.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Sintaks INDEX dan DCL</div>
            <pre><code>-- Membuat indeks unik
CREATE UNIQUE INDEX ID_MHS ON MAHASISWA(NIM);

-- Menghapus indeks
DROP INDEX MHSIDX;

-- GRANT: memberikan hak akses
GRANT SELECT ON MAHASISWA TO ANANDA;
GRANT ALL ON FRI.* TO ADMIN;
GRANT ALL PRIVILEGES ON nama_db.* TO "user" IDENTIFIED BY "password";

-- REVOKE: mencabut hak akses
REVOKE SELECT ON FRI.MAHASISWA FROM ANANDA;
REVOKE ALL ON FRI.DOSEN FROM PUBLIC;</code></pre>
          </div>
        </div>
      </div>
    </section>

    <!-- ──────── MATERI 6 ──────── -->
    <section class="materi-section" id="materi-6" data-materi="6">
      <div class="materi-header" data-materi="6">
        <div class="materi-tag">Materi 6 · DML 1</div>
        <h2 class="materi-title">Data Manipulation Language (DML) 1</h2>
        <p class="materi-desc">Perintah INSERT, UPDATE, DELETE, dan SELECT dasar dengan berbagai operator kondisi, fungsi agregasi, GROUP BY, dan HAVING.</p>
      </div>

      <div class="sub-section open" id="sub-6-1">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">6.1</span>
          <span class="sub-title">INSERT — Menambah Data</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p><strong>DML (Data Manipulation Language)</strong> adalah kumpulan perintah SQL untuk memanipulasi isi data dalam tabel, mencakup empat operasi utama: INSERT (menambah), UPDATE (mengubah), DELETE (menghapus), dan SELECT (menampilkan). Perintah <code>INSERT INTO</code> digunakan untuk menambahkan baris (<em>record</em>) baru ke dalam tabel. Ada dua bentuk sintaks: menyebutkan nama kolom secara eksplisit (berguna jika tidak semua kolom diisi), atau tanpa nama kolom (semua kolom harus diisi sesuai urutan definisi tabel).</p>
            <p>Catatan penting dalam INSERT: field dengan constraint <code>NOT NULL</code> wajib diisi; penulisan data bertipe angka tidak diapit tanda petik; penulisan data bertipe string atau tanggal diapit tanda petik tunggal; dan data pada tabel anak (foreign key) harus ada terlebih dahulu di tabel induknya (<strong>referential integrity</strong>).</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Sintaks dan Contoh INSERT</div>
            <pre><code>-- Sintaks 1: dengan nama kolom
INSERT INTO nama_tabel (kolom_1, kolom_2, ..., kolom_n)
VALUES (nilai_1, nilai_2, ..., nilai_n);

-- Sintaks 2: tanpa nama kolom (semua kolom)
INSERT INTO nama_tabel
VALUES (nilai_1, nilai_2, ..., nilai_n);

-- Contoh:
INSERT INTO MAHASISWA VALUES (2001, 1, 'ANITA', 'MAGELANG', '1-JAN-85');
INSERT INTO MAHASISWA (NIM, NAMA, ThAng) VALUES (9, 'DAUD', 2005);</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-6-2">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">6.2</span>
          <span class="sub-title">UPDATE dan DELETE — Mengubah dan Menghapus Data</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Perintah <code>UPDATE</code> digunakan untuk mengubah nilai satu atau lebih kolom pada baris tertentu (atau semua baris jika tanpa klausa WHERE). Klausa <code>SET</code> menentukan kolom yang diubah beserta nilai barunya, dan klausa <code>WHERE</code> membatasi baris mana yang diubah. Tanpa klausa WHERE, seluruh baris dalam tabel akan diubah — ini harus diperhatikan dengan seksama.</p>
            <p>Perintah <code>DELETE FROM</code> digunakan untuk menghapus baris dari tabel. Sama seperti UPDATE, tanpa klausa <code>WHERE</code> berarti menghapus <em>semua</em> isi tabel (namun struktur tabel tetap ada, berbeda dengan DROP TABLE). Klausa WHERE pada UPDATE dan DELETE mendukung operator kondisi: relasional (=, &gt;, &lt;, &gt;=, &lt;=, &lt;&gt;), boolean (AND, OR, NOT), BETWEEN, IN, IS NULL, dan LIKE dengan <em>wildcard</em> (% untuk banyak karakter, _ untuk satu karakter).</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Sintaks UPDATE dan DELETE</div>
            <pre><code>-- UPDATE satu kolom semua baris
UPDATE mahasiswa SET thmasuk = 2018;

-- UPDATE kolom tertentu pada baris tertentu
UPDATE mahasiswa SET nama = 'Anita Marani' WHERE nim = 200;

-- UPDATE banyak kolom sekaligus
UPDATE Mahasiswa SET nama = 'Anita Mariana', thmasuk = 2000 WHERE nim = 200;

-- UPDATE dengan kondisi LIKE (wildcard)
UPDATE Mahasiswa SET Alamat = NULL WHERE Nama LIKE '%a%';

-- DELETE baris tertentu
DELETE FROM mahasiswa WHERE nama = 'ANITA';

-- DELETE semua isi tabel (struktur tetap ada)
DELETE FROM mahasiswa;</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-6-3">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">6.3</span>
          <span class="sub-title">SELECT Dasar dan Operator Kondisi</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Perintah <code>SELECT</code> adalah perintah SQL yang paling sering digunakan, berfungsi untuk menampilkan data dari satu atau lebih tabel. Struktur dasarnya menggunakan blok <strong>SELECT-FROM-WHERE</strong>: SELECT menentukan kolom yang ditampilkan, FROM menentukan tabel sumber, dan WHERE menentukan kondisi filter. Tanda <code>*</code> (asterik) pada SELECT berarti menampilkan semua kolom. Kata kunci <code>DISTINCT</code> digunakan untuk menghilangkan duplikasi hasil query.</p>
            <p>Klausa <code>ORDER BY</code> digunakan untuk mengurutkan hasil query berdasarkan kolom tertentu, secara <em>ascending</em> (ASC, default) atau <em>descending</em> (DESC). Kondisi pada klausa WHERE menggunakan berbagai operator: relasional (=, &gt;, &lt;, &gt;=, &lt;=, &lt;&gt;), boolean (AND, OR, NOT), BETWEEN (rentang nilai), IN (daftar nilai), IS NULL / IS NOT NULL (pengecekan nilai null), dan LIKE (pencocokan pola karakter).</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Sintaks SELECT dan Contoh</div>
            <pre><code>-- SELECT semua kolom
SELECT * FROM MAHASISWA;

-- SELECT kolom tertentu dengan kondisi
SELECT NIM, NAMA FROM MAHASISWA WHERE ALAMAT = 'YOGYA';

-- SELECT dengan ORDER BY
SELECT NIM, NAMA FROM MAHASISWA ORDER BY NAMA, ALAMAT DESC;

-- SELECT dengan LIKE
SELECT * FROM MAHASISWA WHERE Nama LIKE 'M%';       -- awalan M
SELECT * FROM MAHASISWA WHERE Nama LIKE '%BUDI%';   -- mengandung BUDI

-- SELECT dengan DISTINCT (hilangkan duplikat)
SELECT DISTINCT GAJI FROM KARYAWAN;

-- SELECT dengan BETWEEN dan IN
SELECT * FROM mahasiswa WHERE thmasuk BETWEEN 2019 AND 2022;
SELECT * FROM mahasiswa WHERE kota IN ('Bandung', 'Jakarta');</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-6-4">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">6.4</span>
          <span class="sub-title">Fungsi Agregasi dan GROUP BY</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Fungsi agregasi adalah fungsi SQL untuk mendapatkan informasi statistik dari sekumpulan data. Lima fungsi agregasi utama: <strong>MAX</strong> (nilai tertinggi), <strong>MIN</strong> (nilai terendah), <strong>AVG</strong> (rata-rata), <strong>SUM</strong> (jumlahan), dan <strong>COUNT</strong> (jumlahan item/baris). Catatan penting: fungsi selain COUNT harus menyebutkan nama kolom bertipe angka; COUNT(*) menghitung semua baris termasuk yang NULL.</p>
            <p>Klausa <code>GROUP BY</code> digunakan bersama fungsi agregasi untuk mengelompokkan hasil berdasarkan nilai kolom tertentu — setiap grup menghasilkan satu baris hasil agregasi. Klausa <code>HAVING</code> digunakan untuk memfilter hasil GROUP BY berdasarkan kondisi pada fungsi agregasi (analog dengan WHERE, tetapi HAVING bekerja setelah pengelompokan). Urutan klausa: SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Sintaks Fungsi Agregasi</div>
            <pre><code>-- Rata-rata, maks, min, dan jumlah gaji
SELECT AVG(salary), MAX(salary), MIN(salary), SUM(salary) FROM employees;

-- Jumlah karyawan di departemen tertentu
SELECT COUNT(*) FROM employees WHERE department_name = 'Computer Science';

-- Rata-rata gaji per departemen (GROUP BY)
SELECT department_id, AVG(salary) FROM employees GROUP BY department_id;

-- HAVING: tampilkan departemen dengan gaji maks > 10 juta
SELECT department_id, MAX(salary) FROM employees
GROUP BY department_id HAVING MAX(salary) > 10000000;

-- Rata-rata nilai mahasiswa per NIM (dengan filter dan urutan)
SELECT NIM, AVG(Nilai) FROM KRS
WHERE IdKelas > 1 GROUP BY NIM HAVING COUNT(*) > 1 ORDER BY NIM DESC;</code></pre>
          </div>
        </div>
      </div>
    </section>

    <!-- ──────── MATERI 7 ──────── -->
    <section class="materi-section" id="materi-7" data-materi="7">
      <div class="materi-header" data-materi="7">
        <div class="materi-tag">Materi 7 · DML 2</div>
        <h2 class="materi-title">DML 2: SELECT Lanjutan, SubQuery &amp; JOIN</h2>
        <p class="materi-desc">Operasi himpunan UNION/INTERSECT/EXCEPT, berbagai jenis JOIN (INNER, LEFT, RIGHT, FULL, NATURAL, CROSS), dan SubQuery (Nested Query).</p>
      </div>

      <div class="sub-section open" id="sub-7-1">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">7.1</span>
          <span class="sub-title">Operasi Himpunan: UNION, INTERSECT, EXCEPT</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>Operasi himpunan SQL memungkinkan penggabungan hasil dari dua atau lebih query SELECT. <strong>UNION</strong> menggabungkan semua hasil dari dua query (secara otomatis mengeliminasi duplikat); <strong>UNION ALL</strong> menggabungkan termasuk duplikat. <strong>INTERSECT</strong> mengambil data yang ada di kedua query (irisan); <strong>INTERSECT ALL</strong> mempertahankan duplikat. <strong>EXCEPT</strong> mengambil data yang ada di query pertama tapi tidak ada di query kedua (pengecualian); <strong>EXCEPT ALL</strong> mempertahankan duplikat.</p>
            <p>Syarat utama operasi himpunan adalah kedua query harus menghasilkan jumlah kolom yang sama dan tipe data yang kompatibel. Operasi-operasi ini sangat berguna untuk menggabungkan data dari tabel-tabel yang terpisah, misalnya menggabungkan daftar pelanggan dari dua cabang, mencari pelanggan yang ada di keduanya, atau mencari pelanggan yang hanya ada di satu cabang.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Sintaks Operasi Himpunan</div>
            <pre><code>-- UNION: gabungan, hilangkan duplikat
(SELECT nama_nasabah FROM penabung)
UNION
(SELECT nama_nasabah FROM peminjam);

-- UNION ALL: gabungan, pertahankan duplikat
(SELECT nama_nasabah FROM penabung)
UNION ALL
(SELECT nama_nasabah FROM peminjam);

-- INTERSECT: irisan
(SELECT DISTINCT nama_nasabah FROM penabung)
INTERSECT
(SELECT DISTINCT nama_nasabah FROM peminjam);

-- EXCEPT: pengecualian
(SELECT DISTINCT nama_nasabah FROM penabung)
EXCEPT
(SELECT DISTINCT nama_nasabah FROM peminjam);</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-7-2">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">7.2</span>
          <span class="sub-title">JOIN: Menggabungkan Data dari Banyak Tabel</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p>JOIN digunakan untuk menampilkan data dari lebih dari satu tabel sekaligus dalam satu query. Terdapat beberapa jenis JOIN: <strong>(1) INNER JOIN (= CROSS JOIN dengan kondisi)</strong> — menghasilkan baris yang memiliki nilai yang cocok di kedua tabel; <strong>(2) LEFT JOIN</strong> — menghasilkan semua baris dari tabel kiri plus baris yang cocok dari tabel kanan (baris tanpa pasangan di kanan diisi NULL); <strong>(3) RIGHT JOIN</strong> — kebalikan dari LEFT JOIN; <strong>(4) FULL JOIN</strong> — gabungan LEFT dan RIGHT JOIN (semua baris dari kedua tabel); <strong>(5) NATURAL JOIN</strong> — JOIN otomatis berdasarkan kolom bernama sama, kolom duplikat hanya ditampilkan sekali; <strong>(6) CROSS JOIN</strong> — Kartesian product dari dua tabel (semua kombinasi baris).</p>
            <p>Kesimpulan penting JOIN di MySQL: INNER JOIN = CROSS JOIN = JOIN (dengan klausa ON). Penggunaan <code>USING</code> dapat menghilangkan redundansi kolom yang sama. Pada INNER JOIN, urutan baris mengikuti operand kanan; pada LEFT JOIN, data dan urutan mengikuti operand kiri; pada RIGHT JOIN mengikuti operand kanan. Pada NATURAL JOIN tidak diperlukan klausa ON, WHERE, atau USING.</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Sintaks JOIN</div>
            <pre><code>-- INNER JOIN
SELECT * FROM tb_dosen INNER JOIN tb_mk ON tb_dosen.kode_dsn = tb_mk.kode_dsn;
-- atau:
SELECT * FROM tb_dosen INNER JOIN tb_mk USING (kode_dsn);

-- LEFT JOIN
SELECT * FROM tb_dosen LEFT JOIN tb_mk ON tb_dosen.kode_dsn = tb_mk.kode_dsn;

-- RIGHT JOIN
SELECT * FROM tb_dosen RIGHT JOIN tb_mk USING(kode_dsn);

-- NATURAL JOIN
SELECT * FROM tb_dosen NATURAL JOIN tb_mk;

-- FULL JOIN (simulasi di MySQL)
SELECT * FROM tb_dosen LEFT JOIN tb_mk ON tb_dosen.kode_dsn = tb_mk.kode_dsn
UNION
SELECT * FROM tb_dosen RIGHT JOIN tb_mk ON tb_dosen.kode_dsn = tb_mk.kode_dsn;

-- CROSS JOIN (Kartesian)
SELECT * FROM MAHASISWA CROSS JOIN MATKUL;</code></pre>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Ilustrasi Jenis JOIN</div>
            <pre><code>  Tabel A    Tabel B
  ┌─────┐    ┌─────┐
  │  1  │    │  1  │
  │  2  │    │  2  │
  │  3  │    │  4  │
  └─────┘    └─────┘

  INNER JOIN → 1, 2     (irisan)
  LEFT JOIN  → 1, 2, 3  (semua A + cocok di B)
  RIGHT JOIN → 1, 2, 4  (semua B + cocok di A)
  FULL JOIN  → 1,2,3,4  (semua dari keduanya)</code></pre>
          </div>
        </div>
      </div>

      <div class="sub-section" id="sub-7-3">
        <div class="sub-header" onclick="toggleSub(this.parentElement)">
          <span class="sub-number">7.3</span>
          <span class="sub-title">SubQuery (Nested Query)</span>
          <span class="sub-toggle">+</span>
        </div>
        <div class="sub-body">
          <div class="content-text">
            <p><strong>SubQuery</strong> adalah bentuk query yang berada di dalam query lain (<em>nested query</em> atau <em>subselect</em>) — memungkinkan terdapat pernyataan SELECT di dalam pernyataan SQL lainnya. Kegunaan SubQuery: meng-copy data dari satu tabel ke tabel lain, menerima data dari <em>inline view</em>, mengambil data dari tabel lain untuk di-UPDATE, dan menghapus baris berdasarkan kondisi dari tabel lain.</p>
            <p>Aturan SubQuery yang harus diperhatikan: (1) harus diapit tanda kurung; (2) klausa ORDER BY tidak boleh digunakan di dalam SubQuery; (3) SELECT di SubQuery harus mengandung satu nama kolom tunggal kecuali untuk SubQuery menggunakan EXISTS; (4) SubQuery tidak boleh digunakan sebagai operan dalam ekspresi. Modifier khusus: <strong>ANY</strong> (TRUE jika dipenuhi minimal satu nilai SubQuery), <strong>ALL</strong> (TRUE jika dipenuhi semua nilai SubQuery), <strong>EXISTS</strong> (TRUE jika SubQuery menghasilkan minimal satu baris), <strong>NOT EXISTS</strong> (kebalikan EXISTS).</p>
          </div>
          <div class="callout">
            <div class="callout-title">▸ Contoh SubQuery</div>
            <pre><code>-- Ambil nilai UTS dan UAS mahasiswa yang bernama 'Tiara Lestari'
SELECT nilai_uts, nilai_uas FROM tb_nilai
WHERE nim = (SELECT nim FROM tb_mahasiswa WHERE nama = 'Tiara Lestari');

-- Tampilkan nim, kode_mk yang nilai UAS-nya di atas rata-rata
SELECT nim, kode_mk, uas FROM tb_nilai
WHERE uas > (SELECT AVG(uas) FROM tb_nilai);

-- Ambil nama mahasiswa yang mengikuti ujian (EXISTS)
SELECT nama FROM tb_mahasiswa mhs
WHERE EXISTS (SELECT * FROM tb_nilai WHERE nim = mhs.nim);

-- Penggunaan ANY
SELECT nama FROM tb_mahasiswa
WHERE nim = ANY (SELECT nim FROM tb_nilai WHERE uas > 70);

-- Penggunaan ALL
SELECT nama FROM tb_mahasiswa
WHERE nilai > ALL (SELECT nilai FROM tb_nilai WHERE kelas = 'A');</code></pre>
          </div>
        </div>
      </div>
    </section>

  </main>
</div>

<footer style="text-align:center;padding:32px 20px;border-top:1px solid var(--border);color:var(--text-muted);font-family:'Space Mono',monospace;font-size:11px;letter-spacing:.5px;">
  ISH2G4 · Sistem Basis Data · Telkom University · S1 Sistem Informasi FRI
</footer>

<script>
  let currentMateri = 1;

  function switchMateri(num, btn) {
    // hide all sections
    document.querySelectorAll('.materi-section').forEach(s => s.classList.remove('active'));
    document.getElementById('materi-' + num).classList.add('active');

    // nav buttons
    document.querySelectorAll('.nav-materi-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');

    // sub navs
    document.querySelectorAll('.nav-sub').forEach(s => s.classList.remove('open'));
    const subNav = document.getElementById('sub-nav-' + num);
    if (subNav) subNav.classList.add('open');

    currentMateri = num;

    // scroll to top
    window.scrollTo({ top: 0, behavior: 'smooth' });
  }

  function toggleSub(section) {
    section.classList.toggle('open');
  }

  function scrollToSub(id) {
    const el = document.getElementById('sub-' + id);
    if (el) {
      if (!el.classList.contains('open')) el.classList.add('open');
      setTimeout(() => {
        el.scrollIntoView({ behavior: 'smooth', block: 'start' });
      }, 50);
    }
  }

  function filterNav(val) {
    const query = val.toLowerCase();
    document.querySelectorAll('.nav-section').forEach(section => {
      const links = section.querySelectorAll('.nav-sub-link');
      const btn = section.querySelector('.nav-materi-btn');
      let anyVisible = false;
      links.forEach(link => {
        const match = link.textContent.toLowerCase().includes(query);
        link.style.display = match ? '' : 'none';
        if (match) anyVisible = true;
      });
      // show/hide entire section
      section.style.display = (query === '' || anyVisible || btn.textContent.toLowerCase().includes(query)) ? '' : 'none';
      // open sub-nav if filtering
      if (query && anyVisible) {
        const subNav = section.querySelector('.nav-sub');
        if (subNav) subNav.classList.add('open');
      }
    });
  }

  // Keyboard shortcut: 1-7 to switch materi
  document.addEventListener('keydown', e => {
    if (e.target.tagName === 'INPUT') return;
    const num = parseInt(e.key);
    if (num >= 1 && num <= 7) {
      const btn = document.querySelector(`.nav-materi-btn[data-materi="${num}"]`);
      if (btn) switchMateri(num, btn);
    }
  });
</script>
</body>
</html>
