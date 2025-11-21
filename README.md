<html lang="id">
<head>
    <meta charset="UTF-8"></meta>
    <meta content="width=device-width, initial-scale=1.0" name="viewport"></meta>
    <title>Tarik Tambang Matematika - Kontrol Mulai Manual</title>
    <style>
        /* --- General Styling --- */
        body {
            /* Hapus properti display:flex di body agar tidak mengganggu layout post Blogger */
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            color: #333;
            /* Tambahkan padding agar game tidak menempel ke pinggir layar jika post full-width */
            padding: 10px; 
        }

        #game-container {
            /* Kunci Responsif Blogspot: Gunakan 100% lebar yang tersedia */
            width: 100% !important; 
            max-width: 1200px;
            margin: 20px auto; /* Tambahkan auto margin agar tetap di tengah */
            background-color: white;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 8px 16px rgba(0,0,0,0.15);
            position: relative; 
        }

        h1 {
            color: #4a7bb5;
            text-align: center;
            margin-bottom: 5px;
            font-size: 2em;
        }
        
        /* --- START BUTTON CONTAINER --- */
        #start-button-container {
            text-align: center;
            padding: 50px 0;
            display: block;
        }

        #start-button {
            padding: 20px 40px;
            font-size: 1.8em;
            cursor: pointer;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 123, 255, 0.4);
            transition: background-color 0.2s, transform 0.1s;
        }

        #start-button:hover {
            background-color: #0056b3;
            transform: translateY(-2px);
        }

        /* --- COUNTDOWN OVERLAY --- */
        #countdown-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(255, 255, 255, 0.95);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000; 
        }

        #countdown-display {
            font-size: 8em;
            font-weight: bold;
            color: #4a7bb5;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
            animation: pulse 1s infinite alternate;
        }

        @keyframes pulse {
            0% { transform: scale(1); opacity: 1; }
            100% { transform: scale(1.1); opacity: 0.8; }
        }
        
        /* --- Rope Track --- */
        #rope-track {
            display: flex;
            justify-content: space-between;
            align-items: center;
            height: 60px;
            margin: 20px 0 30px 0;
            background-color: #ddd;
            border-radius: 8px;
            position: relative;
            padding: 0 10px;
        }

        #center-line {
            position: absolute;
            left: 50%;
            width: 4px;
            height: 100%;
            background-color: #dc3545; 
            transform: translateX(-50%);
            z-index: 1;
        }

        #indicator {
            width: 25px;
            height: 25px;
            background-color: gold;
            border-radius: 50%;
            border: 3px solid #333;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            transition: left 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
            z-index: 2;
        }
        
        /* Skor Display - DIHILANGKAN */
        .score-display {
            display: none; 
        }

        /* Styling Pemain */
        .player-info {
            padding: 10px;
            font-weight: bold;
            color: white;
            font-size: 1.1em;
            border-radius: 5px;
            min-width: 150px;
            text-align: center;
            transition: transform 0.3s;
        }
        #player-left { background-color: #007bff; }
        #player-right { background-color: #28a745; }

        /* --- Duel Area --- */
        #duel-area {
            display: flex;
            justify-content: space-between;
            gap: 20px;
            margin-top: 30px;
        }
        
        .player-zone {
            flex: 1;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.05);
            background-color: #ffffff;
        }
        
        .player-zone h2 {
            text-align: center;
            margin-bottom: 15px;
            font-size: 1.5em;
        }

        /* Garis Tengah Pemisah Area Soal */
        #duel-area::after {
            content: '';
            width: 2px;
            background-color: #ccc;
            margin: 10px 0;
        }

        /* Area Soal */
        .question-display {
            font-size: 2.2em;
            font-weight: 700;
            text-align: center;
            min-height: 40px;
            margin-bottom: 25px;
            color: #212529;
        }

        /* Tombol Pilihan */
        .choices-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        .choice-button {
            padding: 12px 5px;
            font-size: 1.3em;
            font-weight: bold;
            background-color: #f0f0f0;
            color: #333;
            border: 2px solid #aaa;
            border-radius: 6px;
            cursor: pointer;
            transition: background-color 0.15s, border-color 0.15s;
        }

        .choice-button:hover:not(:disabled) {
            background-color: #e0e0e0;
        }

        .choice-button:disabled {
            cursor: not-allowed;
            opacity: 0.5;
        }
        
        /* Feedback */
        .message-display {
            margin-top: 15px;
            min-height: 25px;
            font-weight: 600;
            text-align: center;
            font-size: 1.1em;
        }

        .correct-highlight {
            background-color: #28a745 !important;
            color: white !important;
            border-color: #1e7e34 !important;
        }
        .incorrect-highlight {
            background-color: #dc3545 !important;
            color: white !important;
            border-color: #bd2130 !important;
        }
        .win { color: green; }
        .lose { color: red; }

        #reset-button {
            padding: 12px 25px;
            font-size: 1.1em;
            cursor: pointer;
            background-color: #dc3545;
            color: white;
            border: none;
            border-radius: 6px;
            margin-top: 30px;
            width: 100%;
        }

        /* ================================================= */
        /* === MEDIA QUERY UNTUK LAYAR PONSSEL (Max 768px) === */
        /* ================================================= */
        @media (max-width: 768px) {
            #game-container { padding: 15px; margin: 10px auto; }
            #duel-area { flex-direction: column; gap: 15px; }
            #duel-area::after { content: none; }
            h1 { font-size: 1.5em; }
            #rope-track { height: 40px; margin: 15px 0 20px 0; padding: 0 5px; }
            #indicator { width: 20px; height: 20px; }
            .player-info { min-width: 90px; font-size: 0.9em; }
            .player-zone { padding: 15px; }
            .player-zone h2 { font-size: 1.2em; }
            .question-display { font-size: 1.5em; margin-bottom: 15px; }
            .choices-grid { grid-template-columns: repeat(4, 1fr); gap: 5px; }
            .choice-button { padding: 15px 5px; font-size: 1.1em; }
            #countdown-display { font-size: 5em; }
            
            #start-button {
                font-size: 1.5em;
                padding: 15px 30px;
            }
        }
    </style>
