<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Pokémon Battle Simulator</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { background: #0d0d1a; color: #fff; font-family: sans-serif; overflow: hidden; }
        
        /* Efeito de Tela Cheia para o Super Ataque */
        .ray-overlay {
            position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
            background: rgba(255, 255, 255, 0); z-index: 9999; pointer-events: none;
        }
        .ray-active { animation: lightningFlicker 0.2s infinite; }
        @keyframes lightningFlicker {
            0%, 100% { background: rgba(255, 255, 0, 0.3); }
            50% { background: rgba(255, 255, 255, 0.5); }
        }
        @keyframes superShake {
            0%, 100% { transform: translate(0, 0); }
            20% { transform: translate(-20px, 20px); }
            40% { transform: translate(20px, -20px); }
        }
        .shake-super-effect { animation: superShake 0.1s infinite; }

        .battle-container { width: 100vw; height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; }
        .move-btn { padding: 20px; margin: 10px; cursor: pointer; background: #252547; color: white; border: 2px solid #3b3b73; border-radius: 10px; }
    </style>
</head>
<body>

    <div class="ray-overlay" id="lightningRay"></div>
    
    <div class="battle-container">
        <h1 id="status">Pikachu vs Mewtwo</h1>
        <div id="controls">
            <button class="move-btn" onclick="superAtaque()">SUPER ATAQUE (Pikachu)</button>
        </div>
    </div>

    <!-- O ÁUDIO DO MYINSTANTS -->
    <audio id="myinstants" src="https://www.myinstants.com/media/sounds/pikachu-earrape-audio-cutter.mp3"></audio>

    <script>
        const som = document.getElementById('myinstants');
        const overlay = document.getElementById('lightningRay');
        const body = document.body;

        function superAtaque() {
            // Tocar som
            som.currentTime = 0;
            som.play();

            // Efeitos na tela toda
            overlay.classList.add('ray-active');
            body.classList.add('shake-super-effect');

            // Resetar após 2 segundos
            setTimeout(() => {
                overlay.classList.remove('ray-active');
                body.classList.remove('shake-super-effect');
            }, 2000);
        }
    </script>
</body>
</html>
