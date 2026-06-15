<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Pokémon Battle Simulator - Elite</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { background: #0d0d1a; color: #fff; font-family: sans-serif; height: 100vh; display: flex; align-items: center; justify-content: center; overflow: hidden; }
        
        .ray-overlay { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(255, 255, 0, 0); z-index: 9999; pointer-events: none; }
        .ray-active { animation: flash 0.3s; }
        @keyframes flash { 0%, 100% { background: transparent; } 50% { background: rgba(255, 255, 0, 0.6); } }
        
        #game-ui { display: none; text-align: center; }
        .pokemon { width: 150px; height: 150px; }
        .controls { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 20px; }
        .move-btn { padding: 12px; cursor: pointer; background: #252547; color: white; border: 2px solid #3b3b73; border-radius: 8px; font-weight: bold; }
        
        #start-screen { text-align: center; }
        #pokeball { cursor: pointer; font-size: 80px; transition: transform 0.2s; }
        #pokeball:hover { transform: scale(1.1); }
    </style>
</head>
<body>

    <div class="ray-overlay" id="lightningRay"></div>

    <div id="start-screen">
        <div id="pokeball" onclick="iniciarJogo()">🔘</div>
        <p>Clique na Pokébola</p>
    </div>

    <div id="game-ui">
        <div class="hp-info">
            Pikachu: <span id="p-hp">3000</span> | Mewtwo: <span id="e-hp">3000</span>
        </div>
        <div style="display: flex; gap: 50px; justify-content: center;">
            <img src="https://img.pokemondb.net/sprites/black-white/anim/back-normal/pikachu.gif" class="pokemon">
            <img src="https://img.pokemondb.net/sprites/black-white/anim/normal/mewtwo.gif" class="pokemon">
        </div>
        <div class="controls">
            <button class="move-btn" style="border-color: #ff0000;" onclick="atacar(600, true)">TROVÃO</button>
            <button class="move-btn" onclick="atacar(150, false)">Choque</button>
            <button class="move-btn" onclick="atacar(250, false)">Raio</button>
            <button class="move-btn" onclick="atacar(350, false)">Investida</button>
        </div>
    </div>

    <!-- Áudios -->
    <audio id="sfx-click" src="https://www.myinstants.com/media/sounds/poke-ball-click.mp3"></audio>
    <audio id="sfx-thunder" src="https://www.myinstants.com/media/sounds/thunder-sound-effect.mp3"></audio>
    <audio id="sfx-shock" src="https://www.myinstants.com/media/sounds/electric-shock.mp3"></audio>
    <audio id="sfx-zap" src="https://www.myinstants.com/media/sounds/zap_8.mp3"></audio>

    <script>
        let pHP = 3000, eHP = 3000;

        function iniciarJogo() {
            document.getElementById('sfx-click').play();
            document.getElementById('start-screen').style.display = 'none';
            document.getElementById('game-ui').style.display = 'block';
        }

        function atacar(dano, isSuper) {
            const ray = document.getElementById('lightningRay');
            
            if (isSuper) {
                // Som de Trovão bem alto
                const thunder = document.getElementById('sfx-thunder');
                thunder.volume = 1.0;
                thunder.play();
            } else {
                // Som de Choque ou Raio aleatório
                const sons = ['sfx-shock', 'sfx-zap'];
                const sfx = document.getElementById(sons[Math.floor(Math.random() * sons.length)]);
                sfx.volume = 0.8;
                sfx.play();
            }

            // Efeito visual
            ray.classList.add('ray-active');
            setTimeout(() => ray.classList.remove('ray-active'), 400);

            // Dano
            eHP = Math.max(0, eHP - dano);
            document.getElementById('e-hp').innerText = eHP;

            // Contra-ataque Mewtwo
            if (eHP > 0) {
                setTimeout(() => {
                    pHP = Math.max(0, pHP - 200);
                    document.getElementById('p-hp').innerText = pHP;
                    if (pHP === 0) alert("Mewtwo venceu!");
                }, 800);
            } else {
                alert("Você venceu o Mewtwo!");
            }
        }
    </script>
</body>
</html>
