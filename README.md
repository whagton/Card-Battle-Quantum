<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Pokémon Battle Simulator</title>
    <style>
        body { background: #0d0d1a; color: #fff; font-family: sans-serif; display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100vh; overflow: hidden; }
        .ray-overlay { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(255, 255, 0, 0); z-index: 9999; pointer-events: none; }
        .ray-active { animation: flash 0.3s; }
        @keyframes flash { 0%, 100% { background: transparent; } 50% { background: rgba(255, 255, 0, 0.5); } }
        .pokemon { width: 150px; height: 150px; }
        #game-ui { display: none; text-align: center; }
        .controls { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 20px; }
        .move-btn { padding: 10px; cursor: pointer; background: #252547; color: white; border: 2px solid #3b3b73; border-radius: 8px; }
        #pokeball { cursor: pointer; font-size: 60px; transition: transform 0.3s; }
    </style>
</head>
<body>

    <div class="ray-overlay" id="lightningRay"></div>

    <!-- TELA INICIAL -->
    <div id="start-screen">
        <div id="pokeball" onclick="iniciarJogo()">🔘</div>
        <p>Clique na Pokébola</p>
    </div>

    <!-- TELA DO JOGO -->
    <div id="game-ui">
        <div class="hp-info">
            Pikachu: <span id="p-hp">3000</span> | Mewtwo: <span id="e-hp">3000</span>
        </div>
        <div style="display: flex; gap: 50px;">
            <img src="https://img.pokemondb.net/sprites/black-white/anim/back-normal/pikachu.gif" class="pokemon">
            <img src="https://img.pokemondb.net/sprites/black-white/anim/normal/mewtwo.gif" class="pokemon">
        </div>
        <div class="controls">
            <button class="move-btn" onclick="atacar(500, true)">TROVÃO</button>
            <button class="move-btn" onclick="atacar(150, false)">Choque</button>
            <button class="move-btn" onclick="atacar(250, false)">Cauda Ferro</button>
            <button class="move-btn" onclick="atacar(350, false)">Investida</button>
        </div>
    </div>

    <audio id="sfx-click" src="https://www.myinstants.com/media/sounds/poke-ball-click.mp3"></audio>
    <audio id="sfx-thunder" src="https://www.myinstants.com/media/sounds/thunder-sound-effect.mp3"></audio>

    <script>
        let pHP = 3000, eHP = 3000;

        function iniciarJogo() {
            document.getElementById('sfx-click').play();
            document.getElementById('start-screen').style.display = 'none';
            document.getElementById('game-ui').style.display = 'block';
        }

        function atacar(dano, isSuper) {
            // Ataque do Pikachu
            if (isSuper) {
                document.getElementById('sfx-thunder').play();
                const ray = document.getElementById('lightningRay');
                ray.classList.add('ray-active');
                setTimeout(() => ray.classList.remove('ray-active'), 300);
            }
            eHP = Math.max(0, eHP - dano);
            document.getElementById('e-hp').innerText = eHP;

            // Contra-ataque do Mewtwo (atrasado)
            if (eHP > 0) {
                setTimeout(() => {
                    pHP = Math.max(0, pHP - 200);
                    document.getElementById('p-hp').innerText = pHP;
                    if(pHP === 0) alert("Mewtwo venceu!");
                }, 1000);
            } else {
                alert("Você venceu o Mewtwo!");
            }
        }
    </script>
</body>
</html>
