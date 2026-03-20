<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>29Q</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Nunito:wght@400;700;900&family=M+PLUS+Rounded+1c:wght@400;700;900&display=swap');

  :root {
    --sky: #6EC6F0;
    --sun: #FFD93D;
    --grass: #6BCB77;
    --ground: #4D9B57;
    --cloud: #FFFFFF;
    --pink: #FF6B9D;
    --purple: #9B5DE5;
    --orange: #FF9F43;
    --red: #FF6B6B;
    --teal: #00C9A7;
    --navy: #1A1A4E;
    --card-bg: rgba(255,255,255,0.95);
    --shadow: 0 8px 32px rgba(26,26,78,0.18);
    --radius: 20px;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'M PLUS Rounded 1c', 'Nunito', sans-serif;
    background: linear-gradient(180deg, #a8d8f0 0%, #d6eeff 60%, #c8f0c0 100%);
    min-height: 100vh;
    overflow-x: hidden;
    position: relative;
  }



  #app {
    position: relative;
    z-index: 1;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px 16px 40px;
  }

  .screen { display: none; width: 100%; max-width: 640px; }
  .screen.active { display: flex; flex-direction: column; align-items: center; }

  /* Title screen */
  .title-logo {
    margin-top: 30px;
    text-align: center;
  }
  .title-logo h1 {
    font-size: clamp(2.2rem, 8vw, 3.5rem);
    font-weight: 900;
    color: var(--navy);
    text-shadow: 4px 4px 0 var(--sun), 6px 6px 0 rgba(0,0,0,0.1);
    letter-spacing: 2px;
  }
  .title-logo p {
    font-size: 1rem;
    color: var(--navy);
    opacity: 0.7;
    margin-top: 4px;
  }

  .mascot-wrap {
    margin: 18px 0 10px;
  }

  .card {
    background: var(--card-bg);
    border-radius: var(--radius);
    box-shadow: var(--shadow);
    padding: 24px 20px;
    width: 100%;
    margin-bottom: 16px;
  }

  .section-title {
    font-size: 1rem;
    font-weight: 700;
    color: #888;
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-bottom: 12px;
  }

  .btn-grid {
    display: grid;
    gap: 10px;
  }
  .btn-grid.cols-2 { grid-template-columns: 1fr 1fr; }
  .btn-grid.cols-3 { grid-template-columns: 1fr 1fr 1fr; }

  .select-btn {
    padding: 14px 8px;
    border: 3px solid transparent;
    border-radius: 14px;
    font-family: inherit;
    font-size: 1rem;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.15s;
    background: #f0f4ff;
    color: var(--navy);
    text-align: center;
  }
  .select-btn:hover { transform: translateY(-2px); box-shadow: 0 4px 16px rgba(0,0,0,0.1); }
  .select-btn.selected {
    border-color: var(--purple);
    background: var(--purple);
    color: white;
    transform: scale(1.04);
  }
  .select-btn .btn-icon { font-size: 1.5rem; display: block; margin-bottom: 4px; }

  .difficulty-soft.selected { background: var(--teal); border-color: var(--teal); }
  .difficulty-normal.selected { background: var(--orange); border-color: var(--orange); }
  .difficulty-hard.selected { background: var(--red); border-color: var(--red); }

  .start-btn {
    width: 100%;
    padding: 18px;
    border: none;
    border-radius: 16px;
    font-family: inherit;
    font-size: 1.3rem;
    font-weight: 900;
    cursor: pointer;
    background: linear-gradient(135deg, var(--purple), var(--pink));
    color: white;
    box-shadow: 0 6px 20px rgba(155,93,229,0.4);
    transition: all 0.2s;
    letter-spacing: 1px;
  }
  .start-btn:hover { transform: translateY(-3px); box-shadow: 0 10px 28px rgba(155,93,229,0.5); }
  .start-btn:active { transform: translateY(0); }

  /* Game screen */
  #gameScreen { padding-top: 10px; }

  .game-header {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 12px;
    flex-wrap: wrap;
    gap: 8px;
  }

  .timer-wrap {
    display: flex;
    align-items: center;
    gap: 8px;
    background: white;
    border-radius: 50px;
    padding: 8px 16px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  }
  .timer-icon { font-size: 1.3rem; }
  #timerDisplay {
    font-size: 1.3rem;
    font-weight: 900;
    color: var(--navy);
    min-width: 40px;
  }
  .timer-bar-wrap {
    width: 80px;
    height: 10px;
    background: #eee;
    border-radius: 10px;
    overflow: hidden;
  }
  #timerBar {
    height: 100%;
    background: linear-gradient(90deg, var(--teal), var(--grass));
    border-radius: 10px;
    transition: width 0.2s, background 0.5s;
  }

  .score-wrap {
    background: white;
    border-radius: 50px;
    padding: 8px 16px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    font-size: 1rem;
    font-weight: 700;
    color: var(--navy);
  }

  /* Gauge */
  .gauge-wrap {
    width: 100%;
    background: white;
    border-radius: var(--radius);
    padding: 10px 16px;
    box-shadow: var(--shadow);
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .gauge-label { font-size: 0.85rem; font-weight: 700; color: #aaa; white-space: nowrap; }
  .gauge-outer {
    flex: 1;
    height: 18px;
    background: #eee;
    border-radius: 20px;
    overflow: hidden;
  }
  #gauge {
    height: 100%;
    width: 0%;
    background: linear-gradient(90deg, var(--sun), var(--orange), var(--pink));
    border-radius: 20px;
    transition: width 0.4s cubic-bezier(.34,1.56,.64,1);
  }
  .gauge-count { font-size: 0.9rem; font-weight: 700; color: var(--navy); white-space: nowrap; }

  /* Question card */
  .question-card {
    width: 100%;
    background: var(--card-bg);
    border-radius: var(--radius);
    box-shadow: var(--shadow);
    padding: 20px;
    margin-bottom: 14px;
    text-align: center;
  }
  .question-label {
    font-size: 0.8rem;
    color: #bbb;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-bottom: 6px;
  }
  #questionText {
    font-size: clamp(1.8rem, 6vw, 2.8rem);
    font-weight: 900;
    color: var(--navy);
    letter-spacing: 2px;
    min-height: 60px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  /* English input answer display */
  .answer-display {
    margin-top: 10px;
    display: flex;
    gap: 6px;
    justify-content: center;
    flex-wrap: wrap;
  }
  .answer-char {
    width: 36px; height: 36px;
    border-bottom: 3px solid var(--purple);
    display: flex; align-items: center; justify-content: center;
    font-size: 1.3rem; font-weight: 700; color: var(--purple);
  }

  /* 3x3 grid */
  .grid-wrap {
    width: 100%;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    margin-bottom: 14px;
  }
  .grid-cell {
    aspect-ratio: 1;
    background: white;
    border: 3px solid #e0e0f0;
    border-radius: 14px;
    font-size: clamp(1.2rem, 4.5vw, 1.9rem);
    font-weight: 900;
    color: var(--navy);
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.15s;
    box-shadow: 0 3px 8px rgba(0,0,0,0.07);
    user-select: none;
    -webkit-tap-highlight-color: transparent;
    position: relative;
    overflow: hidden;
  }
  .grid-cell:hover { background: #f5f0ff; border-color: var(--purple); transform: scale(1.05); }
  .grid-cell.correct { background: var(--grass); border-color: var(--ground); color: white; animation: popCell 0.3s; }
  .grid-cell.wrong { background: var(--red); border-color: #d44; color: white; animation: shakeCell 0.3s; }
  .grid-cell.selected-word { background: var(--purple); border-color: #7040bb; color: white; }
  @keyframes popCell { 0%{transform:scale(1)} 50%{transform:scale(1.18)} 100%{transform:scale(1)} }
  @keyframes shakeCell { 0%,100%{transform:translateX(0)} 25%{transform:translateX(-6px)} 75%{transform:translateX(6px)} }

  /* Input mode */
  .input-wrap {
    width: 100%;
    display: flex;
    gap: 10px;
    margin-bottom: 14px;
  }
  #inputAnswer {
    flex: 1;
    padding: 14px 18px;
    border: 3px solid #e0e0f0;
    border-radius: 14px;
    font-family: inherit;
    font-size: 1.3rem;
    font-weight: 700;
    color: var(--navy);
    outline: none;
    transition: border 0.2s;
  }
  #inputAnswer:focus { border-color: var(--purple); }
  .submit-btn {
    padding: 14px 22px;
    border: none;
    border-radius: 14px;
    background: var(--purple);
    color: white;
    font-family: inherit;
    font-size: 1.1rem;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.15s;
  }
  .submit-btn:hover { background: #7b4bc5; transform: scale(1.04); }

  /* Feedback */
  .feedback {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%) scale(0.5);
    font-size: 5rem;
    z-index: 100;
    pointer-events: none;
    opacity: 0;
    transition: all 0.3s;
  }
  .feedback.show { transform: translate(-50%,-70%) scale(1); opacity: 1; }
  .feedback.hide { transform: translate(-50%,-90%) scale(1.4); opacity: 0; }

  /* Result screen */
  #resultScreen {
    text-align: center;
  }
  .result-title {
    font-size: clamp(2rem, 8vw, 3rem);
    font-weight: 900;
    margin-bottom: 8px;
  }
  .result-success { color: var(--grass); text-shadow: 3px 3px 0 var(--ground); }
  .result-fail { color: var(--red); text-shadow: 3px 3px 0 #a33; }

  .result-card {
    background: white;
    border-radius: var(--radius);
    padding: 20px;
    width: 100%;
    margin: 12px 0;
    box-shadow: var(--shadow);
  }
  .result-stat {
    display: flex;
    justify-content: space-between;
    padding: 8px 0;
    border-bottom: 1px solid #f0f0f0;
    font-size: 1rem;
    color: var(--navy);
  }
  .result-stat:last-child { border-bottom: none; }
  .result-stat .val { font-weight: 900; color: var(--purple); }

  /* Reward character */
  .reward-wrap {
    margin: 10px 0;
    animation: popIn 0.5s cubic-bezier(.34,1.56,.64,1);
  }
  @keyframes popIn { from{transform:scale(0) rotate(-10deg)} to{transform:scale(1) rotate(0)} }

  /* Collection */
  #collectionScreen { text-align: center; }
  .collection-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
    width: 100%;
    margin: 12px 0;
  }
  .char-slot {
    background: white;
    border-radius: 14px;
    aspect-ratio: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    box-shadow: 0 3px 10px rgba(0,0,0,0.08);
    font-size: 0.65rem;
    color: #888;
    padding: 6px;
    gap: 4px;
    transition: transform 0.2s;
    cursor: default;
  }
  .char-slot.unlocked { cursor: pointer; }
  .char-slot.unlocked:hover { transform: scale(1.08); }
  .char-slot.locked { filter: grayscale(1); opacity: 0.35; }
  .char-slot svg { width: 60%; height: 60%; }

  .nav-wrap {
    display: flex;
    gap: 10px;
    width: 100%;
    margin-top: 8px;
  }
  .nav-btn {
    flex: 1;
    padding: 14px;
    border: none;
    border-radius: 14px;
    font-family: inherit;
    font-size: 1rem;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.15s;
  }
  .nav-btn.primary { background: var(--purple); color: white; }
  .nav-btn.secondary { background: white; color: var(--navy); box-shadow: 0 3px 10px rgba(0,0,0,0.1); }
  .nav-btn:hover { transform: translateY(-2px); }

  /* Stars */
  .stars { color: var(--sun); font-size: 2rem; letter-spacing: 4px; }

  /* Particle */
  .particle {
    position: fixed;
    pointer-events: none;
    z-index: 200;
    font-size: 1.5rem;
    animation: particleFly 1s ease-out forwards;
  }
  @keyframes particleFly {
    0%  { opacity:1; transform: translate(0,0) scale(1); }
    100%{ opacity:0; transform: translate(var(--dx), var(--dy)) scale(0.3); }
  }

  .word-progress {
    font-size: 0.85rem;
    color: #999;
    margin-top: 6px;
    font-weight: 700;
  }
  .hint-text {
    font-size: 0.85rem;
    color: var(--purple);
    font-weight: 700;
    margin-top: 4px;
  }
</style>
</head>
<body>


<div id="app">

  <!-- ===== TITLE SCREEN ===== -->
  <div id="titleScreen" class="screen active">
    <div class="title-logo">
      <h1>29Q</h1>
      <p>たのしく まなぼう！</p>
    </div>

    <div class="mascot-wrap">
      <!-- Mascot: friendly star character -->
      <svg width="120" height="120" viewBox="0 0 120 120" xmlns="http://www.w3.org/2000/svg">
        <!-- Body -->
        <polygon points="60,10 72,42 106,42 79,62 90,95 60,75 30,95 41,62 14,42 48,42" fill="#FFD93D" stroke="#E8A800" stroke-width="3"/>
        <!-- Face -->
        <circle cx="52" cy="52" r="5" fill="#1A1A4E"/>
        <circle cx="68" cy="52" r="5" fill="#1A1A4E"/>
        <path d="M50,64 Q60,74 70,64" stroke="#1A1A4E" stroke-width="3" fill="none" stroke-linecap="round"/>
        <!-- Cheeks -->
        <circle cx="44" cy="60" r="6" fill="#FF9F43" opacity="0.5"/>
        <circle cx="76" cy="60" r="6" fill="#FF9F43" opacity="0.5"/>
        <!-- Shine -->
        <circle cx="49" cy="49" r="2" fill="white" opacity="0.8"/>
        <circle cx="65" cy="49" r="2" fill="white" opacity="0.8"/>
      </svg>
    </div>

    <!-- Subject -->
    <div class="card">
      <div class="section-title">📖 かもく</div>
      <div class="btn-grid cols-2">
        <button class="select-btn" id="btnMath" onclick="selectSubject('math')">
          <span class="btn-icon">🔢</span>さんすう
        </button>
        <button class="select-btn" id="btnEng" onclick="selectSubject('eng')">
          <span class="btn-icon">🔤</span>えいご
        </button>
      </div>
    </div>

    <!-- Eng sub-type -->
    <div class="card" id="engTypeCard" style="display:none">
      <div class="section-title">✏️ えいご もんだい</div>
      <div class="btn-grid cols-3">
        <button class="select-btn" id="btnUpper" onclick="selectEngType('upper')">
          <span class="btn-icon">🅰️</span>大文字
        </button>
        <button class="select-btn" id="btnLower" onclick="selectEngType('lower')">
          <span class="btn-icon">🔡</span>小文字
        </button>
        <button class="select-btn" id="btnWord" onclick="selectEngType('word')">
          <span class="btn-icon">📝</span>たんご
        </button>
      </div>
    </div>

    <!-- Difficulty -->
    <div class="card">
      <div class="section-title">⚡ むずかしさ</div>
      <div class="btn-grid cols-3">
        <button class="select-btn difficulty-soft" id="btnSoft" onclick="selectDiff('soft')">
          <span class="btn-icon">🌱</span>ソフト
        </button>
        <button class="select-btn difficulty-normal" id="btnNormal" onclick="selectDiff('normal')">
          <span class="btn-icon">🐱</span>ノーマル
        </button>
        <button class="select-btn difficulty-hard" id="btnHard" onclick="selectDiff('hard')">
          <span class="btn-icon">🦁</span>ハード
        </button>
      </div>
    </div>

    <!-- Mode -->
    <div class="card">
      <div class="section-title">🎮 もーど</div>
      <div class="btn-grid cols-2">
        <button class="select-btn" id="btnSelect" onclick="selectMode('select')">
          <span class="btn-icon">👆</span>えらぶ
        </button>
        <button class="select-btn" id="btnInput" onclick="selectMode('input')">
          <span class="btn-icon">⌨️</span>にゅうりょく
        </button>
      </div>
    </div>

    <button class="start-btn" onclick="startGame()">✨ スタート！</button>

    <button class="nav-btn secondary" style="margin-top:10px;" onclick="showScreen('collectionScreen')">
      🏆 コレクション
    </button>
  </div>

  <!-- ===== GAME SCREEN ===== -->
  <div id="gameScreen" class="screen">
    <div class="game-header">
      <div class="timer-wrap">
        <span class="timer-icon">⏱️</span>
        <span id="timerDisplay">15</span>
        <div class="timer-bar-wrap"><div id="timerBar" style="width:100%"></div></div>
      </div>
      <div class="score-wrap">❓ <span id="questionNum">1</span>/10</div>
    </div>

    <div class="gauge-wrap">
      <span class="gauge-label">🌟</span>
      <div class="gauge-outer"><div id="gauge"></div></div>
      <span class="gauge-count" id="gaugeCount">0/10</span>
    </div>

    <div class="question-card">
      <div class="question-label" id="questionLabel">もんだい</div>
      <div id="questionText">?</div>
      <div class="word-progress" id="wordProgress" style="display:none"></div>
      <div class="hint-text" id="hintText"></div>
    </div>

    <!-- Grid (select mode) -->
    <div class="grid-wrap" id="gridWrap">
      <div class="grid-cell" id="cell0" onclick="cellClick(0)"></div>
      <div class="grid-cell" id="cell1" onclick="cellClick(1)"></div>
      <div class="grid-cell" id="cell2" onclick="cellClick(2)"></div>
      <div class="grid-cell" id="cell3" onclick="cellClick(3)"></div>
      <div class="grid-cell" id="cell4" onclick="cellClick(4)"></div>
      <div class="grid-cell" id="cell5" onclick="cellClick(5)"></div>
      <div class="grid-cell" id="cell6" onclick="cellClick(6)"></div>
      <div class="grid-cell" id="cell7" onclick="cellClick(7)"></div>
      <div class="grid-cell" id="cell8" onclick="cellClick(8)"></div>
    </div>

    <!-- Input mode -->
    <div class="input-wrap" id="inputWrap" style="display:none">
      <input id="inputAnswer" type="text" placeholder="こたえをにゅうりょく" autocomplete="off" autocorrect="off" autocapitalize="off" spellcheck="false"/>
      <button class="submit-btn" onclick="submitInput()">→</button>
    </div>

    <button class="nav-btn secondary" onclick="endGame(false)" style="margin-top:4px;font-size:0.9rem;">⏹ やめる</button>
  </div>

  <!-- ===== RESULT SCREEN ===== -->
  <div id="resultScreen" class="screen">
    <div class="result-title" id="resultTitle">せいかい！</div>
    <div class="stars" id="resultStars">⭐⭐⭐</div>

    <div class="reward-wrap" id="rewardWrap" style="display:none">
      <p style="font-size:0.9rem;color:#888;margin-bottom:8px;">🎉 キャラゲット！</p>
    </div>

    <div class="result-card">
      <div class="result-stat"><span>せいかい</span><span class="val" id="rCorrect">0/10</span></div>
      <div class="result-stat"><span>せいかいりつ</span><span class="val" id="rRate">0%</span></div>
      <div class="result-stat"><span>かもく</span><span class="val" id="rSubject">-</span></div>
      <div class="result-stat"><span>むずかしさ</span><span class="val" id="rDiff">-</span></div>
    </div>

    <div class="nav-wrap">
      <button class="nav-btn primary" onclick="startGame()">🔄 もういちど</button>
      <button class="nav-btn secondary" onclick="showScreen('titleScreen')">🏠 もどる</button>
    </div>
    <button class="nav-btn secondary" style="margin-top:10px;" onclick="showScreen('collectionScreen')">🏆 コレクション</button>
  </div>

  <!-- ===== COLLECTION SCREEN ===== -->
  <div id="collectionScreen" class="screen">
    <div style="text-align:center;margin:10px 0 16px;">
      <h2 style="font-size:1.6rem;font-weight:900;color:var(--navy);">🏆 コレクション</h2>
      <p style="font-size:0.85rem;color:#999;">ゲームにかって キャラをあつめよう！</p>
    </div>
    <div class="collection-grid" id="collectionGrid"></div>
    <button class="nav-btn secondary" onclick="showScreen('titleScreen')">🏠 もどる</button>
  </div>

</div>

<!-- Feedback emoji -->
<div class="feedback" id="feedback"></div>

<script>
// ============================================================
//  DATA
// ============================================================
const WORDS = {
  soft:   ['cat','dog','red','run','sun','hat','cup','big','ant','map','bus','egg','fox','ink','jam','key','leg','net','owl','pen'],
  normal: ['apple','bread','chair','cloud','dance','earth','fence','green','happy','juice','knife','lemon','music','night','ocean','paint','queen','river','smile','tiger'],
  hard:   ['animal','banana','castle','desert','engine','flower','garden','helmet','island','jungle','kettle','locket','monkey','number','orange','palace','rabbit','school','temple','umbrella']
};

const UPPERCASE_CHARS = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('');
const LOWERCASE_CHARS = 'abcdefghijklmnopqrstuvwxyz'.split('');

// Math question generator
function genMath(diff) {
  const ops = {
    soft:   ['+','-'],
    normal: ['+','-','×'],
    hard:   ['+','-','×','÷']
  }[diff];
  const op = ops[Math.floor(Math.random()*ops.length)];
  let a,b,ans;
  if(op==='+'){
    const max = diff==='soft'?20:diff==='normal'?100:999;
    a = rnd(1,max); b = rnd(1,max); ans = a+b;
  } else if(op==='-'){
    const max = diff==='soft'?20:diff==='normal'?100:999;
    a = rnd(2,max); b = rnd(1,a); ans = a-b;
  } else if(op==='×'){
    const max = diff==='normal'?9:12;
    a = rnd(1,max); b = rnd(1,max); ans = a*b;
  } else { // ÷
    b = rnd(1,12); ans = rnd(1,12); a = b*ans;
  }
  return { text: `${a} ${op} ${b} ＝ ？`, answer: String(ans) };
}

function rnd(min,max){ return Math.floor(Math.random()*(max-min+1))+min; }

function shuffle(arr){ return [...arr].sort(()=>Math.random()-0.5); }

// ============================================================
//  STATE
// ============================================================
let state = {
  subject: null, engType: null, difficulty: null, mode: null,
  questionIdx: 0, correct: 0, totalChars: 0, startTime: null,
  currentQ: null, wordLetterIdx: 0, wordLetterOrder: [],
  timer: null, timeLeft: 0, totalTime: 0,
  unlocked: JSON.parse(localStorage.getItem('unlocked')||'[]')
};

// ============================================================
//  CHARACTERS (SVG)
// ============================================================
const CHARACTERS = [
  { id:'star', name:'スター', color:'#FFD93D', svg: starSVG },
  { id:'bear', name:'クマ',   color:'#c9a06a', svg: bearSVG },
  { id:'cat',  name:'ネコ',   color:'#A0C4FF', svg: catSVG  },
  { id:'bunny',name:'ウサギ', color:'#FFB3C6', svg: bunnySVG},
  { id:'frog', name:'カエル', color:'#6BCB77', svg: frogSVG },
  { id:'robot',name:'ロボット',color:'#9B5DE5',svg: robotSVG},
  { id:'sun',  name:'たいよう',color:'#FFD93D', svg: sunSVG  },
  { id:'moon', name:'つき',   color:'#C8AAFF', svg: moonSVG },
];

function starSVG(){return`<svg viewBox="0 0 80 80" xmlns="http://www.w3.org/2000/svg"><polygon points="40,8 49,30 73,30 54,45 61,68 40,54 19,68 26,45 7,30 31,30" fill="#FFD93D" stroke="#E8A800" stroke-width="2.5"/><circle cx="33" cy="36" r="3.5" fill="#1A1A4E"/><circle cx="47" cy="36" r="3.5" fill="#1A1A4E"/><path d="M33,48 Q40,56 47,48" stroke="#1A1A4E" stroke-width="2.5" fill="none" stroke-linecap="round"/></svg>`}
function bearSVG(){return`<svg viewBox="0 0 80 80" xmlns="http://www.w3.org/2000/svg"><circle cx="40" cy="45" r="26" fill="#c9a06a"/><circle cx="22" cy="24" r="12" fill="#c9a06a"/><circle cx="58" cy="24" r="12" fill="#c9a06a"/><circle cx="22" cy="24" r="7" fill="#e8c49a"/><circle cx="58" cy="24" r="7" fill="#e8c49a"/><ellipse cx="40" cy="52" rx="11" ry="8" fill="#e8c49a"/><circle cx="33" cy="42" r="3.5" fill="#1A1A4E"/><circle cx="47" cy="42" r="3.5" fill="#1A1A4E"/><ellipse cx="40" cy="52" rx="4" ry="2.5" fill="#b07060"/><circle cx="31" cy="50" r="4" fill="#ff9999" opacity="0.5"/><circle cx="49" cy="50" r="4" fill="#ff9999" opacity="0.5"/></svg>`}
function catSVG(){return`<svg viewBox="0 0 80 80" xmlns="http://www.w3.org/2000/svg"><circle cx="40" cy="46" r="24" fill="#A0C4FF"/><polygon points="16,28 24,10 32,28" fill="#A0C4FF"/><polygon points="48,28 56,10 64,28" fill="#A0C4FF"/><polygon points="18,28 24,14 30,28" fill="#FFB3C6"/><polygon points="50,28 56,14 62,28" fill="#FFB3C6"/><circle cx="33" cy="43" r="3.5" fill="#1A1A4E"/><circle cx="47" cy="43" r="3.5" fill="#1A1A4E"/><ellipse cx="40" cy="52" rx="4" ry="2.5" fill="#FFB3C6"/><line x1="26" y1="50" x2="16" y2="48" stroke="#1A1A4E" stroke-width="1.5"/><line x1="26" y1="53" x2="15" y2="53" stroke="#1A1A4E" stroke-width="1.5"/><line x1="54" y1="50" x2="64" y2="48" stroke="#1A1A4E" stroke-width="1.5"/><line x1="54" y1="53" x2="65" y2="53" stroke="#1A1A4E" stroke-width="1.5"/></svg>`}
function bunnySVG(){return`<svg viewBox="0 0 80 80" xmlns="http://www.w3.org/2000/svg"><circle cx="40" cy="48" r="22" fill="#FFB3C6"/><ellipse cx="26" cy="22" rx="8" ry="18" fill="#FFB3C6"/><ellipse cx="54" cy="22" rx="8" ry="18" fill="#FFB3C6"/><ellipse cx="26" cy="22" rx="4" ry="14" fill="#ff8aad"/><ellipse cx="54" cy="22" rx="4" ry="14" fill="#ff8aad"/><circle cx="33" cy="45" r="3.5" fill="#1A1A4E"/><circle cx="47" cy="45" r="3.5" fill="#1A1A4E"/><ellipse cx="40" cy="54" rx="4" ry="2.5" fill="#ff8aad"/></svg>`}
function frogSVG(){return`<svg viewBox="0 0 80 80" xmlns="http://www.w3.org/2000/svg"><circle cx="40" cy="48" r="24" fill="#6BCB77"/><circle cx="26" cy="28" r="10" fill="#6BCB77"/><circle cx="54" cy="28" r="10" fill="#6BCB77"/><circle cx="26" cy="28" r="6" fill="white"/><circle cx="54" cy="28" r="6" fill="white"/><circle cx="26" cy="28" r="3.5" fill="#1A1A4E"/><circle cx="54" cy="28" r="3.5" fill="#1A1A4E"/><path d="M30,58 Q40,66 50,58" stroke="#1A1A4E" stroke-width="2.5" fill="none" stroke-linecap="round"/></svg>`}
function robotSVG(){return`<svg viewBox="0 0 80 80" xmlns="http://www.w3.org/2000/svg"><rect x="16" y="30" width="48" height="36" rx="8" fill="#9B5DE5"/><rect x="24" y="16" width="32" height="20" rx="6" fill="#9B5DE5"/><rect x="28" y="8" width="8" height="10" rx="3" fill="#C8AAFF"/><rect x="44" y="8" width="8" height="10" rx="3" fill="#C8AAFF"/><rect x="30" y="38" width="10" height="10" rx="4" fill="#FFD93D"/><rect x="40" y="38" width="10" height="10" rx="4" fill="#FFD93D"/><rect x="28" y="22" width="10" height="7" rx="3" fill="#FFD93D"/><rect x="42" y="22" width="10" height="7" rx="3" fill="#FFD93D"/><rect x="26" y="54" width="28" height="6" rx="3" fill="#7040bb"/></svg>`}
function sunSVG(){return`<svg viewBox="0 0 80 80" xmlns="http://www.w3.org/2000/svg"><g stroke="#E8A800" stroke-width="3"><line x1="40" y1="6" x2="40" y2="18"/><line x1="40" y1="62" x2="40" y2="74"/><line x1="6" y1="40" x2="18" y2="40"/><line x1="62" y1="40" x2="74" y2="40"/><line x1="16" y1="16" x2="24" y2="24"/><line x1="56" y1="56" x2="64" y2="64"/><line x1="64" y1="16" x2="56" y2="24"/><line x1="24" y1="56" x2="16" y2="64"/></g><circle cx="40" cy="40" r="18" fill="#FFD93D" stroke="#E8A800" stroke-width="2.5"/><circle cx="34" cy="37" r="3" fill="#1A1A4E"/><circle cx="46" cy="37" r="3" fill="#1A1A4E"/><path d="M34,46 Q40,52 46,46" stroke="#1A1A4E" stroke-width="2.5" fill="none" stroke-linecap="round"/></svg>`}
function moonSVG(){return`<svg viewBox="0 0 80 80" xmlns="http://www.w3.org/2000/svg"><path d="M50,14 Q26,20 26,40 Q26,60 50,66 Q28,70 18,52 Q8,34 22,20 Z" fill="#C8AAFF" stroke="#9B5DE5" stroke-width="2"/><circle cx="34" cy="36" r="2.5" fill="#1A1A4E"/><circle cx="44" cy="42" r="2.5" fill="#1A1A4E"/><path d="M32,50 Q38,56 44,50" stroke="#1A1A4E" stroke-width="2" fill="none" stroke-linecap="round"/><circle cx="56" cy="20" r="3" fill="#FFD93D" opacity="0.8"/><circle cx="62" cy="36" r="2" fill="#FFD93D" opacity="0.6"/><circle cx="58" cy="54" r="2.5" fill="#FFD93D" opacity="0.7"/></svg>`}

// ============================================================
//  UI HELPERS
// ============================================================
function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  if(id === 'collectionScreen') renderCollection();
}

function selectSubject(s) {
  state.subject = s;
  ['btnMath','btnEng'].forEach(id => document.getElementById(id).classList.remove('selected'));
  document.getElementById(s==='math'?'btnMath':'btnEng').classList.add('selected');
  document.getElementById('engTypeCard').style.display = s==='eng' ? 'block' : 'none';
  if(s==='math') { state.engType = null; ['btnUpper','btnLower','btnWord'].forEach(id=>document.getElementById(id).classList.remove('selected')); }
}

function selectEngType(t) {
  state.engType = t;
  ['btnUpper','btnLower','btnWord'].forEach(id => document.getElementById(id).classList.remove('selected'));
  document.getElementById('btnUpper'+(t==='upper'?'':'')).classList.remove('selected');
  document.getElementById({upper:'btnUpper',lower:'btnLower',word:'btnWord'}[t]).classList.add('selected');
}

function selectDiff(d) {
  state.difficulty = d;
  ['btnSoft','btnNormal','btnHard'].forEach(id => document.getElementById(id).classList.remove('selected'));
  document.getElementById({soft:'btnSoft',normal:'btnNormal',hard:'btnHard'}[d]).classList.add('selected');
}

function selectMode(m) {
  state.mode = m;
  ['btnSelect','btnInput'].forEach(id => document.getElementById(id).classList.remove('selected'));
  document.getElementById(m==='select'?'btnSelect':'btnInput').classList.add('selected');
}

// ============================================================
//  QUESTION GENERATION
// ============================================================
function generateQuestion() {
  const {subject, engType, difficulty} = state;
  if(subject === 'math') {
    return genMath(difficulty);
  }
  if(engType === 'upper') {
    const correct = UPPERCASE_CHARS[rnd(0,25)];
    return { type:'letter', text: `この もじの おおもじは？\n"${correct.toLowerCase()}"`, answer: correct, chars: UPPERCASE_CHARS };
  }
  if(engType === 'lower') {
    const correct = LOWERCASE_CHARS[rnd(0,25)];
    return { type:'letter', text: `この もじの こもじは？\n"${correct.toUpperCase()}"`, answer: correct, chars: LOWERCASE_CHARS };
  }
  if(engType === 'word') {
    const wordList = WORDS[difficulty];
    const word = wordList[rnd(0, wordList.length-1)];
    return { type:'word', text: word, answer: word, chars: null };
  }
}

// ============================================================
//  GAME LOGIC
// ============================================================
let wordSelectedCells = [];

function startGame() {
  const {subject, engType, difficulty, mode} = state;
  if(!subject) { alert('かもくをえらんでね！'); return; }
  if(subject==='eng' && !engType) { alert('えいごのもんだいをえらんでね！'); return; }
  if(!difficulty) { alert('むずかしさをえらんでね！'); return; }
  if(!mode) { alert('もーどをえらんでね！'); return; }

  state.questionIdx = 0;
  state.correct = 0;
  state.totalChars = 0;
  state.startTime = Date.now();

  // Determine time
  if(subject === 'math') {
    state.totalTime = {soft:18,normal:15,hard:12}[difficulty];
  } else {
    // English: time measured per-round, but we use a per-question timer
    // For letter/word, use generous per-question timer
    state.totalTime = {soft:20,normal:15,hard:10}[difficulty];
  }

  showScreen('gameScreen');
  document.getElementById('gridWrap').style.display = mode==='select' ? 'grid' : 'none';
  document.getElementById('inputWrap').style.display = mode==='input' ? 'flex' : 'none';

  loadQuestion();
}

function loadQuestion() {
  if(state.questionIdx >= 10) { endGame(true); return; }

  const q = generateQuestion();
  state.currentQ = q;
  state.wordLetterIdx = 0;
  wordSelectedCells = [];

  document.getElementById('questionNum').textContent = state.questionIdx+1;
  document.getElementById('gauge').style.width = (state.correct/10*100)+'%';
  document.getElementById('gaugeCount').textContent = state.correct+'/10';

  // Display question
  if(q.type === 'word') {
    document.getElementById('questionLabel').textContent = 'つづりをえらぼう！';
    document.getElementById('questionText').textContent = '🔤 '+q.answer.toUpperCase();
    document.getElementById('wordProgress').style.display = 'block';
    document.getElementById('wordProgress').textContent = `（${q.answer.length}もじのたんご）`;
    document.getElementById('hintText').textContent = 'じゅんばんにタップ！';
  } else {
    document.getElementById('questionLabel').textContent = 'もんだい';
    document.getElementById('questionText').innerHTML = q.text.replace('\n','<br>');
    document.getElementById('wordProgress').style.display = 'none';
    document.getElementById('hintText').textContent = '';
  }

  if(state.mode === 'select') setupGrid(q);
  else setupInput(q);

  startTimer();
}

function setupGrid(q) {
  const cells = document.querySelectorAll('.grid-cell');
  cells.forEach(c => { c.className = 'grid-cell'; c.textContent = ''; });

  if(q.type === 'word') {
    // Show all letters of the word shuffled + some distractors
    const word = q.answer;
    const needed = word.split('');
    // Fill remaining cells with random letters (same letter set)
    const allLetters = 'abcdefghijklmnopqrstuvwxyz'.split('');
    const extras = shuffle(allLetters.filter(l => !needed.includes(l))).slice(0, 9-needed.length);
    const pool = shuffle([...needed, ...extras]).slice(0,9);
    // Pad if needed
    while(pool.length < 9) pool.push(allLetters[rnd(0,25)]);
    pool.forEach((ch,i) => { cells[i].textContent = ch.toUpperCase(); cells[i].dataset.letter = ch; });
    state.wordLetterOrder = needed;
    state.wordLetterIdx = 0;
  } else {
    const correct = q.answer;
    let pool;
    if(q.chars) {
      // Letter question
      const others = shuffle(q.chars.filter(c=>c!==correct)).slice(0,8);
      pool = shuffle([correct, ...others]);
    } else {
      // Math
      const correctNum = parseInt(correct);
      const others = new Set();
      while(others.size < 8) {
        const v = correctNum + rnd(-20,20);
        if(v !== correctNum && v >= 0) others.add(String(v));
      }
      pool = shuffle([correct, ...[...others].slice(0,8)]);
    }
    pool.slice(0,9).forEach((v,i) => { cells[i].textContent = v; });
  }
}

function setupInput(q) {
  const inp = document.getElementById('inputAnswer');
  inp.value = '';
  inp.focus();
}

function cellClick(idx) {
  const q = state.currentQ;
  if(!q) return;
  const cell = document.getElementById('cell'+idx);
  if(cell.classList.contains('correct') || cell.classList.contains('wrong')) return;

  if(q.type === 'word') {
    // Sequential letter tap
    const letter = cell.dataset.letter;
    const expected = q.answer[state.wordLetterIdx];
    if(letter === expected) {
      cell.classList.add('correct', 'selected-word');
      state.wordLetterIdx++;
      document.getElementById('hintText').textContent = `${state.wordLetterIdx}/${q.answer.length} もじ せいかい！`;
      spawnParticles(cell, '✨');
      if(state.wordLetterIdx >= q.answer.length) {
        // Full word correct
        clearInterval(state.timer);
        state.correct++;
        state.questionIdx++;
        document.getElementById('gauge').style.width = (state.correct/10*100)+'%';
        document.getElementById('gaugeCount').textContent = state.correct+'/10';
        showFeedback('🌟');
        setTimeout(loadQuestion, 800);
      }
    } else {
      cell.classList.add('wrong');
      showFeedback('💥');
      setTimeout(()=>{ cell.classList.remove('wrong'); }, 500);
    }
  } else {
    // Normal selection
    const val = cell.textContent;
    if(val === q.answer) {
      cell.classList.add('correct');
      clearInterval(state.timer);
      state.correct++;
      state.questionIdx++;
      document.getElementById('gauge').style.width = (state.correct/10*100)+'%';
      document.getElementById('gaugeCount').textContent = state.correct+'/10';
      showFeedback('⭐');
      spawnParticles(cell, '🎉');
      setTimeout(loadQuestion, 700);
    } else {
      cell.classList.add('wrong');
      showFeedback('💥');
      setTimeout(()=>{ cell.classList.remove('wrong'); }, 500);
    }
  }
}

function submitInput() {
  const q = state.currentQ;
  const val = document.getElementById('inputAnswer').value.trim();
  if(!val) return;

  let isCorrect = false;
  if(q.type === 'word') {
    isCorrect = val.toLowerCase() === q.answer.toLowerCase();
  } else if(q.chars) {
    isCorrect = val.toUpperCase() === q.answer.toUpperCase() || val.toLowerCase() === q.answer.toLowerCase();
  } else {
    isCorrect = val === q.answer;
  }

  document.getElementById('inputAnswer').value = '';

  if(isCorrect) {
    clearInterval(state.timer);
    state.correct++;
    state.questionIdx++;
    document.getElementById('gauge').style.width = (state.correct/10*100)+'%';
    document.getElementById('gaugeCount').textContent = state.correct+'/10';
    showFeedback('⭐');
    setTimeout(loadQuestion, 500);
  } else {
    showFeedback('💥');
    const inp = document.getElementById('inputAnswer');
    inp.style.borderColor = '#FF6B6B';
    setTimeout(()=>{ inp.style.borderColor = ''; }, 500);
  }
}

// Enter key for input mode
document.addEventListener('keydown', function(e) {
  if(e.key === 'Enter' && state.mode === 'input' && document.getElementById('gameScreen').classList.contains('active')) {
    submitInput();
  }
});

// ============================================================
//  TIMER
// ============================================================
function startTimer() {
  clearInterval(state.timer);
  state.timeLeft = state.totalTime;
  updateTimerDisplay();

  state.timer = setInterval(()=>{
    state.timeLeft -= 0.1;
    updateTimerDisplay();
    if(state.timeLeft <= 0) {
      clearInterval(state.timer);
      // Time up for this question: skip
      state.questionIdx++;
      loadQuestion();
    }
  }, 100);
}

function updateTimerDisplay() {
  const t = Math.max(0, state.timeLeft);
  document.getElementById('timerDisplay').textContent = Math.ceil(t);
  const pct = (t / state.totalTime) * 100;
  const bar = document.getElementById('timerBar');
  bar.style.width = pct+'%';
  if(pct < 30) bar.style.background = 'linear-gradient(90deg,#FF6B6B,#FF9F43)';
  else if(pct < 60) bar.style.background = 'linear-gradient(90deg,#FF9F43,#FFD93D)';
  else bar.style.background = 'linear-gradient(90deg,#00C9A7,#6BCB77)';
}

// ============================================================
//  END GAME
// ============================================================
function endGame(completed) {
  clearInterval(state.timer);
  const { correct, subject, engType, difficulty } = state;
  const rate = Math.round(correct/10*100);
  const success = correct >= (completed ? 10 : 10);

  // Check success: 10 correct
  const isSuccess = correct >= 10;

  document.getElementById('rCorrect').textContent = correct+'/10';
  document.getElementById('rRate').textContent = rate+'%';
  document.getElementById('rSubject').textContent = subject==='math' ? 'さんすう' : {upper:'大文字',lower:'小文字',word:'たんご'}[engType];
  document.getElementById('rDiff').textContent = {soft:'ソフト',normal:'ノーマル',hard:'ハード'}[difficulty];

  const stars = rate>=100?'⭐⭐⭐':rate>=70?'⭐⭐':rate>=40?'⭐':'';
  document.getElementById('resultStars').textContent = stars;

  const titleEl = document.getElementById('resultTitle');
  if(isSuccess) {
    titleEl.textContent = '🎉 すごい！ クリア！';
    titleEl.className = 'result-title result-success';
    // Unlock a character
    unlockCharacter();
    document.getElementById('rewardWrap').style.display = 'block';
  } else {
    titleEl.textContent = `😤 あと ${10-correct} もん！`;
    titleEl.className = 'result-title result-fail';
    document.getElementById('rewardWrap').style.display = 'none';
  }

  showScreen('resultScreen');
}

function unlockCharacter() {
  const locked = CHARACTERS.filter(c => !state.unlocked.includes(c.id));
  if(locked.length === 0) return;
  const char = locked[rnd(0, locked.length-1)];
  state.unlocked.push(char.id);
  localStorage.setItem('unlocked', JSON.stringify(state.unlocked));

  const wrap = document.getElementById('rewardWrap');
  wrap.innerHTML = `
    <p style="font-size:0.9rem;color:#888;margin-bottom:8px;">🎉 ${char.name} をゲット！</p>
    <div style="width:80px;height:80px;margin:0 auto;background:${char.color}22;border-radius:50%;display:flex;align-items:center;justify-content:center;padding:8px;animation:popIn 0.5s cubic-bezier(.34,1.56,.64,1);">
      ${char.svg()}
    </div>
  `;
  wrap.style.display = 'block';
}

// ============================================================
//  FEEDBACK
// ============================================================
function showFeedback(emoji) {
  const el = document.getElementById('feedback');
  el.textContent = emoji;
  el.className = 'feedback show';
  setTimeout(()=>{ el.className = 'feedback hide'; }, 600);
}

function spawnParticles(el, emoji) {
  const rect = el.getBoundingClientRect();
  const cx = rect.left + rect.width/2;
  const cy = rect.top + rect.height/2;
  for(let i=0; i<6; i++) {
    const p = document.createElement('div');
    p.className = 'particle';
    p.textContent = ['⭐','🌟','✨','💫'][rnd(0,3)];
    const angle = (i/6)*Math.PI*2;
    const dist = rnd(40,80);
    p.style.setProperty('--dx', Math.cos(angle)*dist+'px');
    p.style.setProperty('--dy', Math.sin(angle)*dist-30+'px');
    p.style.left = cx+'px';
    p.style.top = cy+'px';
    document.body.appendChild(p);
    setTimeout(()=>p.remove(), 1000);
  }
}

// ============================================================
//  COLLECTION
// ============================================================
function renderCollection() {
  const grid = document.getElementById('collectionGrid');
  grid.innerHTML = '';
  CHARACTERS.forEach(char => {
    const unlocked = state.unlocked.includes(char.id);
    const slot = document.createElement('div');
    slot.className = 'char-slot ' + (unlocked ? 'unlocked' : 'locked');
    slot.innerHTML = char.svg() + `<span>${char.name}</span>`;
    grid.appendChild(slot);
  });
}

// ============================================================
//  INIT
// ============================================================
// Default selections
selectSubject('math');
selectDiff('normal');
selectMode('select');
</script>
</body>
</html>