</head>
<body>

    <div id="countdown-overlay">
        <div id="countdown-display"></div>
    </div>

    <div id="game-container">
        <h1>Tarik Tambang Matematika - Kontrol Manual 🎮</h1>
        <p style="text-align: center;">Pemain yang berhasil menarik tambang hingga **batas akhir** memenangkan permainan.</p>

        <div id="start-button-container">
            <button id="start-button">Mulai Permainan 🚀</button>
        </div>

        <div id="rope-track" style="display: none;">
            <span class="score-display" id="score-left"></span>
            <span class="player-info" id="player-left">Pemain 1 (Kiri)</span>
            <div id="center-line"></div>
            <div id="indicator"></div>
            <span class="player-info" id="player-right">Pemain 2 (Kanan)</span>
            <span class="score-display" id="score-right"></span>
        </div>

        <div id="duel-area" style="display: none;">
            
            <div class="player-zone" id="zone-p1">
                <h2 style="color: #007bff;">Soal Pemain 1</h2>
                <div class="question-display" id="question-p1"></div>
                <div class="choices-grid" id="choices-p1"></div>
                <div class="message-display" id="message-p1"></div>
            </div>

            <div class="player-zone" id="zone-p2">
                <h2 style="color: #28a745;">Soal Pemain 2</h2>
                <div class="question-display" id="question-p2"></div>
                <div class="choices-grid" id="choices-p2"></div>
                <div class="message-display" id="message-p2"></div>
            </div>

        </div>

        <button id="reset-button" style="display: none;">Mulai Ulang Permainan</button>
    </div>

    <script>
        const indicator = document.getElementById('indicator');
        const resetButton = document.getElementById('reset-button');
        const startButton = document.getElementById('start-button');
        const startButtonContainer = document.getElementById('start-button-container');
        const ropeTrack = document.getElementById('rope-track');
        const duelArea = document.getElementById('duel-area');
        
        const countdownOverlay = document.getElementById('countdown-overlay');
        const countdownDisplay = document.getElementById('countdown-display');
        const COUNTDOWN_TIME = 3; 

        // Pemain Objects
        const P1 = { id: 1, questionElement: document.getElementById('question-p1'), choicesContainer: document.getElementById('choices-p1'), messageElement: document.getElementById('message-p1'), infoElement: document.getElementById('player-left'), currentQuestion: {} };
        const P2 = { id: 2, questionElement: document.getElementById('question-p2'), choicesContainer: document.getElementById('choices-p2'), messageElement: document.getElementById('message-p2'), infoElement: document.getElementById('player-right'), currentQuestion: {} };
        
        const NUM_CHOICES = 8;
        // PULL_LIMIT mewakili nilai tarikan maksimum dari titik tengah (50%)
        const PULL_LIMIT = 5; 
        const PULL_STRENGTH = 1; // Kekuatan tarik per jawaban benar (1 unit)
        const MAX_PULL = PULL_LIMIT * PULL_STRENGTH; // Total tarikan yang dibutuhkan (5 unit)

        let currentPosition = 0; // Posisi tali saat ini (-MAX_PULL hingga MAX_PULL)
        let isGameActive = false;

        // --- FUNGSI UTILITAS ---
        function generateQuestion() {
            const operators = ['+', '-', '*']; 
            const operator = operators[Math.floor(Math.random() * operators.length)];
            let num1, num2, answer;
            if (operator === '*') {
                num1 = Math.floor(Math.random() * 10) + 1;
                num2 = Math.floor(Math.random() * 5) + 1;
                answer = num1 * num2;
            } else {
                num1 = Math.floor(Math.random() * 15) + 1;
                num2 = Math.floor(Math.random() * 10) + 1; 
                if (operator === '+') {
                    answer = num1 + num2;
                } else if (operator === '-') {
                    if (num1 < num2) { [num1, num2] = [num2, num1]; }
                    answer = num1 - num2;
                }
            }
            return { question: `${num1} ${operator} ${num2}`, answer: answer };
        }

        function generateWrongAnswers(correctAnswer, count) {
            const wrongAnswers = new Set();
            while (wrongAnswers.size < count) {
                let wrongAnswer = correctAnswer + Math.floor(Math.random() * 13) - 6; 
                if (wrongAnswer !== correctAnswer && wrongAnswer >= 0) {
                    wrongAnswers.add(wrongAnswer);
                }
            }
            return Array.from(wrongAnswers);
        }
        
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        // --- LOGIKA COUNTDOWN ---
        function startCountdown() {
            startButtonContainer.style.display = 'none';
            resetButton.style.display = 'none';
            
            ropeTrack.style.display = 'flex';
            duelArea.style.display = 'flex';

            let timeLeft = COUNTDOWN_TIME;
            countdownOverlay.style.display = 'flex';
            
            resetButton.disabled = true;

            countdownDisplay.textContent = timeLeft;

            const timerInterval = setInterval(() => {
                timeLeft--;
                if (timeLeft > 0) {
                    countdownDisplay.textContent = timeLeft;
                } else if (timeLeft === 0) {
                    countdownDisplay.textContent = 'MULAI!';
                } else {
                    clearInterval(timerInterval);
                    countdownOverlay.style.display = 'none';
                    resetButton.disabled = false;
                    initializeGame(); 
                }
            }, 1000);
        }
        
        // --- LOGIKA GAME ---

        function displayDualQuestions() {
            displayQuestionForPlayer(P1);
            displayQuestionForPlayer(P2);
        }
        
        function displayQuestionForPlayer(player) {
            player.currentQuestion = generateQuestion();
            player.questionElement.textContent = `${player.currentQuestion.question} = ?`;
            player.messageElement.textContent = '';
            
            const wrongAnswers = generateWrongAnswers(player.currentQuestion.answer, NUM_CHOICES - 1);
            let choices = [...wrongAnswers, player.currentQuestion.answer];
            choices = shuffleArray(choices);

            player.choicesContainer.innerHTML = '';
            choices.forEach(choice => {
                const button = document.createElement('button');
                button.textContent = choice;
                button.classList.add('choice-button');
                button.setAttribute('data-player-id', player.id);
                button.setAttribute('data-answer', choice);
                button.addEventListener('click', handleChoiceClick);
                player.choicesContainer.appendChild(button);
            });
            
            togglePlayerButtons(player.id, false); 
        }

        function handleChoiceClick(event) {
            if (!isGameActive) return;

            const clickedButton = event.target;
            const playerId = parseInt(clickedButton.getAttribute('data-player-id'), 10);
            const userAnswer = parseInt(clickedButton.getAttribute('data-answer'), 10);
            
            const player = playerId === 1 ? P1 : P2;
            togglePlayerButtons(playerId, true);
            
            let isCorrect = (userAnswer === player.currentQuestion.answer);

            if (isCorrect) {
                clickedButton.classList.add('correct-highlight');
            } else {
                clickedButton.classList.add('incorrect-highlight');
                // Highlight jawaban benar
                player.choicesContainer.querySelectorAll('.choice-button').forEach(btn => {
                    if (parseInt(btn.getAttribute('data-answer'), 10) === player.currentQuestion.answer) {
                        btn.classList.add('correct-highlight');
                    }
                });
            }

            checkAnswer(isCorrect, player);
            
            if (isGameActive) {
                displayQuestionForPlayer(player); 
            }
        }

        function checkAnswer(isCorrect, player) {
            // Player 1 (Kiri) menarik ke nilai positif (+), Player 2 (Kanan) menarik ke nilai negatif (-)
            const pullUnit = (player.id === 1) ? PULL_STRENGTH : -PULL_STRENGTH; 
            const playerName = player.id === 1 ? 'Pemain 1' : 'Pemain 2';
            
            if (isCorrect) {
                // Jawaban benar: tarik tambang ke arah pemain
                const pullText = (player.id === 1) ? 'ke KIRI' : 'ke KANAN';
                player.messageElement.innerHTML = `<span class="win">✅ Benar! ${playerName} menarik tambang ${pullText}.</span>`;
                currentPosition += pullUnit; // MENARIK TALI
            } else {
                // Jawaban salah: tali tidak bergerak (TIDAK ADA PENALTI)
                player.messageElement.innerHTML = `<span class="lose">❌ Salah! ${playerName} gagal menarik tambang. Tali tidak bergerak.</span>`;
            }
            
            // Batasi posisi agar tidak melampaui batas MAX_PULL
            currentPosition = Math.max(-MAX_PULL, Math.min(MAX_PULL, currentPosition));
            
            updateRopeVisual();

            // Cek kondisi menang
            if (currentPosition >= MAX_PULL) {
                endGame('Pemain 1 (Kiri)');
            } else if (currentPosition <= -MAX_PULL) {
                endGame('Pemain 2 (Kanan)');
            }
        }
        
        function togglePlayerButtons(playerId, disabled) {
            const container = playerId === 1 ? P1.choicesContainer : P2.choicesContainer;
            container.querySelectorAll('.choice-button').forEach(button => {
                button.disabled = disabled;
                if (!disabled) {
                    button.classList.remove('correct-highlight', 'incorrect-highlight');
                }
            });
        }

        // --- LOGIKA POSISI TALI ---
        function updateRopeVisual() {
            // Menghitung persentase posisi tali untuk CSS
            const cssLeft = 50 + (currentPosition / MAX_PULL) * 50;
            indicator.style.left = `${cssLeft}%`;
        }


        function endGame(winner) {
            isGameActive = false;
            P1.questionElement.textContent = `PERMAINAN SELESAI!`;
            P2.questionElement.textContent = `PERMAINAN SELESAI!`;
            P1.messageElement.innerHTML = `Pemenang: <span class="win">🏆 ${winner} 🏆</span>`;
            P2.messageElement.innerHTML = `Pemenang: <span class="win">🏆 ${winner} 🏆</span>`;
            
            togglePlayerButtons(1, true); 
            togglePlayerButtons(2, true); 
            
            resetButton.style.display = 'block';
        }

        // Fungsi yang menjalankan inisialisasi Game (setelah countdown)
        function initializeGame() {
            currentPosition = 0;
            isGameActive = true;
            
            updateRopeVisual();
            
            startButtonContainer.style.display = 'none'; 
            displayDualQuestions();
        }

        // --- INISIALISASI EVENT LISTENERS ---
        startButton.addEventListener('click', startCountdown);
        resetButton.addEventListener('click', startCountdown);

    </script>

</body>
</html>
