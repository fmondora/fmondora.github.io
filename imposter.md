---
layout: default
title: Gioco dell'Impostore
permalink: /
---

<div class="container">
    <!-- Schermo Configurazione -->
    <div id="config" class="screen active fade-in">
        <h1>Gioco dell'Impostore</h1>
        <div class="config-grid">
            <label for="numGiocatori">Numero Giocatori (3-10):</label>
            <select id="numGiocatori" aria-label="Numero di giocatori">
                <option value="3">3</option>
                <option value="4">4</option>
                <option value="5">5</option>
                <option value="6">6</option>
                <option value="7">7</option>
                <option value="8">8</option>
                <option value="9">9</option>
                <option value="10">10</option>
            </select>
            <label for="numImpostori">Numero Impostori (1-max 50%):</label>
            <select id="numImpostori" aria-label="Numero di impostori" disabled>
                <option value="1">1</option>
            </select>
        </div>
        <div class="nomi-container" id="nomiContainer"></div>
        <button id="iniziaGioco" disabled aria-label="Inizia il gioco">Inizia Gioco</button>
    </div>

    <!-- Schermo Assegnazione Ruoli -->
    <div id="assign" class="screen">
        <h2 id="turnoInfo">Preparazione...</h2>
        <div id="ruoloInfo" class="overlay-content" style="display: none;">
            <p id="ruoloText"></p>
            <button id="confermaRuolo" aria-label="Conferma e passa al turno successivo">Conferma e Passa</button>
        </div>
    </div>
    <div id="overlay" class="overlay">
        <button id="scopriRuolo" style="padding: 2rem; font-size: 1.5rem;" aria-label="Scopri il tuo ruolo">Scopri il Tuo Ruolo</button>
    </div>

    <!-- Schermo Gioco Principale -->
    <div id="game" class="screen">
        <h2>Discussione in Corso</h2>
        <div class="parola-segreta" id="parolaDisplay" role="status">La parola segreta è: <span id="parolaSpan"></span></div>
        <div class="timer" id="timerDisplay"></div>
        <button id="mostraImpostori" disabled aria-label="Mostra gli impostori">Mostra Impostori (Fine Discussione)</button>
    </div>

    <!-- Schermo Fine Gioco -->
    <div id="end" class="screen">
        <h2 class="vittoria" id="vittoriaText"></h2>
        <p id="impostoriList"></p>
        <button id="nuovaPartita" aria-label="Inizia una nuova partita">Nuova Partita</button>
    </div>

    <!-- Toast per messaggi -->
    <div id="toast" class="toast" role="alert"></div>
</div>

<style>
    /* Reset e base */
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }
    body {
        font-family: Arial, sans-serif;
        background-color: #0a0a0a;
        color: #ffffff;
        line-height: 1.6;
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
        padding: 1rem;
    }
    .container {
        max-width: 800px;
        width: 100%;
        text-align: center;
    }
    h1 {
        color: #ff4444;
        margin-bottom: 2rem;
    }
    /* Schermi nascosti per default */
    .screen {
        display: none;
    }
    .screen.active {
        display: block;
    }
    /* Configurazione */
    .config-grid {
        display: grid;
        gap: 1rem;
        margin-bottom: 2rem;
    }
    select, input {
        padding: 0.5rem;
        background: #1a1a1a;
        border: 1px solid #333;
        color: #fff;
        border-radius: 4px;
        width: 100%;
        max-width: 200px;
    }
    .nomi-container {
        display: grid;
        gap: 0.5rem;
        margin-top: 1rem;
    }
    .nome-input {
        display: flex;
        justify-content: space-between;
        align-items: center;
        background: #1a1a1a;
        padding: 0.5rem;
        border-radius: 4px;
    }
    .nome-input input {
        flex: 1;
        margin-right: 0.5rem;
    }
    button {
        background: #ff4444;
        color: #fff;
        border: none;
        padding: 1rem 2rem;
        border-radius: 4px;
        cursor: pointer;
        font-size: 1rem;
        transition: background 0.3s;
        margin: 0.5rem;
    }
    button:hover:not(:disabled) {
        background: #cc0000;
    }
    button:disabled {
        background: #333;
        cursor: not-allowed;
    }
    /* Assegnazione ruoli */
    .overlay {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.9);
        backdrop-filter: blur(5px);
        display: none;
        justify-content: center;
        align-items: center;
        z-index: 1000;
    }
    .overlay.active {
        display: flex;
    }
    .overlay-content {
        text-align: center;
        background: #1a1a1a;
        padding: 2rem;
        border-radius: 8px;
        max-width: 400px;
    }
    .ruolo-cit {
        color: #00ff00;
    }
    .ruolo-imp {
        color: #ff4444;
    }
    /* Gioco principale */
    .parola-segreta {
        font-size: 2rem;
        color: #00ff00;
        margin: 2rem 0;
        font-weight: bold;
    }
    .timer {
        font-size: 1.5rem;
        color: #ff4444;
        margin: 1rem 0;
    }
    /* Fine gioco */
    .vittoria {
        font-size: 2rem;
        margin: 2rem 0;
    }
    .vittoria.cit {
        color: #00ff00;
    }
    .vittoria.imp {
        color: #ff4444;
    }
    /* Responsive */
    @media (min-width: 600px) {
        .config-grid {
            grid-template-columns: 1fr 1fr;
        }
        .nomi-container {
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
        }
    }
    /* Animazioni */
    .fade-in {
        animation: fadeIn 0.5s ease-in;
    }
    @keyframes fadeIn {
        from { opacity: 0; }
        to { opacity: 1; }
    }
    /* Toast per errori */
    .toast {
        position: fixed;
        top: 1rem;
        right: 1rem;
        background: #ff4444;
        color: #fff;
        padding: 1rem;
        border-radius: 4px;
        display: none;
        z-index: 1001;
    }
    .toast.active {
        display: block;
        animation: fadeIn 0.3s;
    }
    /* Accessibilità */
    button:focus, input:focus, select:focus {
        outline: 2px solid #ff4444;
    }
    [role="status"] {
        aria-live: polite;
    }
