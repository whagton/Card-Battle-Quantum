<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Pokémon Battle - Ready</title>
    <style>
        body { background: #0d0d1a; color: #fff; font-family: sans-serif; display: flex; flex-direction: column; align-items: center; min-height: 100vh; margin: 0; overflow: hidden; }
        
        /* Flash e Trepidação */
        .flash-overlay { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: yellow; opacity: 0; z-index: 9999; pointer-events: none; }
        @keyframes piscar { 0%, 100% { opacity: 0; } 50% { opacity: 0.7; } }
        .animar-flash { animation: piscar 0.2s; }

        @keyframes shake-extreme {
            0% { transform: translate(10px, 10px) rotate(0deg); }
            50% { transform: translate(-10px, -5px) rotate(5deg); }
            100% { transform: translate(10px, 5px) rotate(-5deg); }
        }
        .tremer-tela-intenso { animation: shake-extreme 0.1s infinite; }

        /* Estilo da Pokébola */
        #start-screen { position: fixed; width: 100%; height: 100%; display: flex; flex-direction: column; justify-content: center; align-items: center; z-index: 10000; background: #0d0d1a; }
        .pokeball { width: 150px; height: 150px; background: radial-gradient(circle at 50% 50%, #ff4500 50%, #fff 50%, #fff 60%, #000 60%); border: 8px solid #000; border-radius: 50%; cursor: pointer; transition: transform 0.5s; }
        .open { transform: scale(1.2) rotate(180deg); opacity: 0.5; }
        
        #btn-iniciar { display: none; margin-top: 20px; padding: 15px 30px; font-size: 1.2rem; cursor: pointer; border: none; background: #ffcc00; color: #000; font-weight: bold; border-radius: 10px; }

        /* Jogo */
        #game-ui { display: none; text-align: center; width: 100%; max-width: 500px; padding: 20px; }
        .controls { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 20px; }
        .move-btn { padding: 15px; cursor: pointer; background: #252547; color: white; border: 2px solid #3b3b73; border-radius: 8px; font-weight: bold; }
        .arena { display: flex; justify-content: center; align-items: center; gap: 40px; margin-top: 50px; position: relative; }
        .pokemon { width: 150px; height: 150px; transition: all 0.2s ease-out; }
        
        .atk-anim { transform: translateX(50px) scale(1.1); }
        .super-atk-anim { transform: translateX(150px) scale(1.5) rotate(5deg); z-index: 10; }
    </style>
</head>
<body>

    <div id="start-screen">
        <div class="pokeball" id="pokeball" onclick="abrirPokebola()"></div>
        <button id="btn-iniciar" onclick="iniciarJogo()">INICIAR BATALHA</button>
    </div>

    <div class="flash-overlay" id="flash"></div>

    <div id="game-ui">
        <div class="hp-info">Pikachu: <span id="p-hp">3000</span> | Mewtwo: <span id="e-hp">3000</span></div>
        <div class="controls">
            <button class="move-btn" style="border-color: #ff0000;" onclick="atacar(600, true)">TROVÃO (SUPER)</button>
            <button class="move-btn" onclick="atacar(150, false)">Choque</button>
            <button class="move-btn" onclick="atacar(250, false)">Raio</button>
            <button class="move-btn" onclick="atacar(350, false)">Investida</button>
        </div>
        <div class="arena" id="arena">
            <img id="pikachu" src="https://img.pokemondb.net/sprites/black-white/anim/back-normal/pikachu.gif" class="pokemon">
            <img id="mewtwo" src="https://img.pokemondb.net/sprites/black-white/anim/normal/mewtwo.gif" class="pokemon">
        </div>
    </div>

    <audio id="sfx-thunder" src="https://www.myinstants.com/media/sounds/thunder-sound-effect.mp3"></audio>
    <audio id="sfx-shock" src="https://www.myinstants.com/media/sounds/electric-shock.mp3"></audio>

    <script>
        let pHP = 3000, eHP = 3000;

        function abrirPokebola() {
            document.getElementById('pokeball').classList.add('open');
            document.getElementById('btn-iniciar').style.display = 'block';
        }

        function iniciarJogo() {
            document.getElementById('start-screen').style.display = 'none';
            document.getElementById('game-ui').style.display = 'block';
        }

        function atacar(dano, isSuper) {
            const pikachu = document.getElementById('pikachu');
            const flash = document.getElementById('flash');
            const arena = document.getElementById('arena');

            if (isSuper) {
                const s = document.getElementById('sfx-thunder');
                s.currentTime = 0; s.volume = 1.0; s.play();
                pikachu.classList.add('super-atk-anim');
                
                let count = 0;
                const int = setInterval(() => {
                    flash.classList.add('animar-flash');
                    arena.classList.add('tremer-tela-intenso');
                    setTimeout(() => { flash.classList.remove('animar-flash'); arena.classList.remove('tremer-tela-intenso'); }, 150);
                    count++;
                    if (count >= 4) clearInterval(int);
                }, 250);
                setTimeout(() => pikachu.classList.remove('super-atk-anim'), 600);
            } else {
                const s = document.getElementById('sfx-shock');
                s.currentTime = 0; s.volume = 1.0; s.play();
                pikachu.classList.add('atk-anim');
                flash.classList.add('animar-flash');
                setTimeout(() => flash.classList.remove('animar-flash'), 150);
                setTimeout(() => pikachu.classList.remove('atk-anim'), 300);
            }

            eHP = Math.max(0, eHP - dano);
            document.getElementById('e-hp').innerText = eHP;
            if (eHP <= 0) alert("Você venceu o Mewtwo!");
        }
    </script>
</body>
</html>
