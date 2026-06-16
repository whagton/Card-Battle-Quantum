<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Pokémon Battle Simulator</title>
    </head>
<body>

    <div id="start-screen">
        <div class="pokeball-container" onclick="abrirPokebola()">
            <div class="p-top" id="top"></div>
            <div class="p-bottom" id="bottom"></div>
        </div>
        <button id="btn-iniciar" onclick="irParaSelecao()">INICIAR BATALHA</button>
    </div>

    <div id="select-screen">
        <div class="select-title">ESCOLHA SEU POKÉMON</div>
        <div class="pokemon-options" id="options-container"></div>
        <div class="scoreboard" id="wins-display">Vitórias nesta máquina: 0</div>
    </div>

    <div class="flash-overlay" id="flash"></div>

    <div id="game-ui">
        <div class="battle-arena" id="arena">
            <div class="enemy-container" id="enemy-box">
                <div class="hud">
                    <div class="hp-text"><span id="enemy-name-txt">INIMIGO</span> <span id="enemy-hp-txt">3000/3000</span></div>
                    <div class="hp-bar-container"><div class="hp-bar-fill" id="enemy-hp-bar"></div></div>
                </div>
                <img src="" class="pokemon" id="enemy-img">
            </div>
            
            <div id="battle-log">Um oponente poderoso bloqueia o caminho!</div>

            <div class="player-container" id="player-box">
                <img src="" class="pokemon" id="player-img">
                <div class="hud">
                    <div class="hp-text"><span id="player-name-txt">POKÉMON</span> <span id="player-hp-txt">3000/3000</span></div>
                    <div class="hp-bar-container"><div class="hp-bar-fill" id="player-hp-bar"></div></div>
                </div>
            </div>

            <div class="controls-container">
                <div class="controls" id="action-panel">
                    <button class="move-btn" onclick="atacar('Rapido')">Ataque Rápido</button>
                    <button class="move-btn" onclick="atacar('Forte')">Ataque Forte</button>
                    <button class="move-btn heal-btn" onclick="curar()">Curar (HP)</button>
                    <button class="move-btn ult-btn" onclick="usarUlt()">Especial</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        const pokes = {
            'PIKACHU': 25, 'CHARIZARD': 6, 'GRENINJA': 658, 'LUCARIO': 448, 
            'GENGAR': 94, 'BLASTOISE': 9, 'MEWTWO': 150, 'EEVEE': 133
        };

        function abrirPokebola() {
            document.getElementById('top').classList.add('open-top');
            document.getElementById('bottom').classList.add('open-bottom');
            document.getElementById('btn-iniciar').style.display = 'block';
        }

        function irParaSelecao() {
            document.getElementById('start-screen').style.display = 'none';
            document.getElementById('select-screen').style.display = 'block';
            const container = document.getElementById('options-container');
            for(let name in pokes) {
                container.innerHTML += `
                    <div class="poke-card" onclick="iniciarBatalha('${name}')">
                        <img src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokes[name]}.png">
                        <span>${name}</span>
                    </div>`;
            }
        }

        function iniciarBatalha(nome) {
            document.getElementById('select-screen').style.display = 'none';
            document.getElementById('game-ui').style.display = 'block';
            document.getElementById('player-name-txt').innerText = nome;
            document.getElementById('player-img').src = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokes[nome]}.png`;
            document.getElementById('enemy-img').src = 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/151.png';
            document.getElementById('enemy-name-txt').innerText = 'MEW';
        }

        function atacar(tipo) {
            document.getElementById('battle-log').innerText = "Você usou " + tipo + "!";
        }
        
        function curar() { document.getElementById('battle-log').innerText = "Você recuperou vida!"; }
        function usarUlt() { document.getElementById('battle-log').innerText = "Ataque Especial!"; }
    </script>
</body>
</html>
