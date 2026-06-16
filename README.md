<script>
    // Variáveis de estado do jogo
    let playerHP = 3000;
    let enemyHP = 3000;
    const maxHP = 3000;

    const pokes = {
        'PIKACHU': 25, 'CHARIZARD': 6, 'GRENINJA': 658, 'LUCARIO': 448, 
        'GENGAR': 94, 'BLASTOISE': 9, 'MEWTWO': 150, 'EEVEE': 133
    };

    // ... (Mantenha as funções abrirPokebola e irParaSelecao como estavam) ...

    function iniciarBatalha(nome) {
        document.getElementById('select-screen').style.display = 'none';
        document.getElementById('game-ui').style.display = 'block';
        document.getElementById('player-name-txt').innerText = nome;
        document.getElementById('player-img').src = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokes[nome]}.png`;
        document.getElementById('enemy-img').src = 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/151.png';
        document.getElementById('enemy-name-txt').innerText = 'MEW';
    }

    function atacar(tipo) {
        let dano = (tipo === 'Forte') ? 300 : 150;
        enemyHP -= dano;
        if (enemyHP < 0) enemyHP = 0;
        
        atualizarUI();
        document.getElementById('battle-log').innerText = "Você causou " + dano + " de dano!";

        if (enemyHP > 0) {
            // Oponente contra-ataca após 1 segundo
            setTimeout(contraAtaque, 1000);
        } else {
            document.getElementById('battle-log').innerText = "Você venceu!";
        }
    }

    function contraAtaque() {
        let danoInimigo = 100;
        playerHP -= danoInimigo;
        if (playerHP < 0) playerHP = 0;
        
        atualizarUI();
        document.getElementById('battle-log').innerText = "Mew contra-atacou e deu " + danoInimigo + " de dano!";
        
        if (playerHP === 0) {
            document.getElementById('battle-log').innerText = "Você foi derrotado...";
        }
    }

    function atualizarUI() {
        // Atualiza barras de vida
        document.getElementById('enemy-hp-bar').style.width = (enemyHP / maxHP * 100) + "%";
        document.getElementById('player-hp-bar').style.width = (playerHP / maxHP * 100) + "%";
        document.getElementById('enemy-hp-txt').innerText = enemyHP + "/" + maxHP;
        document.getElementById('player-hp-txt').innerText = playerHP + "/" + maxHP;
    }
    
    // Adicione as funções curar e usarUlt com lógica similar se desejar
</script>
