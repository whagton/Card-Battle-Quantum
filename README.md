<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pokémon Battle Simulator - Gengar vs Mewtwo</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #0d0d1a;
            color: #fff;
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
            position: relative;
            transition: background 0.15s ease;
        }

        /* TELA INICIAL DA POKÉBOLA (SPLASH) */
        .splash-container {
            position: fixed;
            top: 0; left: 0; width: 100vw; height: 100vh;
            background: radial-gradient(circle at center, #151530 0%, #050510 100%);
            z-index: 1000;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            transition: opacity 0.5s ease, transform 0.5s ease;
        }

        .splash-title {
            font-size: 2.2rem;
            color: #00ffcc;
            letter-spacing: 4px;
            margin-bottom: 40px;
            text-shadow: 0 0 15px rgba(0, 255, 204, 0.4);
            text-align: center;
            font-weight: 800;
        }

        .pokeball-wrapper {
            position: relative;
            width: 200px;
            height: 200px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .pokeball-part {
            position: absolute;
            width: 100%;
            height: 50%;
            left: 0;
            border: 6px solid #111;
            transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }
        
        .pokeball-top {
            top: 0;
            background: #ff3355;
            border-radius: 100px 100px 0 0;
            border-bottom: 3px solid #111;
            transform-origin: bottom;
        }

        .pokeball-bottom {
            bottom: 0;
            background: #f0f0f0;
            border-radius: 0 0 100px 100px;
            border-top: 3px solid #111;
            transform-origin: top;
        }

        .pokeball-center-btn {
            position: absolute;
            width: 50px;
            height: 50px;
            background: #fff;
            border: 6px solid #111;
            border-radius: 50%;
            z-index: 10;
            transition: all 0.4s ease;
        }

        .pokeball-wrapper:hover .pokeball-top, 
        .pokeball-wrapper.open .pokeball-top { transform: translateY(-45px); }
        .pokeball-wrapper:hover .pokeball-bottom, 
        .pokeball-wrapper.open .pokeball-bottom { transform: translateY(45px); }
        .pokeball-wrapper:hover .pokeball-center-btn, .pokeball-wrapper.open .pokeball-center-btn { transform: scale(0.6); opacity: 0; }

        .start-btn {
            position: absolute;
            padding: 12px 24px;
            background: linear-gradient(135deg, #00ffcc 0%, #00bfff 100%);
            border: none;
            color: #050510;
            font-size: 1.1rem;
            font-weight: bold;
            border-radius: 25px;
            letter-spacing: 1px;
            cursor: pointer;
            box-shadow: 0 0 20px rgba(0, 255, 204, 0.6);
            opacity: 0;
            transform: scale(0.8);
            transition: all 0.3s ease;
            z-index: 5;
            pointer-events: none;
            white-space: nowrap;
        }

        .pokeball-wrapper:hover .start-btn, .pokeball-wrapper.open .start-btn { opacity: 1; transform: scale(1.05); pointer-events: auto; }

        /* CONTAINER PRINCIPAL DO JOGO */
        .battle-container {
            width: 100%;
            max-width: 700px;
            height: 520px;
            background: linear-gradient(180deg, #14142f 0%, #07070e 100%);
            border: 4px solid #333366;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.7);
            display: flex;
            flex-direction: column;
            position: relative;
            opacity: 0;
            transition: opacity 0.6s ease;
        }

        /* EFEITOS ESPECIAIS DE FLASH NA ARENA */
        .flash-shadow { background: #3a005c !important; }
        .flash-dark { background: #1f1401 !important; }
        .flash-hypnosis { background: #004d26 !important; }
        .flash-damage { background: #520000 !important; }

        /* ANIMAÇÃO DE TREMER A TELA */
        @keyframes shake {
            0%, 100% { transform: translate(0, 0); }
            10%, 30%, 50%, 70%, 90% { transform: translate(-8px, 4px); }
            20%, 40%, 60%, 80% { transform: translate(8px, -4px); }
        }
        .shake-effect { animation: shake 0.35s ease-in-out; }

        /* 1. TELA DE EXIBIÇÃO (ARENA) */
        .screen-area {
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 20px;
            position: relative;
            background: radial-gradient(circle at bottom, rgba(50,50,100,0.15) 0%, transparent 70%);
        }

        .status-hud {
            background: rgba(0, 0, 0, 0.7);
            border: 2px solid #444488;
            border-radius: 10px;
            padding: 10px;
            width: 240px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
            z-index: 5;
            transition: all 0.3s;
        }
        .status-hud.enemy { align-self: flex-start; margin-left: 20px; }
        .status-hud.player { align-self: flex-end; margin-right: 20px; }
        
        .poke-info { display: flex; justify-content: space-between; font-weight: bold; font-size: 0.9rem; margin-bottom: 5px; }
        .level { color: #ffcc00; }

        .hp-bar-bg { width: 100%; height: 12px; background: #333; border-radius: 6px; overflow: hidden; border: 1px solid #555; }
        .hp-bar-fill { width: 100%; height: 100%; background: #00ff66; transition: width 0.4s ease; }
        .hp-text { font-size: 0.75rem; text-align: right; margin-top: 3px; color: #aaa; }

        /* SPRITES DOS POKÉMONS (GIFS HISTÓRICOS) */
        .pokemon-sprite {
            position: absolute;
            height: 130px;
            object-fit: contain;
            transition: transform 0.2s ease, filter 0.2s ease;
        }
        
        /* Posição Mewtwo (Inimigo - de frente) */
        .enemy-sprite {
            top: 15px;
            right: 80px;
            animation: floatEnemy 3.5s ease-in-out infinite;
        }

        /* Posição Gengar (Jogador - de costas/perfil) */
        .player-sprite {
            bottom: 120px;
            left: 80px;
            animation: floatPlayer 3s ease-in-out infinite;
        }

        /* Animações de Movimento Natural */
        @keyframes floatEnemy {
            0%, 100% { transform: translateY(0) scale(1); }
            50% { transform: translateY(-8px) scale(1.02); }
        }
        @keyframes floatPlayer {
            0%, 100% { transform: translateY(0) scale(1); }
            50% { transform: translateY(-5px) scale(1.03); }
        }

        /* Efeitos de Ataque Individuais nos Sprites */
        .attack-dash-left { transform: translate(-50px, 30px) scale(1.1); }
        .attack-dash-right { transform: translate(50px, -30px) scale(1.1); }
        .damage-blink { filter: matrix(1, 0, 0, 1, 0, 0) brightness(5) sepia(1) hue-rotate(-50deg); }

        .shield-effect { border-color: #00bfff !important; box-shadow: 0 0 15px #00bfff !important; }

        /* 2. LOG DE TEXTO */
        .battle-log {
            height: 75px;
            background: #0f0f1d;
            border-top: 3px solid #2d2d54;
            padding: 12px 20px;
            font-size: 0.95rem;
            color: #00ffcc;
            display: flex;
            align-items: center;
            line-height: 1.4;
        }

        /* 3. PAINEL DE COMANDOS */
        .control-panel {
            height: 120px;
            background: #181826;
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            padding: 12px;
            border-top: 2px solid #2d2d54;
        }

        .move-btn {
            background: #252547;
            border: 2px solid #3b3b73;
            color: white;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
        .move-btn:hover:not(:disabled) { background: #353566; border-color: #00ffcc; transform: scale(1.02); }
        .move-btn:disabled { background: #12121f; color: #444; border-color: #222; cursor: not-allowed; }
        .move-btn span { font-size: 0.7rem; color: #aaa; font-weight: normal; margin-top: 2px; }

        .type-ghost { border-left: 6px solid #8a41ff; }
        .type-dark { border-left: 6px solid #f0932b; }
        .type-status { border-left: 6px solid #2ecc71; }

        /* CRÉDITOS DO DESENVOLVEDOR */
        .dev-signature {
            position: fixed;
            bottom: 12px;
            right: 15px;
            font-size: 0.75rem;
            font-family: monospace;
            color: rgba(0, 255, 204, 0.4);
            letter-spacing: 1.5px;
            text-shadow: 0 0 8px rgba(0, 255, 204, 0.2);
            user-select: none;
            z-index: 999;
        }
    </style>
</head>
<body>

    <!-- TELA DA POKÉBOLA INTERATIVA -->
    <div class="splash-screen" id="splashScreen">
        <div class="splash-container" id="splashContainer">
            <h1 class="splash-title">POKÉMON ARENA</h1>
            
            <div class="pokeball-wrapper" id="pokeball" onclick="togglePokeballMobile()">
                <div class="pokeball-part pokeball-top"></div>
                <div class="pokeball-center-btn"></div>
                <button class="start-btn" onclick="iniciarJogo(event)">INICIAR BATALHA</button>
                <div class="pokeball-part pokeball-bottom"></div>
            </div>
        </div>
    </div>

    <!-- TABULEIRO DO JOGO -->
    <div class="battle-container" id="gameContainer">
        
        <div class="screen-area" id="arenaArea">
            <!-- HUD Inimigo -->
            <div class="status-hud enemy" id="enemyHud">
                <div class="poke-info">
                    <span>Mewtwo</span>
                    <span class="level">Lv.100</span>
                </div>
                <div class="hp-bar-bg">
                    <div class="hp-bar-fill" id="enemyHpBar"></div>
                </div>
                <div class="hp-text" id="enemyHpText">1000 / 1000</div>
            </div>

            <!-- SPRITES REAIS ANIMADOS (GIFS DA CONSOLE POKÉMON) -->
            <img src="https://img.pokemondb.net/sprites/black-white/anim/normal/mewtwo.gif" class="pokemon-sprite enemy-sprite" id="mewtwoSprite" alt="Mewtwo">
            <img src="https://img.pokemondb.net/sprites/black-white/anim/back-normal/gengar.gif" class="pokemon-sprite player-sprite" id="gengarSprite" alt="Gengar">

            <!-- HUD Jogador -->
            <div class="status-hud player" id="playerHud">
                <div class="poke-info">
                    <span>Gengar</span>
                    <span class="level">Lv.100</span>
                </div>
                <div class="hp-bar-bg">
                    <div class="hp-bar-fill" id="playerHpBar"></div>
                </div>
                <div class="hp-text" id="playerHpText">1000 / 1000</div>
            </div>
        </div>

        <!-- Histórico Narrativo -->
        <div class="battle-log" id="logBox">
            Um Mewtwo lendário bloqueia seu caminho! O que o Gengar vai fazer?
        </div>

        <!-- Painel de Ataques -->
        <div class="control-panel" id="actionPanel">
            <button class="move-btn type-ghost" onclick="jogarTurno('shadow_ball')">
                Shadow Ball
                <span>Dano: 150 | Efeito Roxo</span>
            </button>
            <button class="move-btn type-dark" onclick="jogarTurno('dark_pulse')">
                Dark Pulse
                <span>Crítico (35%) | Pulso Escuro</span>
            </button>
            <button class="move-btn type-status" onclick="jogarTurno('hypnosis')">
                Hypnosis (Dreno)
                <span>Dano: 80 + Regerar | Flash Verde</span>
            </button>
            <button class="move-btn type-status" onclick="jogarTurno('substitute')">
                Substitute (Escudo)
                <span>Cria Barreira Ciano</span>
            </button>
        </div>
    </div>

    <!-- Assinatura -->
    <div class="dev-signature">BY: DEV WHAGTON</div>

    <script>
        let player = { nome: "Gengar", hp: 1000, maxHp: 1000, escudo: false };
        let enemy = { nome: "Mewtwo", hp: 1000, maxHp: 1000, escudo: false };
        let batalhaAtiva = true;

        const gengarImg = document.getElementById('gengarSprite');
        const mewtwoImg = document.getElementById('mewtwoSprite');
        const gameContainer = document.getElementById('gameContainer');

        function togglePokeballMobile() {
            document.getElementById('pokeball').classList.toggle('open');
        }

        function iniciarJogo(event) {
            event.stopPropagation(); 
            const splash = document.getElementById('splashContainer');
            splash.style.opacity = '0';
            splash.style.transform = 'scale(1.2)';
            
            setTimeout(() => {
                document.getElementById('splashScreen').style.display = 'none';
                gameContainer.style.opacity = '1';
            }, 500);
        }

        function dispararFlashVisual(classeFlash) {
            document.body.classList.add(classeFlash);
            setTimeout(() => document.body.classList.remove(classeFlash), 200);
        }

        function aplicarDanoFisico(alvoSprite) {
            gameContainer.classList.add('shake-effect');
            alvoSprite.classList.add('damage-blink');
            dispararFlashVisual('flash-damage');
            
            setTimeout(() => {
                gameContainer.classList.remove('shake-effect');
                alvoSprite.classList.remove('damage-blink');
            }, 350);
        }

        function jogarTurno(ataqueEscolhido) {
            if (!batalhaAtiva) return;
            travarBotoes(true);

            let danoJogador = 0;
            let logMsg = "";

            // Animação de investida do Gengar
            gengarImg.classList.add('attack-dash-right');
            setTimeout(() => gengarImg.classList.remove('attack-dash-right'), 250);

            // PROCESSAMENTO DOS GOLPES DO JOGADOR
            if (ataqueEscolhido === 'shadow_ball') {
                danoJogador = 150;
                logMsg = `Gengar usou Shadow Ball!`;
                setTimeout(() => dispararFlashVisual('flash-shadow'), 150);
            } 
            else if (ataqueEscolhido === 'dark_pulse') {
                if (Math.random() < 0.35) {
                    danoJogador = 260;
                    logMsg = `💥 Golpe Crítico! Gengar usou Dark Pulse devastador!`;
                } else {
                    danoJogador = 120;
                    logMsg = `Gengar usou Dark Pulse contínuo!`;
                }
                setTimeout(() => dispararFlashVisual('flash-dark'), 150);
            } 
            else if (ataqueEscolhido === 'hypnosis') {
                danoJogador = 80;
                let cura = 120;
                player.hp = Math.min(player.maxHp, player.hp + cura);
                logMsg = `👁️ Gengar usou Hypnosis! Sugou energia vital e regenerou +${cura} de HP!`;
                setTimeout(() => dispararFlashVisual('flash-hypnosis'), 150);
            } 
            else if (ataqueEscolhido === 'substitute') {
                player.escudo = true;
                document.getElementById('playerHud').classList.add('shield-effect');
                logMsg = `🛡️ Gengar moldou energia e ativou um Substitute protetor!`;
            }

            // Aplica efeitos no Mewtwo se houver dano
            setTimeout(() => {
                if (danoJogador > 0) {
                    enemy.hp = Math.max(0, enemy.hp - danoJogador);
                    aplicarDanoFisico(mewtwoImg);
                }
                atualizarBarras();
                document.getElementById('logBox').innerText = logMsg;
            }, 200);

            // Verifica Vitória
            if (enemy.hp === 0) {
                setTimeout(() => {
                    document.getElementById('logBox').innerText = `🏆 Incrível! O poderoso Mewtwo desmaiou. Gengar venceu!`;
                    batalhaAtiva = false;
                }, 1500);
                return;
            }

            // TURNO DO MEWTWO (IA COMBATENTE)
            setTimeout(() => {
                if (!batalhaAtiva) return;

                const golpesMewtwo = [
                    { nome: "Psystrike", dano: 160, flash: "flash-shadow" },
                    { nome: "Aura Sphere", dano: 130, flash: "flash-hypnosis" },
                    { nome: "Psychic", dano: 145, flash: "flash-dark" }
                ];

                let golpeSorteado = golpesMewtwo[Math.floor(Math.random() * golpesMewtwo.length)];
                let msgMewtwo = "";

                // Animação de investida do Mewtwo para frente
                mewtwoImg.classList.add('attack-dash-left');
                setTimeout(() => mewtwoImg.classList.remove('attack-dash-left'), 250);

                setTimeout(() => {
                    dispararFlashVisual(golpeSorteado.flash);
                    
                    if (player.escudo) {
                        msgMewtwo = `Mewtwo utilizou ${golpeSorteado.nome}, mas a barreira do Substitute absorveu o impacto!`;
                        player.escudo = false;
                        document.getElementById('playerHud').classList.remove('shield-effect');
                    } else {
                        player.hp = Math.max(0, player.hp - golpeSorteado.dano);
                        msgMewtwo = `Mewtwo lançou um contra-ataque de ${golpeSorteado.nome} causando -${golpeSorteado.dano} HP!`;
                        aplicarDanoFisico(gengarImg);
                    }

                    atualizarBarras();
                    document.getElementById('logBox').innerText = msgMewtwo;

                    // Verifica Derrota
                    if (player.hp === 0) {
                        setTimeout(() => {
                            document.getElementById('logBox').innerText = `💀 Gengar esgotou suas energias e desmaiou... Fim de jogo.`;
                            batalhaAtiva = false;
                        }, 1200);
                    } else {
                        setTimeout(() => {
                            if(batalhaAtiva) {
                                document.getElementById('logBox').innerText = "O que o Gengar vai fazer?";
                                travarBotoes(false);
                            }
                        }, 1200);
                    }
                }, 200);

            }, 2000);
        }

        function atualizarBarras() {
            let pctPlayer = (player.hp / player.maxHp) * 100;
            const barPlayer = document.getElementById('playerHpBar');
            barPlayer.style.width = pctPlayer + "%";
            document.getElementById('playerHpText').innerText = `${player.hp} / ${player.maxHp}`;
            barPlayer.style.backgroundColor = pctPlayer < 20 ? "#ff3333" : pctPlayer < 50 ? "#ffcc00" : "#00ff66";

            let pctEnemy = (enemy.hp / enemy.maxHp) * 100;
            const barEnemy = document.getElementById('enemyHpBar');
            barEnemy.style.width = pctEnemy + "%";
            document.getElementById('enemyHpText').innerText = `${enemy.hp} / ${enemy.maxHp}`;
            barEnemy.style.backgroundColor = pctEnemy < 20 ? "#ff3333" : pctEnemy < 50 ? "#ffcc00" : "#00ff66";
        }

        function travarBotoes(status) {
            const botoes = document.querySelectorAll('.move-btn');
            botoes.forEach(btn => btn.disabled = status);
        }
    </script>
</body>
</html>
