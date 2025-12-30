<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Labirin Hijaiyah</title>
    <!-- Tailwind CSS untuk styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        /* Animasi Custom */
        @keyframes bounce {
            0%, 100% { transform: translateY(-10%); }
            50% { transform: translateY(0); }
        }
        @keyframes pulse {
            0%, 100% { opacity: 1; transform: scale(1); }
            50% { opacity: .8; transform: scale(1.05); }
        }
        .animate-bounce-custom { animation: bounce 1s infinite; }
        .animate-pulse-custom { animation: pulse 2s infinite; }
        
        body {
            touch-action: manipulation; /* Mencegah zoom saat double tap */
            user-select: none; /* Mencegah seleksi teks */
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        /* Styling Scrollbar agar rapi */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: #888; border-radius: 4px; }
    </style>
</head>
<body class="bg-sky-100 min-h-screen flex items-center justify-center overflow-hidden relative">

    <!-- Background Decoration -->
    <div class="absolute top-0 left-0 w-full h-full pointer-events-none opacity-30 z-0">
        <div class="absolute top-10 left-10 text-yellow-400 animate-pulse-custom"><i data-lucide="star" size="48" fill="currentColor"></i></div>
        <div class="absolute top-20 right-20 text-yellow-400 animate-pulse-custom" style="animation-delay: 0.5s"><i data-lucide="star" size="32" fill="currentColor"></i></div>
        <div class="absolute bottom-10 left-1/4 bg-white rounded-full w-32 h-16 opacity-50 blur-xl"></div>
        <div class="absolute top-1/4 right-10 bg-white rounded-full w-48 h-24 opacity-50 blur-xl"></div>
    </div>

    <!-- Main Game Container -->
    <div class="relative z-10 w-full max-w-5xl aspect-video bg-white/80 backdrop-blur-sm rounded-3xl shadow-2xl border-4 border-sky-300 overflow-hidden flex flex-col m-2" style="height: 95vh; max-height: 700px;">
        
        <!-- Header -->
        <div class="bg-sky-500 text-white p-3 md:p-4 flex justify-between items-center shadow-md z-20 shrink-0">
            <div class="flex items-center gap-2">
                <span id="level-badge" class="bg-yellow-400 text-yellow-900 px-3 py-1 rounded-full font-bold shadow text-sm md:text-base">Level 1</span>
                <span id="score-display" class="font-bold text-lg ml-2 md:ml-4">Skor: 0</span>
            </div>
            <h1 class="text-xl md:text-2xl font-bold hidden md:block">Labirin Hijaiyah</h1>
            <div class="flex gap-2">
                <button onclick="toggleInstructions()" class="p-2 bg-sky-600 rounded-full hover:bg-sky-700 transition" title="Petunjuk">
                    <i data-lucide="help-circle" size="20"></i>
                </button>
                <button onclick="toggleSound()" id="sound-btn" class="p-2 bg-sky-600 rounded-full hover:bg-sky-700 transition">
                    <i data-lucide="volume-2" size="20"></i>
                </button>
                <button onclick="resetGame()" class="p-2 bg-sky-600 rounded-full hover:bg-sky-700 transition" title="Ulangi">
                    <i data-lucide="refresh-cw" size="20"></i>
                </button>
            </div>
        </div>

        <!-- Content Area -->
        <div class="flex-1 relative flex flex-col p-2 md:p-4 overflow-hidden">
            
            <!-- SCREENS -->
            
            <!-- 1. Start Screen -->
            <div id="start-screen" class="absolute inset-0 bg-sky-500/95 z-50 flex flex-col items-center justify-center text-white text-center p-4 transition-opacity duration-500">
                <div class="text-yellow-300 mb-4 animate-bounce-custom"><i data-lucide="star" width="80" height="80" fill="currentColor"></i></div>
                <h1 class="text-4xl md:text-5xl font-bold mb-2">Petualangan Harakat</h1>
                <p class="text-lg md:text-xl mb-6">Bantu Bintang Kecil menemukan huruf yang benar!</p>
                <button onclick="toggleInstructions()" class="mb-6 text-sky-100 hover:text-white underline flex items-center gap-1">
                    <i data-lucide="help-circle" size="16"></i> Cara Bermain
                </button>
                <button onclick="startGame()" class="bg-yellow-400 text-yellow-900 text-xl md:text-2xl font-bold px-8 py-3 rounded-full shadow-lg hover:bg-yellow-300 hover:scale-105 transition transform flex items-center gap-2">
                    <i data-lucide="play" fill="currentColor"></i> Mulai Main
                </button>
            </div>

            <!-- 2. Success Screen -->
            <div id="success-screen" class="hidden absolute inset-0 bg-green-500/95 z-50 flex flex-col items-center justify-center text-white text-center p-4">
                <h2 class="text-4xl md:text-5xl font-bold mb-4">Masya Allah!</h2>
                <div id="success-target" class="text-7xl md:text-8xl mb-6 font-serif bg-white text-green-600 w-32 h-32 rounded-full flex items-center justify-center shadow-lg border-4 border-yellow-400">
                    ?
                </div>
                <p class="text-2xl mb-8">Jawaban kamu benar.</p>
                <button onclick="nextLevel()" class="bg-white text-green-600 text-xl font-bold px-8 py-3 rounded-full shadow-lg hover:bg-gray-100 transition transform">
                    Lanjut Level Berikutnya
                </button>
            </div>

            <!-- 3. Complete Screen -->
            <div id="complete-screen" class="hidden absolute inset-0 bg-purple-500/95 z-50 flex flex-col items-center justify-center text-white text-center p-4">
                <h2 class="text-5xl md:text-6xl font-bold mb-4">Alhamdulillah!</h2>
                <p class="text-2xl mb-8">Kamu sudah menyelesaikan semua level.</p>
                <div class="text-4xl mb-8 bg-purple-700 px-6 py-3 rounded-xl">
                    Skor Akhir: <span id="final-score">0</span>
                </div>
                <button onclick="resetGame()" class="bg-yellow-400 text-yellow-900 text-xl font-bold px-8 py-3 rounded-full shadow-lg hover:bg-yellow-300 transition">
                    Main Lagi
                </button>
            </div>

            <!-- 4. Instructions Modal -->
            <div id="instructions-modal" class="hidden absolute inset-0 z-[60] bg-black/60 flex items-center justify-center p-4">
                <div class="bg-white rounded-2xl p-6 max-w-lg w-full shadow-2xl border-4 border-yellow-400 relative">
                    <button onclick="toggleInstructions()" class="absolute top-2 right-2 p-2 bg-gray-100 rounded-full hover:bg-gray-200 transition">
                        <i data-lucide="x" class="text-gray-600"></i>
                    </button>
                    <div class="text-center mb-6">
                        <h2 class="text-2xl font-bold text-sky-600 flex items-center justify-center gap-2">
                            <i data-lucide="help-circle" class="text-sky-500"></i> Cara Bermain
                        </h2>
                    </div>
                    <ul class="space-y-4 text-slate-700 text-base md:text-lg text-left">
                        <li class="flex gap-4 items-center bg-sky-50 p-3 rounded-lg">
                            <div class="bg-white p-2 rounded-full shadow-sm text-sky-500"><i data-lucide="info" size="24"></i></div>
                            <span><strong>Lihat Soal:</strong> Baca pertanyaan di kotak kiri atas.</span>
                        </li>
                        <li class="flex gap-4 items-center bg-sky-50 p-3 rounded-lg">
                            <div class="bg-white p-2 rounded-full shadow-sm text-sky-500"><i data-lucide="move" size="24"></i></div>
                            <span><strong>Gerakkan Bintang:</strong> Gunakan tombol panah di layar atau keyboard.</span>
                        </li>
                        <li class="flex gap-4 items-center bg-sky-50 p-3 rounded-lg">
                            <div class="bg-white p-2 rounded-full shadow-sm text-yellow-500 font-serif font-bold text-xl w-10 h-10 flex items-center justify-center">بَ</div>
                            <span><strong>Cari Jawaban:</strong> Masuk ke kotak huruf yang benar.</span>
                        </li>
                    </ul>
                    <button onclick="toggleInstructions()" class="w-full mt-6 bg-yellow-400 text-yellow-900 font-bold py-3 rounded-xl shadow hover:bg-yellow-300 transition">
                        Faham, Ayo Main!
                    </button>
                </div>
            </div>

            <!-- 5. Gameplay Area -->
            <div id="gameplay-area" class="hidden w-full h-full flex flex-col md:flex-row gap-4 items-center justify-center">
                
                <!-- Question Panel -->
                <div class="bg-white/90 p-4 rounded-2xl shadow-lg border-2 border-orange-300 w-full md:w-1/4 text-center shrink-0">
                    <p class="text-gray-500 text-sm mb-1">Pertanyaan:</p>
                    <h3 id="question-text" class="text-xl md:text-2xl font-bold text-sky-800 leading-tight">...</h3>
                    <div class="mt-2 text-xs text-gray-400 flex items-center justify-center gap-1">
                        <i data-lucide="info" size="14"></i> Gunakan tombol arah
                    </div>
                </div>

                <!-- Maze Grid Container -->
                <div id="grid-container" class="relative bg-emerald-100 p-2 rounded-xl border-4 border-emerald-400 shadow-inner flex flex-col justify-center items-center">
                    <!-- Grid generated by JS -->
                </div>

                <!-- Controls (Mobile/Touch) -->
                <div class="grid grid-cols-3 gap-2 md:ml-4 shrink-0">
                    <div></div>
                    <button onclick="movePlayer(0, -1)" class="w-12 h-12 md:w-14 md:h-14 bg-sky-400 rounded-full shadow-lg active:scale-95 active:bg-sky-600 flex items-center justify-center text-white">
                        <i data-lucide="arrow-up" size="28"></i>
                    </button>
                    <div></div>
                    
                    <button onclick="movePlayer(-1, 0)" class="w-12 h-12 md:w-14 md:h-14 bg-sky-400 rounded-full shadow-lg active:scale-95 active:bg-sky-600 flex items-center justify-center text-white">
                        <i data-lucide="arrow-left" size="28"></i>
                    </button>
                    <div class="flex items-center justify-center"><div class="w-4 h-4 bg-sky-200 rounded-full"></div></div>
                    <button onclick="movePlayer(1, 0)" class="w-12 h-12 md:w-14 md:h-14 bg-sky-400 rounded-full shadow-lg active:scale-95 active:bg-sky-600 flex items-center justify-center text-white">
                        <i data-lucide="arrow-right" size="28"></i>
                    </button>

                    <div></div>
                    <button onclick="movePlayer(0, 1)" class="w-12 h-12 md:w-14 md:h-14 bg-sky-400 rounded-full shadow-lg active:scale-95 active:bg-sky-600 flex items-center justify-center text-white">
                        <i data-lucide="arrow-down" size="28"></i>
                    </button>
                    <div></div>
                </div>
            </div>

        </div>
    </div>

    <!-- JAVASCRIPT GAME LOGIC -->
    <script>
        // --- DATA LEVEL ---
        const LEVELS = [
            {
                id: 1,
                questionAudio: "Temukan huruf Ba Fathah... Bunyinya Baa!",
                questionText: "Cari Huruf Ba Fathah (BA)",
                targetVal: "بَ",
                decoys: ["بِ", "بُ"],
                layout: [
                    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
                    [1, 2, 0, 0, 1, 0, 0, 0, 0, 0, 0, 9, 1],
                    [1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1],
                    [1, 9, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1],
                    [1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1],
                    [1, 0, 0, 0, 1, 0, 1, 9, 0, 0, 0, 0, 1],
                    [1, 0, 1, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1],
                    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
                ]
            },
            {
                id: 2,
                questionAudio: "Temukan huruf Ta Kasrah... Bunyinya Ti!",
                questionText: "Cari Huruf Ta Kasrah (TI)",
                targetVal: "تِ",
                decoys: ["تَ", "تُ"],
                layout: [
                    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
                    [1, 9, 0, 0, 0, 0, 0, 1, 0, 0, 0, 9, 1],
                    [1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0, 1],
                    [1, 2, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 1],
                    [1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1],
                    [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 9, 1],
                    [1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
                    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
                ]
            },
            {
                id: 3,
                questionAudio: "Temukan huruf Jim Dhammah... Bunyinya Ju!",
                questionText: "Cari Huruf Jim Dhammah (JU)",
                targetVal: "جُ",
                decoys: ["جَ", "جِ"],
                layout: [
                    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
                    [1, 9, 0, 0, 1, 9, 0, 0, 0, 0, 0, 2, 1],
                    [1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 1, 0, 1],
                    [1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1],
                    [1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1],
                    [1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 9, 1],
                    [1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1],
                    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
                ]
            }
        ];

        // --- STATE VARIABLES ---
        let currentLevelIdx = 0;
        let gameState = 'start'; // start, playing, success, complete
        let playerPos = { x: 1, y: 1 };
        let grid = [];
        let answers = [];
        let score = 0;
        let soundEnabled = true;

        // --- DOM ELEMENTS ---
        const domStart = document.getElementById('start-screen');
        const domGame = document.getElementById('gameplay-area');
        const domSuccess = document.getElementById('success-screen');
        const domComplete = document.getElementById('complete-screen');
        const domInstructions = document.getElementById('instructions-modal');
        const domGrid = document.getElementById('grid-container');
        const domQuestion = document.getElementById('question-text');
        const domScore = document.getElementById('score-display');
        const domLevelBadge = document.getElementById('level-badge');
        const domFinalScore = document.getElementById('final-score');
        const domSuccessTarget = document.getElementById('success-target');
        const btnSoundIcon = document.querySelector('#sound-btn i');

        // --- INIT ---
        window.onload = function() {
            lucide.createIcons();
            // Start screen is visible by default via HTML classes
        };

        // --- AUDIO ENGINE ---
        function playSound(type) {
            if (!soundEnabled) return;
            try {
                const AudioContext = window.AudioContext || window.webkitAudioContext;
                if (!AudioContext) return;
                
                const ctx = new AudioContext();
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                
                osc.connect(gain);
                gain.connect(ctx.destination);
                
                const now = ctx.currentTime;

                if (type === 'move') {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(300, now);
                    gain.gain.setValueAtTime(0.1, now);
                    osc.start();
                    osc.stop(now + 0.1);
                } else if (type === 'win') {
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(500, now);
                    osc.frequency.exponentialRampToValueAtTime(1000, now + 0.1);
                    gain.gain.setValueAtTime(0.2, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.5);
                    osc.start();
                    osc.stop(now + 0.5);
                } else if (type === 'lose') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(200, now);
                    osc.frequency.linearRampToValueAtTime(100, now + 0.3);
                    gain.gain.setValueAtTime(0.2, now);
                    osc.start();
                    osc.stop(now + 0.3);
                }
            } catch (e) {
                console.error("Audio error", e);
            }
        }

        function speak(text) {
            if (!soundEnabled) return;
            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel(); // Stop previous
                const utterance = new SpeechSynthesisUtterance(text);
                utterance.lang = 'id-ID';
                utterance.rate = 0.9;
                window.speechSynthesis.speak(utterance);
            }
        }

        // --- GAME LOGIC ---

        function toggleSound() {
            soundEnabled = !soundEnabled;
            // Update Icon
            if (soundEnabled) {
                btnSoundIcon.setAttribute('data-lucide', 'volume-2');
            } else {
                btnSoundIcon.setAttribute('data-lucide', 'volume-x');
            }
            lucide.createIcons();
        }

        function toggleInstructions() {
            if (domInstructions.classList.contains('hidden')) {
                domInstructions.classList.remove('hidden');
            } else {
                domInstructions.classList.add('hidden');
            }
        }

        function startGame() {
            gameState = 'playing';
            score = 0;
            currentLevelIdx = 0;
            updateUI();
            loadLevel(currentLevelIdx);
        }

        function resetGame() {
            gameState = 'start';
            score = 0;
            currentLevelIdx = 0;
            updateUI();
        }

        function updateUI() {
            // Hide all
            domStart.classList.add('hidden');
            domGame.classList.add('hidden');
            domSuccess.classList.add('hidden');
            domComplete.classList.add('hidden');

            if (gameState === 'start') {
                domStart.classList.remove('hidden');
            } else if (gameState === 'playing') {
                domGame.classList.remove('hidden');
                domScore.textContent = `Skor: ${score}`;
                domLevelBadge.textContent = `Level ${currentLevelIdx + 1}`;
            } else if (gameState === 'success') {
                domSuccess.classList.remove('hidden');
                domSuccessTarget.textContent = LEVELS[currentLevelIdx].targetVal;
            } else if (gameState === 'complete') {
                domComplete.classList.remove('hidden');
                domFinalScore.textContent = score;
            }
        }

        function loadLevel(idx) {
            const levelData = LEVELS[idx];
            grid = JSON.parse(JSON.stringify(levelData.layout)); // Deep copy
            let answerSpots = [];
            answers = [];

            // Find spots
            for (let y = 0; y < grid.length; y++) {
                for (let x = 0; x < grid[y].length; x++) {
                    if (grid[y][x] === 2) {
                        playerPos = { x, y };
                    } else if (grid[y][x] === 9) {
                        answerSpots.push({ x, y });
                    }
                }
            }

            // Randomize Answers
            answerSpots.sort(() => 0.5 - Math.random());
            
            // 1 Correct
            answers.push({ ...answerSpots[0], val: levelData.targetVal, isCorrect: true });
            // Decoys
            levelData.decoys.forEach((decoy, i) => {
                if (answerSpots[i + 1]) {
                    answers.push({ ...answerSpots[i + 1], val: decoy, isCorrect: false });
                }
            });

            domQuestion.textContent = levelData.questionText;
            renderGrid();
            speak(levelData.questionAudio);
        }

        function renderGrid() {
            domGrid.innerHTML = ''; // Clear previous
            
            // Determine cell size based on grid width (basic responsive)
            // Default Tailwind classes used instead for simplicity
            
            grid.forEach((row, y) => {
                const rowDiv = document.createElement('div');
                rowDiv.className = 'flex';
                
                row.forEach((cell, x) => {
                    const cellDiv = document.createElement('div');
                    // Base classes: w-8 h-8 mobile, w-10 h-10 desktop
                    let baseClass = "w-8 h-8 md:w-12 md:h-12 flex items-center justify-center text-lg md:text-2xl transition-all duration-200 m-[1px] relative ";
                    
                    const answer = answers.find(a => a.x === x && a.y === y);
                    const isPlayer = playerPos.x === x && playerPos.y === y;

                    if (cell === 1) {
                        // Wall
                        cellDiv.className = baseClass + "bg-emerald-600 rounded-sm shadow-sm overflow-hidden";
                        cellDiv.innerHTML = '<div class="absolute bottom-0 w-full h-1 bg-emerald-800/30"></div>'; // decorative
                    } else {
                        // Path
                        cellDiv.className = baseClass + "bg-emerald-50";

                        // Render Answer
                        if (answer) {
                            const ansDiv = document.createElement('div');
                            ansDiv.className = "absolute inset-0 m-1 bg-white rounded-full border-2 border-orange-200 flex items-center justify-center font-serif text-slate-700 shadow-sm z-10 animate-pulse-custom";
                            ansDiv.textContent = answer.val;
                            cellDiv.appendChild(ansDiv);
                        }

                        // Render Player
                        if (isPlayer) {
                            const pDiv = document.createElement('div');
                            pDiv.className = "absolute inset-0 z-20 flex items-center justify-center transition-all duration-300 transform scale-125 text-yellow-500 drop-shadow-md";
                            // SVG for Star
                            pDiv.innerHTML = `<svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="currentColor" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-star"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></svg>
                                              <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 flex flex-col items-center mt-1">
                                                  <div class="flex gap-1"><div class="w-1 h-1 bg-black rounded-full"></div><div class="w-1 h-1 bg-black rounded-full"></div></div>
                                                  <div class="w-2 h-1 bg-black rounded-b-full mt-[1px]"></div>
                                              </div>`;
                            cellDiv.appendChild(pDiv);
                        }
                    }
                    rowDiv.appendChild(cellDiv);
                });
                domGrid.appendChild(rowDiv);
            });
        }

        function movePlayer(dx, dy) {
            if (gameState !== 'playing') return;

            const newX = playerPos.x + dx;
            const newY = playerPos.y + dy;

            // Collision Check
            if (
                newY >= 0 && newY < grid.length &&
                newX >= 0 && newX < grid[0].length &&
                grid[newY][newX] !== 1
            ) {
                playerPos = { x: newX, y: newY };
                playSound('move');
                renderGrid(); // Re-render to show new position

                // Check Answers
                const hitAnswer = answers.find(a => a.x === newX && a.y === newY);
                if (hitAnswer) {
                    if (hitAnswer.isCorrect) {
                        handleWin();
                    } else {
                        handleFail();
                    }
                }
            }
        }

        function handleWin() {
            gameState = 'success';
            score += 10;
            playSound('win');
            speak("Masya Allah, Benar!");
            updateUI();
        }

        function handleFail() {
            // No state change, just feedback
            playSound('lose');
            speak("Ayo coba lagi!");
        }

        function nextLevel() {
            if (currentLevelIdx < LEVELS.length - 1) {
                currentLevelIdx++;
                gameState = 'playing';
                updateUI();
                loadLevel(currentLevelIdx);
            } else {
                gameState = 'complete';
                speak("Alhamdulillah, semua level selesai!");
                updateUI();
            }
        }

        // Keyboard Listener
        window.addEventListener('keydown', (e) => {
            if (gameState !== 'playing') return;
            switch(e.key) {
                case 'ArrowUp': movePlayer(0, -1); break;
                case 'ArrowDown': movePlayer(0, 1); break;
                case 'ArrowLeft': movePlayer(-1, 0); break;
                case 'ArrowRight': movePlayer(1, 0); break;
            }
        });

    </script>
</body>
</html>
