<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>한 해의 열매 카드 뽑기</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts: Gowun Dodum & Gamja Flower -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Gamja+Flower&family=Gowun+Dodum&display=swap" rel="stylesheet">

    <style>
        * {
            font-family: 'Gowun Dodum', sans-serif;
            user-select: none;
            -webkit-user-select: none;
        }

        .font-gamja {
            font-family: 'Gamja Flower', cursive;
        }

        /* Pink Gingham Pattern for Card Back */
        .bg-gingham {
            background-color: #fff0f3;
            background-image: 
                linear-gradient(90deg, rgba(255, 182, 193, 0.4) 50%, transparent 50%),
                linear-gradient(rgba(255, 182, 193, 0.4) 50%, transparent 50%);
            background-size: 24px 24px;
        }

        /* 3D Card Flip Animation Styles */
        .perspective-1000 {
            perspective: 1000px;
        }
        .transform-style-3d {
            transform-style: preserve-3d;
        }
        .backface-hidden {
            backface-visibility: hidden;
            -webkit-backface-visibility: hidden;
        }
        .rotate-y-180 {
            transform: rotateY(180deg);
        }

        /* Floating Gentle Effect */
        @keyframes float {
            0%, 100% { transform: translateY(0px) rotate(0deg); }
            50% { transform: translateY(-8px) rotate(1.5deg); }
        }
        .animate-float {
            animation: float 3.2s ease-in-out infinite;
        }

        /* Card Shuffling Animation */
        @keyframes shuffleShake {
            0% { transform: scale(1) rotate(0deg); }
            20% { transform: scale(1.05) rotate(-4deg) translateY(-5px); }
            40% { transform: scale(0.98) rotate(4deg) translateY(5px); }
            60% { transform: scale(1.06) rotate(-3deg) translateY(-6px); }
            80% { transform: scale(0.99) rotate(3deg) translateY(3px); }
            100% { transform: scale(1) rotate(0deg); }
        }
        .animate-shuffle {
            animation: shuffleShake 0.7s ease-in-out infinite;
        }

        /* Soft Glow Effect */
        @keyframes softGlow {
            0%, 100% { filter: drop-shadow(0 4px 12px rgba(255, 107, 129, 0.25)); }
            50% { filter: drop-shadow(0 8px 20px rgba(255, 107, 129, 0.45)); }
        }
        .animate-glow {
            animation: softGlow 2.5s infinite;
        }

        /* Sparkle Float Animation */
        @keyframes sparkleUp {
            0% { opacity: 0; transform: translateY(10px) scale(0.5); }
            50% { opacity: 1; transform: translateY(-10px) scale(1.2); }
            100% { opacity: 0; transform: translateY(-30px) scale(0.8); }
        }
        .sparkle-particle {
            position: absolute;
            pointer-events: none;
            animation: sparkleUp 1.8s ease-out infinite;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #fff0f3;
        }
        ::-webkit-scrollbar-thumb {
            background: #fbcfe8;
            border-radius: 999px;
        }
    </style>
