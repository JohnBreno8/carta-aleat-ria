Estrutura do Script

O script é dividido em três partes principais:

1. Simulação de banco de dados em memória.


2. Lógica para registro de usuário.


3. Lógica para envio, recuperação e resposta de cartas.




1. Simulação de Banco de Dados em Memória

const users = [];
const letters = [];
let currentLetterIndex = null;

users: Armazena os dados dos usuários registrados. Cada usuário é representado por um objeto com as propriedades username, password e picture.

letters: Armazena as cartas enviadas. Cada carta é um objeto com as propriedades content (texto da carta) e replies (array contendo as respostas associadas à carta).

currentLetterIndex: Variável que guarda o índice da carta atualmente exibida na interface, facilitando a identificação da carta a ser respondida.



2. Registro de Usuário

Função: registerUser(event)

function registerUser(event) {
    event.preventDefault();

    const username = document.getElementById('username').value.trim();
    const password = document.getElementById('password').value.trim();
    const profilePicture = document.querySelector('#pictureSelector img.selected');

    if (!username || !password || !profilePicture) {
        alert("Todos os campos são obrigatórios!");
        return;
    }

    users.push({ username, password, picture: profilePicture.src });
    alert("Usuário registrado com sucesso!");
    window.location.href = 'index.html';
}

1. Prevenção do comportamento padrão do formulário:

O método event.preventDefault() impede que o formulário recarregue a página ao ser enviado.



2. Validação dos campos:

Verifica se o usuário preencheu todos os campos (username, password e selecionou uma figura).

Caso algum campo esteja vazio, uma mensagem de erro é exibida e a função é encerrada.



3. Adição do usuário ao banco de dados simulado:

Um novo objeto contendo o nome de usuário, senha e URL da figura selecionada é adicionado ao array users.



4. Redirecionamento:

Após o registro, o usuário é redirecionado para a página principal (index.html) usando window.location.href.




Função: generateRandomPictures()

function generateRandomPictures() {
    const pictureSelector = document.getElementById('pictureSelector');
    const pictureUrls = [
        'https://via.placeholder.com/60/FF0000/FFFFFF?text=A',
        'https://via.placeholder.com/60/00FF00/FFFFFF?text=B',
        'https://via.placeholder.com/60/0000FF/FFFFFF?text=C',
        'https://via.placeholder.com/60/FFFF00/FFFFFF?text=D'
    ];

    pictureUrls.forEach((url, index) => {
        const img = document.createElement('img');
        img.src = url;
        img.alt = `Figura ${index + 1}`;
        img.onclick = () => selectPicture(img);
        pictureSelector.appendChild(img);
    });
}

1. Definição de figuras disponíveis:

As URLs das figuras disponíveis são armazenadas no array pictureUrls.



2. Criação dinâmica de elementos <img>:

Para cada URL, é criado um elemento <img> que é configurado com:

src: URL da imagem.

alt: Texto alternativo (descritivo).

onclick: Uma função que será chamada ao clicar na imagem.




3. Adição ao DOM:

Cada imagem é anexada ao elemento HTML com o ID pictureSelector.





Função: selectPicture(img)

function selectPicture(img) {
    const allPictures = document.querySelectorAll('#pictureSelector img');
    allPictures.forEach(pic => pic.classList.remove('selected'));
    img.classList.add('selected');
}

1. Deseleção de imagens:

Remove a classe selected de todas as imagens no seletor.



2. Seleção da imagem clicada:

Adiciona a classe selected à imagem clicada, destacando visualmente a seleção.





3. Gerenciamento de Cartas

Função: sendLetter()

function sendLetter() {
    const content = document.getElementById('letterContent').value.trim();
    if (!content) {
        alert("A carta não pode estar vazia!");
        return;
    }

    letters.push({ content, replies: [] });
    document.getElementById('letterContent').value = '';
    alert("Carta enviada com sucesso!");
}

1. Validação:

Garante que o campo de texto da carta não esteja vazio. Caso esteja, exibe um alerta e encerra a função.



2. Armazenamento da carta:

Adiciona um novo objeto ao array letters, contendo o conteúdo da carta e um array vazio de respostas.



3. Reset do campo de texto:

Limpa o campo de texto após o envio.







Função: getRandomLetter()

function getRandomLetter() {
    if (letters.length === 0) {
        alert("Nenhuma carta disponível.");
        return;
    }

    currentLetterIndex = Math.floor(Math.random() * letters.length);
    const randomLetter = letters[currentLetterIndex];

    document.getElementById('randomLetter').innerHTML = `<p>${randomLetter.content}</p>`;
}

1. Verificação de cartas disponíveis:

Se o array letters estiver vazio, exibe um alerta informando que não há cartas.



2. Seleção de uma carta aleatória:

Usa Math.random() para gerar um índice aleatório baseado no tamanho do array.



3. Exibição da carta:

A carta selecionada é exibida no elemento HTML com o ID randomLetter.






Função: replyLetter()

function replyLetter() {
    if (currentLetterIndex === null) {
        alert("Nenhuma carta selecionada.");
        return;
    }

    const reply = document.getElementById('replyContent').value.trim();
    if (!reply) {
        alert("A resposta não pode estar vazia!");
        return;
    }

    letters[currentLetterIndex].replies.push(reply);
    document.getElementById('replyContent').value = '';
    alert("Resposta enviada!");
    currentLetterIndex = null;
}

1. Validação da carta selecionada:

Verifica se há uma carta atualmente exibida para ser respondida. Caso contrário, exibe um alerta.



2. Validação da resposta:

Garante que o campo de resposta não esteja vazio.



3. Adição da resposta:

A resposta é adicionada ao array replies da carta atualmente selecionada.



4. Reset da interface:

O campo de resposta é limpo e o índice da carta atual é redefinido para null.





Resumo

O script implementa um sistema simples mas funcional de envio e resposta de cartas, além de um registro básico de usuários com seleção de avatar. Ele utiliza estruturas como arrays para armazenar dados localmente e manipula o DOM para interagir com o usuário.

Se fosse integrado com um backend, seria necessário implementar:

1. APIs para persistência de usuários e cartas.


2. Autenticação e autorização para associar respostas ao remetente correto.


3. Gerenciamento mais avançado de dados, como histórico de cartas e notificações.


