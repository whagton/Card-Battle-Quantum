<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Pokémon Battle Simulator</title>
    <style>
        body, html { margin: 0; padding: 0; height: 100%; background: #0d0d1a; display: flex; justify-content: center; align-items: center; overflow: hidden; }
        
        /* Centralização e Início */
        #start-screen { display: flex; flex-direction: column; align-items: center; justify-content: center; z-index: 10000; }
        
        .pokeball-img { width: 200px; cursor: pointer; transition: transform 0.6s ease; }
        .pokeball-open { transform: scale(0.8) rotate(180deg); opacity: 0; pointer-events: none; }
        
        #btn-iniciar { display: none; margin-top: 20px; padding: 20px 40px; font-size: 1.5rem; cursor: pointer; border: 4px solid #ffcc00; background: #0d0d1a; color: #ffcc00; font-weight: bold; border-radius: 15px; }

        /* Jogo (Centralizado) */
        #game-ui { display: none; text-align: center; width: 100%; max-width: 500px; padding: 20px; }
        .controls { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 20px; }
        .move-btn { padding: 15px; cursor: pointer; background: #252547; color: white; border: 2px solid #3b3b73; border-radius: 8px; font-weight: bold; }
        .arena { display: flex; justify-content: center; align-items: center; gap: 40px; margin-top: 50px; position: relative; }
        .pokemon { width: 150px; height: 150px; transition: all 0.2s ease-out; }
        
        /* Efeitos */
        .flash-overlay { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: yellow; opacity: 0; z-index: 9999; pointer-events: none; }
        @keyframes piscar { 0%, 100% { opacity: 0; } 50% { opacity: 0.7; } }
        .animar-flash { animation: piscar 0.2s; }
        @keyframes shake-extreme { 0% { transform: translate(10px, 10px); } 50% { transform: translate(-10px, -5px); } 100% { transform: translate(10px, 5px); } }
        .tremer-tela-intenso { animation: shake-extreme 0.1s infinite; }
        .atk-anim { transform: translateX(50px) scale(1.1); }
        .super-atk-anim { transform: translateX(150px) scale(1.5) rotate(5deg); z-index: 10; }
    </style>
</head>
<body>

    <div id="start-screen">
        <img src="https://upload.wikimedia.org/wikipedia/commons/5/53/Pok%C3%A9_Ball_icon.svg" 
             class="pokeball-img" id="pokeball" onclick="abrirPokebola()">
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
        function abrirPokebola() {
            document.getElementById('pokeball').classList.add('pokeball-open');
            setTimeout(() => { document.getElementById('btn-iniciar').style.display = 'block'; }, 500);
        }

        function iniciarJogo() {
            document.getElementById('start-screen').style.display = 'none';
            document.getElementById('game-ui').style.display = 'block';
        }

        let pHP = 3000, eHP = 3000;
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
