<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Pokémon Battle Simulator</title>
    <style>
        body, html { margin: 0; padding: 0; height: 100%; background: #0d0d1a; color: white; font-family: sans-serif; overflow: hidden; }

        /* Centralização total da Pokébola e Botão */
        #start-screen { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); display: flex; flex-direction: column; align-items: center; z-index: 10000; }
        .pokeball-container { position: relative; width: 150px; height: 150px; cursor: pointer; }
        .p-top, .p-bottom { width: 150px; height: 75px; background: red; border: 6px solid #000; position: absolute; transition: 0.8s; }
        .p-top { top: 0; border-radius: 75px 75px 0 0; z-index: 2; }
        .p-bottom { bottom: 0; background: white; border-radius: 0 0 75px 75px; z-index: 1; }
        .open-top { transform: translateY(-80px); }
        .open-bottom { transform: translateY(80px); }
        #btn-iniciar { display: none; margin-top: 20px; padding: 15px 30px; cursor: pointer; background: #ffcc00; border: none; font-weight: bold; border-radius: 10px; }

        /* Arena */
        #game-ui { display: none; width: 100%; height: 100%; position: relative; }
        .mewtwo-container { position: absolute; top: 15%; right: 20%; text-align: center; }
        .pikachu-container { position: absolute; bottom: 15%; left: 20%; text-align: center; }
        .pokemon { width: 150px; }
        
        /* Ataques fixos acima do Pikachu */
        .controls { position: absolute; bottom: 35%; left: 20%; display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        .move-btn { padding: 10px 20px; cursor: pointer; background: #444; color: white; border: 2px solid #fff; }

        /* Flash Amarelo e Preto */
        .flash-overlay { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; opacity: 0; pointer-events: none; z-index: 9999; }
        @keyframes flash-fast { 0%, 100% { opacity: 0; } 25% { background: yellow; opacity: 0.6; } 50% { background: black; opacity: 0.6; } 75% { background: yellow; opacity: 0.6; } }
        .animar-flash { animation: flash-fast 0.1s 4; }
        
        /* Trepidação */
        @keyframes shake { 0%, 100% { transform: translate(0,0); } 25% { transform: translate(-10px, -10px); } 50% { transform: translate(10px, 10px); } 75% { transform: translate(-10px, 10px); } }
        .tremer-tela { animation: shake 0.1s 4; }
    </style>
</head>
<body>

    <div id="start-screen">
        <div class="pokeball-container" onclick="abrirPokebola()">
            <div class="p-top" id="top"></div>
            <div class="p-bottom" id="bottom"></div>
        </div>
        <button id="btn-iniciar" onclick="iniciarJogo()">INICIAR BATALHA</button>
    </div>

    <div class="flash-overlay" id="flash"></div>

    <div id="game-ui">
        <div class="mewtwo-container">
            <div>HP: 3000</div>
            <img src="https://img.pokemondb.net/sprites/black-white/anim/normal/mewtwo.gif" class="pokemon">
        </div>
        
        <div class="pikachu-container">
            <div class="controls">
                <button class="move-btn" onclick="executarAtaque()">TROVÃO</button>
                <button class="move-btn" onclick="executarAtaque()">CHOQUE</button>
            </div>
            <div>HP: 3000</div>
            <img src="https://img.pokemondb.net/sprites/black-white/anim/back-normal/pikachu.gif" class="pokemon">
        </div>
    </div>

    <audio id="sfx" src="https://www.myinstants.com/media/sounds/thunder-sound-effect.mp3"></audio>

    <script>
        function abrirPokebola() {
            document.getElementById('top').classList.add('open-top');
            document.getElementById('bottom').classList.add('open-bottom');
            setTimeout(() => { document.getElementById('btn-iniciar').style.display = 'block'; }, 600);
        }

        function iniciarJogo() {
            document.getElementById('start-screen').style.display = 'none';
            document.getElementById('game-ui').style.display = 'block';
        }

        function executarAtaque() {
            const flash = document.getElementById('flash');
            const game = document.getElementById('game-ui');
            document.getElementById('sfx').play();
            
            flash.classList.add('animar-flash');
            game.classList.add('tremer-tela');
            
            setTimeout(() => {
                flash.classList.remove('animar-flash');
                game.classList.remove('tremer-tela');
            }, 500);
        }
    </script>
</body>
</html>
