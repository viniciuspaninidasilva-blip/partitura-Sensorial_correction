<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Partitura Sensorial - Vibrações Rítmicas</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap');
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
        }
        .glow-active {
            box-shadow: 0 0 15px rgba(139, 92, 246, 0.4);
        }
        /* Personalização simples do slider */
        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 16px;
            height: 16px;
            border-radius: 50%;
            background: #a78bfa;
            cursor: pointer;
            transition: transform 0.1s;
        }
        input[type="range"]::-webkit-slider-thumb:hover {
            transform: scale(1.2);
        }
    </style>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen flex flex-col justify-between selection:bg-violet-500/30 selection:text-violet-200">

    <!-- Header -->
    <header class="border-b border-slate-800 bg-slate-900/50 backdrop-blur sticky top-0 z-50 px-4 py-3">
        <div class="max-w-4xl mx-auto flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-violet-600 to-fuchsia-600 flex items-center justify-center shadow-lg shadow-violet-500/20">
                    <i data-lucide="music-4" class="w-6 h-6 text-white"></i>
                </div>
                <div>
                    <h1 class="text-lg font-bold leading-none bg-gradient-to-r from-violet-400 to-fuchsia-400 bg-clip-text text-transparent">Partitura Sensorial</h1>
                    <span class="text-[10px] text-slate-400 font-medium tracking-wider uppercase">Polirritmia & Vibração</span>
                </div>
            </div>
            <button id="btnInfo" class="p-2 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-300 transition-colors" title="Como funciona?">
                <i data-lucide="help-circle" class="w-5 h-5"></i>
            </button>
        </div>
    </header>

    <!-- Main Content -->
    <main class="flex-grow max-w-4xl w-full mx-auto p-4 space-y-6">

        <!-- Info/Warning Banner for iOS/Browsers -->
        <div id="vibrationNotice" class="hidden bg-violet-500/10 border border-violet-500/30 rounded-xl p-4 flex gap-3 items-start">
            <i data-lucide="info" class="w-5 h-5 text-violet-400 shrink-0 mt-0.5"></i>
            <div class="text-sm text-slate-200">
                <p class="font-semibold text-violet-300">Modo de Vibração Acústica Ativo (iPhone/iOS):</p>
                <p class="text-slate-300 text-xs mt-1">Dispositivos iOS não permitem vibração física direta via navegador. Ativamos o **módulo acústico**: o alto-falante emitirá ondas sub-graves de ar para fazer o aparelho pulsar em suas mãos. **Aumente o volume de som do seu celular para sentir o efeito!**</p>
            </div>
        </div>

        <!-- Import Section -->
        <section class="bg-slate-900 border border-slate-800 rounded-2xl p-5 shadow-xl">
            <div class="flex items-center gap-2 mb-4">
                <i data-lucide="file-up" class="w-5 h-5 text-violet-400"></i>
                <h2 class="text-base font-semibold text-slate-200">Importar do MuseScore</h2>
            </div>
            
            <p class="text-xs text-slate-400 mb-4">
                No MuseScore, vá em <strong class="text-slate-300">Arquivo > Exportar</strong> e salve como <strong class="text-slate-300">MusicXML (.musicxml ou .xml)</strong>. Notas graves vão para o Tam-tam, médias para o Tamborim/Pandeiro, e agudas para o Triângulo.
            </p>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <!-- File Input Drag-Drop -->
                <label class="border-2 border-dashed border-slate-700 hover:border-violet-500/50 bg-slate-950 rounded-xl p-6 flex flex-col items-center justify-center cursor-pointer transition-all group relative overflow-hidden">
                    <input type="file" id="fileInput" accept=".musicxml,.xml" class="hidden">
                    <i data-lucide="upload-cloud" class="w-8 h-8 text-slate-500 group-hover:text-violet-400 transition-colors mb-2"></i>
                    <span class="text-sm font-medium text-slate-300 group-hover:text-slate-200" id="fileName">Escolher Arquivo .musicxml</span>
                    <span class="text-[10px] text-slate-500 mt-1">Arraste ou clique para navegar</span>
                </label>

                <!-- Preset Options / Genre selection -->
                <div class="bg-slate-950 rounded-xl p-4 border border-slate-800 flex flex-col justify-between">
                    <div>
                        <span class="text-[10px] uppercase tracking-wider font-semibold text-slate-500 block mb-2">Escolha o ritmo de base:</span>
                        <div class="grid grid-cols-2 gap-3">
                            <button onclick="setGenre('samba')" id="genreBtn-samba" class="px-3 py-2.5 text-sm font-bold bg-slate-900 border border-slate-800 rounded-xl transition-all flex items-center justify-center gap-2 text-slate-300 hover:border-violet-500/50">
                                <span class="w-2.5 h-2.5 rounded-full bg-emerald-500"></span> Samba
                            </button>
                            <button onclick="setGenre('baiao')" id="genreBtn-baiao" class="px-3 py-2.5 text-sm font-bold bg-slate-900 border border-slate-800 rounded-xl transition-all flex items-center justify-center gap-2 text-slate-300 hover:border-violet-500/50">
                                <span class="w-2.5 h-2.5 rounded-full bg-amber-500"></span> Baião
                            </button>
                        </div>
                    </div>
                    <div id="importStatus" class="text-xs text-emerald-400 font-medium hidden mt-2 flex items-center gap-1">
                        <i data-lucide="check-circle" class="w-4 h-4"></i> Arquivo carregado com sucesso!
                    </div>
                </div>
            </div>
        </section>

        <!-- Current Rhythm Display & Sequencer Editor -->
        <section class="bg-slate-900 border border-slate-800 rounded-2xl p-5 shadow-xl space-y-5">
            <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3 border-b border-slate-800 pb-4">
                <div class="flex items-center gap-2">
                    <i data-lucide="sliders" class="w-5 h-5 text-violet-400"></i>
                    <div>
                        <h2 class="text-base font-semibold text-slate-200">Mixer e Sequenciador de Ritmo</h2>
                        <p class="text-xs text-slate-400" id="scoreName">Estilo Selecionado: Samba</p>
                    </div>
                </div>
                
                <!-- Quick Settings -->
                <div class="flex items-center gap-3">
                    <!-- BPM Controller -->
                    <div class="flex items-center gap-2 bg-slate-950 border border-slate-800 rounded-xl px-3 py-1.5">
                        <span class="text-xs text-slate-400 font-semibold uppercase">Tempo:</span>
                        <input type="number" id="bpmInput" value="110" min="40" max="280" class="w-12 bg-transparent text-violet-400 font-bold text-center focus:outline-none">
                        <span class="text-[10px] text-slate-500">BPM</span>
                    </div>
                    <!-- Clear -->
                    <button onclick="clearSequencer()" class="text-[10px] bg-slate-800 hover:bg-rose-950/40 hover:text-rose-400 px-2.5 py-2 rounded-xl border border-slate-700 transition-colors flex items-center gap-1" title="Limpar todas as faixas">
                        <i data-lucide="trash-2" class="w-3.5 h-3.5"></i> Limpar
                    </button>
                </div>
            </div>

            <!-- Instrument Rows -->
            <div class="space-y-6" id="tracksContainer">
                <!-- Tracks will be generated dynamically by JavaScript based on chosen genre -->
            </div>

            <!-- Legenda e Tutorial de cliques -->
            <div class="flex flex-wrap items-center justify-between gap-2 pt-3 text-xs text-slate-400 border-t border-slate-800/50">
                <div class="flex gap-4">
                    <div class="flex items-center gap-1.5">
                        <div class="w-3.5 h-3.5 rounded bg-violet-600"></div> <span>Toque Normal</span>
                    </div>
                    <div class="flex items-center gap-1.5">
                        <div class="w-3.5 h-3.5 rounded bg-fuchsia-600"></div> <span>Toque Acentuado (Forte)</span>
                    </div>
                </div>
                <span class="text-[11px] text-slate-500 italic">Cada instrumento possui canais independentes de som, mute e vibração.</span>
            </div>
        </section>

        <!-- Engine Playback Control -->
        <section class="bg-gradient-to-r from-violet-950/30 to-fuchsia-950/30 border border-violet-900/30 rounded-2xl p-6 shadow-xl text-center space-y-6">
            <h2 class="text-sm font-semibold text-slate-300 uppercase tracking-wider">Controle do Feedback Tátil Geral</h2>

            <!-- Metronome Playback Ring / Visual Pulse -->
            <div class="relative w-36 h-36 mx-auto flex items-center justify-center">
                <!-- Pulse outer ring -->
                <div id="visualPulse" class="absolute inset-0 rounded-full bg-violet-500/5 border-2 border-violet-500/20 transition-all duration-75"></div>
                <!-- Play/Pause Button -->
                <button id="btnPlay" class="relative z-10 w-28 h-28 rounded-full bg-gradient-to-tr from-violet-600 to-fuchsia-600 hover:from-violet-500 hover:to-fuchsia-500 text-white flex flex-col items-center justify-center gap-1 shadow-xl shadow-violet-500/20 active:scale-95 transition-all">
                    <i data-lucide="play" class="w-10 h-10 fill-current" id="playIcon"></i>
                    <span class="text-xs font-bold uppercase tracking-wider" id="playText">Iniciar</span>
                </button>
            </div>

            <!-- Global Toggles -->
            <div class="max-w-md mx-auto grid grid-cols-3 gap-3">
                <button id="toggleVibrate" class="p-3 rounded-xl bg-slate-900/80 border border-slate-800 text-slate-300 transition-all flex flex-col items-center gap-1.5 hover:bg-slate-800">
                    <i data-lucide="smartphone" class="w-5 h-5 text-violet-400"></i>
                    <span class="text-[11px] font-medium" id="vibrateLabelText">Vibração Geral</span>
                    <span class="text-[9px] text-slate-500" id="vibrateStatus">Ativada</span>
                </button>
                <button id="toggleSound" class="p-3 rounded-xl bg-slate-900/80 border border-slate-800 text-slate-300 transition-all flex flex-col items-center gap-1.5 hover:bg-slate-800">
                    <i data-lucide="volume-2" class="w-5 h-5 text-fuchsia-400"></i>
                    <span class="text-[11px] font-medium">Som Geral</span>
                    <span class="text-[9px] text-slate-500" id="soundStatus">Ativado</span>
                </button>
                <button id="toggleFlash" class="p-3 rounded-xl bg-slate-900/80 border border-slate-800 text-slate-300 transition-all flex flex-col items-center gap-1.5 hover:bg-slate-800">
                    <i data-lucide="zap" class="w-5 h-5 text-amber-400"></i>
                    <span class="text-[11px] font-medium">Flash Visual</span>
                    <span class="text-[9px] text-slate-500" id="flashStatus">Ativado</span>
                </button>
            </div>

            <!-- Intensity controls -->
            <div class="max-w-xs mx-auto space-y-2">
                <div class="flex justify-between text-xs text-slate-400">
                    <span>Duração Base de Vibração</span>
                    <span id="vibrateDurationLabel">60ms</span>
                </div>
                <input type="range" id="vibrateDurationRange" min="20" max="200" step="10" value="60" class="w-full accent-violet-500 bg-slate-800 rounded-lg appearance-none h-1.5 cursor-pointer">
            </div>
        </section>

    </main>

    <!-- Guide Modal -->
    <div id="infoModal" class="fixed inset-0 z-50 bg-slate-950/80 backdrop-blur-sm flex items-center justify-center p-4 hidden">
        <div class="bg-slate-900 border border-slate-800 rounded-2xl max-w-lg w-full p-6 space-y-4">
            <div class="flex justify-between items-start">
                <div class="flex items-center gap-2">
                    <i data-lucide="help-circle" class="w-6 h-6 text-violet-400"></i>
                    <h3 class="text-lg font-bold">Guia de Uso - Multi-faixas</h3>
                </div>
                <button id="btnCloseModal" class="p-1 rounded-lg hover:bg-slate-800 text-slate-400 hover:text-slate-200">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>
            
            <div class="text-sm text-slate-300 space-y-3">
                <p>Nesta versão avançada do app, você pode ouvir e sentir múltiplos instrumentos trabalhando juntos, exatamente como em uma banda de percussão!</p>
                
                <h4 class="font-semibold text-slate-100 flex items-center gap-1.5 mt-4">
                    <i data-lucide="sliders-horizontal" class="w-4 h-4 text-violet-400"></i> Mixer de Instrumentos:
                </h4>
                <ul class="list-disc pl-5 space-y-1 text-xs">
                    <li>Alterne entre os ritmos <strong class="text-violet-300">Samba</strong> e <strong class="text-violet-300">Baião</strong> para alterar os instrumentos disponíveis.</li>
                    <li>Use o ícone de <strong class="text-fuchsia-400">Alto-falante</strong> em cada linha de instrumento para habilitar ou silenciar aquele som individual.</li>
                    <li>Use o ícone de <strong class="text-violet-400">Celular</strong> na linha de cada instrumento para definir se ele deve disparar a vibração do smartphone.</li>
                </ul>

                <h4 class="font-semibold text-slate-100 flex items-center gap-1.5 mt-4">
                    <i data-lucide="smartphone" class="w-4 h-4 text-violet-400"></i> Vibração no iPhone (iOS):
                </h4>
                <p class="text-xs text-slate-400">Como o iOS limita a vibração comum de navegadores, usamos a **Vibração Acústica**. Ondas sonoras de ar de baixíssima frequência ($45\text{ Hz}$) fazem o seu iPhone vibrar fisicamente na sua mão. **Certifique-se de aumentar o volume de som do aparelho!**</p>
            </div>

            <button id="btnCloseModalBtn" class="w-full py-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-200 font-semibold text-sm transition-colors">
                Vamos Começar!
            </button>
        </div>
    </div>

    <!-- Footer -->
    <footer class="border-t border-slate-900 bg-slate-950/80 py-4 text-center px-4 text-xs text-slate-500">
        <p>Desenvolvido para Aprendizado Rítmico Acessível. Sinta a polirritmia em suas mãos.</p>
    </footer>

    <script>
        // System Config and State
        const state = {
            isPlaying: false,
            bpm: 110,
            vibrateEnabled: true,
            soundEnabled: true,
            flashEnabled: true,
            vibrationBaseDuration: 60, // ms
            currentStep: 0,
            playbackInterval: null,
            currentGenre: 'samba', // 'samba' or 'baiao'
            isIOS: false,
            
            // Instrument Tracks
            tracks: {
                // --- SAMBA TRACKS ---
                samba_tamborim: {
                    name: "Tamborim",
                    color: "fuchsia",
                    soundEnabled: true,
                    vibrateEnabled: false,
                    sequence: [2, 0, 1, 0, 0, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 0]
                },
                samba_tamtam: {
                    name: "Tam-tam (Surdo)",
                    color: "emerald",
                    soundEnabled: true,
                    vibrateEnabled: true,
                    sequence: [0, 0, 0, 0, 2, 0, 0, 0, 0, 0, 0, 0, 2, 0, 0, 0]
                },
                samba_pandeiro: {
                    name: "Pandeiro",
                    color: "violet",
                    soundEnabled: true,
                    vibrateEnabled: false,
                    sequence: [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
                },

                // --- BAIÃO TRACKS ---
                baiao_tamtam: {
                    name: "Tam-tam (Zabumba)",
                    color: "emerald",
                    soundEnabled: true,
                    vibrateEnabled: true,
                    sequence: [2, 0, 0, 1, 0, 1, 0, 0, 2, 0, 0, 1, 0, 1, 0, 0]
                },
                baiao_triangulo: {
                    name: "Triângulo",
                    color: "amber",
                    soundEnabled: true,
                    vibrateEnabled: false,
                    sequence: [1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1]
                },
                baiao_pandeiro: {
                    name: "Pandeiro",
                    color: "violet",
                    soundEnabled: true,
                    vibrateEnabled: false,
                    sequence: [1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1]
                }
            }
        };

        // Web Audio API setup
        let audioCtx = null;

        // Ready elements
        const btnPlay = document.getElementById('btnPlay');
        const playIcon = document.getElementById('playIcon');
        const playText = document.getElementById('playText');
        const visualPulse = document.getElementById('visualPulse');
        const fileInput = document.getElementById('fileInput');
        const fileNameLabel = document.getElementById('fileName');
        const bpmInput = document.getElementById('bpmInput');
        const scoreName = document.getElementById('scoreName');
        const importStatus = document.getElementById('importStatus');
        const tracksContainer = document.getElementById('tracksContainer');
        
        // Global Toggles
        const toggleVibrate = document.getElementById('toggleVibrate');
        const toggleSound = document.getElementById('toggleSound');
        const toggleFlash = document.getElementById('toggleFlash');
        const vibrateDurationRange = document.getElementById('vibrateDurationRange');
        const vibrateDurationLabel = document.getElementById('vibrateDurationLabel');
        const vibrateLabelText = document.getElementById('vibrateLabelText');

        // Modal
        const infoModal = document.getElementById('infoModal');
        const btnInfo = document.getElementById('btnInfo');
        const btnCloseModal = document.getElementById('btnCloseModal');
        const btnCloseModalBtn = document.getElementById('btnCloseModalBtn');
        const vibrationNotice = document.getElementById('vibrationNotice');

        // Initialize
        window.addEventListener('DOMContentLoaded', () => {
            lucide.createIcons();
            detectPlatformAndVibration();
            setGenre('samba');
        });

        // Detect Platform & setup Vibration mode
        function detectPlatformAndVibration() {
            // Check for iOS
            state.isIOS = /iPad|iPhone|iPod/.test(navigator.userAgent) || (navigator.platform === 'MacIntel' && navigator.maxTouchPoints > 1);
            
            if (state.isIOS) {
                // For iOS, activate acoustic mode and update UI
                vibrationNotice.classList.remove('hidden');
                vibrateLabelText.textContent = "Vibração Acústica";
                updateToggleUI(toggleVibrate, true, "Modo Acústico");
            } else if (!("vibrate" in navigator)) {
                // Non-compatible, non-iOS browser fallback to acoustic too
                vibrationNotice.classList.remove('hidden');
                vibrateLabelText.textContent = "Vibração Acústica";
                updateToggleUI(toggleVibrate, true, "Modo Acústico");
            } else {
                // Android/Standard browser with native vibrate support
                updateToggleUI(toggleVibrate, true, "Ativada");
            }
        }

        // Toggle Status UI Helpers
        function updateToggleUI(element, isEnabled, labelText = null) {
            const statusLabel = element.querySelector('span:last-child');
            if (isEnabled) {
                element.classList.remove('border-slate-800', 'bg-slate-900/80');
                element.classList.add('border-violet-500/30', 'bg-violet-950/20');
                if (statusLabel) statusLabel.textContent = labelText || "Ativado";
                if (statusLabel) statusLabel.classList.remove('text-slate-500');
                if (statusLabel) statusLabel.classList.add('text-violet-400');
            } else {
                element.classList.remove('border-violet-500/30', 'bg-violet-950/20');
                element.classList.add('border-slate-800', 'bg-slate-900/80');
                if (statusLabel) statusLabel.textContent = labelText || "Desativado";
                if (statusLabel) statusLabel.classList.remove('text-violet-400');
                if (statusLabel) statusLabel.classList.add('text-slate-500');
            }
        }

        // Set Up Event Listeners
        toggleVibrate.addEventListener('click', () => {
            state.vibrateEnabled = !state.vibrateEnabled;
            const text = state.isIOS ? "Modo Acústico" : "Ativada";
            updateToggleUI(toggleVibrate, state.vibrateEnabled, state.vibrateEnabled ? text : "Desativado");
        });

        toggleSound.addEventListener('click', () => {
            state.soundEnabled = !state.soundEnabled;
            updateToggleUI(toggleSound, state.soundEnabled);
        });

        toggleFlash.addEventListener('click', () => {
            state.flashEnabled = !state.flashEnabled;
            updateToggleUI(toggleFlash, state.flashEnabled);
        });

        vibrateDurationRange.addEventListener('input', (e) => {
            const val = parseInt(e.target.value);
            state.vibrationBaseDuration = val;
            vibrateDurationLabel.textContent = `${val}ms`;
        });

        // Set current genre and rebuild DOM
        function setGenre(genre) {
            stopPlayback();

            state.currentGenre = genre;
            scoreName.textContent = `Estilo Selecionado: ${genre.charAt(0).toUpperCase() + genre.slice(1)}`;

            const sambaBtn = document.getElementById('genreBtn-samba');
            const baiaoBtn = document.getElementById('genreBtn-baiao');
            
            if (genre === 'samba') {
                sambaBtn.className = "px-3 py-2.5 text-sm font-bold bg-violet-950/30 border border-violet-500 rounded-xl transition-all flex items-center justify-center gap-2 text-violet-200 shadow-lg shadow-violet-500/10";
                baiaoBtn.className = "px-3 py-2.5 text-sm font-semibold bg-slate-900/80 border border-slate-800 rounded-xl transition-all flex items-center justify-center gap-2 text-slate-400 hover:text-slate-200 hover:border-slate-700";
                state.bpm = 110;
            } else {
                baiaoBtn.className = "px-3 py-2.5 text-sm font-bold bg-amber-950/30 border border-amber-500 rounded-xl transition-all flex items-center justify-center gap-2 text-amber-200 shadow-lg shadow-amber-500/10";
                sambaBtn.className = "px-3 py-2.5 text-sm font-semibold bg-slate-900/80 border border-slate-800 rounded-xl transition-all flex items-center justify-center gap-2 text-slate-400 hover:text-slate-200 hover:border-slate-700";
                state.bpm = 105;
            }
            bpmInput.value = state.bpm;

            buildMultiSequencer();
        }

        // Build Multi-Track Grid
        function buildMultiSequencer() {
            tracksContainer.innerHTML = '';
            
            const activeTrackKeys = Object.keys(state.tracks).filter(key => key.startsWith(state.currentGenre));

            activeTrackKeys.forEach(trackKey => {
                const track = state.tracks[trackKey];
                
                const trackDiv = document.createElement('div');
                trackDiv.className = "space-y-2 bg-slate-950/50 p-4 border border-slate-800/60 rounded-xl";
                
                const trackHeader = document.createElement('div');
                trackHeader.className = "flex flex-col sm:flex-row sm:items-center justify-between gap-2 border-b border-slate-900 pb-2";
                
                const titleWrapper = document.createElement('div');
                titleWrapper.className = "flex items-center gap-2";
                
                let dotColorClass = "bg-violet-500";
                if (track.color === 'emerald') dotColorClass = "bg-emerald-500";
                if (track.color === 'amber') dotColorClass = "bg-amber-400";
                if (track.color === 'fuchsia') dotColorClass = "bg-fuchsia-500";

                titleWrapper.innerHTML = `
                    <span class="w-3 h-3 rounded-full ${dotColorClass}"></span>
                    <span class="font-bold text-sm text-slate-300">${track.name}</span>
                `;

                const controlsWrapper = document.createElement('div');
                controlsWrapper.className = "flex items-center gap-2";

                // Mute/Unmute
                const soundBtn = document.createElement('button');
                soundBtn.className = `p-1.5 rounded-lg border transition-all flex items-center gap-1 text-[11px] font-medium ${
                    track.soundEnabled 
                        ? 'bg-fuchsia-950/20 border-fuchsia-500/30 text-fuchsia-400' 
                        : 'bg-slate-900 border-slate-800 text-slate-500'
                }`;
                soundBtn.innerHTML = `<i data-lucide="${track.soundEnabled ? 'volume-2' : 'volume-x'}" class="w-3.5 h-3.5"></i> <span>Som</span>`;
                soundBtn.onclick = () => {
                    track.soundEnabled = !track.soundEnabled;
                    buildMultiSequencer();
                };

                // Individual Track Vibrate Trigger
                const vibrateBtn = document.createElement('button');
                vibrateBtn.className = `p-1.5 rounded-lg border transition-all flex items-center gap-1 text-[11px] font-medium ${
                    track.vibrateEnabled 
                        ? 'bg-violet-950/20 border-violet-500/30 text-violet-400' 
                        : 'bg-slate-900 border-slate-800 text-slate-500'
                }`;
                vibrateBtn.innerHTML = `<i data-lucide="smartphone" class="w-3.5 h-3.5"></i> <span>Vibrar</span>`;
                vibrateBtn.onclick = () => {
                    track.vibrateEnabled = !track.vibrateEnabled;
                    buildMultiSequencer();
                };

                controlsWrapper.appendChild(soundBtn);
                controlsWrapper.appendChild(vibrateBtn);

                trackHeader.appendChild(titleWrapper);
                trackHeader.appendChild(controlsWrapper);

                const stepGrid = document.createElement('div');
                stepGrid.className = "grid grid-cols-8 md:grid-cols-16 gap-1.5";

                for (let i = 0; i < 16; i++) {
                    const stepVal = track.sequence[i];
                    const stepBtn = document.createElement('button');
                    stepBtn.id = `btn-${trackKey}-${i}`;
                    
                    let stateColors = "bg-slate-900 border-slate-800/80 hover:border-slate-700";
                    if (stepVal === 1) {
                        stateColors = "bg-violet-600 border-violet-500 text-white font-bold";
                    } else if (stepVal === 2) {
                        stateColors = "bg-fuchsia-600 border-fuchsia-500 text-white font-extrabold";
                    }

                    const isBeatStart = i % 4 === 0;
                    const beatBorder = isBeatStart ? "ring-1 ring-slate-700" : "";

                    stepBtn.className = `h-10 rounded-lg text-[10px] flex flex-col items-center justify-center transition-all relative ${stateColors} ${beatBorder} active:scale-90`;
                    
                    stepBtn.innerHTML = `
                        <span class="opacity-60 text-[8px]">${i+1}</span>
                        <div class="w-1.5 h-1.5 rounded-full mt-0.5 ${stepVal > 0 ? 'bg-white' : 'bg-transparent'}"></div>
                        <div id="playing-${trackKey}-${i}" class="absolute -bottom-0.5 left-1/2 -translate-x-1/2 w-1 h-1 rounded-full bg-cyan-400 opacity-0 transition-opacity"></div>
                    `;

                    stepBtn.onclick = () => {
                        track.sequence[i] = (track.sequence[i] + 1) % 3;
                        buildMultiSequencer();
                    };

                    stepGrid.appendChild(stepBtn);
                }

                trackDiv.appendChild(trackHeader);
                trackDiv.appendChild(stepGrid);
                tracksContainer.appendChild(trackDiv);
            });

            lucide.createIcons();
        }

        // Reset sequencer pattern on active tracks
        function clearSequencer() {
            const activeTrackKeys = Object.keys(state.tracks).filter(key => key.startsWith(state.currentGenre));
            activeTrackKeys.forEach(trackKey => {
                state.tracks[trackKey].sequence = Array(16).fill(0);
            });
            buildMultiSequencer();
        }

        // Sound Synthesis Models
        function synthesizeSound(type, isAccent) {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }

            const now = audioCtx.currentTime;

            if (type.includes('tamtam')) {
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                
                osc.type = 'triangle';
                const baseFreq = isAccent ? 85 : 70;
                osc.frequency.setValueAtTime(baseFreq, now);
                osc.frequency.exponentialRampToValueAtTime(30, now + 0.15);
                
                gain.gain.setValueAtTime(0.5, now);
                gain.gain.exponentialRampToValueAtTime(0.001, now + 0.18);
                
                osc.start(now);
                osc.stop(now + 0.2);

            } else if (type.includes('triangulo')) {
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                
                osc.type = 'sine';
                osc.frequency.setValueAtTime(isAccent ? 3100 : 2800, now);
                
                gain.gain.setValueAtTime(0.25, now);
                gain.gain.exponentialRampToValueAtTime(0.001, now + (isAccent ? 0.25 : 0.12));
                
                osc.start(now);
                osc.stop(now + 0.3);

            } else if (type.includes('tamborim')) {
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                
                osc.type = 'sine';
                osc.frequency.setValueAtTime(isAccent ? 480 : 380, now);
                osc.frequency.exponentialRampToValueAtTime(100, now + 0.04);
                
                gain.gain.setValueAtTime(0.4, now);
                gain.gain.exponentialRampToValueAtTime(0.001, now + 0.05);
                
                osc.start(now);
                osc.stop(now + 0.06);

            } else if (type.includes('pandeiro')) {
                const bufferSize = audioCtx.sampleRate * 0.05;
                const buffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
                const data = buffer.getChannelData(0);
                for (let i = 0; i < bufferSize; i++) {
                    data[i] = Math.random() * 2 - 1;
                }
                
                const noise = audioCtx.createBufferSource();
                noise.buffer = buffer;
                
                const filter = audioCtx.createBiquadFilter();
                filter.type = 'bandpass';
                filter.frequency.setValueAtTime(4500, now);
                
                const noiseGain = audioCtx.createGain();
                noiseGain.gain.setValueAtTime(isAccent ? 0.25 : 0.15, now);
                noiseGain.gain.exponentialRampToValueAtTime(0.001, now + 0.04);
                
                noise.connect(filter);
                filter.connect(noiseGain);
                noiseGain.connect(audioCtx.destination);
                
                noise.start(now);
                noise.stop(now + 0.05);
                
                const osc = audioCtx.createOscillator();
                const drumGain = audioCtx.createGain();
                osc.connect(drumGain);
                drumGain.connect(audioCtx.destination);
                osc.frequency.setValueAtTime(160, now);
                drumGain.gain.setValueAtTime(0.2, now);
                drumGain.gain.exponentialRampToValueAtTime(0.001, now + 0.03);
                osc.start(now);
                osc.stop(now + 0.04);
            }
        }

        // Acoustic Haptic generator for iOS Devices (Generates ultra low-end physical movement)
        function playAcousticHaptic(duration) {
            if (!audioCtx) return;
            const now = audioCtx.currentTime;
            
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            
            osc.type = 'sine';
            // 45Hz is perfect to push maximum air and shake phone chassis physically
            osc.frequency.setValueAtTime(45, now); 
            
            gain.gain.setValueAtTime(1.0, now); // Max gain to force physical movement
            gain.gain.exponentialRampToValueAtTime(0.001, now + (duration / 1000));
            
            osc.start(now);
            osc.stop(now + (duration / 1000));
        }

        // Central Metronome engine loop
        function startPlayback() {
            if (state.isPlaying) return;
            state.isPlaying = true;

            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }

            playIcon.setAttribute('data-lucide', 'pause');
            playText.textContent = "Pausar";
            lucide.createIcons();

            const calculateIntervalMs = () => (15 / state.bpm) * 1000;

            const runCycle = () => {
                const previousStep = (state.currentStep - 1 + 16) % 16;
                const activeTrackKeys = Object.keys(state.tracks).filter(key => key.startsWith(state.currentGenre));
                
                let triggerVibrateNow = false;
                let vibrationStrength = 1;

                activeTrackKeys.forEach(trackKey => {
                    const track = state.tracks[trackKey];
                    const hitType = track.sequence[state.currentStep];

                    const prevMarker = document.getElementById(`playing-${trackKey}-${previousStep}`);
                    if (prevMarker) prevMarker.style.opacity = '0';

                    const curMarker = document.getElementById(`playing-${trackKey}-${state.currentStep}`);
                    if (curMarker) curMarker.style.opacity = '1';

                    // Sound trigger
                    if (hitType > 0 && track.soundEnabled && state.soundEnabled) {
                        synthesizeSound(trackKey, hitType === 2);
                    }

                    // Tactile queue accumulator
                    if (hitType > 0 && track.vibrateEnabled) {
                        triggerVibrateNow = true;
                        if (hitType === 2) vibrationStrength = 2;
                    }
                });

                // Trigger physical/acoustic haptics
                if (triggerVibrateNow && state.vibrateEnabled) {
                    const isAccent = vibrationStrength === 2;
                    const duration = isAccent ? state.vibrationBaseDuration * 1.5 : state.vibrationBaseDuration;
                    
                    if (state.isIOS) {
                        // iOS Acoustic Workaround
                        playAcousticHaptic(duration);
                    } else if ("vibrate" in navigator) {
                        // Standard physical Android engine
                        navigator.vibrate(duration);
                    } else {
                        // Fallback browser acoustic
                        playAcousticHaptic(duration);
                    }

                    if (state.flashEnabled) {
                        visualPulse.classList.remove('bg-violet-500/5', 'border-violet-500/20');
                        if (isAccent) {
                            visualPulse.classList.add('bg-fuchsia-500/25', 'border-fuchsia-500/50', 'scale-110');
                        } else {
                            visualPulse.classList.add('bg-violet-500/20', 'border-violet-500/40', 'scale-105');
                        }

                        setTimeout(() => {
                            visualPulse.classList.remove('bg-fuchsia-500/25', 'border-fuchsia-500/50', 'bg-violet-500/20', 'border-violet-500/40', 'scale-110', 'scale-105');
                            visualPulse.classList.add('bg-violet-500/5', 'border-violet-500/20');
                        }, 100);
                    }
                }

                state.currentStep = (state.currentStep + 1) % 16;
                state.playbackInterval = setTimeout(runCycle, calculateIntervalMs());
            };

            runCycle();
        }

        function stopPlayback() {
            if (!state.isPlaying) return;
            state.isPlaying = false;
            
            clearTimeout(state.playbackInterval);
            state.playbackInterval = null;

            // Clear visual indicators
            const activeTrackKeys = Object.keys(state.tracks).filter(key => key.startsWith(state.currentGenre));
            activeTrackKeys.forEach(trackKey => {
                for (let i = 0; i < 16; i++) {
                    const marker = document.getElementById(`playing-${trackKey}-${i}`);
                    if (marker) marker.style.opacity = '0';
                }
            });

            playIcon.setAttribute('data-lucide', 'play');
            playText.textContent = "Iniciar";
            lucide.createIcons();
        }

        // Toggle engine playback
        btnPlay.addEventListener('click', () => {
            if (state.isPlaying) {
                stopPlayback();
            } else {
                startPlayback();
            }
        });

        // Watch BPM adjustment
        bpmInput.addEventListener('change', (e) => {
            let val = parseInt(e.target.value);
            if (isNaN(val) || val < 40) val = 40;
            if (val > 280) val = 280;
            state.bpm = val;
            e.target.value = val;
        });

        // Parse MusicXML file imported from MuseScore
        fileInput.addEventListener('change', (event) => {
            const file = event.target.files[0];
            if (!file) return;

            fileNameLabel.textContent = file.name;
            const reader = new FileReader();

            reader.onload = function(e) {
                const xmlText = e.target.result;
                try {
                    parseMusicXML(xmlText, file.name);
                } catch (err) {
                    alert('Erro ao analisar o arquivo. Verifique se é um arquivo MusicXML válido exportado do MuseScore.');
                    console.error(err);
                }
            };

            reader.readAsText(file);
        });

        // MusicXML Multi-track Parser mapping
        function parseMusicXML(xmlString, originalName) {
            const parser = new DOMParser();
            const xmlDoc = parser.parseFromString(xmlString, "text/xml");

            const notes = xmlDoc.getElementsByTagName('note');
            if (notes.length === 0) {
                alert('Nenhuma nota musical detectada no arquivo MusicXML.');
                return;
            }

            let detectedBPM = 110;
            const soundTags = xmlDoc.getElementsByTagName('sound');
            if (soundTags.length > 0 && soundTags[0].getAttribute('tempo')) {
                detectedBPM = parseInt(soundTags[0].getAttribute('tempo'));
            } else {
                const metronome = xmlDoc.getElementsByTagName('metronome');
                if (metronome.length > 0) {
                    const perMinute = metronome[0].getElementsByTagName('per-minute')[0];
                    if (perMinute) {
                        detectedBPM = parseInt(perMinute.textContent);
                    }
                }
            }

            let divisions = 1;
            const divisionsTags = xmlDoc.getElementsByTagName('divisions');
            if (divisionsTags.length > 0) {
                divisions = parseInt(divisionsTags[0].textContent);
            }

            const sixteenthDuration = divisions / 4;
            
            const impTrack1 = Array(16).fill(0);
            const impTrack2 = Array(16).fill(0);
            const impTrack3 = Array(16).fill(0);
            
            let currentGridTime = 0;

            for (let i = 0; i < notes.length; i++) {
                const note = notes[i];
                const isRest = note.getElementsByTagName('rest').length > 0;
                const durationTag = note.getElementsByTagName('duration')[0];
                const noteDuration = durationTag ? parseInt(durationTag.textContent) : divisions;

                let isAccented = false;
                const notations = note.getElementsByTagName('notations')[0];
                if (notations) {
                    const articulations = notations.getElementsByTagName('articulations')[0];
                    if (articulations && articulations.getElementsByTagName('accent').length > 0) {
                        isAccented = true;
                    }
                }

                let pitchValue = 60;
                const pitchTag = note.getElementsByTagName('pitch')[0];
                if (pitchTag) {
                    const step = pitchTag.getElementsByTagName('step')[0]?.textContent || 'C';
                    const octave = parseInt(pitchTag.getElementsByTagName('octave')[0]?.textContent || '4');
                    
                    const stepOffsets = { 'C':0, 'D':2, 'E':4, 'F':5, 'G':7, 'A':9, 'B':11 };
                    pitchValue = 12 * (octave + 1) + stepOffsets[step];
                }

                if (!isRest) {
                    const stepIndex = Math.round(currentGridTime / sixteenthDuration);
                    if (stepIndex >= 0 && stepIndex < 16) {
                        const hitVal = isAccented ? 2 : 1;
                        
                        if (pitchValue < 60) {
                            impTrack1[stepIndex] = hitVal;
                        } else if (pitchValue >= 72) {
                            impTrack3[stepIndex] = hitVal;
                        } else {
                            impTrack2[stepIndex] = hitVal;
                        }
                    }
                }

                currentGridTime += noteDuration;
                if (currentGridTime >= divisions * 4) {
                    break;
                }
            }

            const activeKeys = Object.keys(state.tracks).filter(key => key.startsWith(state.currentGenre));
            
            if (activeKeys.length >= 3) {
                state.tracks[activeKeys[0]].sequence = impTrack2;
                state.tracks[activeKeys[1]].sequence = impTrack1;
                state.tracks[activeKeys[2]].sequence = impTrack3;
            }

            state.bpm = detectedBPM;
            bpmInput.value = detectedBPM;
            scoreName.textContent = `Importado: ${originalName.replace(/\.[^/.]+$/, "")}`;
            
            buildMultiSequencer();

            importStatus.innerHTML = '<i data-lucide="check-circle" class="w-4 h-4"></i> Partitura polirrítmica mapeada com sucesso!';
            importStatus.classList.remove('hidden');
            lucide.createIcons();
            setTimeout(() => importStatus.classList.add('hidden'), 5000);
        }

        // Info Modal control
        btnInfo.addEventListener('click', () => {
            infoModal.classList.remove('hidden');
        });
        
        const closeModal = () => {
            infoModal.classList.add('hidden');
        };
        
        btnCloseModal.addEventListener('click', closeModal);
        btnCloseModalBtn.addEventListener('click', closeModal);
    </script>
</body>
</html>
