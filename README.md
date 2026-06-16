<script>
    // Variáveis Globais
    let playerHP = 3000;
    let enemyHP = 3000;
    const maxHP = 3000;

    // Garante que o estado inicial esteja correto
    document.addEventListener("DOMContentLoaded", () => {
        document.getElementById('start-screen').style.display = 'flex';
        document.getElementById('select-screen').style.display = 'none';
        document.getElementById('game-ui').style.display = 'none';
    });

    function abrirPokebola() {
        document.getElementById('top').classList.add('open-top');
        document.getElementById('bottom').classList.add('open-bottom');
        // Delay para aparecer o botão
        setTimeout(() => {
            document.getElementById('btn-iniciar').style.display = 'block';
        }, 500);
    }

    function irParaSelecao() {
        document.getElementById('start-screen').style.display = 'none';
        document.getElementById('select-screen').style.display = 'block';
        
        // Carrega os Pokémons se o container estiver vazio
        const container = document.getElementById('options-container');
        if (container.innerHTML === "") {
            const pokes = {
                'PIKACHU': 25, 'CHARIZARD': 6, 'GRENINJA': 658, 'LUCARIO': 448, 
                'GENGAR': 94, 'BLASTOISE': 9, 'MEWTWO': 150, 'EEVEE': 133
            };
            for(let name in pokes) {
                container.innerHTML += `
                    <div class="poke-card" onclick="iniciarBatalha('${name}')">
                        <img src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokes[name]}.png">
                        <span>${name}</span>
                    </div>`;
            }
        }
    }

    function iniciarBatalha(nome) {
        document.getElementById('select-screen').style.display = 'none';
        document.getElementById('game-ui').style.display = 'block';
        // Lógica de setar nomes e imagens
        document.getElementById('player-name-txt').innerText = nome;
        // ... restante da lógica de batalha
    }
</script>
