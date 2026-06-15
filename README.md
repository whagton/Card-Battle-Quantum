<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pokémon Battle Simulator - Elite</title>
    <style>
        /* [Seu CSS permanece o mesmo] */
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: sans-serif; background: #0d0d1a; color: #fff; min-height: 100vh; display: flex; align-items: center; justify-content: center; overflow: hidden; position: relative; }
        .splash-container { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: radial-gradient(circle at center, #151530 0%, #050510 100%); z-index: 1000; display: flex; flex-direction: column; align-items: center; justify-content: center; transition: opacity 0.5s; }
        .battle-container { width: 100%; max-width: 700px; height: 520px; background: linear-gradient(180deg, #14142f 0%, #07070e 100%); border: 4px solid #333366; border-radius: 15px; display: flex; flex-direction: column; position: relative; opacity: 0; transition: opacity 0.6s; }
        .status-hud { background: rgba(0,0,0,0.7); border: 2px solid #444; border-radius: 10px; padding: 10px; width: 240px; }
        .status-hud.enemy { align-self: flex-start; margin-left: 20px; }
        .status-hud.player { align-self: flex-end; margin-right: 20px; }
        .hp-bar-bg { width: 100%; height: 12px; background: #333; border-radius: 6px; overflow: hidden; }
        .hp-bar-fill { width: 100%; height: 100%; background: #00ff66; transition: width 0.4s; }
        .pokemon-sprite { position: absolute; height: 130px; }
        .enemy-sprite { top: 15px; right: 80px; }
        .player-sprite { bottom: 120px; left: 80px; height: 110px; }
        .battle-log { height: 75px; background: #0f0f1d; border-top: 3px solid #2d2d54; padding: 12px; }
        .control-panel { height: 120px; background: #181826; display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; padding: 12px; }
        .move-btn { background: #252547; border: 2px solid #3b3b73; color: white; border-radius: 8px; cursor: pointer; }
    </style>
</head>
<body>

    <audio id="sfx-thunder" src="https://www.myinstants.com/media/sounds/thunder-sound-effect.mp3"></audio>
    <audio id="sfx-shock" src="https://www.myinstants.com/media/sounds/electric-shock.mp3"></audio>
    <audio id="sfx-zap" src="https://www.myinstants.com/media/sounds/zap_8.mp3"></audio>

    <div class="splash-container" id="splashContainer">
        <h1>POKÉMON ARENA</h1>
        <button onclick="iniciarJogo(event)" style="padding: 20px 40px; cursor:pointer;">INICIAR BATALHA</button>
    </div>

    <div class="battle-container" id="gameContainer">
        <div class="screen-area">
            <div class="status-hud enemy">
                <p>Mewtwo</p>
                <div class="hp-bar-bg"><div class="hp-bar-fill" id="enemyHpBar"></div></div>
            </div>
            <img src="https://img.pokemondb.net/sprites/black-white/anim/normal/mewtwo.gif" class="pokemon-sprite enemy-sprite">
            <img src="https://img.pokemondb.net/sprites/black-white/anim/back-normal/pikachu.gif" class="pokemon-sprite player-sprite">
            <div class="status-hud player">
                <p>Pikachu</p>
                <div class="hp-bar-bg"><div class="hp-bar-fill" id="playerHpBar"></div></div>
            </div>
        </div>
        <div class="battle-log" id="logBox">Vai, Pikachu!</div>
        <div class="control-panel" id="actionPanel"></div>
    </div>

    <script>
        let enemy = { hp: 3000, maxHp: 3000 };
        let player = { hp: 3000, maxHp: 3000 };

        function iniciarJogo(e) {
            document.getElementById('splashContainer').style.display = 'none';
            document.getElementById('gameContainer').style.opacity = '1';
            gerarBotoes();
        }

        function tocarSom(isSuper) {
            if (isSuper) {
                const s = document.getElementById('sfx-thunder');
                s.volume = 1.0; 
                s.currentTime = 0;
                s.play();
            } else {
                const sons = ['sfx-shock', 'sfx-zap'];
                const s = document.getElementById(sons[Math.floor(Math.random() * sons.length)]);
                s.volume = 0.7;
                s.currentTime = 0;
                s.play();
            }
        }

        function gerarBotoes() {
            const painel = document.getElementById('actionPanel');
            const ataques = [
                { nome: "TROVÃO (SUPER)", dano: 600, super: true },
                { nome: "Choque", dano: 150, super: false },
                { nome: "Raio", dano: 250, super: false },
                { nome: "Investida", dano: 350, super: false }
            ];
            painel.innerHTML = '';
            ataques.forEach(a => {
                let btn = document.createElement('button');
                btn.className = 'move-btn';
                btn.innerText = a.nome;
                btn.onclick = () => atacar(a);
                painel.appendChild(btn);
            });
        }

        function atacar(golpe) {
            tocarSom(golpe.super);
            enemy.hp = Math.max(0, enemy.hp - golpe.dano);
            document.getElementById('enemyHpBar').style.width = (enemy.hp / 3000 * 100) + "%";
            
            if (enemy.hp > 0) {
                setTimeout(() => {
                    player.hp = Math.max(0, player.hp - 200);
                    document.getElementById('playerHpBar').style.width = (player.hp / 3000 * 100) + "%";
                }, 500);
            } else {
                alert("Você venceu!");
            }
        }
    </script>
</body>
</html>
