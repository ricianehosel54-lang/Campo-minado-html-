!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Campo Minado HTML - Responsivo</title>
    <style>
        :root {
            --grid-gap: 8px;
            --background-color: #212529;
            --cell-color-initial: #6c757d;   
            --cell-color-named-html: #e34f26; /* Laranja HTML */
            --cell-color-revealed: #ffffff; 
            --text-color-light: #ffffff;
            --text-color-dark: #000000;
            --border-color: #dee2e6;
            --border-radius: 8px;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: var(--background-color);
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        
        h1 {
            color: #f8f9fa;
            margin-bottom: 20px;
            text-align: center;
        }

        .grid-container {
            display: grid;
            width: 100%;
            max-width: 1400px;
            grid-template-columns: repeat(10, 1fr);
            gap: var(--grid-gap);
        }

        .cell {
            aspect-ratio: 1 / 1; 
            width: 100%;
            border-radius: var(--border-radius);
            cursor: pointer;
            display: flex;
            justify-content: center;
            align-items: center;
            text-align: center;
            font-weight: bold;
            padding: 5px;
            box-sizing: border-box;
            transition: transform 0.3s ease, background-color 0.3s, color 0.3s;
            user-select: none;
            background-color: var(--cell-color-initial);
            color: var(--text-color-light);
            font-size: clamp(14px, 3vw, 24px);
        }

        .cell:not(.revealed):hover {
            transform: scale(1.05);
            z-index: 10;
        }

        .cell.named.type-html {
            background-color: var(--cell-color-named-html);
        }
        
        .cell.named {
            color: var(--text-color-light);
            font-size: clamp(10px, 1.8vw, 16px);
            word-break: break-all;
        }
        
        .cell.revealed {
            background-color: var(--cell-color-revealed);
            color: var(--text-color-dark);
            border: 1px solid var(--border-color);
            font-weight: normal;
            cursor: default;
            justify-content: center;
            align-items: center;
            padding: 8px;
            font-size: clamp(9px, 1.2vw, 14px);
            line-height: 1.2;
        }
    </style>
</head>
<body>

<h1>Campo Minado:Tags CSS</h1>
<div class="grid-container" id="minefield"></div>

<script>
    const minefield = document.getElementById('minefield');
    
    // Lista expandida para 100 itens, apenas com CSS
    const items = [
    { type: 'css', name: 'color', answer: 'Define a cor do texto.' },
    { type: 'css', name: 'font-size', answer: 'Define o tamanho da fonte.' },
    { type: 'css', name: 'font-family', answer: 'Define a família da fonte (tipo de letra).' },
    { type: 'css', name: 'font-weight', answer: 'Define a espessura da fonte (ex: bold/negrito).' },
    { type: 'css', name: 'font-style', answer: 'Define o estilo da fonte (italic, normal).' },
    { type: 'css', name: 'text-align', answer: 'Define o alinhamento horizontal do texto.' },
    { type: 'css', name: 'text-decoration', answer: 'Adiciona sublinhado, tachado ou remove links.' },
    { type: 'css', name: 'line-height', answer: 'Define a altura da linha (espaçamento vertical).' },
    { type: 'css', name: 'letter-spacing', answer: 'Define o espaçamento entre as letras.' },
    { type: 'css', name: 'text-transform', answer: 'Muda para MAIÚSCULO, minúsculo ou Capitalizado.' },
    { type: 'css', name: 'text-shadow', answer: 'Adiciona sombra ao texto.' },
    { type: 'css', name: 'word-spacing', answer: 'Define o espaçamento entre as palavras.' },
    { type: 'css', name: 'width', answer: 'Define a largura de um elemento.' },
    { type: 'css', name: 'height', answer: 'Define a altura de um elemento.' },
    { type: 'css', name: 'padding', answer: 'Espaçamento interno (entre o conteúdo e a borda).' },
    { type: 'css', name: 'margin', answer: 'Espaçamento externo (fora da borda).' },
    { type: 'css', name: 'border', answer: 'Define espessura, estilo e cor da borda.' },
    { type: 'css', name: 'border-radius', answer: 'Arredonda os cantos do elemento.' },
    { type: 'css', name: 'box-sizing', answer: 'Define como o tamanho total do elemento é calculado.' },
    { type: 'css', name: 'max-width', answer: 'Define uma largura máxima (bom para responsividade).' },
    { type: 'css', name: 'min-height', answer: 'Define uma altura mínima para o elemento.' },
    { type: 'css', name: 'outline', answer: 'Linha externa à borda, usada para foco e acessibilidade.' },
    { type: 'css', name: 'overflow', answer: 'Define o que acontece se o conteúdo transbordar a caixa.' },
    { type: 'css', name: 'box-shadow', answer: 'Adiciona sombra ao redor da caixa do elemento.' },
    { type: 'css', name: 'background-color', answer: 'Define a cor de fundo.' },
    { type: 'css', name: 'background-image', answer: 'Define uma imagem como fundo.' },
    { type: 'css', name: 'background-size', answer: 'Ajusta o tamanho da imagem de fundo (ex: cover).' },
    { type: 'css', name: 'background-position', answer: 'Define onde a imagem de fundo começa.' },
    { type: 'css', name: 'background-repeat', answer: 'Define se a imagem de fundo se repete ou não.' },
    { type: 'css', name: 'opacity', answer: 'Define o nível de transparência (0 a 1).' },
    { type: 'css', name: 'display', answer: 'Define o tipo de renderização (block, inline, flex, grid).' },
    { type: 'css', name: 'position', answer: 'Define o tipo de posicionamento (relative, absolute, fixed).' },
    { type: 'css', name: 'top / bottom', answer: 'Move o elemento para cima ou baixo (se posicionado).' },
    { type: 'css', name: 'left / right', answer: 'Move o elemento para os lados (se posicionado).' },
    { type: 'css', name: 'z-index', answer: 'Define a ordem de empilhamento (quem fica na frente).' },
    { type: 'css', name: 'float', answer: 'Faz o elemento flutuar à esquerda ou direita.' },
    { type: 'css', name: 'clear', answer: 'Impede que elementos flutuem ao lado deste.' },
    { type: 'css', name: 'visibility', answer: 'Esconde o elemento, mas mantém o espaço ocupado.' },
    { type: 'css', name: 'cursor', answer: 'Muda o desenho do mouse ao passar por cima (ex: pointer).' },
    { type: 'css', name: 'vertical-align', answer: 'Define o alinhamento vertical (em tabelas ou inline).' },
    { type: 'css', name: 'flex-direction', answer: 'Define a direção dos itens flex (linha ou coluna).' },
    { type: 'css', name: 'justify-content', answer: 'Alinha itens no eixo principal (horizontal).' },
    { type: 'css', name: 'align-items', answer: 'Alinha itens no eixo transversal (vertical).' },
    { type: 'css', name: 'gap', answer: 'Define o espaço entre itens de flex ou grid.' },
    { type: 'css', name: 'grid-template-columns', answer: 'Define as colunas de um container Grid.' },
    { type: 'css', name: 'flex-wrap', answer: 'Define se os itens quebram para a próxima linha.' },
    { type: 'css', name: 'align-self', answer: 'Alinha um item específico dentro do flex/grid.' },
    { type: 'css', name: 'transition', answer: 'Cria transições suaves entre mudanças de estilo.' },
    { type: 'css', name: 'transform', answer: 'Aplica rotação, escala ou inclinação ao elemento.' },
    { type: 'css', name: 'filter', answer: 'Aplica efeitos como desfoque (blur) ou preto e branco.' }

    ];

    function shuffle(array) {
        for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
        }
    }

    shuffle(items);

    items.forEach((item, index) => {
        const cell = document.createElement('div');
        cell.classList.add('cell');
        cell.textContent = index + 1;
        cell.dataset.name = item.name;
        cell.dataset.answer = item.answer;
        cell.dataset.type = item.type;

        cell.addEventListener('click', function() {
            if (this.classList.contains('revealed')) return;
            
            if (this.classList.contains('named')) {
                this.classList.remove('named', 'type-html');
                this.classList.add('revealed');
                this.textContent = this.dataset.answer;
                return;
            }
            
            this.classList.add('named', 'type-html');
            this.textContent = this.dataset.name;
        });

        minefield.appendChild(cell);
    });
</script>

</body>
</html>
