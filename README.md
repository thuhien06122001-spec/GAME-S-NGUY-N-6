<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Giải Cứu Vương Quốc Số Nguyên - Toán 6</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts: Press Start 2P & Play -->
    <link href="https://fonts.googleapis.com/css2?family=VT323&family=Nunito:wght@400;600;700;800;900&display=swap" rel="stylesheet">

    <style>
        body {
            font-family: 'Nunito', sans-serif;
            user-select: none;
            -webkit-user-select: none;
        }
        .pixel-font {
            font-family: 'VT323', monospace;
        }
        .pixel-box {
            border: 4px solid #1e293b;
            box-shadow: 4px 4px 0px #0f172a;
        }
        .pixel-btn {
            transition: all 0.1s ease;
            box-shadow: 0px 4px 0px #00000033;
        }
        .pixel-btn:active {
            transform: translateY(2px);
            box-shadow: 0px 2px 0px #00000033;
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #1e293b;
        }
        ::-webkit-scrollbar-thumb {
            background: #3b82f6;
            border-radius: 4px;
        }
        .glass-panel {
            background: rgba(15, 23, 42, 0.85);
            backdrop-filter: blur(8px);
        }
        .floating {
            animation: float 3s ease-in-out infinite;
        }
        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-8px); }
        }
    </style>
