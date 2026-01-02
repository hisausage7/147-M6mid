<吃飽太閒做的網站，能幫到你我覺得很開心>
<html lang="zh-Hant">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>147測驗｜單一檔案版</title>

    <!-- 預先套用主題避免閃爍 -->
    <script>
      (function () {
        try {
          var saved = localStorage.getItem('theme');
          if (saved === 'dark' || (!saved && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
            document.documentElement.classList.add('preload-dark');
            document.documentElement.setAttribute('data-theme','dark');
          }
        } catch(e){}
      })();
    </script>

    <style>
      :root{
        --bg:#f0f4f8; --txt:#000; --card:#fff; --rule:#e9ecef; --muted:#666;
        --primary:#007bff; --primary-800:#0056b3; --danger:#dc3545;
        --green:#28a745; --green-800:#218838; --teal:#17a2b8; --teal-800:#138496;
        --table-bd:#ccc;

        /* 正解強調（淺色） */
        --ans-chip-bg:#e7f6ec; --ans-chip-fg:#136d3a;
        --ans-row-bg:#f5fbf7; --ans-row-bd:#cce8d5;
      }
      body.dark {
        --bg:#121212; --txt:#fff; --card:#1e1e1e; --rule:#333; --table-bd:#444; --muted:#aaa;

        /* 正解強調（深色） */
        --ans-chip-bg:#1f3a29; --ans-chip-fg:#b9f6ca;
        --ans-row-bg:#12301f; --ans-row-bd:#2c5a3f;
      }

      html, body { height: 100%; }
      body {
        font-family: Arial, sans-serif;
        margin: 0;
        padding: 40px;
        /* 基礎自適應字級（桌機普遍上限 24px） */
        font-size: clamp(16px, 1.1vw + 12px, 24px);
        background: var(--bg);
        color: var(--txt);
        transition: background-color .4s, color .4s;
      }

      #container {
        max-width: 1320px; margin: auto; background: var(--card); padding: 40px;
        border-radius: 20px; box-shadow: 0 0 10px rgba(0,0,0,.1);
        transition: background-color .4s, color .4s;
      }

      .hidden { display: none; }
      h1, h2 { text-align: center; font-size: 1.6em; margin: .2em 0 .6em; }
      #rules { background: var(--rule); padding: 16px; margin-bottom: 28px; border-radius: 10px; font-size: .95em; }

      .btn {
        background: var(--primary); color: #fff; border: none; padding: 14px 24px;
        margin: 8px; border-radius: 10px; cursor: pointer; font-size: .95em;
        transition: background-color .2s, transform .1s;
        white-space: nowrap;
      }
      .btn:hover { background: var(--primary-800); }
      #leaveBtn { background: var(--danger); }
      #openBankBtn { background: var(--teal); }
      #openBankBtn:hover { background: var(--teal-800); }

      /* 進度條 */
      #progress, #timer { font-weight: bold; font-size: 1em; }
      .progress-container { background: #ddd; height: 10px; border-radius: 5px; margin-top: 14px; overflow: hidden; }
      body.dark .progress-container { background: #555; }
      .progress-bar { height: 100%; width: 0; background: var(--primary); transition: width .6s ease; }

      .question { margin: 24px 0 12px; font-size: 1.2em; }
      .options label { display: block; margin-bottom: 12px; font-size: 1em; }

      /* 表格與響應式容器 */
      .table-responsive { width: 100%; overflow-x: auto; -webkit-overflow-scrolling: touch; }
      table { width: 100%; border-collapse: collapse; margin-top: 16px; font-size: 1em; min-width: 460px; }
      th, td {
        border: 1px solid var(--table-bd); padding: 12px 14px; text-align: left; vertical-align: top;
        color: var(--txt); word-break: break-word;
      }

      /* 錯題列色（深色模式改亮字） */
      tr.wrong { background-color: #ffe6e6; }
      body.dark tr.wrong { background-color: #5a1a1a; }
      body.dark tr.wrong td { color: #fff; }

      /* 正解強調（提高優先度，避免被錯題底色吃掉） */
      .ans-chip { display:inline-block; padding:2px 8px; border-radius:999px; background: var(--ans-chip-bg); color: var(--ans-chip-fg); font-size:.9em; margin-right:6px; }
      .ans-cell  { background: var(--ans-row-bg) !important; border-left: 3px solid var(--ans-row-bd) !important; color: var(--txt) !important; }
      .opt-row   { display:block; padding:4px 6px; margin:2px 0; border-radius:6px; }
      .opt-row.is-correct { background: var(--ans-row-bg) !important; border-left: 3px solid var(--ans-row-bd) !important; color: var(--txt) !important; }
      tr.wrong td.ans-cell { background: var(--ans-row-bg) !important; color: var(--txt) !important; }

      /* 右上角按鈕固定 */
      #darkModeToggle, #homeBtn {
        position: fixed; right: 20px; padding: 10px 16px; border: none; border-radius: 8px;
        cursor: pointer; font-size: .95em; z-index: 2000; box-shadow: 0 4px 12px rgba(0,0,0,.15);
      }
      #darkModeToggle { top: 20px; background: #333; color: #fff; }
      #darkModeToggle:hover { background: #555; }
      #homeBtn { top: 68px; background: var(--green); color: #fff; text-decoration: none; }
      #homeBtn:hover { background: var(--green-800); }

      .mono { font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Courier New", monospace; }
      .muted { opacity: .8; color: var(--muted); }

      /* 小尺寸優化 */
      @media (max-width: 768px) {
        body { padding: 20px; font-size: clamp(14px, 2.2vw + 8px, 17px); }
        #container { padding: 20px; border-radius: 16px; }
        .btn { padding: 12px 16px; margin: 6px; font-size: .95em; }
        #darkModeToggle, #homeBtn { right: 12px; }
        #darkModeToggle { top: 12px; }
        #homeBtn { top: 56px; }
      }
      @media (max-width: 540px) {
        /* 手機上把「您的答案」隱藏，保留題目/正解/OX */
        #results table th:nth-child(2), #results table td:nth-child(2) { display:none; }
      }
      @media (max-width: 420px) {
        #bank .controls { display: grid; grid-template-columns: 1fr; gap: 8px; }
      }

      /* —— 大螢幕加碼放大（到 4K 前） —— */
      @media (min-width: 1200px) {
        body       { font-size: clamp(18px, 0.9vw + 12px, 26px); }
        #container { max-width: 1440px; }
        h1, h2     { font-size: 2rem; }
      }
      @media (min-width: 1800px) {
        body       { font-size: clamp(20px, 0.7vw + 12px, 28px); }
        #container { max-width: 1680px; }
        h1, h2     { font-size: 2.2rem; }
      }
      @media (min-width: 2400px) {
        body       { font-size: clamp(22px, 0.6vw + 14px, 32px); }
        #container { max-width: 1920px; }
        h1, h2     { font-size: 2.4rem; }
      }

      /* —— 2560×1440（含以上）專用上限 —— */
      @media (min-width: 2560px) and (min-height: 1400px) {
        body       { font-size: clamp(22px, 0.55vw + 16px, 34px); } /* 上限 34px */
        #container { max-width: 2000px; }
        h1, h2     { font-size: 2.6rem; }
      }

      /* 預載深色（head 腳本使用），load 後會移除 */
      .preload-dark body, .preload-dark { background:#121212; color:#fff; }
    </style>
  </head>
  <body>
    <button id="darkModeToggle" aria-pressed="false">深色模式 / Dark Mode</button>
    <a id="homeBtn" href="https://hisausage7.github.io/147test/" title="回首頁">🏠 回首頁</a>

    <div id="container">
      <!-- 歡迎頁 -->
      <div id="welcome">
        <h1>147測驗M6-期中考</h1>
        <div id="rules">
          <p><strong>考試注意事項 / Exam Rules:</strong></p>
          <p>1. 請輸入姓名後才能開始作答。 / You must enter your name to start the quiz.</p>
          <!-- 這行改成可自訂 -->
          <p>2. 考試限時可自訂（預設 80 分鐘,可設定至999分鐘），開始後自動倒數。 / You can set the time limit (default 80 minutes); countdown starts on begin.</p>
          <p>3. 作答途中可隨時點擊「離開考試」提前結束。 / You can click "Leave Quiz" anytime to finish early.</p>
          <p>4. 完成後會自動顯示所有答題結果與成績。 / Results and scores will be displayed after completion.</p>
          <p>5. 答對題目顯示O，答錯題目顯示X。 / Correct answers will show O, incorrect answers will show X.</p>
          <p>6. 測驗過程為亂序出題。 / The test process was chaotic and disordered in setting questions.</p>
          <p>!版權及源代碼所有-航機系008沈崑宸!</p>
          <p>!僅作為自我測驗使用!</p>
         
        </div>
        <input type="text" id="nameInput" placeholder="輸入姓名 / Enter your name"
               style="width:100%;padding:8px;margin-bottom:10px;font-size:1em;" />
        <input type="number" id="questionLimit" placeholder="輸入題數,至多100題 / Enter number of questions"
               style="width:100%;padding:8px;margin-bottom:10px;font-size:1em;" />
        <!-- 新增：作答時間（分鐘），預設 80，限制 1~300 -->
        <input type="number" id="durationInput" value="80" min="1" max="300"
               placeholder="作答時間（分鐘，預設 80,可設定置999分鐘） / Duration in minutes"
               style="width:100%;padding:8px;margin-bottom:10px;font-size:1em;" />
        <div style="text-align:center; display:flex; flex-wrap:wrap; justify-content:center;">
          <button id="startBtn" class="btn">開始測驗 / Start Quiz</button>
          <button id="openBankBtn" class="btn">題庫瀏覽 / Browse Bank</button>
        </div>
      </div>

      <!-- 測驗頁 -->
      <div id="quiz" class="hidden">
        <div style="display:flex; align-items:center; gap:8px;">
          <span id="welcomeName"></span>
          <span id="timer" style="margin-left:auto">80:00</span>
        </div>
        <div class="mono" id="progress">題數: <span id="current">0</span> / <span id="total">0</span></div>
        <div class="progress-container"><div id="progressBar" class="progress-bar"></div></div>
        <div class="question" id="questionText"></div>
        <div class="options" id="options"></div>
        <div style="display:flex;gap:10px;flex-wrap:wrap">
          <button id="prevBtn" class="btn" style="background:#6c757d">上一題 / Previous</button>
          <button id="leaveBtn" class="btn">離開考試 / Leave Quiz</button>
        </div>
      </div>

      <!-- 結果頁 -->
      <div id="results" class="hidden">
        <h2>測驗結果 / Results</h2>
        <div style="display:flex; flex-wrap:wrap; justify-content:center;">
          <button id="retryBtn" class="btn">重新開始 / Retry</button>
          <button id="showWrongBtn" class="btn" style="background:#ffc107">只看錯題 / Wrong Only</button>
          <button id="showAllBtn" class="btn">看全部結果 / Show All</button>
          <button id="toBankFromResults" class="btn" style="background:#17a2b8">題庫瀏覽 / Browse Bank</button>
        </div>
        <div class="table-responsive">
          <table>
            <thead>
              <tr><th>題目</th><th>您的答案</th><th>正確答案</th><th>結果</th></tr>
            </thead>
            <tbody id="resultsBody"></tbody>
          </table>
        </div>
        <div id="scoreSummary" style="text-align:center;margin-top:12px;font-size:1em"></div>
      </div>

      <!-- 題庫瀏覽頁 -->
      <div id="bank" class="hidden">
        <h2>題庫瀏覽 / Question Bank</h2>
        <div class="controls">
          <input id="bankSearch" type="text" placeholder="關鍵字搜尋（會搜尋題目與選項）" />
          <label>每頁顯示
            <select id="perPage">
              <option value="5">5</option>
              <option value="10" selected>10</option>
              <option value="20">20</option>
              <option value="50">50</option>
              <option value="all">全部</option>
            </select>
            題
          </label>
          <button id="backToWelcome" class="btn" style="padding:12px 18px">返回首頁 / Home</button>
        </div>
        <div class="table-responsive">
          <table>
            <thead>
              <tr>
                <th style="width:70px">#</th>
                <th>題目</th>
                <th style="width:34%">選項</th>
                <th style="width:160px">正確答案</th>
              </tr>
            </thead>
            <tbody id="bankBody"></tbody>
          </table>
        </div>
        <div class="pagination" style="display:flex;justify-content:center;align-items:center;gap:10px;margin-top:10px;">
          <button id="prevPage" class="btn" style="padding:10px 16px">上一頁</button>
          <span id="pageInfo" class="mono"></span>
          <button id="nextPage" class="btn" style="padding:10px 16px">下一頁</button>
        </div>
        <div style="text-align:center;margin-top:8px">
          <small class="muted">提示：可用右上角「深色模式」切換外觀；搜尋支援中英文與數字。</small>
        </div>
      </div>
    </div>
    <script>
            document.addEventListener("DOMContentLoaded", function () {
              const questions = [
                // === PART 1 ===
      
         { question: "Pure copper can be hardened by:", options: ["A. heat treatment","B. cold working","C. case hardening"], answer: "B" },
  { question: "Precipitation treatment is given to some alloys in order to:", options: ["A. make them softer and more ductile","B. harden them","C. relieve internal locked up stresses"], answer: "B" },
  { question: "Non-ferrous metals are defined as metals that:", options: ["A. contain no iron","B. are non-magnetic","C. do not contain iron as a major constituent"], answer: "C" },
  { question: "Bronze is basically an alloy of:", options: ["A. copper and tin","B. copper and zinc","C. tin and zinc"], answer: "A" },
  { question: "Pure aluminium possesses:", options: ["A. poor ductility","B. high strength","C. high corrosion resistance"], answer: "C" },
  { question: "When copper and nickel are alloyed together, they form a:", options: ["A. solid solution that cannot be precipitation hardened","B. partially solid solution that can be precipitation hardened","C. solid solution that can be precipitation hardened"], answer: "A" },
  { question: "'Alclad' is the trade name for:", options: ["A. pure aluminium sheet, clad with 5% thickness of Duralumin on each side","B. Duralumin sheet, clad with 5% thickness of pure aluminium on each side","C. Duralumin sheet, clad with 2.5% thickness of zinc on each side"], answer: "B" },
  { question: "A small percentage of copper is alloyed with aluminium alloy to:", options: ["A. increase electrical conductivity","B. reduce brittleness","C. permit precipitation hardening"], answer: "C" },
  { question: "The main constituent in the refractory 'super-alloys', such as Nimonic 80A is:", options: ["A. nickel","B. chromium","C. tungsten"], answer: "A" },
  { question: "Following the solution treatment of Duralumin, refrigeration will:", options: ["A. delay age hardening","B. accelerate age hardening","C. have no affect on age hardening"], answer: "A" },
  { question: "The specification for an aluminium alloy is 2024 – 0. The suffix 0 means the material is:", options: ["A. hardened","B. annealed","C. solution treated"], answer: "B" },
  { question: "When you have fabricated a component from a heat treatable aluminium alloy, the final heat treatment given prior to releasing it to service is:", options: ["A. stress relief and annealing","B. precipitation treatment only","C. solution treatment and precipitation treatment"], answer: "C" },
  { question: "A light alloy that would be most suited for an aircraft skin that has a working temperature of 180°C is:", options: ["A. Duralumin","B. titanium alloy","C. aluminium - magnesium alloy"], answer: "B" },
  { question: "When caustic soda is swabbed onto the surface of Duralumin, the surface turns:", options: ["A. black","B. white","C. effervescent white"], answer: "A" },
  { question: "The material that is most suited for use in the manufacture of flexible bellows and metal diaphragms in fluid control system units is:", options: ["A. aluminium bronze","B. antimony bronze","C. beryllium bronze"], answer: "C" },
  { question: "The aluminium alloy 2024 – T6 is:", options: ["A. aluminium – copper alloy that has been solution treated and artificially aged","B. aluminium – magnesium alloy that has been strain hardened","C. aluminium – zinc alloy that has been annealed"], answer: "A" },
  { question: "Alloys that are described as 'refractories' will:", options: ["A. maintain their mechanical properties at very high temperatures","B. lose their mechanical properties at very high temperatures","C. maintain their mechanical properties at very low temperatures"], answer: "A" },
  { question: "The temperatures and times to be used for the heat treatment of metals are:", options: ["A. contained in heat treatment tables issued by the standards authority","B. contained in the material specification sheet","C. marked on the material as a temper code"], answer: "B" },
  { question: "Heat treatable, clad aluminium copper alloy can be solution treated:", options: ["A. any number of times","B. a maximum of once only","C. a maximum of three times only"], answer: "C" },
  { question: "Antimony may be added to solder to:", options: ["A. harden it","B. soften it","C. act as a flux"], answer: "A" },
  { question: "The letter that accompanies a Rockwell hardness value (HR) indicates the:", options: ["A. material tested","B. additional force used","C. scale used"], answer: "C" },
  { question: "Solder is an alloy of:", options: ["A. zinc and lead","B. tin and lead","C. copper and lead"], answer: "B" },
  { question: "Tungsten is most suited for use in:", options: ["A. high strength, lightweight alloys","B. high-speed steels","C. softening nickel alloys"], answer: "B" },
  { question: "When annealing a 2XXX series aluminium alloy, it should be cooled:", options: ["A. rapidly by quenching in cold water","B. in air or by quenching","C. at a slow controlled rate"], answer: "C" },
  { question: "Tungsten, silicon and chromium belong to a group of materials that are well known for their ability to form:", options: ["A. oxides","B. molecules","C. carbides"], answer: "C" },
  {
    question: "Ferrous metals are so-called because their main constituent is:",
    options: ["A. carbon", "B. iron", "C. cementite"],
    answer: "B"
  },
  {
    question: "A metal's resistance to fracture under the action of external bending or impact forces is called:",
    options: ["A. hardness", "B. strength", "C. toughness"],
    answer: "C"
  },
  {
    question: "The maximum percentage of carbon that can be dissolved in iron at room temperature is:",
    options: [
      "A. less than when the iron is above its upper critical temperature",
      "B. more than when it is above its upper critical temperature",
      "C. the same regardless of the temperature of the iron"
    ],
    answer: "A"
  },
  {
    question: "Steel is a term given to:",
    options: [
      "A. iron that contains a percentage of carbon in solution",
      "B. iron that contains one or more elements in addition to carbon",
      "C. a metal that is predominantly made of iron"
    ],
    answer: "A"
  },
  {
    question: "High carbon steel contains:",
    options: [
      "A. 0.2% to 0.5% carbon",
      "B. 0.7% to 1.5% carbon",
      "C. 2% to 5% carbon"
    ],
    answer: "B"
  },
  {
    question: "An alloy containing 75% nickel and 25% cobalt would be called:",
    options: [
      "A. nickel steel alloy",
      "B. high nickel alloy",
      "C. low cobalt steel alloy"
    ],
    answer: "B"
  },
  {
    question: "Stainless steels must contain a minimum chromium percentage of:",
    options: ["A. 3%", "B. 5%", "C. 12%"],
    answer: "C"
  },
  {
    question: "Chromium is added to steel to:",
    options: [
      "A. give a fine grain structure and reduce brittleness",
      "B. increase its work-hardening capacity",
      "C. increase hardness and corrosion resistance"
    ],
    answer: "C"
  },
  {
    question: "Temper brittleness is a feature of:",
    options: [
      "A. nickel steel",
      "B. austenitic stainless steel",
      "C. high carbon steel"
    ],
    answer: "B"
  },
  {
    question: "A property of 'Invar' steel is that it:",
    options: [
      "A. has an extremely low coefficient of expansion",
      "B. has very high permeability",
      "C. self-hardens under mechanical pressure"
    ],
    answer: "A"
  },
  {
    question: "A small percentage of lead is added to some steels to:",
    options: [
      "A. improve their machinability",
      "B. reduce their brittleness",
      "C. improve their wear resistance"
    ],
    answer: "A"
  },
  {
    question: "A suitable material for use in an aircraft landing gear structure would be:",
    options: [
      "A. high carbon steel",
      "B. tungsten, cobalt alloy steel",
      "C. chrome, molybdenum alloy steel"
    ],
    answer: "C"
  },
  {
    question: "A suitable alloy for use in an engine turbine wheel would be:",
    options: [
      "A. ferritic stainless steel",
      "B. martensitic stainless steel",
      "C. medium carbon steel"
    ],
    answer: "A"
  },
  {
    question: "Austenitic steel:",
    options: [
      "A. does not expand or contract",
      "B. does not respond to heat treatment",
      "C. is magnetic"
    ],
    answer: "B"
  },
  {
    question: "When carbon steel is being 'tempered' it is cooled:",
    options: [
      "A. slowly in the furnace",
      "B. freely in still air",
      "C. by quenching"
    ],
    answer: "C"
  },
  {
    question: "When nickel is added to steel its main effect is to increase:",
    options: [
      "A. toughness",
      "B. hardness",
      "C. corrosion resistance"
    ],
    answer: "A"
  },
  {
    question: "The heat treatment that softens steel and makes it more ductile is called:",
    options: [
      "A. tempering",
      "B. annealing",
      "C. normalising"
    ],
    answer: "B"
  },
  {
    question: "The heat treatment that relieves locked up stresses and refines grain structure is called:",
    options: [
      "A. tempering",
      "B. annealing",
      "C. normalising"
    ],
    answer: "C"
  },
  {
    question: "The 'fatigue limit' for a component is defined as:",
    options: [
      "A. the maximum load that can be endured for an infinite number of load cycles",
      "B. the load at which the component will instantly fail",
      "C. the minimum load that can be endured for an infinite number of load cycles"
    ],
    answer: "A"
  },
  {
    question: "Yield stress is the maximum stress that can be endured without:",
    options: [
      "A. fracture",
      "B. permanent distortion",
      "C. work hardening"
    ],
    answer: "B"
  },
  {
    question: "The hardness test that is most suited to very hard and very soft materials is the:",
    options: [
      "A. Brinell hardness test",
      "B. Vickers hardness test",
      "C. Barcol hardness test"
    ],
    answer: "B"
  },
  {
    question: "An impact test result for metal test pieces is a measure of the:",
    options: [
      "A. energy remaining after impacting and breaking the test piece",
      "B. energy at the start of the pendulum's swing",
      "C. energy absorbed by the test piece"
    ],
    answer: "C"
  },
  {
    question: "Which of these statements is false? (Remember? Read the question!)",
    options: [
      "A. brittle material cannot have high tensile strength",
      "B. brittle material cannot have high toughness",
      "C. brittle material cannot have high ductility"
    ],
    answer: "A"
  },
  {
    question: "Tungsten and chromium will harden carbon steel by:",
    options: [
      "A. dissolving in ferrite and creating a fine grain structure",
      "B. distorting the grain structure",
      "C. reacting with carbon and forming carbide"
    ],
    answer: "C"
  },
  {
    question: "The term 'high speed steel' refers to alloy steels that are used for:",
    options: [
      "A. high-speed aircraft",
      "B. high-speed cutting tools",
      "C. high rotational speed components"
    ],
    answer: "B"
  },
  {
    question: "A polymer that cannot be re-moulded by heat once it has cured is classed as a:",
    options: ["A. thermoplastic", "B. thermoset", "C. elastomer"],
    answer: "B"
  },
  {
    question: "A cold curing resin will:",
    options: [
      "A. produce heat as it cures",
      "B. not produce heat as it cures",
      "C. require an external source of heat to cure"
    ],
    answer: "A"
  },
  {
    question: "The catalyst used for curing polyester resins is:",
    options: ["A. styrene based", "B. peroxide based", "C. epoxy based"],
    answer: "B"
  },
  {
    question: "An accelerator is mixed with the:",
    options: [
      "A. catalyst first before mixing it into the resin",
      "B. resin before adding the catalyst",
      "C. resin after the catalyst has been mixed into it"
    ],
    answer: "B"
  },
  {
    question: "The two tests that are related to adhesive bonded joints are:",
    options: ["A. tensile and hardness", "B. impact and shear", "C. peel and shear"],
    answer: "C"
  },
  {
    question: "Compared to a large bulk of activated resin, the pot life of a smaller bulk will be:",
    options: ["A. longer", "B. shorter", "C. similar"],
    answer: "A"
  },
  {
    question: "A typical polyester resin/glass fibre ratio would be:",
    options: ["A. 2:1", "B. 1:1", "C. 1:2"],
    answer: "A"
  },
  {
    question: "A thermosetting polymer cures by:",
    options: [
      "A. forming long-chain molecules",
      "B. covalent cross-linking between molecules",
      "C. ionic bonding between molecules"
    ],
    answer: "B"
  },
  {
    question: "Increasing the temperature of a cold cure resin will:",
    options: [
      "A. decrease the pot life",
      "B. increase the pot life",
      "C. increase the strength of the bond"
    ],
    answer: "A"
  },
  {
    question: "Aluminium oxide is classed as a:",
    options: ["A. carbide", "B. silicate", "C. ceramic"],
    answer: "C"
  },
  {
    question: "When a crack travelling through a fibre reinforced plastics matrix meets a fibre it:",
    options: ["A. stops", "B. goes over it", "C. travels along it"],
    answer: "C"
  },
  {
    question: "If the thickness of a beam is trebled its stiffness increases by a factor of:",
    options: ["A. 3", "B. 6", "C. 9"],
    answer: "C"
  },
  {
    question: "Chopped strand matting is used to reinforce general purpose plastics because it:",
    options: [
      "A. gives more strength than woven fibres",
      "B. gives strength equally in all directions",
      "C. has short fibres that make it lighter"
    ],
    answer: "B"
  },
  {
    question: "A ‘cermet’ is the term given to describe:",
    options: ["A. ceramic reinforced metal", "B. compacted ceramic powder", "C. carbides"],
    answer: "A"
  },
  {
    question: "A ‘step’ curing process is carried out:",
    options: [
      "A. automatically by a programmed controller",
      "B. by installing the repair in a vacuum bag",
      "C. manually by an operator"
    ],
    answer: "C"
  },
  {
    question: "A satin weave would be stronger than a plain woven material because:",
    options: [
      "A. all the fibres lay in the warp direction",
      "B. there is less crimp",
      "C. the fibres are equally divided in two directions"
    ],
    answer: "B"
  },
  {
    question: "Ceramics materials are:",
    options: [
      "A. hard and brittle and have good electrical and thermal resistance",
      "B. hard and brittle and are good conductors of heat and electricity",
      "C. tough and strong and are good heat conductors but poor electrical conductors"
    ],
    answer: "A"
  },
  {
    question: "Sintering is a process that involves:",
    options: [
      "A. the bonding of molecules by cross-linking with catalysts",
      "B. breaking molecular bonds during polymerisation",
      "C. the cold fusion and heat treatment of powdered materials"
    ],
    answer: "C"
  },
  {
    question: "Dispersion hardening is a process where:",
    options: [
      "A. particles of ceramic materials lock the slip planes in cermets",
      "B. carbides are cemented to the tips of cutting tools",
      "C. micro-balloons are added to resins to harden them"
    ],
    answer: "A"
  },
  {
    question: "Damaged honeycomb core is removed from a panel by:",
    options: ["A. drilling", "B. routing", "C. mechanical sanding"],
    answer: "B"
  },
  {
    question: "When raising the temperature of a cold cure repair to hasten curing, the upper temperature limit is approximately:",
    options: ["A. 265°C", "B. 25°C", "C. 65°C"],
    answer: "C"
  },
  {
    question: "Plastics materials are most prone to deterioration from:",
    options: ["A. moisture", "B. ultra-violet radiation", "C. low ambient temperatures"],
    answer: "B"
  },
  {
    question: "A problem experienced when drilling aramids is that the:",
    options: [
      "A. ends of the fibres ‘fuzz’ the edges of the hole",
      "B. hole tends to be oversized",
      "C. drill quickly overheats and blunts"
    ],
    answer: "A"
  },
  {
    question: "When scarf cutting a composite laminate in preparation for a repair, the approximate ratio of repair depth to repair site diameter is between:",
    options: ["A. 5:1 and 10:1", "B. 10:1 and 20:1", "C. 30:1 and 50:1"],
    answer: "C"
  },
  {
    question: "The pressure applied to a vacuum bagged repair is primarily supplied by:",
    options: ["A. vacuum", "B. atmospheric pressure", "C. a calking plate"],
    answer: "B"
  },
  {
    question: "The type of wood most commonly used for aircraft structure is:",
    options: ["A. balsa", "B. yellow birch", "C. spruce"],
    answer: "C"
  },
  {
    question: "The maximum grain inclination for the grade-A timber that is most commonly used in aircraft structure is:",
    options: ["A. 1 in 20", "B. 1 in 15", "C. 1 in 5"],
    answer: "B"
  },
  {
    question: "The correct moisture content for aircraft timber is:",
    options: ["A. 15% plus or minus 2%", "B. 12% plus or minus 1%", "C. 60% plus or minus 5%"],
    answer: "A"
  },
  {
    question: "The factor presenting the greatest risk of deterioration in wooden aircraft is:",
    options: ["A. temperature change", "B. moisture penetration", "C. sunlight"],
    answer: "B"
  },
  {
    question: "The least percentage of wood fibres that should be seen adhered to the glue faces of a broken test sample that is classed as acceptable is:",
    options: ["A. 50%", "B. 75%", "C. 100%"],
    answer: "B"
  },
  {
    question: "The type of adhesive extensively used for wooden aircraft structural joints is:",
    options: ["A. organic glue", "B. casein glue", "C. synthetic resin"],
    answer: "C"
  },
  {
    question: "A compression shake is a:",
    options: [
      "A. split that runs along the grain of the wood",
      "B. split between two annular rings",
      "C. thin fracture line running across the grain"
    ],
    answer: "C"
  },
  {
    question: "As a general rule, the maximum permissible diameter for a knot in aviation grade timber is:",
    options: ["A. 0.25 inch", "B. 0.5 inch", "C. 1 inch"],
    answer: "A"
  },
  {
    question: "When checking the integrity of a glue line you should use a:",
    options: ["A. spring balance", "B. feeler gauge", "C. impact tester"],
    answer: "B"
  },
  {
    question: "Timber displays the greatest strength:",
    options: [
      "A. across the grain direction",
      "B. equally across and in the grain direction",
      "C. in the grain direction"
    ],
    answer: "C"
  },
  {
    question: "When converting timber, the cut that produces the least warp is called:",
    options: ["A. rift sawing", "B. tangential sawing", "C. longitudinal sawing"],
    answer: "A"
  },
  {
    question: "Oily brownish yellow patches appearing on the surface of timber are a sign of:",
    options: ["A. dote disease", "B. dry rot", "C. seasoning"],
    answer: "A"
  },
  {
    question: "White Ash is a:",
    options: ["A. brittle hardwood", "B. flexible hardwood", "C. weak softwood"],
    answer: "B"
  },
  {
    question: "Glued joints in wooden structures are normally designed to accept:",
    options: ["A. tensile loads", "B. bending loads", "C. shear loads"],
    answer: "C"
  },
  {
    question: "End grain glued joints are:",
    options: ["A. ineffective", "B. the strongest form of glued joint", "C. only used in laminated woods"],
    answer: "A"
  },
  {
    question: "The grain directions of the face plies in plywood are:",
    options: ["A. at ninety degrees to each other", "B. the same", "C. at forty-five degrees to each other"],
    answer: "B"
  },
  {
    question: "The face plies of plywood are manufactured from:",
    options: ["A. softwood", "B. wood composite", "C. hardwood"],
    answer: "C"
  },
  {
    question: "The process of 'seasoning' is carried out on timber to:",
    options: ["A. increase the moisture content", "B. reduce the moisture content", "C. remove all moisture"],
    answer: "B"
  },
  {
    question: "If the glue face on a failed joint displays only the imprint of wood grain the most likely cause of the failure is:",
    options: [
      "A. the glue had cured before pressure was applied to the joint",
      "B. the timber in the joint had failed",
      "C. the pot life of the glue had expired before it was applied"
    ],
    answer: "A"
  },
  {
    question: "Gusset plates are fitted either side of joints in wooden cross members to:",
    options: [
      "A. carry loads across end grain joints",
      "B. protect the glue lines from moisture penetration",
      "C. react tensile loads"
    ],
    answer: "A"
  },
  {
    question: "Powder resins are prepared by mixing them with:",
    options: ["A. methylated spirit", "B. catalyst", "C. water"],
    answer: "C"
  },
  {
    question: "When a glued joint is assembled the clamping pressure should be re-checked after:",
    options: ["A. 24 hours", "B. 10 minutes", "C. 1 hour"],
    answer: "B"
  },
  {
    question: "The 'Closed Assembly Time' in relation to a glued joint is the elapsed time between:",
    options: [
      "A. assembling the joint and the application of pressure",
      "B. application of adhesive and the assembly of the joint",
      "C. the application of pressure and the curing of the adhesive"
    ],
    answer: "A"
  },
  {
    question: "A 'close contact adhesive' is a:",
    options: [
      "A. gap filling adhesive suitable for joints that are not in close contact",
      "B. single spread adhesive that is applied to one joint face only",
      "C. non-gap-filling adhesive suitable for joints that are in close contact"
    ],
    answer: "C"
  },
  {
    question: "The rate of growth of timber suitable for aircraft use should not be less than:",
    options: [
      "A. six annual rings per inch (25mm)",
      "B. twelve annual rings per inch (25mm)",
      "C. six annual rings per centimetre"
    ],
    answer: "A"
  }


 
              ];

              function shuffle(array) {
          for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
          }
          return array;
        }
        function fmtTime(s) {
          const m = Math.floor(s / 60).toString().padStart(2, "0");
          const sec = (s % 60).toString().padStart(2, "0");
          return m + ":" + sec;
        }
        function showView(idToShow) {
          ["welcome","quiz","results","bank"].forEach(id => {
            const el = document.getElementById(id);
            if (!el) return;
            if (id === idToShow) el.classList.remove("hidden");
            else el.classList.add("hidden");
          });
          window.scrollTo({ top: 0, behavior: "smooth" });
        }

        /* ====== 深色模式：持久化 ====== */
        const darkBtn = document.getElementById("darkModeToggle");
        function applyInitialTheme() {
          try {
            const saved = localStorage.getItem('theme');
            const isDark = (saved === 'dark') || (!saved && window.matchMedia('(prefers-color-scheme: dark)').matches);
            document.body.classList.toggle('dark', isDark);
            darkBtn.setAttribute('aria-pressed', String(isDark));
          } catch(e) {}
          document.documentElement.classList.remove('preload-dark');
        }
        applyInitialTheme();
        darkBtn.addEventListener('click', function(){
          const willDark = !document.body.classList.contains('dark');
          document.body.classList.toggle('dark', willDark);
          darkBtn.setAttribute('aria-pressed', String(willDark));
          try { localStorage.setItem('theme', willDark ? 'dark' : 'light'); } catch(e){}
        });

        /* ====== 測驗狀態 ====== */
        let shuffledQuestions;
        let current = 0, total = 0, timer = 80 * 60, interval, answers = [];
        let quizActive = false;
        const progressBar = document.getElementById("progressBar");

        /* ====== 元件 ====== */
        const startBtn = document.getElementById("startBtn");
        const prevBtn = document.getElementById("prevBtn");
        const leaveBtn = document.getElementById("leaveBtn");
        const retryBtn = document.getElementById("retryBtn");
        const showWrongBtn = document.getElementById("showWrongBtn");
        const showAllBtn = document.getElementById("showAllBtn");
        const openBankBtn = document.getElementById("openBankBtn");
        const toBankFromResults = document.getElementById("toBankFromResults");

        /* ====== 題庫瀏覽 ====== */
        const bankBody = document.getElementById("bankBody");
        const bankSearch = document.getElementById("bankSearch");
        const perPageSel = document.getElementById("perPage");
        const pageInfo = document.getElementById("pageInfo");
        const prevPageBtn = document.getElementById("prevPage");
        const nextPageBtn = document.getElementById("nextPage");
        const backToWelcome = document.getElementById("backToWelcome");
        const pagination = document.querySelector('#bank .pagination');

        // 抓取自訂時間輸入框
        const durationInput = document.getElementById("durationInput");

        let bankFiltered = questions.slice();
        let page = 1;
        function totalPages() {
          const sel = perPageSel.value;
          if (sel === 'all') return 1;
          return Math.max(1, Math.ceil(bankFiltered.length / parseInt(sel || "10")));
        }

        /* ====== 測驗流程 ====== */
        startBtn.addEventListener("click", () => {
          const n = document.getElementById("nameInput").value.trim();
          const qLimit = parseInt(document.getElementById("questionLimit").value);

          if (!n) return alert("請輸入姓名 / Enter your name");
          if (!qLimit || qLimit <= 0) return alert("請輸入要作答的題數,最多5題 / Enter number of questions");

          // 讀取自訂時間（分鐘）；預設 80，限制 1~300 分鐘
          let minutes = parseInt((durationInput && durationInput.value) ? durationInput.value : "80");
          if (isNaN(minutes)) minutes = 80;
          minutes = Math.max(1, Math.min(999, minutes));

          shuffledQuestions = shuffle(questions.slice()).slice(0, Math.min(qLimit, questions.length));
          total = shuffledQuestions.length;
          current = 0; answers = [];
          timer = minutes * 60;            // 用自訂分鐘
          quizActive = true;

          document.getElementById("welcomeName").innerText = "歡迎: " + n;
          document.getElementById("total").innerText = total;
          // 進場前先把右上角時計顯示成正確的起始值
          document.getElementById("timer").innerText = fmtTime(timer);
          updateProgress();

          // 確保不會重複計時
          clearInterval(interval);
          interval = setInterval(() => {
            if (timer > 0 && quizActive) {
              timer--;
              document.getElementById("timer").innerText = fmtTime(timer);
            } else if (quizActive && timer <= 0) {
              clearInterval(interval);
              finish();
            }
          }, 1000);

          showQ();
          showView("quiz");
        });

        prevBtn.addEventListener("click", () => {
          if (current > 0) {
            current--;
            answers.pop();
            showQ();
          }
        });

        leaveBtn.addEventListener("click", finish);
        retryBtn.addEventListener("click", () => location.reload());
        showWrongBtn.addEventListener("click", () => renderResults(answers.filter(a => !a.correct)));
        showAllBtn.addEventListener("click", () => renderResults(answers));
        openBankBtn.addEventListener("click", enterBank);
        toBankFromResults.addEventListener("click", enterBank);

        /* ====== 題庫事件 ====== */
        bankSearch.addEventListener("input", applyFilter);
        perPageSel.addEventListener("change", () => { page = 1; renderBank(); });
        prevPageBtn.addEventListener("click", () => { if (page > 1) { page--; renderBank(); } });
        nextPageBtn.addEventListener("click", () => { if (page < totalPages()) { page++; renderBank(); } });
        backToWelcome.addEventListener("click", () => showView("welcome"));

        function enterBank() {
          if (quizActive) { alert("測驗進行中不可瀏覽題庫。請先完成或離開考試。"); return; }
          if (!bankSearch.value) { bankFiltered = questions.slice(); page = 1; }
          renderBank();
          showView("bank");
        }

        function applyFilter() {
          const kw = bankSearch.value.trim().toLowerCase();
          bankFiltered = kw
            ? questions.filter(q => q.question.toLowerCase().includes(kw) ||
                                    q.options.some(o => o.toLowerCase().includes(kw)))
            : questions.slice();
          page = 1; renderBank();
        }

        function renderBank() {
          bankBody.innerHTML = "";
          const sel = perPageSel.value;
          const per = (sel === 'all') ? bankFiltered.length : parseInt(sel || "10");
          const start = (page - 1) * per;
          const items = (sel === 'all') ? bankFiltered.slice() : bankFiltered.slice(start, start + per);

          items.forEach((q, idx) => {
            const tr = document.createElement("tr");
            const num = (sel === 'all') ? (idx + 1) : (start + idx + 1);
            const correctText = q.options.find(o => o.charAt(0) === q.answer) || "";

            const optsHtml = q.options.map(o => {
              const isC = (o.charAt(0) === q.answer);
              return `<span class="opt-row ${isC ? 'is-correct' : ''}">${isC ? '<span class="ans-chip">正解</span>' : ''}${o}</span>`;
            }).join("");

            tr.innerHTML =
              '<td class="mono">#' + num + "</td>" +
              "<td>" + q.question + "</td>" +
              "<td>" + optsHtml + "</td>" +
              '<td class="mono ans-cell">' + (q.answer + "｜" + correctText) + "</td>";
            bankBody.append(tr);
          });

          const tp = totalPages();
          if (sel === 'all') {
            if (pagination) pagination.style.display = 'none';
            pageInfo.textContent = "全部顯示（共 " + bankFiltered.length + " 題）";
            prevPageBtn.disabled = true; nextPageBtn.disabled = true;
          } else {
            if (pagination) pagination.style.display = 'flex';
            pageInfo.textContent = "第 " + page + " / " + tp + " 頁（共 " + bankFiltered.length + " 題）";
            prevPageBtn.disabled = (page <= 1); nextPageBtn.disabled = (page >= tp);
          }
        }

        /* ====== 測驗：出題/進度/結束 ====== */
        function showQ() {
          if (current >= total) return finish();
          document.getElementById("current").innerText = current + 1;
          updateProgress();

          const q = shuffledQuestions[current];
          document.getElementById("questionText").innerText = q.question;

          const optDiv = document.getElementById("options");
          optDiv.innerHTML = "";

          let optionsWithFlag = q.options.map(option => ({ text: option, isAnswer: option.charAt(0) === q.answer }));
          optionsWithFlag = shuffle(optionsWithFlag);

          optionsWithFlag.forEach(opt => {
            const lbl = document.createElement("label");
            const rd = document.createElement("input");
            rd.type = "radio"; rd.name = "opt"; rd.value = opt.text;
            rd.onchange = function () {
              answers.push({
                q, selectedText: opt.text,
                correctText: q.options.find(optItem => optItem.charAt(0) === q.answer),
                correct: opt.isAnswer
              });
              current++; showQ();
            };
            lbl.append(rd, " ", opt.text);
            optDiv.append(lbl);
          });
        }

        function updateProgress() {
          const percent = (current / total) * 100;
          progressBar.style.width = percent + "%";
        }

        function finish() {
          clearInterval(interval);
          quizActive = false; // 結束後可進入題庫
          showView("results");
          renderResults(answers);
          document.getElementById("scoreSummary").innerText =
            "答對 " + answers.filter(a => a.correct).length +
            " 題 / 已作答 " + answers.length + " 題 / 共 " + total + " 題";
        }

        function renderResults(data) {
          const tb = document.getElementById("resultsBody");
          tb.innerHTML = "";
          data.forEach(a => {
            const tr = document.createElement("tr");
            tr.innerHTML =
              "<td>" + a.q.question + "</td>" +
              "<td>" + a.selectedText + "</td>" +
              '<td class="ans-cell">' + a.correctText + "</td>" +
              "<td>" + (a.correct ? "O" : "X") + "</td>";
            if (!a.correct) tr.classList.add("wrong");
            tb.append(tr);
          });
        }
      });
    </script>
  </body>
</html>

</html>
