```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>구구단 마스터</title>
  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: "Pretendard", "Noto Sans KR", Arial, sans-serif;
      background: linear-gradient(135deg, #e0f2fe, #f5f3ff);
      min-height: 100vh;
      color: #1e293b;
    }

    .container {
      width: min(920px, 94%);
      margin: 0 auto;
      padding: 40px 0;
    }

    header {
      text-align: center;
      margin-bottom: 30px;
    }

    header h1 {
      margin: 0 0 10px;
      font-size: 2.4rem;
      color: #4f46e5;
    }

    header p {
      margin: 0;
      color: #64748b;
      font-size: 1.05rem;
    }

    .score-board {
      display: flex;
      justify-content: center;
      gap: 15px;
      margin-bottom: 25px;
    }

    .score-card {
      background: white;
      border-radius: 16px;
      padding: 14px 28px;
      box-shadow: 0 8px 25px rgba(0,0,0,0.08);
      text-align: center;
    }

    .score-card span {
      display: block;
      font-size: 0.85rem;
      color: #64748b;
      margin-bottom: 4px;
    }

    .score-card strong {
      font-size: 1.5rem;
      color: #4f46e5;
    }

    .stage-screen,
    .quiz-screen {
      background: white;
      border-radius: 24px;
      padding: 30px;
      box-shadow: 0 15px 45px rgba(0,0,0,0.10);
    }

    .stage-screen h2,
    .quiz-screen h2 {
      text-align: center;
      margin-top: 0;
    }

    .stages {
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 14px;
      margin-top: 25px;
    }

    .stage-btn {
      border: none;
      border-radius: 16px;
      padding: 20px 10px;
      background: #eef2ff;
      color: #4338ca;
      font-size: 1.1rem;
      font-weight: 700;
      cursor: pointer;
      transition: 0.2s;
    }

    .stage-btn:hover {
      transform: translateY(-3px);
      background: #c7d2fe;
    }

    .stage-btn.random {
      background: #fef3c7;
      color: #b45309;
    }

    .stage-btn.random:hover {
      background: #fde68a;
    }

    .quiz-top {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
      color: #64748b;
      font-weight: 600;
    }

    .progress {
      height: 10px;
      background: #e2e8f0;
      border-radius: 999px;
      overflow: hidden;
      margin-bottom: 35px;
    }

    .progress-bar {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, #6366f1, #8b5cf6);
      transition: width 0.3s;
    }

    .question {
      text-align: center;
      font-size: 3.5rem;
      font-weight: 800;
      margin: 30px 0;
      color: #1e293b;
    }

    .answer-area {
      display: flex;
      justify-content: center;
      gap: 12px;
    }

    #answer {
      width: 180px;
      padding: 16px;
      border: 3px solid #cbd5e1;
      border-radius: 14px;
      font-size: 1.8rem;
      text-align: center;
      outline: none;
    }

    #answer:focus {
      border-color: #6366f1;
    }

    .submit-btn {
      border: none;
      border-radius: 14px;
      padding: 0 25px;
      background: #4f46e5;
      color: white;
      font-size: 1.15rem;
      font-weight: 700;
      cursor: pointer;
    }

    .submit-btn:hover {
      background: #4338ca;
    }

    .message {
      min-height: 55px;
      text-align: center;
      margin-top: 20px;
      font-size: 1.15rem;
      font-weight: 700;
    }

    .message.correct {
      color: #16a34a;
    }

    .message.wrong {
      color: #ea580c;
    }

    .back-btn {
      display: block;
      margin: 25px auto 0;
      border: none;
      background: transparent;
      color: #64748b;
      cursor: pointer;
      font-size: 1rem;
    }

    .back-btn:hover {
      color: #4f46e5;
    }

    .result {
      text-align: center;
    }

    .result .emoji {
      font-size: 4rem;
    }

    .result h2 {
      color: #4f46e5;
    }

    .result-score {
      font-size: 2rem;
      font-weight: 800;
      color: #16a34a;
      margin: 20px 0;
    }

    .restart-btn {
      border: none;
      border-radius: 14px;
      padding: 15px 30px;
      background: #4f46e5;
      color: white;
      font-size: 1.1rem;
      font-weight: 700;
      cursor: pointer;
    }

    .hidden {
      display: none;
    }

    @media (max-width: 650px) {
      .container {
        padding: 25px 0;
      }

      header h1 {
        font-size: 1.9rem;
      }

      .stages {
        grid-template-columns: repeat(2, 1fr);
      }

      .question {
        font-size: 2.7rem;
      }

      .answer-area {
        flex-direction: column;
        align-items: center;
      }

      #answer {
        width: 100%;
        max-width: 250px;
      }

      .submit-btn {
        height: 50px;
        width: 100%;
        max-width: 250px;
      }
    }
  </style>
</head>

<body>
  <div class="container">

    <header>
      <h1>✨ 구구단 마스터</h1>
      <p>구구단을 하나씩 연습하고 실력을 키워보세요!</p>
    </header>

    <div class="score-board">
      <div class="score-card">
        <span>누적 점수</span>
        <strong id="totalScore">0점</strong>
      </div>
    </div>

    <!-- 스테이지 선택 -->
    <section id="stageScreen" class="stage-screen">
      <h2>📚 스테이지를 선택하세요</h2>
      <p style="text-align:center; color:#64748b;">
        각 스테이지는 30문제로 구성됩니다.
      </p>

      <div class="stages">
        <button class="stage-btn" onclick="startStage(1)">1단</button>
        <button class="stage-btn" onclick="startStage(2)">2단</button>
        <button class="stage-btn" onclick="startStage(3)">3단</button>
        <button class="stage-btn" onclick="startStage(4)">4단</button>
        <button class="stage-btn" onclick="startStage(5)">5단</button>
        <button class="stage-btn" onclick="startStage(6)">6단</button>
        <button class="stage-btn" onclick="startStage(7)">7단</button>
        <button class="stage-btn" onclick="startStage(8)">8단</button>
        <button class="stage-btn" onclick="startStage(9)">9단</button>
        <button class="stage-btn random" onclick="startStage('random')">
          🎲 랜덤
        </button>
      </div>
    </section>

    <!-- 문제 화면 -->
    <section id="quizScreen" class="quiz-screen hidden">

      <div class="quiz-top">
        <span id="stageTitle">1단</span>
        <span id="questionNumber">1 / 30</span>
      </div>

      <div class="progress">
        <div id="progressBar" class="progress-bar"></div>
      </div>

      <div id="question" class="question">
        1 × 1 = ?
      </div>

      <div class="answer-area">
        <input
          type="number"
          id="answer"
          inputmode="numeric"
          autocomplete="off"
          placeholder="정답"
        >
        <button class="submit-btn" onclick="checkAnswer()">
          정답 확인
        </button>
      </div>

      <div id="message" class="message"></div>

      <button class="back-btn" onclick="goToStages()">
        ← 스테이지 선택으로 돌아가기
      </button>
    </section>

    <!-- 결과 화면 -->
    <section id="resultScreen" class="quiz-screen hidden">
      <div class="result">
        <div class="emoji">🎉</div>
        <h2>스테이지 완료!</h2>
        <p>30문제를 모두 풀었습니다.</p>

        <div class="result-score" id="resultScore">
          +300점
        </div>

        <p id="resultMessage"></p>

        <button class="restart-btn" onclick="goToStages()">
          다른 스테이지 도전하기
        </button>
      </div>
    </section>

  </div>

  <script>
    const TOTAL_QUESTIONS = 30;
    const POINT_PER_CORRECT = 10;

    let currentStage = null;
    let questions = [];
    let currentQuestionIndex = 0;
    let stageEarnedScore = 0;
    let totalScore = 0;

    const stageScreen = document.getElementById("stageScreen");
    const quizScreen = document.getElementById("quizScreen");
    const resultScreen = document.getElementById("resultScreen");

    const stageTitle = document.getElementById("stageTitle");
    const questionNumber = document.getElementById("questionNumber");
    const questionElement = document.getElementById("question");
    const answerInput = document.getElementById("answer");
    const messageElement = document.getElementById("message");
    const progressBar = document.getElementById("progressBar");
    const totalScoreElement = document.getElementById("totalScore");

    // 배열을 무작위로 섞는 함수
    function shuffle(array) {
      const copied = [...array];

      for (let i = copied.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));

        [copied[i], copied[j]] = [copied[j], copied[i]];
      }

      return copied;
    }

    // 문제 30개 생성
    function generateQuestions(stage) {
      let pool = [];

      if (stage === "random") {
        // 1~9단 전체 문제
        for (let dan = 1; dan <= 9; dan++) {
          for (let number = 1; number <= 9; number++) {
            pool.push({
              a: dan,
              b: number,
              answer: dan * number
            });
          }
        }
      } else {
        // 선택한 단의 문제
        for (let number = 1; number <= 9; number++) {
          pool.push({
            a: stage,
            b: number,
            answer: stage * number
          });
        }
      }

      /*
       * 문제 수가 30개이므로 기본 문제를 여러 번 사용할 수 있습니다.
       * 동일한 문제가 다시 나오는 것을 허용합니다.
       */
      const result = [];

      for (let i = 0; i < TOTAL_QUESTIONS; i++) {
        const randomProblem =
          pool[Math.floor(Math.random() * pool.length)];

        result.push({ ...randomProblem });
      }

      return result;
    }

    function startStage(stage) {
      currentStage = stage;
      currentQuestionIndex = 0;
      stageEarnedScore = 0;

      questions = generateQuestions(stage);

      stageScreen.classList.add("hidden");
      resultScreen.classList.add("hidden");
      quizScreen.classList.remove("hidden");

      if (stage === "random") {
        stageTitle.textContent = "🎲 랜덤 구구단";
      } else {
        stageTitle.textContent = `${stage}단`;
      }

      showQuestion();
    }

    function showQuestion() {
      const current = questions[currentQuestionIndex];

      questionNumber.textContent =
        `${currentQuestionIndex + 1} / ${TOTAL_QUESTIONS}`;

      questionElement.textContent =
        `${current.a} × ${current.b} = ?`;

      progressBar.style.width =
        `${(currentQuestionIndex / TOTAL_QUESTIONS) * 100}%`;

      answerInput.value = "";
      answerInput.disabled = false;
      messageElement.textContent = "";
      messageElement.className = "message";

      answerInput.focus();
    }

    function checkAnswer() {
      const userAnswer = Number(answerInput.value);

      if (answerInput.value.trim() === "") {
        messageElement.textContent = "정답을 입력해 주세요! 😊";
        messageElement.className = "message wrong";
        answerInput.focus();
        return;
      }

      const current = questions[currentQuestionIndex];

      if (userAnswer === current.answer) {
        // 정답이면 10점 적립
        stageEarnedScore += POINT_PER_CORRECT;
        totalScore += POINT_PER_CORRECT;

        totalScoreElement.textContent = `${totalScore}점`;

        messageElement.textContent =
          "🎉 정답이에요! +10점";

        messageElement.className = "message correct";

        answerInput.disabled = true;

        setTimeout(() => {
          currentQuestionIndex++;

          if (currentQuestionIndex >= TOTAL_QUESTIONS) {
            finishStage();
          } else {
            showQuestion();
          }
        }, 700);

      } else {
        // 오답일 경우 같은 문제를 다시 풀도록 함
        messageElement.textContent =
          "🤔 다시 생각해보세요! 구구단을 천천히 떠올려 봐요.";

        messageElement.className = "message wrong";

        answerInput.select();
      }
    }

    function finishStage() {
      quizScreen.classList.add("hidden");
      resultScreen.classList.remove("hidden");

      resultScore.textContent =
        `+${stageEarnedScore}점`;

      resultMessage.textContent =
        `현재까지 총 ${totalScore}점을 모았어요!`;

      progressBar.style.width = "100%";
    }

    function goToStages() {
      quizScreen.classList.add("hidden");
      resultScreen.classList.add("hidden");
      stageScreen.classList.remove("hidden");
    }

    // Enter 키로 정답 제출
    answerInput.addEventListener("keydown", function(event) {
      if (event.key === "Enter") {
        checkAnswer();
      }
    });
  </script>
</body>
</html>
```