</style>

<script>
    // Oggetto stato del gioco
    const gameState = {
        numGiocatori: 3,
        numImpostori: 1,
        nomiGiocatori: [],
        parolaSegreta: '',
        ruoli: [],
        currentPlayerIndex: 0,
        timer: null,
        timerInterval: null
    };

    // Array di parole segrete (almeno 50 parole semplici in italiano)
    const parole = [
        'casa', 'cane', 'albero', 'fiore', 'libro', 'acqua', 'sole', 'luna', 'stella', 'fiume',
        'montagna', 'mare', 'cielo', 'terra', 'fuoco', 'vento', 'pioggia', 'neve', 'ghiaccio', 'sabbia',
        'pietra', 'legno', 'metallo', 'vetro', 'carta', 'penna', 'tavolo', 'sedia', 'porta', 'finestra',
        'letto', 'lampada', 'orologio', 'telefono', 'computer', 'auto', 'treno', 'aereo', 'barca', 'bici',
        'palla', 'scarpa', 'cappello', 'guanto', 'sciarpa', 'giacca', 'pantaloni', 'maglia', 'scarpe', 'borsa',
        'chiave', 'moneta', 'biglietto', 'foto', 'mappa', 'busso', 'cucchiaio', 'forchetta', 'coltello', 'piatto'
    ];

    // Funzione per mostrare toast
    function showToast(message, duration = 3000) {
        const toast = document.getElementById('toast');
        toast.textContent = message;
        toast.classList.add('active');
        setTimeout(() => toast.classList.remove('active'), duration);
    }

    // Funzione per generare nomi default
    function generaNomiDefault(num) {
        return Array.from({length: num}, (_, i) => `Giocatore ${i + 1}`);
    }

    // Funzione per aggiornare dropdown impostori
    function updateImpostoriDropdown() {
        const numG = parseInt(document.getElementById('numGiocatori').value);
        const selectI = document.getElementById('numImpostori');
        selectI.innerHTML = '';
        const maxI = Math.floor(numG / 2);
        for (let i = 1; i <= maxI; i++) {
            const opt = document.createElement('option');
            opt.value = i;
            opt.textContent = i;
            selectI.appendChild(opt);
        }
        selectI.disabled = false;
    }

    // Funzione per popolare nomi
    function popolaNomi() {
        const numG = parseInt(document.getElementById('numGiocatori').value);
        gameState.nomiGiocatori = generaNomiDefault(numG);
        const container = document.getElementById('nomiContainer');
        container.innerHTML = '';
        gameState.nomiGiocatori.forEach((nome, index) => {
            const div = document.createElement('div');
            div.className = 'nome-input';
            div.innerHTML = `
                <input type="text" value="${nome}" placeholder="Nome ${index + 1}" aria-label="Nome giocatore ${index + 1}" data-index="${index}">
                <button type="button" onclick="rimuoviNome(${index})" aria-label="Rimuovi giocatore ${index + 1}">X</button>
            `;
            container.appendChild(div);
        });
        // Aggiungi event listener per update nomi
        document.querySelectorAll('.nome-input input').forEach(input => {
            input.addEventListener('input', (e) => {
                const idx = parseInt(e.target.dataset.index);
                gameState.nomiGiocatori[idx] = e.target.value || generaNomiDefault(1)[0];
                checkConfigValid();
            });
        });
    }

    // Funzione per rimuovere nome (ma mantieni numG fisso, solo edit)
    function rimuoviNome(index) {
        // Non rimuovere, solo clear - per semplicità, disabilita rimozione
        showToast('Non è possibile rimuovere giocatori durante la config.');
    }

    // Controlla validità config
    function checkConfigValid() {
        const numG = parseInt(document.getElementById('numGiocatori').value);
        const numI = parseInt(document.getElementById('numImpostori').value);
        const btn = document.getElementById('iniziaGioco');
        const valid = numG >= 3 && numI >= 1 && numI <= numG / 2 && gameState.nomiGiocatori.every(n => n.trim());
        btn.disabled = !valid;
    }

    // Inizializza config
    function initConfig() {
        document.getElementById('numGiocatori').addEventListener('change', () => {
            updateImpostoriDropdown();
            popolaNomi();
            checkConfigValid();
        });
        updateImpostoriDropdown();
        popolaNomi();
        checkConfigValid();

        document.getElementById('numImpostori').addEventListener('change', checkConfigValid);
        document.getElementById('iniziaGioco').addEventListener('click', startAssignRoles);
    }

    // Genera parola segreta
    function generaParola() {
        const idx = Math.floor(Math.random() * parole.length);
        return parole[idx];
    }

    // Shuffle Fisher-Yates
    function shuffle(array) {
        for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
        }
        return array;
    }

    // Assegna ruoli
    function assegnaRuoli() {
        const ruoli = Array(gameState.numGiocatori).fill('Cittadino');
        for (let i = 0; i < gameState.numImpostori; i++) {
            ruoli[i] = 'Impostore';
        }
        gameState.ruoli = shuffle([...ruoli]);
        console.log('Ruoli assegnati:', gameState.ruoli); // Debug
        console.log('Parola segreta:', gameState.parolaSegreta); // Debug
    }

    // Avvia assegnazione ruoli
    async function startAssignRoles() {
        gameState.numGiocatori = parseInt(document.getElementById('numGiocatori').value);
        gameState.numImpostori = parseInt(document.getElementById('numImpostori').value);
        gameState.nomiGiocatori = Array.from(document.querySelectorAll('.nome-input input'), input => input.value.trim() || generaNomiDefault(1)[0]);
        gameState.parolaSegreta = generaParola();
        assegnaRuoli();
        gameState.currentPlayerIndex = 0;

        // Salva in localStorage per reload
        localStorage.setItem('gameState', JSON.stringify(gameState));

        switchScreen('assign');
        await assignRolesSequentially();
    }

    // Assegnazione sequenziale
    async function assignRolesSequentially() {
        for (let i = 0; i < gameState.numGiocatori; i++) {
            gameState.currentPlayerIndex = i;
            document.getElementById('turnoInfo').textContent = `Turno di ${gameState.nomiGiocatori[i]} / ${gameState.numGiocatori}`;
            document.getElementById('turnoInfo').setAttribute('aria-live', 'polite');

            // Mostra overlay
            document.getElementById('overlay').classList.add('active');
            document.getElementById('scopriRuolo').focus();

            // Aspetta click su scopri
            await waitForClick('scopriRuolo');

            // Nascondi overlay, mostra ruolo
            document.getElementById('overlay').classList.remove('active');
            const ruoloInfo = document.getElementById('ruoloInfo');
            const ruoloText = document.getElementById('ruoloText');
            ruoloInfo.style.display = 'block';
            ruoloInfo.classList.add('fade-in');
            const ruolo = gameState.ruoli[i];
            const nome = gameState.nomiGiocatori[i];
            if (ruolo === 'Cittadino') {
                ruoloText.innerHTML = `<span class="ruolo-cit">Sei un Cittadino, ${nome}!</span><br>La parola è: <strong>${gameState.parolaSegreta}</strong>`;
                ruoloText.setAttribute('aria-live', 'polite');
            } else {
                ruoloText.innerHTML = `<span class="ruolo-imp">Sei un Impostore, ${nome}!</span><br>Indovina la parola...`;
                ruoloText.setAttribute('aria-live', 'polite');
            }
            document.getElementById('confermaRuolo').focus();

            // Aspetta conferma
            await waitForClick('confermaRuolo');

            // Nascondi ruolo
            ruoloInfo.style.display = 'none';
            ruoloText.innerHTML = '';

            // Delay 2-3 sec
            await new Promise(resolve => setTimeout(resolve, 2500));
        }

        // Fine assegnazione
        document.getElementById('overlay').classList.remove('active');
        startGame();
    }

    // Utility per aspettare click
    function waitForClick(id) {
        return new Promise(resolve => {
            const btn = document.getElementById(id);
            const listener = () => {
                btn.removeEventListener('click', listener);
                resolve();
            };
            btn.addEventListener('click', listener);
        });
    }

    // Avvia gioco principale
    function startGame() {
        switchScreen('game');
        document.getElementById('parolaSpan').textContent = gameState.parolaSegreta; // Visibile per host/cittadini
        document.getElementById('mostraImpostori').disabled = false;
        document.getElementById('mostraImpostori').addEventListener('click', revealImpostors);

        // Timer opzionale 5 min
        let tempo = 300; // 5 min in sec
        const timerDisplay = document.getElementById('timerDisplay');
        gameState.timerInterval = setInterval(() => {
            const min = Math.floor(tempo / 60);
            const sec = tempo % 60;
            timerDisplay.textContent = `Tempo rimanente: ${min}:${sec.toString().padStart(2, '0')}`;
            tempo--;
            if (tempo < 0) {
                clearInterval(gameState.timerInterval);
                revealImpostors(); // Auto-rivelazione a fine tempo
            }
        }, 1000);
    }

    // Rivela impostori
    function revealImpostors() {
        clearInterval(gameState.timerInterval);
        const impostoriNomi = gameState.ruoli.map((ruolo, i) => ruolo === 'Impostore' ? gameState.nomiGiocatori[i] : null).filter(Boolean);
        // Semplifica: Vittoria basata su... per ora, rivela e reset. Regola: Impostori vincono se indovinano, ma offline, quindi solo rivela.
        const vittoriaText = document.getElementById('vittoriaText');
        vittoriaText.textContent = impostoriNomi.length > 0 ? 'Vittoria Impostori!' : 'Vittoria Cittadini!'; // Semplificato
        vittoriaText.className = `vittoria ${impostoriNomi.length > 0 ? 'imp' : 'cit'}`;
        document.getElementById('impostoriList').textContent = `I veri impostori sono: ${impostoriNomi.join(', ')}`;
        switchScreen('end');
        document.getElementById('nuovaPartita').addEventListener('click', resetGame);
    }

    // Reset gioco
    function resetGame() {
        Object.assign(gameState, {
            numGiocatori: 3,
            numImpostori: 1,
            nomiGiocatori: [],
            parolaSegreta: '',
            ruoli: [],
            currentPlayerIndex: 0,
            timer: null,
            timerInterval: null
        });
        localStorage.removeItem('gameState');
        switchScreen('config');
        initConfig();
    }

    // Switch screen
    function switchScreen(id) {
        document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
        document.getElementById(id).classList.add('active');
    }

    // Event listeners globali
    document.getElementById('scopriRuolo').addEventListener('click', () => {
        document.getElementById('overlay').classList.remove('active');
        // Trigger reveal nel loop
    });
    document.getElementById('confermaRuolo').addEventListener('click', () => {
        document.getElementById('ruoloInfo').style.display = 'none';
        // Trigger next nel loop
    });

    // Init
    function init() {
        // Carica da localStorage se presente
        const saved = localStorage.getItem('gameState');
        if (saved) {
            Object.assign(gameState, JSON.parse(saved));
            // Resume se in mezzo
            if (gameState.currentPlayerIndex < gameState.numGiocatori && gameState.ruoli.length > 0) {
                switchScreen('assign');
                assignRolesSequentially(); // Resume
            } else if (gameState.ruoli.length > 0) {
                startGame();
            }
        } else {
            initConfig();
        }
    }

    // Suoni opzionali (semplice beep se supportato)
    function playBeep() {
        if ('AudioContext' in window) {
            const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            const oscillator = audioCtx.createOscillator();
            oscillator.type = 'sine';
            oscillator.frequency.setValueAtTime(800, audioCtx.currentTime);
            oscillator.connect(audioCtx.destination);
            oscillator.start();
            setTimeout(() => oscillator.stop(), 200);
        }
    }
    // Aggiungi a buttons se vuoi: addEventListener('click', playBeep);

    // Avvia app
    init();
</script>
