// ==========================================
// VARIÁVEIS GLOBAIS E CONFIGURAÇÕES
// ==========================================
let playerHP = 3000;
let enemyHP = 3000;
const maxHP = 3000;
let playerPokemon = "";
let enemyPokemon = "";
let turnoJogador = true;

const pokesData = {
    'PIKACHU':   { id: 25,  moves: ['Choque do Trovão', 'Investida', 'Cauda de Ferro', 'Relâmpago'] },
    'CHARIZARD': { id: 6,   moves: ['Lança-Chamas', 'Garras de Metal', 'Ataque de Asa', 'Superaquecimento'] },
    'GRENINJA':  { id: 658, moves: ['Shuriken de Água', 'Corte Noturno', 'Pulso d\'Água', 'Multiplicação'] },
    'LUCARIO':   { id: 448, moves: ['Esfera de Aura', 'Soco Incrementado', 'Velocidade Extrema', 'Pulso Sombrio'] },
    'GENGAR':    { id: 94,  moves: ['Bola Sombria', 'Pulso Sombrio', 'Bomba de Lodo', 'Pesadelo'] },
    'BLASTOISE': { id: 9,   moves: ['Jato d\'Água', 'Quebra-Crânio', 'Hidrocanhão', 'Mordida'] },
    'MEWTWO':    { id: 150, moves: ['Psíquico', 'Esfera de Aura', 'Bola Sombria', 'Golpe Psíquico'] },
    'EEVEE':     { id: 133, moves: ['Investida', 'Ataque Rápido', 'Mordida', 'Estrela Cadente'] }
};

// ==========================================
// LÓGICA DE BATALHA (Refatorada)
// ==========================================

function iniciarBatalha(nomeEscolhido) {
    playerHP = maxHP;
    enemyHP = maxHP;
    turnoJogador = true;
    playerPokemon = nomeEscolhido;

    const nomesDisponiveis = Object.keys(pokesData).filter(p => p !== playerPokemon);
    enemyPokemon = nomesDisponiveis[Math.floor(Math.random() * nomesDisponiveis.length)];

    // Atualização de UI
    document.getElementById('select-screen').style.display = 'none';
    document.getElementById('game-ui').style.display = 'block';

    document.getElementById('player-name-txt').innerText = playerPokemon;
    document.getElementById('enemy-name-txt').innerText = enemyPokemon;

    // Correção da URL da API (Template Literal com $)
    const baseUrl = "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/";
    document.getElementById('player-sprite').src = `${baseUrl}${pokesData[playerPokemon].id}.png`;
    document.getElementById('enemy-sprite').src = `${baseUrl}${pokesData[enemyPokemon].id}.png`;

    atualizarBarrasVida();
    gerarBotoesAtaque();
    logMensagem(`Um ${enemyPokemon} selvagem apareceu! Vai, ${playerPokemon}!`);
}

function executarAtaqueJogador(nomeAtaque) {
    if (!turnoJogador) return;

    bloquearBotoes(true);
    const dano = Math.floor(Math.random() * 350) + 300;
    enemyHP = Math.max(0, enemyHP - dano);
    
    logMensagem(`${playerPokemon} usou ${nomeAtaque}! Causou ${dano} de dano.`);
    atualizarBarrasVida();

    if (enemyHP <= 0) {
        finalizarJogo(`${enemyPokemon} desmaiou! Você venceu!`);
    } else {
        turnoJogador = false;
        setTimeout(executarAtaqueInimigo, 1800);
    }
}

function executarAtaqueInimigo() {
    const ataquesInimigo = pokesData[enemyPokemon].moves;
    const ataqueSorteado = ataquesInimigo[Math.floor(Math.random() * ataquesInimigo.length)];
    const dano = Math.floor(Math.random() * 350) + 300;
    
    playerHP = Math.max(0, playerHP - dano);
    logMensagem(`${enemyPokemon} usou ${ataqueSorteado}! Causou ${dano} de dano.`);
    atualizarBarrasVida();

    if (playerHP <= 0) {
        finalizarJogo(`${playerPokemon} desmaiou! Você perdeu.`);
    } else {
        turnoJogador = true;
        bloquearBotoes(false);
    }
}

// ==========================================
// UTILITÁRIOS
// ==========================================

function atualizarBarrasVida() {
    const pctPlayer = (playerHP / maxHP) * 100;
    const pctEnemy = (enemyHP / maxHP) * 100;

    document.getElementById('player-hp-bar').style.width = `${pctPlayer}%`;
    document.getElementById('enemy-hp-bar').style.width = `${pctEnemy}%`;
    document.getElementById('player-hp-text').innerText = `${playerHP}/${maxHP}`;
    document.getElementById('enemy-hp-text').innerText = `${enemyHP}/${maxHP}`;
}

function logMensagem(texto) {
    const box = document.getElementById('battle-dialog-box');
    if (box) box.innerText = texto;
}

function bloquearBotoes(status) {
    const botoes = document.querySelectorAll('.btn-move');
    botoes.forEach(btn => btn.disabled = status);
}

function finalizarJogo(mensagemFinal) {
    logMensagem(mensagemFinal);
    bloquearBotoes(true);
    setTimeout(() => {
        alert(mensagemFinal);
        document.getElementById('game-ui').style.display = 'none';
        document.getElementById('select-screen').style.display = 'block';
    }, 2000);
}

function gerarBotoesAtaque() {
    const containerAtaques = document.getElementById('moves-container'); 
    containerAtaques.innerHTML = "";
    pokesData[playerPokemon].moves.forEach(ataque => {
        const btn = document.createElement('button');
        btn.className = 'btn-move';
        btn.innerText = ataque;
        btn.onclick = () => executarAtaqueJogador(ataque);
        containerAtaques.appendChild(btn);
    });
}
