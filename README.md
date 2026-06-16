// ==========================================
// VARIÁVEIS GLOBAIS E CONFIGURAÇÕES
// ==========================================
let playerHP = 3000;
let enemyHP = 3000;
const maxHP = 3000;

let playerPokemon = "";
let enemyPokemon = "";
let turnoJogador = true;

// Banco de dados dos Pokémons e seus ataques
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
// FLUXO DE TELAS E INTERFACE
// ==========================================
document.addEventListener("DOMContentLoaded", () => {
    document.getElementById('start-screen').style.display = 'flex';
    document.getElementById('select-screen').style.display = 'none';
    document.getElementById('game-ui').style.display = 'none';
});

function abrirPokebola() {
    document.getElementById('top').classList.add('open-top');
    document.getElementById('bottom').classList.add('open-bottom');
    setTimeout(() => {
        document.getElementById('btn-iniciar').style.display = 'block';
    }, 500);
}

function irParaSelecao() {
    document.getElementById('start-screen').style.display = 'none';
    document.getElementById('select-screen').style.display = 'block';
    
    const container = document.getElementById('options-container');
    if (container.innerHTML === "") {
        for(let name in pokesData) {
            container.innerHTML += `
                <div class="poke-card" onclick="iniciarBatalha('${name}')">
                    <img src="https://githubusercontent.com{pokesData[name].id}.png" alt="${name}">
                    <span>${name}</span>
                </div>`;
        }
    }
}

// ==========================================
// LÓGICA DE BATALHA
// ==========================================
function iniciarBatalha(nomeEscolhido) {
    // Reset de HP e Turno
    playerHP = maxHP;
    enemyHP = maxHP;
    turnoJogador = true;
    playerPokemon = nomeEscolhido;

    // Sorteia um inimigo diferente do jogador
    const nomesDisponiveis = Object.keys(pokesData).filter(p => p !== playerPokemon);
    enemyPokemon = nomesDisponiveis[Math.floor(Math.random() * nomesDisponiveis.length)];

    // Transição de tela
    document.getElementById('select-screen').style.display = 'none';
    document.getElementById('game-ui').style.display = 'block';

    // Inicializa textos e imagens na UI
    document.getElementById('player-name-txt').innerText = playerPokemon;
    document.getElementById('enemy-name-txt').innerText = enemyPokemon;
    
    // Altere os IDs abaixo se os seus elementos HTML de imagem tiverem nomes diferentes
    document.getElementById('player-sprite').src = `https://githubusercontent.com{pokesData[playerPokemon].id}.png`;
    document.getElementById('enemy-sprite').src = `https://githubusercontent.com{pokesData[enemyPokemon].id}.png`;

    atualizarBarrasVida();
    gerarBotoesAtaque();
    logMensagem(`Um ${enemyPokemon} selvagem apareceu! Vai, ${playerPokemon}!`);
}

function gerarBotoesAtaque() {
    // Altere o ID abaixo para o container onde ficam seus botões de ataque no HTML
    const containerAtaques = document.getElementById('moves-container'); 
    containerAtaques.innerHTML = "";
    
    pokesData[playerPokemon].moves.forEach(ataque => {
        containerAtaques.innerHTML += `
            <button class="btn-move" onclick="executarAtaqueJogador('${ataque}')">${ataque}</button>
        `;
    });
}

function executarAtaqueJogador(nomeAtaque) {
    if (!turnoJogador || playerHP <= 0 || enemyHP <= 0) return;

    turnoJogador = false;
    bloquearBotoes(true);

    // Cálculo de dano aleatório (entre 300 e 650)
    const dano = Math.floor(Math.random() * 350) + 300;
    enemyHP = Math.max(0, enemyHP - dano);
    
    logMensagem(`${playerPokemon} usou ${nomeAtaque}! Causou ${dano} de dano.`);
    atualizarBarrasVida();

    if (enemyHP <= 0) {
        setTimeout(() => finalizarJogo(`${enemyPokemon} desmaiou! Você venceu!`), 1500);
    } else {
        setTimeout(executarAtaqueInimigo, 1800);
    }
}

function executarAtaqueInimigo() {
    if (playerHP <= 0) return;

    // Sorteia um ataque do banco de dados do inimigo
    const ataquesInimigo = pokesData[enemyPokemon].moves;
    const ataqueSorteado = ataquesInimigo[Math.floor(Math.random() * ataquesInimigo.length)];
    
    const dano = Math.floor(Math.random() * 350) + 300;
    playerHP = Math.max(0, playerHP - dano);

    logMensagem(`${enemyPokemon} respondeu com ${ataqueSorteado}! Causou ${dano} de dano.`);
    atualizarBarrasVida();

    if (playerHP <= 0) {
        setTimeout(() => finalizarJogo(`${playerPokemon} desmaiou! Você perdeu.`), 1500);
    } else {
        turnoJogador = true;
        bloquearBotoes(false);
    }
}

// ==========================================
// FUNÇÕES AUXILIARES / UTILITÁRIAS
// ==========================================
function atualizarBarrasVida() {
    const pctPlayer = (playerHP / maxHP) * 100;
    const pctEnemy = (enemyHP / maxHP) * 100;

    // Ajusta a largura das barras (certifique-se de ter esses IDs no seu HTML/CSS)
    document.getElementById('player-hp-bar').style.width = `${pctPlayer}%`;
    document.getElementById('enemy-hp-bar').style.width = `${pctEnemy}%`;

    // Atualiza o texto numérico
    document.getElementById('player-hp-text').innerText = `${playerHP}/${maxHP}`;
    document.getElementById('enemy-hp-text').innerText = `${enemyHP}/${maxHP}`;
}

function logMensagem(texto) {
    // Altere para o ID do seu elemento de texto/caixa de diálogo da batalha
    const box = document.getElementById('battle-dialog-box');
    if (box) box.innerText = texto;
}

function bloquearBotoes(status) {
    const botoes = document.querySelectorAll('.btn-move');
    botoes.forEach(btn => btn.disabled = status);
}

function finalizarJogo(mensagemFinal) {
    logMensagem(mensagemFinal);
    setTimeout(() => {
        alert(mensagemFinal);
        // Reseta o jogo voltando para a tela de seleção
        document.getElementById('game-ui').style.display = 'none';
        irParaSelecao();
    }, 2000);
}
