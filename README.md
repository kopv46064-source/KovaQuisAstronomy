<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>ISEA — International Space Education Association | Kuis Kosmos</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&family=Space+Grotesk:wght@600;700;800&display=swap');

    :root {
      --bg-void: #04050a;
      --glass-bg: rgba(12, 16, 30, 0.68);
      --glass-border: rgba(130, 170, 255, 0.18);
      --accent-bh: #ff8a3d;
      --accent-ns: #38bdf8;
      --accent-purple: #a855f7;
      --text-main: #f8fafc;
      --text-muted: #94a3b8;
      --correct: #10b981;
      --wrong: #f43f5e;
      --ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1);
      --ease-smooth: cubic-bezier(0.22, 1, 0.36, 1);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      -webkit-tap-highlight-color: transparent;
    }

    body {
      background-color: var(--bg-void);
      color: var(--text-main);
      font-family: 'Plus Jakarta Sans', system-ui, -apple-system, sans-serif;
      min-height: 100dvh;
      overflow-x: hidden;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-between;
      position: relative;
    }

    canvas#cosmos-canvas {
      position: fixed;
      inset: 0;
      width: 100%;
      height: 100%;
      z-index: 0;
      pointer-events: none;
    }

    /* Top Controls */
    .top-bar {
      position: fixed;
      top: 16px;
      left: 16px;
      right: 16px;
      z-index: 20;
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 12px;
      pointer-events: none;
    }

    .pill-group {
      display: flex;
      gap: 8px;
      pointer-events: auto;
    }

    .glass-btn {
      background: rgba(15, 23, 42, 0.72);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      border: 1px solid var(--glass-border);
      color: var(--text-main);
      padding: 8px 14px;
      border-radius: 999px;
      font-size: 0.82rem;
      font-weight: 600;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      transition: transform 0.35s var(--ease-spring), background 0.25s ease, border-color 0.25s ease;
    }

    .glass-btn:hover {
      transform: translateY(-2px) scale(1.03);
      background: rgba(30, 41, 59, 0.85);
      border-color: rgba(148, 163, 184, 0.45);
    }

    .glass-btn:active {
      transform: scale(0.96);
    }

    .glass-btn.active-bh {
      border-color: var(--accent-bh);
      box-shadow: 0 0 18px rgba(255, 138, 61, 0.25);
    }

    .glass-btn.active-ns {
      border-color: var(--accent-ns);
      box-shadow: 0 0 18px rgba(56, 189, 248, 0.25);
    }

    /* Main Container */
    .app-shell {
      position: relative;
      z-index: 10;
      width: 100%;
      max-width: 680px;
      padding: 80px 20px 20px;
      margin: auto 0;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    /* ISEA Branding Header */
    .org-header {
      text-align: center;
      margin-bottom: 18px;
      animation: cardEnter 0.7s var(--ease-spring) forwards;
    }

    .org-acronym {
      font-family: 'Space Grotesk', sans-serif;
      font-size: 1.55rem;
      font-weight: 800;
      letter-spacing: 0.28em;
      padding-left: 0.28em;
      background: linear-gradient(90deg, #7dd3fc, #f8fafc 50%, #fdba74);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      text-shadow: 0 0 24px rgba(56, 189, 248, 0.3);
    }

    .org-fullname {
      font-size: 0.72rem;
      font-weight: 700;
      letter-spacing: 0.16em;
      text-transform: uppercase;
      color: var(--text-muted);
      margin-top: 2px;
    }

    .card {
      width: 100%;
      background: var(--glass-bg);
      backdrop-filter: blur(18px);
      -webkit-backdrop-filter: blur(18px);
      border: 1px solid var(--glass-border);
      border-radius: 28px;
      padding: 32px 28px;
      box-shadow: 0 24px 60px rgba(0, 0, 0, 0.65), inset 0 1px 0 rgba(255, 255, 255, 0.08);
      transition: transform 0.6s var(--ease-spring), opacity 0.45s var(--ease-smooth);
    }

    .card.hidden {
      display: none;
    }

    .card.enter {
      animation: cardEnter 0.65s var(--ease-spring) forwards;
    }

    @keyframes cardEnter {
      0% { opacity: 0; transform: translateY(28px) scale(0.95); }
      100% { opacity: 1; transform: translateY(0) scale(1); }
    }

    @keyframes shake {
      0%, 100% { transform: translateX(0); }
      20% { transform: translateX(-8px); }
      40% { transform: translateX(8px); }
      60% { transform: translateX(-5px); }
      80% { transform: translateX(5px); }
    }

    .shake {
      animation: shake 0.45s var(--ease-smooth);
    }

    /* Start Screen */
    .badge {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      padding: 6px 14px;
      border-radius: 999px;
      background: linear-gradient(135deg, rgba(255, 138, 61, 0.16), rgba(56, 189, 248, 0.16));
      border: 1px solid rgba(255, 255, 255, 0.14);
      font-size: 0.78rem;
      font-weight: 700;
      letter-spacing: 0.04em;
      text-transform: uppercase;
      color: #e2e8f0;
      margin-bottom: 16px;
    }

    h1 {
      font-family: 'Space Grotesk', sans-serif;
      font-size: clamp(1.9rem, 5vw, 2.65rem);
      line-height: 1.12;
      font-weight: 700;
      background: linear-gradient(135deg, #ffffff 15%, #93c5fd 55%, #fdba74 95%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      margin-bottom: 14px;
    }

    .subtitle {
      color: var(--text-muted);
      font-size: 1rem;
      line-height: 1.6;
      margin-bottom: 26px;
    }

    .feature-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 12px;
      margin-bottom: 28px;
    }

    .feature-item {
      background: rgba(15, 23, 42, 0.55);
      border: 1px solid rgba(255, 255, 255, 0.07);
      border-radius: 18px;
      padding: 14px 16px;
      display: flex;
      align-items: center;
      gap: 12px;
      transition: transform 0.35s var(--ease-spring);
    }

    .feature-item:hover {
      transform: translateY(-3px);
    }

    .feature-icon {
      font-size: 1.4rem;
    }

    .feature-text strong {
      display: block;
      font-size: 0.9rem;
      color: #f1f5f9;
    }

    .feature-text span {
      font-size: 0.78rem;
      color: var(--text-muted);
    }

    .primary-btn {
      width: 100%;
      padding: 16px 24px;
      border: none;
      border-radius: 18px;
      background: linear-gradient(135deg, #f97316, #a855f7 55%, #0ea5e9);
      background-size: 200% 200%;
      color: #fff;
      font-family: 'Space Grotesk', sans-serif;
      font-size: 1.08rem;
      font-weight: 700;
      cursor: pointer;
      box-shadow: 0 12px 30px rgba(168, 85, 247, 0.35);
      transition: transform 0.4s var(--ease-spring), box-shadow 0.3s ease, background-position 0.5s ease;
    }

    .primary-btn:hover {
      transform: translateY(-3px) scale(1.015);
      background-position: 100% 50%;
      box-shadow: 0 18px 38px rgba(56, 189, 248, 0.42);
    }

    .primary-btn:active {
      transform: scale(0.97);
    }

    /* Quiz Screen */
    .quiz-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 14px;
      gap: 12px;
    }

    .topic-tag {
      font-size: 0.76rem;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 0.05em;
      padding: 5px 12px;
      border-radius: 999px;
      background: rgba(56, 189, 248, 0.14);
      color: #7dd3fc;
      border: 1px solid rgba(56, 189, 248, 0.28);
    }

    .topic-tag.bh {
      background: rgba(255, 138, 61, 0.14);
      color: #fdba74;
      border-color: rgba(255, 138, 61, 0.28);
    }

    .stats-row {
      display: flex;
      align-items: center;
      gap: 14px;
      font-size: 0.88rem;
      font-weight: 700;
    }

    .streak-pill {
      color: #fbbf24;
      display: inline-flex;
      align-items: center;
      gap: 4px;
    }

    .progress-track {
      width: 100%;
      height: 7px;
      background: rgba(255, 255, 255, 0.08);
      border-radius: 999px;
      overflow: hidden;
      margin-bottom: 22px;
    }

    .progress-fill {
      height: 100%;
      width: 0%;
      border-radius: 999px;
      background: linear-gradient(90deg, #ff8a3d, #a855f7, #38bdf8);
      transition: width 0.6s var(--ease-spring);
    }

    .question-text {
      font-family: 'Space Grotesk', sans-serif;
      font-size: clamp(1.22rem, 3.2vw, 1.48rem);
      line-height: 1.38;
      font-weight: 700;
      margin-bottom: 22px;
      color: #f8fafc;
    }

    .options-list {
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .option-btn {
      width: 100%;
      text-align: left;
      background: rgba(15, 23, 42, 0.62);
      border: 1px solid rgba(148, 163, 184, 0.2);
      border-radius: 18px;
      padding: 15px 18px;
      color: #f1f5f9;
      font-family: inherit;
      font-size: 0.96rem;
      font-weight: 600;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 14px;
      transition: transform 0.35s var(--ease-spring), background 0.25s ease, border-color 0.25s ease, box-shadow 0.25s ease;
      opacity: 0;
      animation: optionSlide 0.45s var(--ease-spring) forwards;
    }

    @keyframes optionSlide {
      from { opacity: 0; transform: translateX(-14px); }
      to { opacity: 1; transform: translateX(0); }
    }

    .option-btn:hover:not(:disabled) {
      transform: translateX(6px) scale(1.01);
      background: rgba(30, 41, 59, 0.85);
      border-color: rgba(125, 211, 252, 0.55);
      box-shadow: 0 8px 24px rgba(0, 0, 0, 0.35);
    }

    .option-btn:active:not(:disabled) {
      transform: scale(0.98);
    }

    .option-letter {
      width: 32px;
      height: 32px;
      border-radius: 10px;
      background: rgba(255, 255, 255, 0.08);
      display: grid;
      place-items: center;
      font-weight: 800;
      font-size: 0.85rem;
      flex-shrink: 0;
      transition: background 0.25s ease, color 0.25s ease;
    }

    .option-btn.correct {
      background: rgba(16, 185, 129, 0.18);
      border-color: var(--correct);
      box-shadow: 0 0 24px rgba(16, 185, 129, 0.25);
      transform: scale(1.015);
    }

    .option-btn.correct .option-letter {
      background: var(--correct);
      color: #022c22;
    }

    .option-btn.wrong {
      background: rgba(244, 63, 94, 0.18);
      border-color: var(--wrong);
    }

    .option-btn.wrong .option-letter {
      background: var(--wrong);
      color: #fff;
    }

    .option-btn:disabled {
      cursor: default;
    }

    /* Explanation Box */
    .explanation-box {
      margin-top: 20px;
      padding: 16px 18px;
      border-radius: 18px;
      background: rgba(15, 23, 42, 0.78);
      border-left: 4px solid var(--accent-ns);
      font-size: 0.9rem;
      line-height: 1.55;
      color: #cbd5e1;
      display: none;
      animation: cardEnter 0.45s var(--ease-spring) forwards;
    }

    .explanation-box.visible {
      display: block;
    }

    .explanation-box strong {
      display: block;
      color: #f8fafc;
      margin-bottom: 4px;
    }

    .next-btn {
      margin-top: 18px;
      display: none;
    }

    .next-btn.visible {
      display: block;
      animation: cardEnter 0.4s var(--ease-spring) forwards;
    }

    /* Result Screen */
    .result-score-ring {
      width: 132px;
      height: 132px;
      border-radius: 50%;
      margin: 0 auto 20px;
      display: grid;
      place-items: center;
      background: radial-gradient(circle at 30% 30%, rgba(56, 189, 248, 0.25), rgba(168, 85, 247, 0.2));
      border: 2px solid rgba(125, 211, 252, 0.45);
      box-shadow: 0 0 40px rgba(56, 189, 248, 0.25);
      animation: floatSlow 4s ease-in-out infinite;
    }

    @keyframes floatSlow {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-8px); }
    }

    .result-score-ring span {
      font-family: 'Space Grotesk', sans-serif;
      font-size: 2.3rem;
      font-weight: 800;
      color: #fff;
    }

    .result-title {
      text-align: center;
      font-family: 'Space Grotesk', sans-serif;
      font-size: 1.75rem;
      margin-bottom: 8px;
    }

    .result-desc {
      text-align: center;
      color: var(--text-muted);
      font-size: 0.96rem;
      line-height: 1.55;
      margin-bottom: 24px;
    }

    .summary-stats {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;
      margin-bottom: 24px;
    }

    .stat-box {
      background: rgba(15, 23, 42, 0.6);
      border: 1px solid rgba(255, 255, 255, 0.08);
      border-radius: 16px;
      padding: 14px 10px;
      text-align: center;
    }

    .stat-box .val {
      font-family: 'Space Grotesk', sans-serif;
      font-size: 1.35rem;
      font-weight: 700;
      color: #f8fafc;
    }

    .stat-box .lbl {
      font-size: 0.75rem;
      color: var(--text-muted);
      margin-top: 2px;
    }

    /* Footer Signature (KovaTheGreat) */
    .creator-footer {
      position: relative;
      z-index: 10;
      padding: 14px 20px 22px;
      text-align: center;
      font-size: 0.8rem;
      color: var(--text-muted);
      letter-spacing: 0.04em;
    }

    .creator-badge {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      padding: 6px 14px;
      border-radius: 999px;
      background: rgba(15, 23, 42, 0.65);
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
      border: 1px solid rgba(255, 255, 255, 0.09);
      transition: transform 0.35s var(--ease-spring), border-color 0.25s ease;
    }

    .creator-badge:hover {
      transform: translateY(-2px);
      border-color: rgba(125, 211, 252, 0.4);
    }

    .creator-name {
      font-family: 'Space Grotesk', sans-serif;
      font-weight: 700;
      background: linear-gradient(90deg, #38bdf8, #c084fc, #fb923c);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    @media (max-width: 520px) {
      .card { padding: 24px 18px; border-radius: 22px; }
      .feature-grid { grid-template-columns: 1fr; }
      .glass-btn span.hide-mobile { display: none; }
      .org-acronym { font-size: 1.3rem; }
      .org-fullname { font-size: 0.65rem; letter-spacing: 0.11em; }
    }
  </style>
</head>
<body>
  <canvas id="cosmos-canvas"></canvas>

  <header class="top-bar">
    <div class="pill-group">
      <button id="btn-focus-bh" class="glass-btn active-bh" title="Fokus ke Black Hole">
        🕳️ <span>Black Hole</span>
      </button>
      <button id="btn-focus-ns" class="glass-btn" title="Fokus ke Neutron Star">
        💫 <span>Neutron Star</span>
      </button>
    </div>
    <div class="pill-group">
      <button id="btn-audio" class="glass-btn" title="Suara Kosmik">
        🔇 <span class="hide-mobile">Audio</span>
      </button>
    </div>
  </header>

  <main class="app-shell">
    <!-- ISEA IDENTITY HEADER -->
    <div class="org-header">
      <div class="org-acronym">ISEA</div>
      <div class="org-fullname">International Space Education Association</div>
    </div>

    <!-- START SCREEN -->
    <section id="screen-start" class="card enter">
      <div class="badge">✨ Simulasi Kosmik Interaktif</div>
      <h1>Misteri Black Hole & Neutron Star</h1>
      <p class="subtitle">
        Uji pengetahuan astrofisika kamu bersama <strong>ISEA</strong> sambil menjelajahi lengkungan ruang-waktu di sekitar lubang hitam supermasif dan bintang neutron berkecepatan tinggi.
      </p>

      <div class="feature-grid">
        <div class="feature-item">
          <div class="feature-icon">🕳️</div>
          <div class="feature-text">
            <strong>Event Horizon</strong>
            <span>Piringan akresi & lensa gravitasi</span>
          </div>
        </div>
        <div class="feature-item">
          <div class="feature-icon">⚡</div>
          <div class="feature-text">
            <strong>Pulsar Relativistik</strong>
            <span>Rotasi ekstrem & jet magnetik</span>
          </div>
        </div>
      </div>

      <button id="btn-start" class="primary-btn">Mulai Ekspedisi Kuis 🚀</button>
    </section>

    <!-- QUIZ SCREEN -->
    <section id="screen-quiz" class="card hidden">
      <div class="quiz-header">
        <span id="q-topic" class="topic-tag bh">Black Hole</span>
        <div class="stats-row">
          <span id="q-streak" class="streak-pill">🔥 0</span>
          <span id="q-counter">1 / 10</span>
        </div>
      </div>

      <div class="progress-track">
        <div id="progress-fill" class="progress-fill"></div>
      </div>

      <h2 id="q-text" class="question-text">Memuat pertanyaan...</h2>

      <div id="options-container" class="options-list"></div>

      <div id="explanation-box" class="explanation-box">
        <strong id="exp-title">Penjelasan Ilmiah:</strong>
        <span id="exp-text"></span>
      </div>

      <button id="btn-next" class="primary-btn next-btn">Lanjut ke Sektor Berikutnya →</button>
    </section>

    <!-- RESULT SCREEN -->
    <section id="screen-result" class="card hidden">
      <div class="result-score-ring">
        <span id="res-score">0%</span>
      </div>
      <h2 id="res-title" class="result-title">Penjelajah Kosmik</h2>
      <p id="res-desc" class="result-desc"></p>

      <div class="summary-stats">
        <div class="stat-box">
          <div id="res-correct" class="val">0/10</div>
          <div class="lbl">Jawaban Benar</div>
        </div>
        <div class="stat-box">
          <div id="res-points" class="val">0</div>
          <div class="lbl">Total Skor</div>
        </div>
        <div class="stat-box">
          <div id="res-best-streak" class="val">0</div>
          <div class="lbl">Streak Terbaik</div>
        </div>
      </div>

      <button id="btn-restart" class="primary-btn">Ulangi Ekspedisi 🔄</button>
    </section>
  </main>

  <!-- FOOTER SIGNATURE -->
  <footer class="creator-footer">
    <div class="creator-badge">
      <span>ISEA • International Space Education Association</span>
      <span>•</span>
      <span class="creator-name">KovaTheGreat</span>
    </div>
  </footer>

  <script>
    /* =========================================
       1. BANK SOAL ASTROFISIKA (BLACK HOLE & NEUTRON STAR)
    ========================================= */
    const QUESTIONS = [
      {
        topic: "Black Hole",
        mode: "bh",
        question: "Apa nama batas di sekitar lubang hitam di mana kecepatan lepasnya melebihi kecepatan cahaya?",
        options: [
          "Singularitas Kuantum",
          "Event Horizon (Horizon Peristiwa)",
          "Fotosfer Relativistik",
          "Batas Chandrasekhar"
        ],
        answer: 1,
        explanation: "Event Horizon adalah batas 'tanpa jalan kembali'. Setelah melewati titik ini, tidak ada materi maupun radiasi elektromagnetik (termasuk cahaya) yang bisa lolos dari tarikan gravitasi lubang hitam."
      },
      {
        topic: "Neutron Star",
        mode: "ns",
        question: "Seberapa padat materi penyusun sebuah bintang neutron?",
        options: [
          "Satu sendok teh materinya bermassa sekitar 1 miliar ton",
          "Setara dengan kepadatan inti planet Jupiter",
          "Sepuluh kali lebih padat dari besi murni di Bumi",
          "Memiliki kepadatan yang sama dengan inti Matahari"
        ],
        answer: 0,
        explanation: "Bintang neutron memampatkan massa sekitar 1,4 hingga 2 kali massa Matahari ke dalam bola berdiameter hanya ~20 kilometer, membuat satu sendok teh materinya seberat gunung di Bumi."
      },
      {
        topic: "Black Hole",
        mode: "bh",
        question: "Fenomena peregangan ekstrem pada objek yang jatuh mendekati lubang hitam akibat perbedaan gaya pasang surut gravitasi disebut...",
        options: [
          "Lensing Gravitasi",
          "Radiasi Hawking",
          "Spaghettification (Spagetifikasi)",
          "Efek Doppler Relativistik"
        ],
        answer: 2,
        explanation: "Spagetifikasi terjadi karena gravitasi di bagian tubuh yang lebih dekat ke lubang hitam jauh lebih kuat daripada bagian yang lebih jauh, sehingga objek tertarik memanjang seperti mi."
      },
      {
        topic: "Neutron Star",
        mode: "ns",
        question: "Bintang neutron yang berputar sangat cepat dan memancarkan berkas radiasi dari kutub magnetiknya seperti mercusuar disebut...",
        options: [
          "Quasar",
          "Pulsar",
          "Blazar",
          "Supernova Tipe Ia"
        ],
        answer: 1,
        explanation: "Pulsar memancarkan sinar gelombang radio atau sinar-X dari kutub magnetiknya. Ketika berkas itu menyapu arah Bumi saat bintang berotasi, kita mendeteksinya sebagai denyutan teratur."
      },
      {
        topic: "Black Hole",
        mode: "bh",
        question: "Mengapa cahaya tampak melengkung membentuk cincin terang di atas dan bawah bayangan lubang hitam?",
        options: [
          "Pantulan cahaya dari permukaan padat lubang hitam",
          "Gravitational Lensing (Pembelokan ruang-waktu)",
          "Pembiasan atmosfer gas helium dingin",
          "Medan magnetik bintang pendamping"
        ],
        answer: 1,
        explanation: "Massa lubang hitam melengkungkan jaringan ruang-waktu secara ekstrem sehingga cahaya dari bagian belakang piringan akresi dibelokkan ke atas dan ke bawah."
      },
      {
        topic: "Neutron Star",
        mode: "ns",
        question: "Jenis bintang neutron dengan medan magnet paling kuat yang pernah diketahui di alam semesta adalah...",
        options: [
          "Katai Putih",
          "Magnetar",
          "Protobintang",
          "Bintang Wolf-Rayet"
        ],
        answer: 1,
        explanation: "Magnetar memiliki medan magnet hingga seribu triliun kali lebih kuat dari medan magnet Bumi, cukup kuat untuk merusak struktur atom dari jarak ribuan kilometer."
      },
      {
        topic: "Black Hole",
        mode: "bh",
        question: "Apa nama lubang hitam supermasif yang berada di pusat galaksi Bima Sakti kita?",
        options: [
          "Messier 87*",
          "Cygnus X-1",
          "Sagittarius A*",
          "TON 618"
        ],
        answer: 2,
        explanation: "Sagittarius A* (dibaca Sagittarius A-star) memiliki massa sekitar 4 juta kali massa Matahari dan dicitrakan secara langsung oleh Event Horizon Telescope pada tahun 2022."
      },
      {
        topic: "Neutron Star",
        mode: "ns",
        question: "Peristiwa dahsyat apa yang biasanya melahirkan sebuah bintang neutron?",
        options: [
          "Tabrakan dua komet raksasa",
          "Ledakan Supernova runtuh-inti dari bintang masif",
          "Pendinginan bintang katai merah",
          "Penggabungan dua planet gas"
        ],
        answer: 1,
        explanation: "Ketika bintang bermassa sekitar 8–25 kali massa Matahari kehabisan bahan bakar nuklir, intinya runtuh oleh gravitasinya sendiri dan memicu ledakan supernova."
      },
      {
        topic: "Black Hole",
        mode: "bh",
        question: "Menurut teori Stephen Hawking, bagaimana lubang hitam dapat perlahan kehilangan massanya dalam waktu sangat lama?",
        options: [
          "Melalui pancaran Radiasi Hawking di dekat Event Horizon",
          "Karena penguapan air di piringan akresi",
          "Akibat angin matahari dari bintang terdekat",
          "Melalui peluruhan radioaktif inti besi"
        ],
        answer: 0,
        explanation: "Fluktuasi kuantum di dekat Event Horizon menciptakan pasangan partikel-antipartikel; salah satu jatuh ke dalam sementara yang lain lolos sebagai Radiasi Hawking."
      },
      {
        topic: "Neutron Star",
        mode: "ns",
        question: "Tabrakan dua bintang neutron (kilonova) sangat penting bagi kosmologi karena menghasilkan...",
        options: [
          "Gas hidrogen murni pertama di alam semesta",
          "Gelombang gravitasi dan unsur berat seperti emas & platinum",
          "Lapisan ozon di planet-planet terestrial",
          "Cincin es di sekitar planet raksasa"
        ],
        answer: 1,
        explanation: "Penggabungan bintang neutron memancarkan riak gelombang gravitasi dan memicu proses nukleosintesis cepat yang menciptakan logam berat seperti emas dan platinum."
      }
    ];

    /* =========================================
       2. WEB AUDIO API (SYNTHESIZER KOSMIK)
    ========================================= */
    class CosmicAudio {
      constructor() {
        this.ctx = null;
        this.enabled = false;
        this.droneOsc = null;
        this.droneGain = null;
      }

      init() {
        if (!this.ctx) {
          const AudioCtx = window.AudioContext || window.webkitAudioContext;
          this.ctx = new AudioCtx();
        }
        if (this.ctx.state === 'suspended') {
          this.ctx.resume();
        }
      }

      toggle() {
        this.init();
        this.enabled = !this.enabled;
        if (this.enabled) {
          this.startAmbient();
          this.playTone(520, 'sine', 0.12, 0.08);
        } else {
          this.stopAmbient();
        }
        return this.enabled;
      }

      startAmbient() {
        if (!this.ctx || this.droneOsc) return;
        this.droneOsc = this.ctx.createOscillator();
        const subOsc = this.ctx.createOscillator();
        this.droneGain = this.ctx.createGain();
        const filter = this.ctx.createBiquadFilter();

        this.droneOsc.type = 'sine';
        this.droneOsc.frequency.value = 55;
        subOsc.type = 'triangle';
        subOsc.frequency.value = 82.41;

        filter.type = 'lowpass';
        filter.frequency.value = 180;

        this.droneGain.gain.value = 0.045;

        this.droneOsc.connect(filter);
        subOsc.connect(filter);
        filter.connect(this.droneGain);
        this.droneGain.connect(this.ctx.destination);

        this.droneOsc.start();
        subOsc.start();
        this.subOsc = subOsc;
      }

      stopAmbient() {
        if (this.droneOsc) {
          try {
            this.droneOsc.stop();
            this.subOsc.stop();
          } catch (e) {}
          this.droneOsc = null;
          this.subOsc = null;
        }
      }

      playTone(freq, type = 'sine', duration = 0.25, volume = 0.12) {
        if (!this.enabled || !this.ctx) return;
        const now = this.ctx.currentTime;
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.type = type;
        osc.frequency.setValueAtTime(freq, now);
        gain.gain.setValueAtTime(volume, now);
        gain.gain.exponentialRampToValueAtTime(0.0001, now + duration);
        osc.connect(gain);
        gain.connect(this.ctx.destination);
        osc.start(now);
        osc.stop(now + duration);
      }

      playCorrect() {
        this.playTone(523.25, 'sine', 0.25, 0.14);
        setTimeout(() => this.playTone(659.25, 'sine', 0.28, 0.14), 80);
        setTimeout(() => this.playTone(783.99, 'triangle', 0.4, 0.14), 160);
      }

      playWrong() {
        this.playTone(190, 'sawtooth', 0.28, 0.09);
        setTimeout(() => this.playTone(145, 'sawtooth', 0.35, 0.09), 110);
      }

      playClick() {
        this.playTone(440, 'sine', 0.08, 0.06);
      }
    }

    const audio = new CosmicAudio();

    /* =========================================
       3. SIMULASI VISUAL CANVAS (BLACK HOLE & PULSAR)
    ========================================= */
    const canvas = document.getElementById('cosmos-canvas');
    const ctx = canvas.getContext('2d');

    let width = canvas.width = window.innerWidth;
    let height = canvas.height = window.innerHeight;

    window.addEventListener('resize', () => {
      width = canvas.width = window.innerWidth;
      height = canvas.height = window.innerHeight;
    });

    const scene = {
      targetMode: 0,
      blend: 0,
      mouseX: 0,
      mouseY: 0,
      smoothX: 0,
      smoothY: 0,
      time: 0,
      burstParticles: []
    };

    window.addEventListener('pointermove', (e) => {
      scene.mouseX = (e.clientX / width - 0.5) * 2;
      scene.mouseY = (e.clientY / height - 0.5) * 2;
    });

    const stars = Array.from({ length: 160 }, () => ({
      x: Math.random(),
      y: Math.random(),
      z: Math.random() * 0.8 + 0.2,
      size: Math.random() * 1.7 + 0.4,
      twinkle: Math.random() * Math.PI * 2
    }));

    const diskParticles = Array.from({ length: 140 }, () => ({
      angle: Math.random() * Math.PI * 2,
      radius: 75 + Math.random() * 145,
      speed: 0.008 + Math.random() * 0.015,
      size: Math.random() * 2.2 + 0.8,
      hue: 22 + Math.random() * 28
    }));

    function spawnBurst(color) {
      const cx = width * 0.5;
      const cy = height * 0.42;
      for (let i = 0; i < 36; i++) {
        const angle = (Math.PI * 2 * i) / 36 + Math.random() * 0.2;
        const speed = 2.5 + Math.random() * 5.5;
        scene.burstParticles.push({
          x: cx,
          y: cy,
          vx: Math.cos(angle) * speed,
          vy: Math.sin(angle) * speed,
          life: 1,
          color
        });
      }
    }

    function drawBlackHole(cx, cy, alpha) {
      if (alpha <= 0.01) return;
      ctx.save();
      ctx.globalAlpha = alpha;
      ctx.translate(cx, cy);

      const ambientGlow = ctx.createRadialGradient(0, 0, 50, 0, 0, 260);
      ambientGlow.addColorStop(0, 'rgba(255, 130, 45, 0.28)');
      ambientGlow.addColorStop(0.5, 'rgba(180, 65, 20, 0.1)');
      ambientGlow.addColorStop(1, 'rgba(0, 0, 0, 0)');
      ctx.fillStyle = ambientGlow;
      ctx.beginPath();
      ctx.arc(0, 0, 260, 0, Math.PI * 2);
      ctx.fill();

      ctx.save();
      ctx.strokeStyle = 'rgba(255, 185, 110, 0.42)';
      ctx.lineWidth = 10;
      ctx.shadowColor = '#ff7b29';
      ctx.shadowBlur = 24;
      ctx.beginPath();
      ctx.arc(0, 0, 74, 0, Math.PI * 2);
      ctx.stroke();
      ctx.restore();

      ctx.save();
      ctx.rotate(-0.22);
      ctx.scale(1, 0.28);

      const diskGrad = ctx.createRadialGradient(0, 0, 72, 0, 0, 230);
      diskGrad.addColorStop(0, 'rgba(255, 240, 200, 0.95)');
      diskGrad.addColorStop(0.25, 'rgba(255, 140, 40, 0.8)');
      diskGrad.addColorStop(0.65, 'rgba(190, 55, 15, 0.35)');
      diskGrad.addColorStop(1, 'rgba(0, 0, 0, 0)');

      ctx.fillStyle = diskGrad;
      ctx.beginPath();
      ctx.arc(0, 0, 230, 0, Math.PI * 2);
      ctx.fill();

      diskParticles.forEach(p => {
        p.angle += p.speed;
        const px = Math.cos(p.angle) * p.radius;
        const py = Math.sin(p.angle) * p.radius;
        ctx.fillStyle = `hsla(${p.hue}, 100%, 72%, 0.85)`;
        ctx.beginPath();
        ctx.arc(px, py, p.size, 0, Math.PI * 2);
        ctx.fill();
      });
      ctx.restore();

      ctx.fillStyle = '#010103';
      ctx.beginPath();
      ctx.arc(0, 0, 66, 0, Math.PI * 2);
      ctx.fill();

      ctx.strokeStyle = 'rgba(255, 225, 175, 0.9)';
      ctx.lineWidth = 2;
      ctx.beginPath();
      ctx.arc(0, 0, 67, 0, Math.PI * 2);
      ctx.stroke();

      ctx.restore();
    }

    function drawNeutronStar(cx, cy, alpha, t) {
      if (alpha <= 0.01) return;
      ctx.save();
      ctx.globalAlpha = alpha;
      ctx.translate(cx, cy);

      const corona = ctx.createRadialGradient(0, 0, 12, 0, 0, 210);
      corona.addColorStop(0, 'rgba(186, 230, 253, 0.85)');
      corona.addColorStop(0.25, 'rgba(56, 189, 248, 0.35)');
      corona.addColorStop(0.65, 'rgba(99, 102, 241, 0.12)');
      corona.addColorStop(1, 'rgba(0, 0, 0, 0)');
      ctx.fillStyle = corona;
      ctx.beginPath();
      ctx.arc(0, 0, 210, 0, Math.PI * 2);
      ctx.fill();

      const rot = t * 2.8;
      ctx.save();
      ctx.rotate(Math.sin(rot) * 0.45);

      ctx.strokeStyle = 'rgba(125, 211, 252, 0.24)';
      ctx.lineWidth = 1.4;
      for (let i = 1; i <= 3; i++) {
        const rx = i * 42;
        const ry = i * 28;
        ctx.beginPath();
        ctx.ellipse(-rx, 0, rx, ry, 0, 0, Math.PI * 2);
        ctx.stroke();
        ctx.beginPath();
        ctx.ellipse(rx, 0, rx, ry, 0, 0, Math.PI * 2);
        ctx.stroke();
      }

      const pulseScale = 0.85 + Math.abs(Math.cos(rot)) * 0.35;
      const jetGradTop = ctx.createLinearGradient(0, 0, 0, -290 * pulseScale);
      jetGradTop.addColorStop(0, 'rgba(224, 242, 254, 0.95)');
      jetGradTop.addColorStop(0.4, 'rgba(56, 189, 248, 0.45)');
      jetGradTop.addColorStop(1, 'rgba(56, 189, 248, 0)');

      ctx.fillStyle = jetGradTop;
      ctx.beginPath();
      ctx.moveTo(-4, 0);
      ctx.lineTo(-34 * pulseScale, -290 * pulseScale);
      ctx.lineTo(34 * pulseScale, -290 * pulseScale);
      ctx.lineTo(4, 0);
      ctx.closePath();
      ctx.fill();

      const jetGradBot = ctx.createLinearGradient(0, 0, 0, 290 * pulseScale);
      jetGradBot.addColorStop(0, 'rgba(224, 242, 254, 0.95)');
      jetGradBot.addColorStop(0.4, 'rgba(56, 189, 248, 0.45)');
      jetGradBot.addColorStop(1, 'rgba(56, 189, 248, 0)');

      ctx.fillStyle = jetGradBot;
      ctx.beginPath();
      ctx.moveTo(-4, 0);
      ctx.lineTo(-34 * pulseScale, 290 * pulseScale);
      ctx.lineTo(34 * pulseScale, 290 * pulseScale);
      ctx.lineTo(4, 0);
      ctx.closePath();
      ctx.fill();

      ctx.restore();

      ctx.shadowColor = '#38bdf8';
      ctx.shadowBlur = 32;
      ctx.fillStyle = '#ffffff';
      ctx.beginPath();
      ctx.arc(0, 0, 18, 0, Math.PI * 2);
      ctx.fill();

      ctx.restore();
    }

    function animate() {
      scene.time += 0.016;
      scene.blend += (scene.targetMode - scene.blend) * 0.05;
      scene.smoothX += (scene.mouseX - scene.smoothX) * 0.06;
      scene.smoothY += (scene.mouseY - scene.smoothY) * 0.06;

      ctx.clearRect(0, 0, width, height);

      stars.forEach(s => {
        const px = (s.x * width + scene.smoothX * 26 * s.z + width) % width;
        const py = (s.y * height + scene.smoothY * 26 * s.z + height) % height;
        const twinkle = 0.45 + Math.sin(scene.time * 2.2 + s.twinkle) * 0.45;
        ctx.fillStyle = `rgba(226, 232, 240, ${twinkle * s.z})`;
        ctx.beginPath();
        ctx.arc(px, py, s.size, 0, Math.PI * 2);
        ctx.fill();
      });

      const cx = width * 0.5 + scene.smoothX * 22;
      const cy = height * 0.25 + scene.smoothY * 18;

      drawBlackHole(cx, cy, 1 - scene.blend);
      drawNeutronStar(cx, cy, scene.blend, scene.time);

      for (let i = scene.burstParticles.length - 1; i >= 0; i--) {
        const p = scene.burstParticles[i];
        p.x += p.vx;
        p.y += p.vy;
        p.vx *= 0.95;
        p.vy *= 0.95;
        p.life -= 0.025;
        if (p.life <= 0) {
          scene.burstParticles.splice(i, 1);
          continue;
        }
        ctx.fillStyle = p.color;
        ctx.globalAlpha = p.life;
        ctx.beginPath();
        ctx.arc(p.x, p.y, 3.2 * p.life, 0, Math.PI * 2);
        ctx.fill();
        ctx.globalAlpha = 1;
      }

      requestAnimationFrame(animate);
    }

    animate();

    /* =========================================
       4. LOGIKA PERMAINAN KUIS
    ========================================= */
    let currentIndex = 0;
    let score = 0;
    let correctCount = 0;
    let streak = 0;
    let bestStreak = 0;
    let answered = false;

    const screenStart = document.getElementById('screen-start');
    const screenQuiz = document.getElementById('screen-quiz');
    const screenResult = document.getElementById('screen-result');

    const btnFocusBH = document.getElementById('btn-focus-bh');
    const btnFocusNS = document.getElementById('btn-focus-ns');
    const btnAudio = document.getElementById('btn-audio');

    function setVisualMode(mode) {
      if (mode === 'bh') {
        scene.targetMode = 0;
        btnFocusBH.classList.add('active-bh');
        btnFocusNS.classList.remove('active-ns');
      } else {
        scene.targetMode = 1;
        btnFocusNS.classList.add('active-ns');
        btnFocusBH.classList.remove('active-bh');
      }
    }

    btnFocusBH.addEventListener('click', () => {
      audio.playClick();
      setVisualMode('bh');
    });

    btnFocusNS.addEventListener('click', () => {
      audio.playClick();
      setVisualMode('ns');
    });

    btnAudio.addEventListener('click', () => {
      const isOn = audio.toggle();
      btnAudio.innerHTML = isOn
        ? '🔊 <span class="hide-mobile">Audio</span>'
        : '🔇 <span class="hide-mobile">Audio</span>';
    });

    function showScreen(target) {
      [screenStart, screenQuiz, screenResult].forEach(el => {
        el.classList.add('hidden');
        el.classList.remove('enter');
      });
      target.classList.remove('hidden');
      void target.offsetWidth;
      target.classList.add('enter');
    }

    function startQuiz() {
      audio.playClick();
      currentIndex = 0;
      score = 0;
      correctCount = 0;
      streak = 0;
      bestStreak = 0;
      showScreen(screenQuiz);
      renderQuestion();
    }

    function renderQuestion() {
      answered = false;
      const q = QUESTIONS[currentIndex];
      setVisualMode(q.mode);

      const topicEl = document.getElementById('q-topic');
      topicEl.textContent = q.topic;
      topicEl.className = `topic-tag ${q.mode}`;

      document.getElementById('q-streak').textContent = `🔥 ${streak}`;
      document.getElementById('q-counter').textContent = `${currentIndex + 1} / ${QUESTIONS.length}`;
      document.getElementById('progress-fill').style.width = `${(currentIndex / QUESTIONS.length) * 100}%`;
      document.getElementById('q-text').textContent = q.question;

      const expBox = document.getElementById('explanation-box');
      expBox.classList.remove('visible');
      const nextBtn = document.getElementById('btn-next');
      nextBtn.classList.remove('visible');
      nextBtn.textContent = currentIndex === QUESTIONS.length - 1
        ? 'Lihat Hasil Ekspedisi ✨'
        : 'Lanjut ke Sektor Berikutnya →';

      const container = document.getElementById('options-container');
      container.innerHTML = '';

      const letters = ['A', 'B', 'C', 'D'];
      q.options.forEach((optText, idx) => {
        const btn = document.createElement('button');
        btn.className = 'option-btn';
        btn.style.animationDelay = `${idx * 0.07}s`;
        btn.innerHTML = `
          <span class="option-letter">${letters[idx]}</span>
          <span>${optText}</span>
        `;
        btn.addEventListener('click', () => handleAnswer(idx, btn));
        container.appendChild(btn);
      });
    }

    function handleAnswer(selectedIdx, selectedBtn) {
      if (answered) return;
      answered = true;

      const q = QUESTIONS[currentIndex];
      const allBtns = document.querySelectorAll('.option-btn');
      allBtns.forEach(b => (b.disabled = true));

      const isCorrect = selectedIdx === q.answer;
      const expBox = document.getElementById('explanation-box');
      const expTitle = document.getElementById('exp-title');
      const expText = document.getElementById('exp-text');

      if (isCorrect) {
        selectedBtn.classList.add('correct');
        correctCount++;
        streak++;
        if (streak > bestStreak) bestStreak = streak;
        score += 100 + streak * 20;
        audio.playCorrect();
        spawnBurst(q.mode === 'bh' ? '#ff8a3d' : '#38bdf8');
        expTitle.textContent = '✅ Tepat Sekali!';
        expBox.style.borderLeftColor = 'var(--correct)';
      } else {
        selectedBtn.classList.add('wrong');
        allBtns[q.answer].classList.add('correct');
        streak = 0;
        audio.playWrong();
        screenQuiz.classList.remove('shake');
        void screenQuiz.offsetWidth;
        screenQuiz.classList.add('shake');
        expTitle.textContent = '❌ Kurang Tepat — Fakta Kosmik:';
        expBox.style.borderLeftColor = 'var(--wrong)';
      }

      document.getElementById('q-streak').textContent = `🔥 ${streak}`;
      document.getElementById('progress-fill').style.width = `${((currentIndex + 1) / QUESTIONS.length) * 100}%`;
      expText.textContent = q.explanation;
      expBox.classList.add('visible');
      document.getElementById('btn-next').classList.add('visible');
    }

    function nextStep() {
      audio.playClick();
      currentIndex++;
      if (currentIndex < QUESTIONS.length) {
        renderQuestion();
      } else {
        showResults();
      }
    }

    function showResults() {
      showScreen(screenResult);
      const pct = Math.round((correctCount / QUESTIONS.length) * 100);
      document.getElementById('res-score').textContent = `${pct}%`;
      document.getElementById('res-correct').textContent = `${correctCount}/${QUESTIONS.length}`;
      document.getElementById('res-points').textContent = score;
      document.getElementById('res-best-streak').textContent = `${bestStreak}x`;

      const titleEl = document.getElementById('res-title');
      const descEl = document.getElementById('res-desc');

      if (pct >= 80) {
        titleEl.textContent = 'Astrofisikawan Ulung! 🌌';
        descEl.textContent = 'Pemahamanmu tentang gravitasi ekstrem, horizon peristiwa, dan bintang pulsar luar biasa tajam.';
        spawnBurst('#38bdf8');
      } else if (pct >= 50) {
        titleEl.textContent = 'Navigator Antarbintang 🚀';
        descEl.textContent = 'Hasil yang solid! Kamu sudah menguasai dasar-dasar objek paling ekstrem di alam semesta.';
      } else {
        titleEl.textContent = 'Kadet Ruang Angkasa 🛰️';
        descEl.textContent = 'Perjalanan kosmik baru dimulai. Ulangi misi untuk menaklukkan misteri lubang hitam dan bintang neutron!';
      }
    }

    document.getElementById('btn-start').addEventListener('click', startQuiz);
    document.getElementById('btn-next').addEventListener('click', nextStep);
    document.getElementById('btn-restart').addEventListener('click', startQuiz);
  </script>
</body>
</html>
