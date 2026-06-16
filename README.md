<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Pokémon Battle Simulator</title>
    <style>
        body, html { margin: 0; padding: 0; height: 100%; background: #0d0d1a; color: white; font-family: sans-serif; overflow: hidden; display: flex; justify-content: center; align-items: center; }
        #start-screen { position: absolute; z-index: 1000; text-align: center; }
        #select-screen, #game-ui { display: none; text-align: center; }
        .pokemon-options { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; }
        .poke-card { border: 2px solid #444; padding: 10px; cursor: pointer; background: #222; }
        .poke-card img { width: 60px; }
    </style>
</head>
<body>

    <div id="start-screen">
        <div class="pokeball-container" onclick="abrirPokebola()">
            <div class="p-top" id="top"></div>
            <div class="p-bottom" id="bottom"></div>
        </div>
        <button id="btn-iniciar" onclick="irParaSelecao()" style="display:none;">INICIAR BATALHA</button>
    </div>

    <div id="select-screen">
        <div class="select-title">ESCOLHA SEU POKÉMON</div>
        <div class="pokemon-options" id="options-container"></div>
    </div>

    <div id="game-ui">
        <h1>BATALHA EM CURSO</h1>
        <div id="battle-log">O jogo começou!</div>
        <button onclick="location.reload()">SAIR</button>
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
            document.getElementById('battle-log').innerText = "Você escolheu " + nome + "! A luta vai começar.";
        }
    </script>
</body>
</html>