</head>
<body class="bg-slate-900 text-white min-h-screen flex flex-col justify-between overflow-x-hidden selection:bg-amber-400 selection:text-slate-900">

    <!-- CANVAS DÙNG CHO PHÁO HOA/HIỆU ỨNG THẮNG LỢI -->
    <canvas id="fxCanvas" class="fixed inset-0 pointer-events-none z-50 w-full h-full"></canvas>

    <!-- HEADER / GAME BAR -->
    <header class="bg-slate-800 border-b-4 border-slate-700 p-3 sm:p-4 sticky top-0 z-40 shadow-lg">
        <div class="max-w-5xl mx-auto flex flex-wrap justify-between items-center gap-2">
            <!-- Tên Trò Chơi -->
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 bg-amber-500 rounded-lg flex items-center justify-center text-slate-900 font-black text-2xl shadow-inner">
                    <i class="fa-solid fa-crown"></i>
                </div>
                <div>
                    <h1 class="pixel-font text-2xl sm:text-3xl font-bold tracking-wider text-amber-400 leading-none">VƯƠNG QUỐC SỐ NGUYÊN</h1>
                    <p class="text-xs text-slate-400 hidden sm:block">Ôn tập Toán 6 - Cuộc phiêu lưu kỳ thú</p>
                </div>
            </div>

            <!-- Chỉ Số Game (Điểm, Mạng, Âm thanh) -->
            <div class="flex items-center gap-4 bg-slate-900/80 px-4 py-2 rounded-xl border border-slate-700">
                <!-- Tim / Mạng -->
                <div class="flex items-center gap-1.5" title="Số mạng còn lại">
                    <i class="fa-solid fa-heart text-red-500 text-lg animate-pulse"></i>
                    <span id="livesText" class="font-bold text-lg text-red-400">3</span>
                </div>
                <div class="h-5 w-px bg-slate-700"></div>
                <!-- Điểm số -->
                <div class="flex items-center gap-1.5" title="Điểm số">
                    <i class="fa-solid fa-star text-amber-400 text-lg"></i>
                    <span id="scoreText" class="font-bold text-lg text-amber-300">0</span>
                </div>
                <div class="h-5 w-px bg-slate-700"></div>
                <!-- Bật/Tắt Âm Thanh -->
                <button id="toggleAudioBtn" onclick="toggleAudio()" class="text-slate-400 hover:text-white transition" title="Bật/Tắt Âm Thanh">
                    <i id="audioIcon" class="fa-solid fa-volume-high text-lg"></i>
                </button>
            </div>
        </div>
    </header>

    <!-- MAIN CONTENT CONTAINER -->
    <main class="flex-grow max-w-5xl w-full mx-auto p-4 flex flex-col justify-center relative">

        <!-- SCREEN 1: BẮT ĐẦU (START SCREEN) -->
        <section id="startScreen" class="text-center py-8 flex flex-col items-center">
            <div class="relative mb-6">
                <div class="w-28 h-28 sm:w-36 sm:h-36 bg-gradient-to-br from-amber-400 to-orange-600 rounded-2xl flex items-center justify-center text-6xl text-slate-900 shadow-2xl floating border-4 border-amber-300">
                    <i class="fa-solid fa-shield-halved"></i>
                </div>
                <span class="absolute -bottom-2 -right-2 bg-indigo-600 text-white px-3 py-1 rounded-full text-xs font-bold border-2 border-slate-900">Lớp 6</span>
            </div>

            <h2 class="text-3xl sm:text-5xl font-black mb-3 text-transparent bg-clip-text bg-gradient-to-r from-amber-300 via-orange-400 to-yellow-200">
                HIỆP SĨ SỐ NGUYÊN
            </h2>
            <p class="text-slate-300 max-w-xl text-sm sm:text-base mb-8 px-4 leading-relaxed">
                Vương quốc Số Nguyên đang bị Ma Vương Âm Độc xâm chiếm! Hãy vượt qua 5 vùng đất thử thách, vận dụng kiến thức về <span class="text-amber-400 font-bold">Số âm, Phép tính, Quy tắc dấu, Ước & Bội</span> để giải cứu công chúa và đem lại bình yên cho vương quốc!
            </p>

            <div class="flex flex-col sm:flex-row gap-4 w-full max-w-md">
                <button onclick="startGame()" class="flex-1 bg-gradient-to-r from-emerald-500 to-green-600 hover:from-emerald-400 hover:to-green-500 text-white font-extrabold py-4 px-6 rounded-xl border-b-4 border-green-800 text-xl tracking-wide pixel-btn flex items-center justify-center gap-2">
                    <i class="fa-solid fa-play"></i> BẮT ĐẦU CHƠI
                </button>
                <button onclick="showInstructions()" class="bg-slate-700 hover:bg-slate-600 text-slate-200 font-bold py-4 px-6 rounded-xl border-b-4 border-slate-900 text-lg pixel-btn">
                    <i class="fa-solid fa-circle-info"></i> TẬP HUYỆN
                </button>
            </div>
        </section>

        <!-- SCREEN 2: BẢN ĐỒ CỬA ẢI (MAP SCREEN) -->
        <section id="mapScreen" class="hidden py-4">
            <div class="bg-slate-800/90 rounded-2xl p-4 sm:p-6 border-2 border-slate-700 shadow-2xl relative overflow-hidden">
                <!-- Background Map Deco -->
                <div class="absolute inset-0 opacity-10 bg-[radial-gradient(#38bdf8_1px,transparent_1px)] [background-size:16px_16px]"></div>

                <div class="relative z-10">
                    <div class="flex justify-between items-center mb-6">
                        <div>
                            <h3 class="text-2xl font-bold text-amber-400 flex items-center gap-2">
                                <i class="fa-solid fa-map-location-dot"></i> BẢN ĐỒ PHIÊU LƯU
                            </h3>
                            <p class="text-xs text-slate-400">Chọn cửa ải khả dụng để tiến vào thử thách</p>
                        </div>
                        <div id="progressBadge" class="bg-indigo-900/80 text-indigo-200 text-xs font-bold px-3 py-1.5 rounded-full border border-indigo-700">
                            Tiến độ: 0/5 Cửa
                        </div>
                    </div>

                    <!-- Danh sách các Ải / Stage Cards -->
                    <div id="stagesContainer" class="grid grid-cols-1 md:grid-cols-5 gap-4">
                        <!-- JS Render Stages Here -->
                    </div>
                </div>
            </div>
        </section>

        <!-- SCREEN 3: MÀN CHƠI / CÂU HỎI (GAMEPLAY SCREEN) -->
        <section id="gameplayScreen" class="hidden py-4">
            <div class="bg-slate-800 border-2 border-slate-700 rounded-2xl p-4 sm:p-6 shadow-2xl relative">
                
                <!-- Stage Header -->
                <div class="flex flex-wrap justify-between items-center gap-2 mb-4 pb-3 border-b border-slate-700">
                    <div class="flex items-center gap-3">
                        <span id="stageBadgeIcon" class="w-10 h-10 rounded-xl bg-indigo-600 flex items-center justify-center text-xl text-white">
                            <i class="fa-solid fa-mountain"></i>
                        </span>
                        <div>
                            <h4 id="stageTitle" class="font-bold text-lg text-amber-300">Tên Cửa Ải</h4>
                            <p id="questionProgress" class="text-xs text-slate-400">Câu 1/3</p>
                        </div>
                    </div>
                    <button onclick="backToMap()" class="text-xs bg-slate-700 hover:bg-slate-600 text-slate-300 px-3 py-2 rounded-lg font-bold border border-slate-600 transition">
                        <i class="fa-solid fa-map"></i> Về bản đồ
                    </button>
                </div>

                <!-- Trục số minh họa (Hiển thị nếu dạng bài Trục Số) -->
                <div id="numberLineContainer" class="hidden mb-6 p-4 bg-slate-900/90 rounded-xl border border-slate-700 overflow-x-auto">
                    <p class="text-xs text-center text-slate-400 mb-2">Mô phỏng trục số nguyên:</p>
                    <div class="min-w-[480px] flex justify-between items-center relative py-4 px-2 border-b-2 border-amber-400">
                        <!-- Render dynamic number line -->
                        <div id="numberLineContent" class="w-full flex justify-between items-center text-xs font-bold font-mono"></div>
                    </div>
                </div>

                <!-- Đề Bài / Câu Hỏi -->
                <div class="mb-6 bg-slate-900/80 p-5 rounded-xl border border-slate-700">
                    <div id="questionCategory" class="inline-block bg-indigo-600/30 text-indigo-300 text-xs font-bold px-2.5 py-1 rounded-md mb-2 border border-indigo-500/30">
                        Dạng bài
                    </div>
                    <h3 id="questionText" class="text-lg sm:text-xl font-bold text-slate-100 leading-relaxed">
                        Nội dung câu hỏi sẽ ở đây...
                    </h3>
                </div>

                <!-- Các Lựa Chọn Đáp Án (Multiple Choice Options) -->
                <div id="optionsContainer" class="grid grid-cols-1 sm:grid-cols-2 gap-3 mb-6">
                    <!-- Dynamic Options -->
                </div>

                <!-- Khung Phản Hồi / Lời Giải (Feedback Panel) -->
                <div id="feedbackPanel" class="hidden p-4 rounded-xl border transition-all duration-300">
                    <div class="flex items-start gap-3">
                        <div id="feedbackIcon" class="text-2xl mt-0.5"></div>
                        <div class="flex-1">
                            <h5 id="feedbackTitle" class="font-bold text-base mb-1">Đúng rồi!</h5>
                            <p id="feedbackExplanation" class="text-sm text-slate-300 leading-relaxed">Lời giải thích chi tiết...</p>
                        </div>
                    </div>
                    <div class="mt-3 flex justify-end">
                        <button id="nextQuestionBtn" onclick="nextQuestion()" class="bg-amber-500 hover:bg-amber-400 text-slate-900 font-extrabold px-5 py-2 rounded-lg text-sm pixel-btn">
                            TIẾP TỤC <i class="fa-solid fa-arrow-right ml-1"></i>
                        </button>
                    </div>
                </div>

            </div>
        </section>

        <!-- SCREEN 4: KẾT THÚC GAME / CHIẾN THẮNG (VICTORY / GAMEOVER SCREEN) -->
        <section id="resultScreen" class="hidden py-6 text-center">
            <div class="bg-slate-800 border-2 border-slate-700 rounded-2xl p-6 sm:p-8 max-w-lg mx-auto shadow-2xl relative overflow-hidden">
                <div id="resultIconBox" class="w-24 h-24 mx-auto rounded-full bg-amber-500/20 text-amber-400 flex items-center justify-center text-5xl mb-4 border-2 border-amber-400">
                    <i id="resultIcon" class="fa-solid fa-trophy"></i>
                </div>

                <h2 id="resultTitle" class="text-3xl font-black text-amber-400 mb-2">CHIẾN THẮNG HOÀN HẢO!</h2>
                <p id="resultSubtitle" class="text-slate-300 text-sm mb-6">Bạn đã giải cứu thành công Vương Quốc Số Nguyên!</p>

                <!-- Bảng Thống Kê Kết Quả -->
                <div class="bg-slate-900/80 rounded-xl p-4 border border-slate-700 text-left mb-6 space-y-3">
                    <div class="flex justify-between items-center text-sm">
                        <span class="text-slate-400"><i class="fa-solid fa-star text-amber-400 w-5"></i> Tổng điểm số:</span>
                        <span id="finalScore" class="font-bold text-amber-300 text-lg">0</span>
                    </div>
                    <div class="flex justify-between items-center text-sm">
                        <span class="text-slate-400"><i class="fa-solid fa-heart text-red-400 w-5"></i> Mạng còn lại:</span>
                        <span id="finalLives" class="font-bold text-red-400 text-lg">3</span>
                    </div>
                    <div class="flex justify-between items-center text-sm">
                        <span class="text-slate-400"><i class="fa-solid fa-medal text-indigo-400 w-5"></i> Danh hiệu đạt được:</span>
                        <span id="finalBadge" class="font-bold text-indigo-300">Đại Hiệp Sĩ Số Nguyên</span>
                    </div>
                </div>

                <!-- Bằng Khen / Certificate Display -->
                <div id="certificateBox" class="p-4 bg-amber-100/10 border-2 border-dashed border-amber-400/50 rounded-xl mb-6 text-slate-200 text-xs leading-relaxed italic">
                    "Chứng nhận Dũng sĩ Lớp 6 đã làm chủ kiến thức Tập hợp Số nguyên, Phép cộng trừ nhân chia và Quy tắc dấu xuất sắc!"
                </div>

                <div class="flex gap-3">
                    <button onclick="restartGame()" class="flex-1 bg-gradient-to-r from-indigo-600 to-blue-600 hover:from-indigo-500 hover:to-blue-500 text-white font-bold py-3 px-4 rounded-xl border-b-4 border-indigo-900 pixel-btn">
                        <i class="fa-solid fa-rotate-right"></i> CHƠI LẠI
                    </button>
                    <button onclick="backToMap()" id="resultBackMapBtn" class="flex-1 bg-slate-700 hover:bg-slate-600 text-slate-200 font-bold py-3 px-4 rounded-xl border-b-4 border-slate-900 pixel-btn">
                        <i class="fa-solid fa-map"></i> BẢN ĐỒ
                    </button>
                </div>
            </div>
        </section>

    </main>

    <!-- FOOTER BANNER -->
    <footer class="bg-slate-950 border-t border-slate-800 p-3 text-center text-xs text-slate-500">
        <p>Game Ôn Tập Toán 6 - Chủ Đề Số Nguyên &bull; Thiết kế Tương Tác Trực Quan</p>
    </footer>

    <!-- MODAL HUYỆN TẬP / HƯỚNG DẪN -->
    <div id="instructionModal" class="fixed inset-0 bg-black/80 z-50 hidden flex items-center justify-center p-4">
        <div class="bg-slate-800 border-2 border-slate-700 rounded-2xl max-w-lg w-full p-6 shadow-2xl relative">
            <button onclick="closeInstructions()" class="absolute top-4 right-4 text-slate-400 hover:text-white text-xl">
                <i class="fa-solid fa-xmark"></i>
            </button>
            <h3 class="text-xl font-bold text-amber-400 mb-4 flex items-center gap-2">
                <i class="fa-solid fa-book-open"></i> BÍ KÍP CHINH PHỤC SỐ NGUYÊN
            </h3>
            <div class="space-y-3 text-xs sm:text-sm text-slate-300 max-h-96 overflow-y-auto pr-2">
                <div class="bg-slate-900/60 p-3 rounded-lg border border-slate-700">
                    <span class="font-bold text-amber-300">1. Quy tắc dấu phép cộng:</span>
                    <p>• Cùng dấu: Cộng 2 giá trị tuyệt đối, giữ nguyên dấu.</p>
                    <p>• Khác dấu: Lấy số lớn hơn trừ số nhỏ hơn (về giá trị tuyệt đối), mang dấu của số có giá trị tuyệt đối lớn hơn.</p>
                </div>
                <div class="bg-slate-900/60 p-3 rounded-lg border border-slate-700">
                    <span class="font-bold text-amber-300">2. Quy tắc dấu phép nhân & chia:</span>
                    <p>• (+).(+) = (+), (-).(-) = (+)</p>
                    <p>• (+).(-) = (-), (-).(+) = (-)</p>
                </div>
                <div class="bg-slate-900/60 p-3 rounded-lg border border-slate-700">
                    <span class="font-bold text-amber-300">3. Quy tắc phá ngoặc:</span>
                    <p>• Trước ngoặc là dấu (+): Giữ nguyên dấu các số hạng trong ngoặc.</p>
                    <p>• Trước ngoặc là dấu (-): Đổi dấu tất cả số hạng trong ngoặc (+ thành -, - thành +).</p>
                </div>
            </div>
            <div class="mt-5 text-right">
                <button onclick="closeInstructions()" class="bg-amber-500 hover:bg-amber-400 text-slate-900 font-bold px-4 py-2 rounded-lg text-sm">
                    Đã Hiểu!
                </button>
            </div>
        </div>
    </div>

    <!-- GAME LOGIC SCRIPT -->
    <script>
        /* ==========================================================================
           1. ÂM THANH (WEB AUDIO API - Không xài file mp3 ngoài)
           ========================================================================== */
        class SoundFX {
            constructor() {
                this.ctx = null;
                this.enabled = true;
            }
            init() {
                if (!this.ctx) {
                    this.ctx = new (window.AudioContext || window.webkitAudioContext)();
                }
            }
            playTone(freq, type, duration, delay = 0) {
                if (!this.enabled) return;
                this.init();
                setTimeout(() => {
                    try {
                        const osc = this.ctx.createOscillator();
                        const gain = this.ctx.createGain();
                        osc.type = type;
                        osc.frequency.setValueAtTime(freq, this.ctx.currentTime);
                        gain.gain.setValueAtTime(0.15, this.ctx.currentTime);
                        gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + duration);
                        osc.connect(gain);
                        gain.connect(this.ctx.destination);
                        osc.start();
                        osc.stop(this.ctx.currentTime + duration);
                    } catch(e){}
                }, delay);
            }
            playCorrect() {
                this.playTone(523.25, 'triangle', 0.15, 0);   // C5
                this.playTone(659.25, 'triangle', 0.15, 120); // E5
                this.playTone(783.99, 'triangle', 0.25, 240); // G5
            }
            playWrong() {
                this.playTone(220, 'sawtooth', 0.2, 0);   // A3
                this.playTone(180, 'sawtooth', 0.3, 150); // F3
            }
            playClick() {
                this.playTone(400, 'sine', 0.05, 0);
            }
            playVictory() {
                this.playTone(523.25, 'square', 0.2, 0);
                this.playTone(659.25, 'square', 0.2, 150);
                this.playTone(783.99, 'square', 0.2, 300);
                this.playTone(1046.50, 'square', 0.4, 450);
            }
        }
        const sfx = new SoundFX();

        function toggleAudio() {
            sfx.enabled = !sfx.enabled;
            const icon = document.getElementById('audioIcon');
            if (sfx.enabled) {
                icon.className = "fa-solid fa-volume-high text-lg";
            } else {
                icon.className = "fa-solid fa-volume-xmark text-lg text-red-400";
            }
        }

        /* ==========================================================================
           2. DỮ LIỆU CỬA ẢI VÀ CÂU HỎI (STAGE & QUESTION DATABASE)
           ========================================================================== */
        const STAGES = [
            {
                id: 1,
                name: "Đảo Số Âm",
                desc: "So sánh, biểu diễn trục số & Giá trị tuyệt đối",
                icon: "fa-island-tropical",
                color: "from-teal-500 to-emerald-700",
                badgeColor: "bg-teal-600",
                questions: [
                    {
                        category: "Tập hợp Số Nguyên",
                        text: "Trong các khẳng định sau, khẳng định nào ĐÚNG?",
                        options: [
                            "-5 > -2",
                            "-8 < -3",
                            "0 < -7",
                            "-10 > 0"
                        ],
                        correct: 1,
                        explanation: "Trên trục số nằm ngang, số bên trái nhỏ hơn số bên phải. Vì -8 nằm bên trái -3 nên -8 < -3."
                    },
                    {
                        category: "Số Đối & Giá Trị Tuyệt Đối",
                        text: "Số đối của |-15| là bao nhiêu?",
                        options: [
                            "15",
                            "-15",
                            "0",
                            "Không xác định"
                        ],
                        correct: 1,
                        explanation: "Ta có |-15| = 15. Do đó số đối của |-15| chính là -15."
                    },
                    {
                        category: "Biểu diễn Trục Số",
                        type: "numberline",
                        numberLineVal: -4,
                        text: "Điểm A trên trục số biểu diễn số nguyên nào nếu nó nằm bên trái gốc 0 đúng 4 đơn vị?",
                        options: [
                            "4",
                            "-4",
                            "-8",
                            "0"
                        ],
                        correct: 1,
                        explanation: "Nằm bên trái gốc 0 đúng 4 đơn vị biểu diễn số nguyên âm -4."
                    }
                ]
            },
            {
                id: 2,
                name: "Thung Lũng Cộng Trừ",
                desc: "Thực hiện phép cộng & trừ các số nguyên",
                icon: "fa-hill-rockslide",
                color: "from-blue-500 to-indigo-700",
                badgeColor: "bg-blue-600",
                questions: [
                    {
                        category: "Cộng Số Nguyên Khác Dấu",
                        text: "Tính giá trị của biểu thức: A = 18 + (-25)",
                        options: [
                            "43",
                            "-43",
                            "-7",
                            "7"
                        ],
                        correct: 2,
                        explanation: "Cộng hai số nguyên khác dấu: Lấy 25 - 18 = 7, mang dấu của -25 nên kết quả là -7."
                    },
                    {
                        category: "Trừ Số Nguyên",
                        text: "Kết quả của phép tính: (-12) - (-20) là:",
                        options: [
                            "-32",
                            "8",
                            "-8",
                            "32"
                        ],
                        correct: 1,
                        explanation: "(-12) - (-20) = (-12) + 20 = 20 - 12 = 8."
                    },
                    {
                        category: "Bài Toán Thực Tế",
                        text: "Nhiệt độ lúc 6 giờ sáng tại Sa Pa là -2°C, đến 12 giờ trưa nhiệt độ tăng thêm 5°C. Hỏi nhiệt độ lúc 12 giờ trưa là bao nhiêu?",
                        options: [
                            "3°C",
                            "-7°C",
                            "7°C",
                            "-3°C"
                        ],
                        correct: 0,
                        explanation: "Nhiệt độ lúc 12 giờ trưa là: (-2) + 5 = 3°C."
                    }
                ]
            },
            {
                id: 3,
                name: "Hang Động Nhân Chia",
                desc: "Phép nhân, chia & Quy tắc dấu",
                icon: "fa-dungeon",
                color: "from-purple-600 to-pink-800",
                badgeColor: "bg-purple-600",
                questions: [
                    {
                        category: "Phép Nhân Số Nguyên",
                        text: "Tính giá trị của P = (-4) . (-5) . (-2)",
                        options: [
                            "40",
                            "-40",
                            "20",
                            "-20"
                        ],
                        correct: 1,
                        explanation: "Tích của một số lẻ các thừa số nguyên âm là một số nguyên âm: (-4).(-5).(-2) = 20 . (-2) = -40."
                    },
                    {
                        category: "Phép Chia Số Nguyên",
                        text: "Tìm x biết: (-36) : x = 9",
                        options: [
                            "x = 4",
                            "x = -4",
                            "x = -27",
                            "x = 27"
                        ],
                        correct: 1,
                        explanation: "x = (-36) : 9 = -4."
                    },
                    {
                        category: "Tính Chất Phân Phối",
                        text: "Tính nhanh: S = (-15) . 37 + (-15) . 63",
                        options: [
                            "-1500",
                            "1500",
                            "-150",
                            "0"
                        ],
                        correct: 0,
                        explanation: "S = (-15) . (37 + 63) = (-15) . 100 = -1500."
                    }
                ]
            },
            {
                id: 4,
                name: "Lâu Đài Thứ Tự Phép Tính",
                desc: "Quy tắc bỏ ngoặc, chuyển tế & Thứ tự tính",
                icon: "fa-castle",
                color: "from-amber-600 to-red-800",
                badgeColor: "bg-amber-600",
                questions: [
                    {
                        category: "Quy Tắc Bỏ Ngoặc",
                        text: "Bỏ dấu ngoặc của biểu thức: A = 10 - (x - 5 + y)",
                        options: [
                            "10 - x - 5 + y",
                            "10 - x + 5 - y",
                            "10 - x - 5 - y",
                            "10 + x - 5 + y"
                        ],
                        correct: 1,
                        explanation: "Khi bỏ dấu ngoặc có dấu '-' đằng trước, ta đổi dấu tất cả số hạng: -(x - 5 + y) = -x + 5 - y."
                    },
                    {
                        category: "Chuyển Tế Tìm X",
                        text: "Tìm x nguyên biết: x + 15 = -5",
                        options: [
                            "x = 10",
                            "x = 20",
                            "x = -20",
                            "x = -10"
                        ],
                        correct: 2,
                        explanation: "Chuyển 15 sang vế phải thành -15: x = -5 - 15 => x = -20."
                    },
                    {
                        category: "Thứ Tự Thực Hiện Phép Tính",
                        text: "Giá trị của B = 50 - [30 - (8 - 2)³ : 12] là:",
                        options: [
                            "32",
                            "38",
                            "20",
                            "42"
                        ],
                        correct: 1,
                        explanation: "Tính trong ngoặc tròn: (8-2)=6 => 6³=216. Trong ngoặc vuông: 30 - 216:12 = 30 - 18 = 12. Kết quả: 50 - 12 = 38."
                    }
                ]
            },
            {
                id: 5,
                name: "Rương Kho Báu Tổng Hợp",
                desc: "Ước & Bội số nguyên & Toán ứng dụng",
                icon: "fa-chest-treasure",
                color: "from-yellow-500 to-amber-700",
                badgeColor: "bg-yellow-600",
                questions: [
                    {
                        category: "Ước Và Bội Số Nguyên",
                        text: "Tập hợp tất cả các ước nguyên của 6 là:",
                        options: [
                            "{1; 2; 3; 6}",
                            "{-6; -3; -2; -1}",
                            "{-6; -3; -2; -1; 1; 2; 3; 6}",
                            "{0; 6; 12; 18}"
                        ],
                        correct: 2,
                        explanation: "Ước của 6 bao gồm cả các số nguyên dương và số nguyên âm tương ứng: ±1, ±2, ±3, ±6."
                    },
                    {
                        category: "Toán Thực Tế Tài Chính",
                        text: "Tài khoản ngân hàng của cô Lan có -200.000 đồng (nợ). Sau khi cô gửi thêm 1.500.000 đồng, số dư mới là bao nhiêu?",
                        options: [
                            "1.700.000 đồng",
                            "1.300.000 đồng",
                            "-1.300.000 đồng",
                            "1.500.000 đồng"
                        ],
                        correct: 1,
                        explanation: "Số dư mới = (-200.000) + 1.500.000 = 1.300.000 đồng."
                    },
                    {
                        category: "Ứng Dụng Độ Cao",
                        text: "Một thợ lặn ở độ cao -15m so với mực nước biển. Người đó bơi lặn sâu xuống thêm 8m nữa. Hỏi thợ lặn đang ở độ cao bao nhiêu?",
                        options: [
                            "-7m",
                            "-23m",
                            "23m",
                            "-12m"
                        ],
                        correct: 1,
                        explanation: "Lặn sâu thêm 8m tương ứng với -8m. Độ cao mới: (-15) + (-8) = -23m."
                    }
                ]
            }
        ];

        /* ==========================================================================
           3. TRẠNG THÁI GAME (GAME STATE)
           ========================================================================== */
        let gameState = {
            lives: 3,
            score: 0,
            unlockedStage: 1,
            completedStages: [],
            currentStageIdx: 0,
            currentQuestionIdx: 0,
            stageScores: {}
        };

        /* ==========================================================================
           4. ĐIỀU KHIỂN GIAO DIỆN & LUỒNG GAME
           ========================================================================== */
        function startGame() {
            sfx.playClick();
            document.getElementById('startScreen').classList.add('hidden');
            document.getElementById('mapScreen').classList.remove('hidden');
            renderMap();
        }

        function showInstructions() {
            sfx.playClick();
            document.getElementById('instructionModal').classList.remove('hidden');
        }

        function closeInstructions() {
            sfx.playClick();
            document.getElementById('instructionModal').classList.add('hidden');
        }

        function renderMap() {
            const container = document.getElementById('stagesContainer');
            container.innerHTML = '';

            let completedCount = gameState.completedStages.length;
            document.getElementById('progressBadge').innerText = `Tiến độ: ${completedCount}/5 Cửa`;

            STAGES.forEach((stage, idx) => {
                const isUnlocked = stage.id <= gameState.unlockedStage;
                const isCompleted = gameState.completedStages.includes(stage.id);

                const card = document.createElement('div');
                card.className = `pixel-box rounded-xl p-4 flex flex-col justify-between relative transition-all ${
                    isUnlocked 
                        ? 'bg-slate-800 hover:border-amber-400 cursor-pointer transform hover:-translate-y-1' 
                        : 'bg-slate-900 opacity-60 cursor-not-allowed border-slate-700'
                }`;

                card.onclick = () => {
                    if (isUnlocked) enterStage(idx);
                };

                card.innerHTML = `
                    <div class="flex items-center justify-between mb-3">
                        <div class="w-10 h-10 rounded-lg bg-gradient-to-br ${stage.color} flex items-center justify-center text-white text-xl shadow">
                            <i class="fa-solid ${stage.icon}"></i>
                        </div>
                        ${isCompleted 
                            ? '<span class="bg-green-500/20 text-green-400 text-xs px-2 py-1 rounded font-bold border border-green-500/30"><i class="fa-solid fa-check"></i> Xong</span>' 
                            : (isUnlocked 
                                ? '<span class="bg-amber-500/20 text-amber-300 text-xs px-2 py-1 rounded font-bold border border-amber-500/30">Mở</span>' 
                                : '<span class="bg-slate-700 text-slate-400 text-xs px-2 py-1 rounded font-bold"><i class="fa-solid fa-lock"></i> Khóa</span>')}
                    </div>
                    <div>
                        <span class="text-[10px] uppercase font-black text-slate-400">Cửa Ải 0${stage.id}</span>
                        <h4 class="font-bold text-slate-100 text-base mb-1">${stage.name}</h4>
                        <p class="text-xs text-slate-400 leading-snug">${stage.desc}</p>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        function enterStage(stageIdx) {
            sfx.playClick();
            gameState.currentStageIdx = stageIdx;
            gameState.currentQuestionIdx = 0;

            document.getElementById('mapScreen').classList.add('hidden');
            document.getElementById('gameplayScreen').classList.remove('hidden');

            loadQuestion();
        }

        function loadQuestion() {
            const stage = STAGES[gameState.currentStageIdx];
            const q = stage.questions[gameState.currentQuestionIdx];

            // Reset UI State
            document.getElementById('feedbackPanel').classList.add('hidden');
            
            // Header Infos
            document.getElementById('stageTitle').innerText = `${stage.name}`;
            document.getElementById('questionProgress').innerText = `Câu ${gameState.currentQuestionIdx + 1}/${stage.questions.length}`;
            document.getElementById('stageBadgeIcon').className = `w-10 h-10 rounded-xl ${stage.badgeColor} flex items-center justify-center text-xl text-white`;
            document.getElementById('stageBadgeIcon').innerHTML = `<i class="fa-solid ${stage.icon}"></i>`;

            // Question Details
            document.getElementById('questionCategory').innerText = q.category;
            document.getElementById('questionText').innerText = q.text;

            // Render Trục Số nếu là Dạng Trục Số
            const numLineBox = document.getElementById('numberLineContainer');
            if (q.type === 'numberline') {
                numLineBox.classList.remove('hidden');
                renderNumberLine(q.numberLineVal);
            } else {
                numLineBox.classList.add('hidden');
            }

            // Render Options
            const optionsBox = document.getElementById('optionsContainer');
            optionsBox.innerHTML = '';

            q.options.forEach((optText, optIdx) => {
                const btn = document.createElement('button');
                btn.className = `w-full text-left bg-slate-700/80 hover:bg-slate-600 border-2 border-slate-600 hover:border-amber-400 p-4 rounded-xl font-bold text-sm sm:text-base transition pixel-btn flex items-center gap-3 text-slate-100`;
                btn.onclick = () => checkAnswer(optIdx);

                const label = String.fromCharCode(65 + optIdx); // A, B, C, D
                btn.innerHTML = `
                    <span class="w-8 h-8 rounded-lg bg-slate-800 flex items-center justify-center text-amber-400 font-black border border-slate-600 text-sm">${label}</span>
                    <span class="flex-1">${optText}</span>
                `;
                optionsBox.appendChild(btn);
            });
        }

        function renderNumberLine(targetVal) {
            const lineContent = document.getElementById('numberLineContent');
            lineContent.innerHTML = '';

            // Render range -5 to 5
            for (let i = -5; i <= 5; i++) {
                const item = document.createElement('div');
                item.className = "flex flex-col items-center relative";
                
                const isTarget = (i === targetVal);
                item.innerHTML = `
                    ${isTarget ? '<div class="absolute -top-6 text-amber-400 text-base animate-bounce"><i class="fa-solid fa-location-dot"></i> A</div>' : ''}
                    <div class="h-3 w-0.5 ${i === 0 ? 'bg-red-500 h-5' : 'bg-slate-400'}"></div>
                    <span class="mt-1 ${i === 0 ? 'text-red-400 font-bold' : (isTarget ? 'text-amber-300 font-bold' : 'text-slate-400')}">${i}</span>
                `;
                lineContent.appendChild(item);
            }
        }

        function checkAnswer(selectedIdx) {
            const stage = STAGES[gameState.currentStageIdx];
            const q = stage.questions[gameState.currentQuestionIdx];
            const isCorrect = (selectedIdx === q.correct);

            // Disable all option buttons
            const optionBtns = document.getElementById('optionsContainer').querySelectorAll('button');
            optionBtns.forEach((btn, idx) => {
                btn.disabled = true;
                btn.classList.remove('hover:border-amber-400', 'hover:bg-slate-600');
                if (idx === q.correct) {
                    btn.classList.add('bg-emerald-800/80', 'border-emerald-500', 'text-emerald-200');
                } else if (idx === selectedIdx && !isCorrect) {
                    btn.classList.add('bg-red-900/80', 'border-red-500', 'text-red-200');
                }
            });

            const feedbackPanel = document.getElementById('feedbackPanel');
            const feedbackIcon = document.getElementById('feedbackIcon');
            const feedbackTitle = document.getElementById('feedbackTitle');
            const feedbackExplanation = document.getElementById('feedbackExplanation');

            feedbackPanel.classList.remove('hidden', 'bg-emerald-950/80', 'border-emerald-700', 'bg-red-950/80', 'border-red-700');

            if (isCorrect) {
                sfx.playCorrect();
                gameState.score += 100;
                updateStatsUI();

                feedbackPanel.classList.add('bg-emerald-950/80', 'border-emerald-700');
                feedbackIcon.innerHTML = '<i class="fa-solid fa-circle-check text-emerald-400"></i>';
                feedbackTitle.innerText = "CHÍNH XÁC! (+100 ĐIỂM)";
                feedbackTitle.className = "font-bold text-base text-emerald-400 mb-1";
            } else {
                sfx.playWrong();
                gameState.lives--;
                updateStatsUI();

                feedbackPanel.classList.add('bg-red-950/80', 'border-red-700');
                feedbackIcon.innerHTML = '<i class="fa-solid fa-circle-xmark text-red-400"></i>';
                feedbackTitle.innerText = "CHƯA ĐÚNG! (-1 MẠNG)";
                feedbackTitle.className = "font-bold text-base text-red-400 mb-1";

                if (gameState.lives <= 0) {
                    setTimeout(() => {
                        showGameOverScreen();
                    }, 1200);
                    return;
                }
            }

            feedbackExplanation.innerText = q.explanation;
        }

        function nextQuestion() {
            sfx.playClick();
            const stage = STAGES[gameState.currentStageIdx];
            gameState.currentQuestionIdx++;

            if (gameState.currentQuestionIdx < stage.questions.length) {
                loadQuestion();
            } else {
                // Hoàn thành Cửa Ải Hiện Tại
                if (!gameState.completedStages.includes(stage.id)) {
                    gameState.completedStages.push(stage.id);
                }
                if (gameState.unlockedStage <= stage.id && gameState.unlockedStage < STAGES.length) {
                    gameState.unlockedStage = stage.id + 1;
                }

                if (gameState.completedStages.length === STAGES.length) {
                    // Thắng toàn bộ game
                    showVictoryScreen();
                } else {
                    // Trở về bản đồ để chọn cửa tiếp theo
                    backToMap();
                }
            }
        }

        function backToMap() {
            sfx.playClick();
            document.getElementById('gameplayScreen').classList.add('hidden');
            document.getElementById('resultScreen').classList.add('hidden');
            document.getElementById('mapScreen').classList.remove('hidden');
            renderMap();
        }

        function updateStatsUI() {
            document.getElementById('livesText').innerText = gameState.lives;
            document.getElementById('scoreText').innerText = gameState.score;
        }

        function showVictoryScreen() {
            sfx.playVictory();
            startFireworks();

            document.getElementById('gameplayScreen').classList.add('hidden');
            document.getElementById('resultScreen').classList.remove('hidden');

            document.getElementById('resultIconBox').className = "w-24 h-24 mx-auto rounded-full bg-amber-500/20 text-amber-400 flex items-center justify-center text-5xl mb-4 border-2 border-amber-400 floating";
            document.getElementById('resultIcon').className = "fa-solid fa-trophy";
            document.getElementById('resultTitle').innerText = "VƯƠNG QUỐC ĐƯỢC GIẢI CỨU!";
            document.getElementById('resultSubtitle').innerText = "Bạn đã hoàn thành xuất sắc tất cả 5 cửa ải thử thách!";

            document.getElementById('finalScore').innerText = gameState.score;
            document.getElementById('finalLives').innerText = gameState.lives;
            document.getElementById('finalBadge').innerText = "Đại Hiệp Sĩ Số Nguyên 🏆";
        }

        function showGameOverScreen() {
            document.getElementById('gameplayScreen').classList.add('hidden');
            document.getElementById('resultScreen').classList.remove('hidden');

            document.getElementById('resultIconBox').className = "w-24 h-24 mx-auto rounded-full bg-red-500/20 text-red-400 flex items-center justify-center text-5xl mb-4 border-2 border-red-500";
            document.getElementById('resultIcon').className = "fa-solid fa-skull-crossbones";
            document.getElementById('resultTitle').innerText = "THỬ THÁCH THẤT BẠI!";
            document.getElementById('resultTitle').className = "text-3xl font-black text-red-400 mb-2";
            document.getElementById('resultSubtitle').innerText = "Bạn đã hết mạng sống. Hãy ôn lại lý thuyết và thử lại nhé!";

            document.getElementById('finalScore').innerText = gameState.score;
            document.getElementById('finalLives').innerText = "0";
            document.getElementById('finalBadge').innerText = "Tập Sự Hiệp Sĩ";
        }

        function restartGame() {
            sfx.playClick();
            gameState = {
                lives: 3,
                score: 0,
                unlockedStage: 1,
                completedStages: [],
                currentStageIdx: 0,
                currentQuestionIdx: 0,
                stageScores: {}
            };
            updateStatsUI();
            backToMap();
        }

        /* ==========================================================================
           5. HIỆU ỨNG PHÁO HOA / CELEBRATION FIREWORKS (CANVAS)
           ========================================================================== */
        const canvas = document.getElementById('fxCanvas');
        const ctx = canvas.getContext('2d');
        let particles = [];

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        function startFireworks() {
            particles = [];
            const colors = ['#f59e0b', '#10b981', '#3b82f6', '#ec4899', '#8b5cf6'];
            
            for (let i = 0; i < 120; i++) {
                particles.push({
                    x: canvas.width / 2,
                    y: canvas.height / 2,
                    vx: (Math.random() - 0.5) * 12,
                    vy: (Math.random() - 0.5) * 12 - 2,
                    size: Math.random() * 4 + 2,
                    color: colors[Math.floor(Math.random() * colors.length)],
                    alpha: 1,
                    decay: Math.random() * 0.02 + 0.015
                });
            }
            animateParticles();
        }

        function animateParticles() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            let active = false;

            particles.forEach(p => {
                if (p.alpha > 0) {
                    active = true;
                    p.x += p.vx;
                    p.y += p.vy;
                    p.vy += 0.1; // gravity
                    p.alpha -= p.decay;

                    ctx.save();
                    ctx.globalAlpha = Math.max(0, p.alpha);
                    ctx.fillStyle = p.color;
                    ctx.beginPath();
                    ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.restore();
                }
            });

            if (active) {
                requestAnimationFrame(animateParticles);
            }
        }
    </script>
</body>
</html># GAME-S-NGUY-N-6