</head>
<body class="bg-slate-100 min-h-screen flex items-center justify-center p-0 md:p-4 antialiased overflow-x-hidden">

    <!-- Mobile Outer Screen Frame -->
    <div class="w-full max-w-sm sm:max-w-md h-[100vh] sm:h-[840px] bg-emerald-50/40 rounded-none sm:rounded-[36px] shadow-2xl border-0 sm:border-8 border-white overflow-hidden relative flex flex-col justify-between select-none">
        
        <!-- Header Bar -->
        <header class="w-full px-5 pt-4 pb-2 flex items-center justify-between z-30 bg-transparent">
            <div class="flex items-center gap-1.5 bg-white/80 backdrop-blur-md px-3 py-1.5 rounded-full border border-pink-100 shadow-sm">
                <span class="text-pink-500 font-bold text-xs font-gamja text-sm">🍎 한 해의 열매</span>
            </div>
            <div class="flex items-center gap-2">
                <!-- Sound Toggle -->
                <button id="soundToggleBtn" onclick="toggleSound()" class="w-9 h-9 rounded-full bg-white/80 backdrop-blur-md border border-pink-100 flex items-center justify-center text-pink-400 hover:bg-pink-50 transition active:scale-95 shadow-sm" title="음향 온/오프">
                    <i id="soundIcon" class="fas fa-volume-up text-sm"></i>
                </button>
                <!-- Fruit Gallery Modal Trigger -->
                <button onclick="openGalleryModal()" class="w-9 h-9 rounded-full bg-white/80 backdrop-blur-md border border-pink-100 flex items-center justify-center text-pink-400 hover:bg-pink-50 transition active:scale-95 shadow-sm" title="열매 도감 보기">
                    <i class="fas fa-th-large text-sm"></i>
                </button>
            </div>
        </header>

        <!-- SCREEN 1: Main Landing Screen -->
        <section id="screen1" class="flex-1 flex flex-col items-center justify-between px-6 pb-6 pt-2 text-center transition-all duration-500 ease-in-out">
            <!-- Main Title Section -->
            <div class="mt-2 space-y-1">
                <p class="text-xs font-semibold text-emerald-600 bg-emerald-100/70 inline-block px-3 py-1 rounded-full">올해의 나를 돌아보는 질문</p>
                <h1 class="text-xl sm:text-2xl font-bold text-gray-800 leading-snug pt-1">
                    지금, <br>
                    <span class="text-pink-500">당신의 한 해</span>가 담긴<br>
                    열매 카드를 뽑아보세요!
                </h1>
                <div class="flex items-center justify-center gap-1 text-pink-300 text-xs pt-1">
                    <span>\</span><span>\</span><span>♥</span><span>/</span><span>/</span>
                </div>
            </div>

            <!-- Fruit Basket SVG Illustration -->
            <div class="relative my-auto w-full flex justify-center items-center py-4">
                <!-- Sparkle Accents -->
                <span class="absolute top-2 left-10 text-amber-300 text-lg animate-pulse">✦</span>
                <span class="absolute top-10 right-10 text-pink-400 text-sm animate-bounce">♥</span>
                <span class="absolute bottom-6 left-8 text-emerald-400 text-base">✨</span>
                
                <div class="w-64 h-64 relative flex items-center justify-center animate-float">
                    <svg viewBox="0 0 200 200" class="w-full h-full drop-shadow-lg">
                        <!-- Glow Background -->
                        <circle cx="100" cy="110" r="75" fill="#fef2f2" opacity="0.8"/>
                        
                        <!-- Grape (Purple) -->
                        <g transform="translate(52, 60)">
                            <circle cx="12" cy="12" r="10" fill="#a78bfa"/>
                            <circle cx="24" cy="12" r="10" fill="#8b5cf6"/>
                            <circle cx="18" cy="22" r="10" fill="#7c3aed"/>
                            <circle cx="18" cy="12" r="4" fill="#ddd6fe"/>
                            <circle cx="15" cy="18" r="1.5" fill="#2e1065"/>
                            <circle cx="21" cy="18" r="1.5" fill="#2e1065"/>
                        </g>
                        
                        <!-- Apple (Red) Center -->
                        <g transform="translate(85, 45)">
                            <circle cx="18" cy="22" r="20" fill="#ff5252"/>
                            <path d="M18 2 C 18 2, 22 -6, 26 2 Z" fill="#4ade80"/>
                            <circle cx="12" cy="18" r="2.5" fill="#ffe4e6"/>
                            <circle cx="12" cy="22" r="1.8" fill="#1a1a1a"/>
                            <circle cx="24" cy="22" r="1.8" fill="#1a1a1a"/>
                            <ellipse cx="9" cy="26" rx="2.5" ry="1.5" fill="#f43f5e" opacity="0.6"/>
                            <ellipse cx="27" cy="26" rx="2.5" ry="1.5" fill="#f43f5e" opacity="0.6"/>
                            <path d="M 16 26 Q 18 29 20 26" stroke="#1a1a1a" stroke-width="1.5" fill="none" stroke-linecap="round"/>
                        </g>

                        <!-- Peach (Pink) Right -->
                        <g transform="translate(118, 55)">
                            <path d="M20,5 C32,5 40,15 40,28 C40,40 28,48 20,48 C12,48 0,40 0,28 C0,15 8,5 20,5 Z" fill="#ffb7b2"/>
                            <path d="M20,5 Q 18,20 20,32" stroke="#ff8b94" stroke-width="1.5" fill="none" opacity="0.5"/>
                            <circle cx="14" cy="26" r="1.8" fill="#1a1a1a"/>
                            <circle cx="26" cy="26" r="1.8" fill="#1a1a1a"/>
                            <ellipse cx="10" cy="29" rx="2.5" ry="1.5" fill="#e11d48" opacity="0.4"/>
                            <ellipse cx="30" cy="29" rx="2.5" ry="1.5" fill="#e11d48" opacity="0.4"/>
                            <path d="M 18 30 Q 20 33 22 30" stroke="#1a1a1a" stroke-width="1.5" fill="none" stroke-linecap="round"/>
                        </g>

                        <!-- Tangerine (Orange) Front Left -->
                        <g transform="translate(58, 80)">
                            <circle cx="18" cy="18" r="18" fill="#fbbf24"/>
                            <path d="M18,0 Q 22,5 18,7" stroke="#15803d" stroke-width="2" fill="none"/>
                            <path d="M18,2 C 22,-2 26,2 24,5 Z" fill="#4ade80"/>
                            <circle cx="12" cy="18" r="1.8" fill="#1a1a1a"/>
                            <circle cx="24" cy="18" r="1.8" fill="#1a1a1a"/>
                            <ellipse cx="9" cy="22" rx="2.2" ry="1.2" fill="#d97706" opacity="0.6"/>
                            <ellipse cx="27" cy="22" rx="2.2" ry="1.2" fill="#d97706" opacity="0.6"/>
                            <path d="M 16 22 Q 18 25 20 22" stroke="#1a1a1a" stroke-width="1.5" fill="none" stroke-linecap="round"/>
                        </g>

                        <!-- Strawberry (Pink) Front Right -->
                        <g transform="translate(108, 75)">
                            <path d="M20,6 C32,6 38,18 32,36 C28,42 22,46 20,46 C18,46 12,42 8,36 C2,18 8,6 20,6 Z" fill="#ff6a88"/>
                            <path d="M12,8 Q20,12 28,8 Q22,4 20,0 Q18,4 12,8 Z" fill="#22c55e"/>
                            <circle cx="12" cy="18" r="1" fill="#fff" opacity="0.7"/>
                            <circle cx="28" cy="18" r="1" fill="#fff" opacity="0.7"/>
                            <circle cx="20" cy="24" r="1" fill="#fff" opacity="0.7"/>
                            <circle cx="15" cy="26" r="1.8" fill="#1a1a1a"/>
                            <circle cx="25" cy="26" r="1.8" fill="#1a1a1a"/>
                            <ellipse cx="11" cy="29" rx="2" ry="1.2" fill="#be123c" opacity="0.5"/>
                            <ellipse cx="29" cy="29" rx="2" ry="1.2" fill="#be123c" opacity="0.5"/>
                            <path d="M 18 29 Q 20 32 22 29" stroke="#1a1a1a" stroke-width="1.5" fill="none" stroke-linecap="round"/>
                        </g>

                        <!-- Basket Body -->
                        <path d="M 30,105 Q 100,100 170,105 L 152,160 Q 100,175 48,160 Z" fill="#d97706"/>
                        <path d="M 32,108 Q 100,103 168,108 L 150,158 Q 100,172 50,158 Z" fill="#f59e0b"/>
                        <path d="M 45,115 C 70,140 130,140 155,115" stroke="#b45309" stroke-width="2.5" fill="none" opacity="0.4"/>
                        <path d="M 50,130 C 75,155 125,155 150,130" stroke="#b45309" stroke-width="2.5" fill="none" opacity="0.4"/>
                        <path d="M 70,108 L 60,158 M 100,106 L 100,165 M 130,108 L 140,158" stroke="#b45309" stroke-width="2" fill="none" opacity="0.3"/>
                        
                        <!-- White Heart Badge on Basket -->
                        <circle cx="100" cy="138" r="13" fill="#ffffff"/>
                        <path d="M100 142 C97 139 91 135 91 131 C91 128 93.5 126 96 126 C98 126 99.5 127 100 128 C100.5 127 102 126 104 126 C106.5 126 109 128 109 131 C109 135 103 139 100 142 Z" fill="#f43f5e"/>
                    </svg>
                </div>
            </div>

            <!-- Main CTA & Footer -->
            <div class="w-full space-y-4">
                <button onclick="startDrawing()" class="w-full py-4 bg-gradient-to-r from-rose-400 to-pink-500 hover:from-rose-500 hover:to-pink-600 text-white font-bold text-lg rounded-full shadow-lg hover:shadow-xl transition transform active:scale-95 flex items-center justify-center gap-2 border-2 border-white/50 animate-glow">
                    <span>카드 뽑기</span>
                    <span class="text-pink-100">💓</span>
                </button>
                <p class="text-xs text-pink-500 font-medium tracking-tight">
                    한 해의 열매가 우리의 일상에 가득하길 ♥
                </p>
            </div>
        </section>

        <!-- SCREEN 2: Drawing Progress Screen -->
        <section id="screen2" class="hidden flex-1 flex flex-col items-center justify-between px-6 py-8 text-center bg-pink-50/50">
            <div class="mt-4 space-y-2">
                <h2 class="text-2xl font-bold text-pink-600 tracking-tight font-gamja text-3xl">
                    두근두근...
                </h2>
                <p class="text-sm font-medium text-gray-600">
                    지금, 열매 카드를 뽑고 있어요!
                </p>
            </div>

            <!-- Shuffling Card Graphic -->
            <div class="my-auto flex items-center justify-center relative">
                <i class="fas fa-heart text-pink-300 absolute -top-8 -left-6 text-xl animate-bounce"></i>
                <i class="fas fa-sparkles text-amber-300 absolute top-4 -right-8 text-lg animate-pulse"></i>
                <i class="fas fa-star text-purple-300 absolute -bottom-6 -left-6 text-base"></i>
                <i class="fas fa-leaf text-emerald-300 absolute -bottom-4 -right-6 text-lg"></i>

                <div class="w-56 h-88 sm:w-60 sm:h-96 rounded-3xl bg-gingham border-8 border-white shadow-2xl p-4 flex flex-col items-center justify-center relative animate-shuffle">
                    <div class="w-full h-full border-2 border-dashed border-pink-300/60 rounded-2xl flex items-center justify-center">
                        <div class="w-16 h-16 rounded-full bg-white shadow-md flex items-center justify-center animate-pulse">
                            <i class="fas fa-heart text-pink-400 text-2xl"></i>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Progress Bar -->
            <div class="w-48 bg-pink-100 rounded-full h-2 mb-4 overflow-hidden">
                <div id="drawProgressBar" class="bg-gradient-to-r from-pink-400 to-rose-400 h-2 rounded-full w-0 transition-all duration-[2400ms] ease-linear"></div>
            </div>
        </section>

        <!-- SCREEN 3: Card Reveal Screen -->
        <section id="screen3" class="hidden flex-1 flex flex-col items-center justify-between px-5 py-6 text-center relative overflow-y-auto">
            <!-- Confetti Container -->
            <div id="confettiContainer" class="absolute inset-0 pointer-events-none overflow-hidden"></div>

            <div class="text-center space-y-1">
                <span class="text-xs text-pink-500 font-bold bg-pink-100 px-3 py-1 rounded-full">올해 나에게 도착한 열매</span>
            </div>

            <!-- 3D Card Container -->
            <div class="my-auto py-2 perspective-1000 w-full flex justify-center">
                <div id="card3D" class="w-64 sm:w-72 h-[410px] sm:h-[430px] rounded-3xl transition-transform duration-700 transform-style-3d relative shadow-xl">
                    
                    <!-- Card Back -->
                    <div class="absolute inset-0 w-full h-full bg-gingham rounded-3xl border-8 border-white p-4 flex items-center justify-center backface-hidden shadow-md">
                        <div class="w-full h-full border-2 border-dashed border-pink-300/60 rounded-2xl flex items-center justify-center">
                            <i class="fas fa-heart text-pink-400 text-3xl"></i>
                        </div>
                    </div>

                    <!-- Card Front -->
                    <div id="cardFront" class="absolute inset-0 w-full h-full bg-white rounded-3xl border-8 border-rose-200 p-5 flex flex-col items-center justify-between backface-hidden rotate-y-180 shadow-md">
                        <div class="w-full flex justify-between items-center text-pink-300 text-xs">
                            <span>♥</span>
                            <h3 id="cardFruitName" class="text-2xl font-bold text-gray-800 font-gamja tracking-wide">사과</h3>
                            <span>♥</span>
                        </div>

                        <!-- Fruit SVG Slot -->
                        <div id="cardFruitSvgHolder" class="w-32 h-32 my-1 flex items-center justify-center"></div>

                        <!-- Badge -->
                        <div id="cardBadge" class="px-5 py-1.5 rounded-full text-white font-bold text-sm shadow-sm">
                            감사의 열매
                        </div>

                        <!-- Question Text -->
                        <div class="bg-rose-50/60 p-3.5 rounded-2xl w-full border border-pink-100">
                            <p id="cardQuestion" class="text-xs sm:text-sm font-medium text-gray-700 leading-relaxed word-keep-all">
                                올해를 돌아보며 가장 감사했던 일은 무엇인가요?
                            </p>
                        </div>

                        <div class="w-full flex justify-between items-center text-pink-300 text-xs pt-1">
                            <span>✿</span>
                            <span class="text-[10px] text-pink-400/80">한 해의 5가지 열매</span>
                            <span>✿</span>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Action Buttons -->
            <div class="w-full space-y-2 pt-2 z-10">
                <div class="grid grid-cols-2 gap-2.5">
                    <button onclick="goToDrawingAgain()" class="py-3 bg-purple-100 hover:bg-purple-200 text-purple-700 font-bold text-sm rounded-2xl transition active:scale-95 flex items-center justify-center gap-1.5">
                        <i class="fas fa-rotate-right"></i>
                        <span>다시 뽑기</span>
                    </button>
                    <button onclick="openShareModal()" class="py-3 bg-rose-500 hover:bg-rose-600 text-white font-bold text-sm rounded-2xl transition active:scale-95 shadow-md flex items-center justify-center gap-1.5">
                        <i class="fas fa-share-nodes"></i>
                        <span>공유 / 저장</span>
                    </button>
                </div>
            </div>
        </section>

        <!-- SCREEN 4: Share / Retry Modal Popup -->
        <div id="shareModal" class="hidden absolute inset-0 bg-black/60 backdrop-blur-sm z-50 flex items-center justify-center p-5 transition-opacity duration-300">
            <div class="bg-white rounded-3xl p-6 w-full max-w-xs text-center shadow-2xl relative border-4 border-pink-100 animate-float">
                <button onclick="closeShareModal()" class="absolute top-4 right-4 text-gray-400 hover:text-gray-600 w-8 h-8 rounded-full bg-gray-100 flex items-center justify-center transition">
                    <i class="fas fa-xmark text-lg"></i>
                </button>

                <div class="flex items-center justify-center gap-1 text-emerald-600 text-xs font-semibold mb-1">
                    <i class="fas fa-seedling text-sm"></i>
                    <span>오늘의 한 해 열매 카드</span>
                </div>

                <div id="modalMiniPreview" class="bg-pink-50 p-4 rounded-2xl my-3 border border-pink-200 flex flex-col items-center gap-2">
                    <div id="modalFruitSvg" class="w-16 h-16"></div>
                    <h4 id="modalFruitTitle" class="text-lg font-bold text-gray-800 font-gamja">사과</h4>
                    <span id="modalBadge" class="px-3 py-0.5 rounded-full text-white text-xs font-bold">감사의 열매</span>
                    <p id="modalQuestion" class="text-xs text-gray-600 mt-1 leading-normal">올해를 돌아보며 가장 감사했던 일은 무엇인가요?</p>
                </div>

                <div class="space-y-2 mt-4">
                    <button onclick="copyCardContent()" class="w-full py-3 bg-pink-500 hover:bg-pink-600 text-white font-bold text-sm rounded-xl shadow-sm transition active:scale-95 flex items-center justify-center gap-2">
                        <i class="fas fa-copy"></i>
                        <span>카드 내용 복사하기</span>
                    </button>
                    <button onclick="closeModalAndGoEnd()" class="w-full py-2.5 bg-gray-100 hover:bg-gray-200 text-gray-700 font-bold text-xs rounded-xl transition active:scale-95 flex items-center justify-center gap-1.5">
                        <i class="fas fa-check"></i>
                        <span>완료 화면으로 이동</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- SCREEN 5: Completion & Restart Screen -->
        <section id="screen5" class="hidden flex-1 flex flex-col items-center justify-between px-6 py-8 text-center bg-emerald-50/30">
            <div class="mt-4 space-y-1">
                <h2 class="text-2xl font-bold text-gray-800 font-gamja text-3xl">
                    다시 뽑고 싶다면 언제든지!
                </h2>
                <div class="flex items-center justify-center gap-1 text-amber-400 text-xs">
                    <span>\</span><span>\</span><span>♥</span><span>/</span><span>/</span>
                </div>
            </div>

            <!-- Row of 5 Fruits Icons -->
            <div class="my-auto w-full flex items-center justify-center py-4">
                <div class="w-full max-w-xs bg-white/90 p-4 rounded-3xl shadow-md border-2 border-pink-100 flex items-center justify-around">
                    <div class="flex flex-col items-center gap-1">
                        <div class="w-10 h-10">
                            <svg viewBox="0 0 40 40"><circle cx="20" cy="20" r="16" fill="#ff5252"/><path d="M20 4 Q24 -2 26 4 Z" fill="#4ade80"/><circle cx="15" cy="18" r="1.5" fill="#1a1a1a"/><circle cx="25" cy="18" r="1.5" fill="#1a1a1a"/><path d="M18 22 Q20 25 22 22" stroke="#1a1a1a" stroke-width="1" fill="none"/></svg>
                        </div>
                        <span class="text-[10px] font-bold text-rose-500">사과</span>
                    </div>

                    <div class="flex flex-col items-center gap-1">
                        <div class="w-10 h-10">
                            <svg viewBox="0 0 40 40"><circle cx="15" cy="18" r="7" fill="#a78bfa"/><circle cx="25" cy="18" r="7" fill="#8b5cf6"/><circle cx="20" cy="25" r="7" fill="#7c3aed"/><circle cx="17" cy="20" r="1" fill="#fff"/><circle cx="23" cy="20" r="1" fill="#fff"/></svg>
                        </div>
                        <span class="text-[10px] font-bold text-purple-500">포도</span>
                    </div>

                    <div class="flex flex-col items-center gap-1">
                        <div class="w-10 h-10">
                            <svg viewBox="0 0 40 40"><path d="M20,6 C30,6 36,15 36,26 C36,36 26,38 20,38 C14,38 4,36 4,26 C4,15 10,6 20,6 Z" fill="#ffb7b2"/><circle cx="15" cy="22" r="1.5" fill="#1a1a1a"/><circle cx="25" cy="22" r="1.5" fill="#1a1a1a"/></svg>
                        </div>
                        <span class="text-[10px] font-bold text-pink-400">복숭아</span>
                    </div>

                    <div class="flex flex-col items-center gap-1">
                        <div class="w-10 h-10">
                            <svg viewBox="0 0 40 40"><circle cx="20" cy="20" r="16" fill="#fbbf24"/><circle cx="15" cy="18" r="1.5" fill="#1a1a1a"/><circle cx="25" cy="18" r="1.5" fill="#1a1a1a"/><path d="M18 22 Q20 25 22 22" stroke="#1a1a1a" stroke-width="1" fill="none"/></svg>
                        </div>
                        <span class="text-[10px] font-bold text-amber-500">귤</span>
                    </div>

                    <div class="flex flex-col items-center gap-1">
                        <div class="w-10 h-10">
                            <svg viewBox="0 0 40 40"><path d="M20,5 C30,5 34,16 30,30 C26,36 22,38 20,38 C18,38 14,36 10,30 C6,16 10,5 20,5 Z" fill="#ff6a88"/><path d="M12,6 Q20,10 28,6 Q22,2 20,0 Q18,2 12,6 Z" fill="#22c55e"/><circle cx="15" cy="22" r="1.5" fill="#1a1a1a"/><circle cx="25" cy="22" r="1.5" fill="#1a1a1a"/></svg>
                        </div>
                        <span class="text-[10px] font-bold text-rose-600">딸기</span>
                    </div>
                </div>
            </div>

            <!-- Footer Message & CTA -->
            <div class="w-full space-y-4">
                <p class="text-xs text-gray-600 leading-relaxed">
                    다섯 가지 열매가 당신의 일상에<br>늘 함께하길 ♥
                </p>
                <button onclick="goToFirstScreen()" class="w-full py-4 bg-purple-400 hover:bg-purple-500 text-white font-bold text-base rounded-full shadow-lg transition transform active:scale-95 flex items-center justify-center gap-2 border-2 border-white">
                    <i class="fas fa-house"></i>
                    <span>처음으로</span>
                </button>
            </div>
        </section>

        <!-- Fruit Gallery Modal -->
        <div id="galleryModal" class="hidden absolute inset-0 bg-black/60 backdrop-blur-md z-50 flex flex-col justify-end transition-all duration-300">
            <div class="bg-white rounded-t-3xl p-5 w-full h-[88%] flex flex-col justify-between shadow-2xl relative border-t-4 border-pink-200">
                <div class="flex items-center justify-between pb-3 border-b border-gray-100">
                    <div class="flex items-center gap-2">
                        <span class="text-pink-500 text-lg">🍎</span>
                        <h3 class="font-bold text-gray-800 text-base">한 해의 열매 도감 (5종)</h3>
                    </div>
                    <button onclick="closeGalleryModal()" class="w-8 h-8 rounded-full bg-gray-100 flex items-center justify-center text-gray-500 hover:bg-gray-200">
                        <i class="fas fa-xmark"></i>
                    </button>
                </div>

                <div class="flex-1 overflow-y-auto my-3 space-y-3 pr-1">
                    <p class="text-xs text-gray-500 text-center">카드를 확인하여 올 한 해를 돌아보세요!</p>
                    <div id="galleryGrid" class="grid grid-cols-1 gap-3"></div>
                </div>

                <button onclick="closeGalleryModal()" class="w-full py-3 bg-pink-500 text-white font-bold text-sm rounded-2xl">
                    닫기
                </button>
            </div>
        </div>

        <!-- Toast Notification Element -->
        <div id="toast" class="absolute bottom-6 left-1/2 transform -translate-x-1/2 bg-gray-800/90 text-white text-xs px-4 py-2.5 rounded-full shadow-2xl opacity-0 transition-opacity duration-300 pointer-events-none z-50 flex items-center gap-2 border border-gray-700">
            <i class="fas fa-check-circle text-emerald-400"></i>
            <span id="toastMsg">클립보드에 복사되었습니다!</span>
        </div>

    </div>

    <script>
        /* -------------------------------------------------------------
         * 1. FRUIT CARD DATABASE (5 TYPES - UPDATED QUESTIONS)
         * ------------------------------------------------------------- */
        const FRUITS_DATABASE = [
            {
                id: 'apple',
                name: '사과',
                badge: '감사의 열매',
                badgeBg: 'bg-rose-500',
                themeColor: '#ff5252',
                cardBg: 'bg-rose-50/50',
                borderColor: 'border-rose-300',
                question: '올해를 돌아보며 가장 감사했던 일은 무엇인가요?',
                svg: `<svg viewBox="0 0 120 120" class="w-full h-full drop-shadow">
                    <circle cx="60" cy="65" r="42" fill="#ff5252"/>
                    <path d="M60 23 C 60 23, 68 10, 78 22 Z" fill="#4ade80"/>
                    <path d="M60 25 L 60 12" stroke="#78350f" stroke-width="3" stroke-linecap="round"/>
                    <circle cx="48" cy="58" r="4" fill="#1a1a1a"/>
                    <circle cx="72" cy="58" r="4" fill="#1a1a1a"/>
                    <circle cx="50" cy="56" r="1.5" fill="#fff"/>
                    <circle cx="74" cy="56" r="1.5" fill="#fff"/>
                    <ellipse cx="40" cy="66" rx="6" ry="3.5" fill="#f43f5e" opacity="0.6"/>
                    <ellipse cx="80" cy="66" rx="6" ry="3.5" fill="#f43f5e" opacity="0.6"/>
                    <path d="M 54 68 Q 60 75 66 68" stroke="#1a1a1a" stroke-width="3" fill="none" stroke-linecap="round"/>
                    <circle cx="42" cy="45" r="5" fill="#ffffff" opacity="0.4"/>
                </svg>`
            },
            {
                id: 'grape',
                name: '포도',
                badge: '기쁨의 열매',
                badgeBg: 'bg-purple-500',
                themeColor: '#8b5cf6',
                cardBg: 'bg-purple-50/50',
                borderColor: 'border-purple-300',
                question: '올해 나에게 가장 큰 기쁨을 준 순간은 언제였나요?',
                svg: `<svg viewBox="0 0 120 120" class="w-full h-full drop-shadow">
                    <g transform="translate(10, 10)">
                        <circle cx="42" cy="40" r="20" fill="#a78bfa"/>
                        <circle cx="58" cy="40" r="20" fill="#8b5cf6"/>
                        <circle cx="30" cy="60" r="20" fill="#7c3aed"/>
                        <circle cx="70" cy="60" r="20" fill="#6d28d9"/>
                        <circle cx="50" cy="78" r="20" fill="#5b21b6"/>
                        <path d="M50 20 Q 30 5 20 20 Q 35 30 50 20 Z" fill="#22c55e"/>
                        <circle cx="42" cy="60" r="3.5" fill="#1a1a1a"/>
                        <circle cx="58" cy="60" r="3.5" fill="#1a1a1a"/>
                        <circle cx="43" cy="58" r="1.2" fill="#fff"/>
                        <circle cx="59" cy="58" r="1.2" fill="#fff"/>
                        <ellipse cx="36" cy="66" rx="4" ry="2.5" fill="#c084fc" opacity="0.8"/>
                        <ellipse cx="64" cy="66" rx="4" ry="2.5" fill="#c084fc" opacity="0.8"/>
                        <path d="M 46 66 Q 50 72 54 66" stroke="#1a1a1a" stroke-width="2.5" fill="none" stroke-linecap="round"/>
                    </g>
                </svg>`
            },
            {
                id: 'peach',
                name: '복숭아',
                badge: '사랑의 열매',
                badgeBg: 'bg-pink-400',
                themeColor: '#ffb7b2',
                cardBg: 'bg-pink-50/50',
                borderColor: 'border-pink-300',
                question: '올해 누군가에게 받았던 사랑 중 기억에 남는 순간은 무엇인가요?',
                svg: `<svg viewBox="0 0 120 120" class="w-full h-full drop-shadow">
                    <path d="M60,18 C90,18 106,40 106,66 C106,94 86,106 60,106 C34,106 14,94 14,66 C14,40 30,18 60,18 Z" fill="#ffb7b2"/>
                    <path d="M60,18 Q 54,50 60,85" stroke="#ff8b94" stroke-width="3" fill="none" opacity="0.6"/>
                    <path d="M60 18 Q 75 8 85 18 Z" fill="#4ade80"/>
                    <circle cx="46" cy="62" r="4" fill="#1a1a1a"/>
                    <circle cx="74" cy="62" r="4" fill="#1a1a1a"/>
                    <circle cx="48" cy="60" r="1.5" fill="#fff"/>
                    <circle cx="76" cy="60" r="1.5" fill="#fff"/>
                    <ellipse cx="38" cy="70" rx="6" ry="3.5" fill="#e11d48" opacity="0.4"/>
                    <ellipse cx="82" cy="70" rx="6" ry="3.5" fill="#e11d48" opacity="0.4"/>
                    <path d="M 54 72 Q 60 78 66 72" stroke="#1a1a1a" stroke-width="3" fill="none" stroke-linecap="round"/>
                </svg>`
            },
            {
                id: 'tangerine',
                name: '귤',
                badge: '성장의 열매',
                badgeBg: 'bg-amber-500',
                themeColor: '#fbbf24',
                cardBg: 'bg-amber-50/50',
                borderColor: 'border-amber-300',
                question: '올해의 나를 돌아봤을 때, 가장 성장했다고 느끼는 부분은 무엇인가요?',
                svg: `<svg viewBox="0 0 120 120" class="w-full h-full drop-shadow">
                    <circle cx="60" cy="65" r="44" fill="#fbbf24"/>
                    <path d="M60 21 C 68 12 76 18 72 25 Z" fill="#22c55e"/>
                    <circle cx="60" cy="22" r="3" fill="#15803d"/>
                    <circle cx="46" cy="60" r="4" fill="#1a1a1a"/>
                    <circle cx="74" cy="60" r="4" fill="#1a1a1a"/>
                    <circle cx="48" cy="58" r="1.5" fill="#fff"/>
                    <circle cx="76" cy="58" r="1.5" fill="#fff"/>
                    <ellipse cx="38" cy="68" rx="5" ry="3" fill="#d97706" opacity="0.5"/>
                    <ellipse cx="82" cy="68" rx="5" ry="3" fill="#d97706" opacity="0.5"/>
                    <path d="M 54 70 Q 60 77 66 70" stroke="#1a1a1a" stroke-width="3" fill="none" stroke-linecap="round"/>
                    <circle cx="32" cy="50" r="1" fill="#d97706" opacity="0.5"/>
                    <circle cx="88" cy="50" r="1" fill="#d97706" opacity="0.5"/>
                    <circle cx="60" cy="98" r="1" fill="#d97706" opacity="0.5"/>
                </svg>`
            },
            {
                id: 'strawberry',
                name: '딸기',
                badge: '소망의 열매',
                badgeBg: 'bg-rose-500',
                themeColor: '#ff6a88',
                cardBg: 'bg-rose-50/50',
                borderColor: 'border-rose-300',
                question: '앞으로 꼭 이루어가고 싶은 소망은 무엇인가요?',
                svg: `<svg viewBox="0 0 120 120" class="w-full h-full drop-shadow">
                    <path d="M60,18 C90,18 104,45 88,90 C78,105 66,110 60,110 C54,110 42,105 32,90 C16,45 30,18 60,18 Z" fill="#ff6a88"/>
                    <path d="M38,20 Q60,30 82,20 Q66,10 60,0 Q54,10 38,20 Z" fill="#22c55e"/>
                    <circle cx="78" cy="22" r="6" fill="#ffffff"/>
                    <circle cx="78" cy="22" r="2" fill="#f59e0b"/>
                    <circle cx="38" cy="45" r="1.5" fill="#ffffff" opacity="0.6"/>
                    <circle cx="82" cy="45" r="1.5" fill="#ffffff" opacity="0.6"/>
                    <circle cx="60" cy="52" r="1.5" fill="#ffffff" opacity="0.6"/>
                    <circle cx="44" cy="88" r="1.5" fill="#ffffff" opacity="0.6"/>
                    <circle cx="76" cy="88" r="1.5" fill="#ffffff" opacity="0.6"/>
                    <circle cx="48" cy="65" r="4" fill="#1a1a1a"/>
                    <circle cx="72" cy="65" r="4" fill="#1a1a1a"/>
                    <circle cx="50" cy="63" r="1.5" fill="#fff"/>
                    <circle cx="74" cy="63" r="1.5" fill="#fff"/>
                    <ellipse cx="40" cy="73" rx="5" ry="3" fill="#be123c" opacity="0.5"/>
                    <ellipse cx="80" cy="73" rx="5" ry="3" fill="#be123c" opacity="0.5"/>
                    <path d="M 54 74 Q 60 80 66 74" stroke="#1a1a1a" stroke-width="3" fill="none" stroke-linecap="round"/>
                </svg>`
            }
        ];

        /* -------------------------------------------------------------
         * 2. STATE & AUDIO SYNTHESIZER
         * ------------------------------------------------------------- */
        let currentDrawnFruit = null;
        let isAudioMuted = false;
        let audioCtx = null;

        function getAudioContext() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
            return audioCtx;
        }

        function playPopSound() {
            if (isAudioMuted) return;
            try {
                const ctx = getAudioContext();
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(400, ctx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(800, ctx.currentTime + 0.1);
                gain.gain.setValueAtTime(0.3, ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime + 0.1);
                osc.connect(gain);
                gain.connect(ctx.destination);
                osc.start();
                osc.stop(ctx.currentTime + 0.1);
            } catch (e) { console.log(e); }
        }

        function playShuffleSound() {
            if (isAudioMuted) return;
            try {
                const ctx = getAudioContext();
                for (let i = 0; i < 4; i++) {
                    setTimeout(() => {
                        const osc = ctx.createOscillator();
                        const gain = ctx.createGain();
                        osc.type = 'triangle';
                        osc.frequency.setValueAtTime(200 + Math.random() * 200, ctx.currentTime);
                        gain.gain.setValueAtTime(0.1, ctx.currentTime);
                        gain.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime + 0.08);
                        osc.connect(gain);
                        gain.connect(ctx.destination);
                        osc.start();
                        osc.stop(ctx.currentTime + 0.08);
                    }, i * 150);
                }
            } catch (e) { console.log(e); }
        }

        function playFanfareSound() {
            if (isAudioMuted) return;
            try {
                const ctx = getAudioContext();
                const notes = [523.25, 659.25, 783.99, 1046.50];
                notes.forEach((freq, idx) => {
                    setTimeout(() => {
                        const osc = ctx.createOscillator();
                        const gain = ctx.createGain();
                        osc.type = 'sine';
                        osc.frequency.setValueAtTime(freq, ctx.currentTime);
                        gain.gain.setValueAtTime(0.25, ctx.currentTime);
                        gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.4);
                        osc.connect(gain);
                        gain.connect(ctx.destination);
                        osc.start();
                        osc.stop(ctx.currentTime + 0.4);
                    }, idx * 90);
                });
            } catch (e) { console.log(e); }
        }

        function toggleSound() {
            isAudioMuted = !isAudioMuted;
            const icon = document.getElementById('soundIcon');
            if (isAudioMuted) {
                icon.className = "fas fa-volume-xmark text-gray-400";
            } else {
                icon.className = "fas fa-volume-up text-pink-400";
                playPopSound();
            }
        }

        /* -------------------------------------------------------------
         * 3. SCREEN SWITCHING & DRAWING FLOW
         * ------------------------------------------------------------- */
        function showScreen(screenId) {
            ['screen1', 'screen2', 'screen3', 'screen5'].forEach(id => {
                const el = document.getElementById(id);
                if (id === screenId) {
                    el.classList.remove('hidden');
                } else {
                    el.classList.add('hidden');
                }
            });
        }

        function startDrawing() {
            playPopSound();
            showScreen('screen2');

            const pBar = document.getElementById('drawProgressBar');
            pBar.style.width = '0%';
            setTimeout(() => {
                pBar.style.width = '100%';
            }, 50);

            playShuffleSound();

            const randomIndex = Math.floor(Math.random() * FRUITS_DATABASE.length);
            currentDrawnFruit = FRUITS_DATABASE[randomIndex];

            setTimeout(() => {
                revealCardResult();
            }, 2500);
        }

        function revealCardResult() {
            showScreen('screen3');

            const card3D = document.getElementById('card3D');
            const cardFruitName = document.getElementById('cardFruitName');
            const cardFruitSvgHolder = document.getElementById('cardFruitSvgHolder');
            const cardBadge = document.getElementById('cardBadge');
            const cardQuestion = document.getElementById('cardQuestion');

            cardFruitName.innerText = currentDrawnFruit.name;
            cardFruitSvgHolder.innerHTML = currentDrawnFruit.svg;
            cardBadge.innerText = currentDrawnFruit.badge;
            cardBadge.className = `px-5 py-1.5 rounded-full text-white font-bold text-sm shadow-sm ${currentDrawnFruit.badgeBg}`;
            cardQuestion.innerText = currentDrawnFruit.question;

            card3D.classList.remove('rotate-y-180');

            setTimeout(() => {
                card3D.classList.add('rotate-y-180');
                playFanfareSound();
                triggerConfetti();
            }, 200);
        }

        function goToDrawingAgain() {
            playPopSound();
            startDrawing();
        }

        /* -------------------------------------------------------------
         * 4. SHARE MODAL & COPY FUNCTIONS
         * ------------------------------------------------------------- */
        function openShareModal() {
            playPopSound();
            const modal = document.getElementById('shareModal');
            
            document.getElementById('modalFruitSvg').innerHTML = currentDrawnFruit.svg;
            document.getElementById('modalFruitTitle').innerText = currentDrawnFruit.name;
            const badge = document.getElementById('modalBadge');
            badge.innerText = currentDrawnFruit.badge;
            badge.className = `px-3 py-0.5 rounded-full text-white text-xs font-bold ${currentDrawnFruit.badgeBg}`;
            document.getElementById('modalQuestion').innerText = currentDrawnFruit.question;

            modal.classList.remove('hidden');
        }

        function closeShareModal() {
            playPopSound();
            document.getElementById('shareModal').classList.add('hidden');
        }

        function closeModalAndGoEnd() {
            closeShareModal();
            showScreen('screen5');
        }

        function copyCardContent() {
            playPopSound();
            const textToCopy = `[오늘의 한 해 열매 카드]\n열매: ${currentDrawnFruit.name} (${currentDrawnFruit.badge})\n질문: "${currentDrawnFruit.question}"\n\n한 해의 열매가 당신의 일상에 늘 함께하길 바라요! ♥`;
            
            if (navigator.clipboard && navigator.clipboard.writeText) {
                navigator.clipboard.writeText(textToCopy).then(() => {
                    showToast('카드 내용이 복사되었습니다!');
                }).catch(() => {
                    fallbackCopyText(textToCopy);
                });
            } else {
                fallbackCopyText(textToCopy);
            }
        }

        function fallbackCopyText(text) {
            const textArea = document.createElement("textarea");
            textArea.value = text;
            document.body.appendChild(textArea);
            textArea.select();
            try {
                document.execCommand('copy');
                showToast('카드 내용이 복사되었습니다!');
            } catch (err) {
                showToast('복사에 실패했습니다.');
            }
            document.body.removeChild(textArea);
        }

        /* -------------------------------------------------------------
         * 5. RESTART, GALLERY & CONFETTI UTILS
         * ------------------------------------------------------------- */
        function goToFirstScreen() {
            playPopSound();
            showScreen('screen1');
        }

        function openGalleryModal() {
            playPopSound();
            const grid = document.getElementById('galleryGrid');
            grid.innerHTML = '';

            FRUITS_DATABASE.forEach(fruit => {
                const card = document.createElement('div');
                card.className = `p-3.5 rounded-2xl border ${fruit.borderColor} ${fruit.cardBg} flex items-center gap-3 shadow-sm hover:shadow transition`;
                card.innerHTML = `
                    <div class="w-14 h-14 flex-shrink-0">
                        ${fruit.svg}
                    </div>
                    <div class="flex-1 text-left space-y-1">
                        <div class="flex items-center gap-2">
                            <span class="font-bold text-gray-800 text-sm font-gamja">${fruit.name}</span>
                            <span class="px-2 py-0.5 rounded-full text-[10px] text-white font-bold ${fruit.badgeBg}">${fruit.badge}</span>
                        </div>
                        <p class="text-xs text-gray-600 line-clamp-2">${fruit.question}</p>
                    </div>
                `;
                grid.appendChild(card);
            });

            document.getElementById('galleryModal').classList.remove('hidden');
        }

        function closeGalleryModal() {
            playPopSound();
            document.getElementById('galleryModal').classList.add('hidden');
        }

        function showToast(msg) {
            const toast = document.getElementById('toast');
            document.getElementById('toastMsg').innerText = msg;
            toast.classList.remove('opacity-0');
            setTimeout(() => {
                toast.classList.add('opacity-0');
            }, 2200);
        }

        function triggerConfetti() {
            const container = document.getElementById('confettiContainer');
            container.innerHTML = '';
            const colors = ['#ff6a88', '#fbbf24', '#a78bfa', '#ffb7b2', '#4ade80', '#38bdf8'];

            for (let i = 0; i < 22; i++) {
                const p = document.createElement('div');
                p.className = 'sparkle-particle';
                p.style.left = `${Math.random() * 90 + 5}%`;
                p.style.top = `${Math.random() * 60 + 20}%`;
                p.style.color = colors[Math.floor(Math.random() * colors.length)];
                p.style.fontSize = `${Math.random() * 12 + 10}px`;
                p.style.animationDelay = `${Math.random() * 0.8}s`;
                p.innerHTML = ['✦', '♥', '✨', '🌸', '★'][Math.floor(Math.random() * 5)];
                container.appendChild(p);
            }
        }

        window.addEventListener('DOMContentLoaded', () => {
            showScreen('screen1');
        });
    </script>
</body>
</html>
