<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Pokémon Battle Simulator - Elite</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { background: #0d0d1a; color: #fff; font-family: sans-serif; overflow: hidden; transition: background 0.1s; }
        
        /* Efeito de Tela Cheia */
        .ray-overlay {
            position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
            background: rgba(255, 255, 0, 0); z-index: 9999; pointer-events: none;
        }
        .ray-active { animation: lightningFlicker 0.15s infinite; }
        @keyframes lightningFlicker {
            0% { background: rgba(255, 255, 0, 0.4); }
            50% { background: rgba(255, 255, 255, 0.6); }
            100% { background: rgba(0, 191, 255, 0.4); }
        }
        @keyframes superShake {
            0%, 100% { transform: translate(0, 0); }
            20% { transform: translate(-25px, 25px); }
            40% { transform: translate(25px, -25px); }
        }
        .shake-super-effect { animation: superShake 0.1s infinite; }

        .battle-container { width: 100vw; height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; }
        .controls { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        .move-btn { padding: 15px 25px; cursor: pointer; background: #252547; color: white; border: 2px solid #3b3b73; border-radius: 8px; font-weight: bold; }
        .hp-info { font-size: 1.5rem; margin-bottom: 20px; }
    </style>
</head>
<body>

    <div class="ray-overlay" id="lightningRay"></div>
    
    <div class="battle-container">
        <div class="hp-info">
            Pikachu: <span id="p-hp">3000</span> | Mewtwo: <span id="e-hp">3000</span>
        </div>
        <div class="controls" id="controls">
            <button class="move-btn" onclick="executarAtaque(500, true)">TROVÃO DIVINO</button>
            <button class="move-btn" onclick="executarAtaque(200, false)">Choque do Trovão</button>
            <button class="move-btn" onclick="executarAtaque(350, false)">Cauda de Ferro</button>
            <button class="move-btn" onclick="executarAtaque(450, false)">Investida Volt</button>
        </div>
        <br>
        <div id="pokeball" style="cursor:pointer; font-size: 50px;" onclick="iniciarJogo()">🔘 CLIQUE PARA INICIAR</div>
    </div>

    <!-- Áudios -->
    <audio id="sfx-click" src="https://www.myinstants.com/media/sounds/poke-ball-click.mp3"></audio>
    <audio id="sfx-thunder" src="https://www.myinstants.com/media/sounds/thunder-sound-effect.mp3"></audio>

    <script>
        let pHP = 3000;
        let eHP = 3000;

        function iniciarJogo() {
            document.getElementById('sfx-click').play();
            alert("A batalha começou!");
        }

        function executarAtaque(dano, isSuper) {
            if (isSuper) {
                // Toca som de trovão
                document.getElementById('sfx-thunder').play();
                
                // Efeito de tela cheia
                const ray = document.getElementById('lightningRay');
                ray.classList.add('ray-active');
                document.body.classList.add('shake-super-effect');

                setTimeout(() => {
                    ray.classList.remove('ray-active');
                    document.body.classList.remove('shake-super-effect');
                }, 2000);
            }

            eHP -= dano;
            if (eHP < 0) eHP = 0;
            document.getElementById('e-hp').innerText = eHP;
        }
    </script>
</body>
</html>
