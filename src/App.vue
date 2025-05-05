<script setup>
import { ref, computed } from 'vue'

const paginaAtual = ref('home')
const carrinho = ref([])
const favoritos = ref([])
const livros = [
  {
    id: 1,
    imagem: 'https://m.media-amazon.com/images/I/618fXbK+OkL._AC_UF1000,1000_QL80_.jpg',
    titulo: 'Crepúsculo',
    autor: 'Stephenie Meyer',
    preco: 39.99,
  },
  {
    id: 2,
    imagem: 'https://m.media-amazon.com/images/I/7153D5oFumL._AC_UF1000,1000_QL80_.jpg',
    titulo: 'Lua Nova',
    autor: 'Stephenie Meyer',
    preco: 34.99,
  },
  {
    id: 3,
    imagem:
      'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQn8HMVXm56_FdqLrorhnR27uV3RMlyebvgYw&s',
    titulo: 'Eclipse',
    autor: 'Stephenie Meyer',
    preco: 42.99,
  },
  {
    id: 4,
    imagem: 'https://m.media-amazon.com/images/I/51+ljDi72mL._AC_UF1000,1000_QL80_.jpg',
    titulo: 'Amanhecer',
    autor: 'Stephenie Meyer',
    preco: 65.99,
  },
  {
    id: 5,
    imagem: 'https://m.media-amazon.com/images/I/815mbcFisDL.jpg',
    titulo: 'Sol da meia noite',
    autor: 'Stephenie Meyer',
    preco: 70.99,
  },
  {
    id: 6,
    imagem: 'https://m.media-amazon.com/images/I/81ql6xkkliL._AC_UF1000,1000_QL80_.jpg',
    titulo: 'A Seleção',
    autor: 'Kiera Cass',
    preco: 40.99,
  },
  {
    id: 7,
    imagem: 'https://m.media-amazon.com/images/I/91ILPH1tDSL.jpg',
    titulo: 'A Elite',
    autor: 'Kiera Cass',
    preco: 45.99,
  },
  {
    id: 8,
    imagem: 'https://m.media-amazon.com/images/I/81htmSSRhXL.jpg',
    titulo: 'A Escolha',
    autor: 'Kiera Cass',
    preco: 49.99,
  },
  {
    id: 9,
    imagem: 'https://m.media-amazon.com/images/I/91EEn1ZVw+L._UF894,1000_QL80_.jpg',
    titulo: 'Corrupt',
    autor: 'Penelope Douglas',
    preco: 79.99,
  },
  {
    id: 10,
    imagem: 'https://m.media-amazon.com/images/I/91Iym6-6xML._UF894,1000_QL80_.jpg',
    titulo: 'Hideaway',
    autor: 'Penelope Douglas',
    preco: 89.99,
  },
  {
    id: 11,
    imagem:
      'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTXYnaVE0hzobDui_AAfu1Fnk9sh8P_pDesOg&s',
    titulo: 'Kill switch',
    autor: 'Penelope Douglas',
    preco: 89.99,
  },
  {
    id: 12,
    imagem: 'https://m.media-amazon.com/images/I/71QbHTPna7L._UF894,1000_QL80_.jpg',
    titulo: 'Nightfall',
    autor: 'Penelope Douglas',
    preco: 89.99,
  },
]

const searchQuery = ref('')

const filteredBooks = computed(() => {
  const query = searchQuery.value.toLowerCase()
  return livros.filter((book) => book.titulo.toLowerCase().includes(query))
})

function adicionarAoCarrinho(produto) {
  const existente = carrinho.value.find((item) => item.id === produto.id)
  if (existente) {
    existente.quantidade += 1
  } else {
    carrinho.value.push({ ...produto, quantidade: 1 })
  }
}

function removerDoCarrinho(id) {
  carrinho.value = carrinho.value.filter((item) => item.id !== id)
}

function aumentarQuantidade(item) {
  item.quantidade += 1
}

function diminuirQuantidade(item) {
  if (item.quantidade > 1) {
    item.quantidade -= 1
  } else {
    removerDoCarrinho(item.id)
  }
}

const totalCarrinho = computed(() => {
  return carrinho.value.reduce((acc, item) => acc + (item.preco * item.quantidade), 0)
})

const codigoCupom = ref('')
const cupomAtivo = ref(false)
const mensagemCupom = ref('')

function aplicarCupom() {
  const cupom = codigoCupom.value.toLowerCase()
  if (cupom !== 'devweb10') {
    mensagemCupom.value = 'Cupom inválido.'
    return
  }
  if (cupomAtivo.value) {
    mensagemCupom.value = 'Cupom já foi usado.'
  } else {
    cupomAtivo.value = true
    mensagemCupom.value = 'Cupom aplicado com sucesso!'
  }
}

const totalComDesconto = computed(() => {
  return cupomAtivo.value ? totalCarrinho.value * 0.9 : totalCarrinho.value
})


function adicionarAosFavoritos(livro) {
  const existe = favoritos.value.find((item) => item.id === livro.id)
  if (!existe) {
    favoritos.value.push(livro)
  }
}

function removerDosFavoritos(id) {
  favoritos.value = favoritos.value.filter((item) => item.id !== id)
}

</script>

<template>
  <header class="menu">
    <nav>
      <ul class="antes-barra">
        <li><a href="index.html" class="logo">BeckAnd books</a></li>
        <li class="barrinha"></li>
        <li class="slogan">
          Aprecie a <br />
          Leitura
        </li>
      </ul>
      <div class="barra-pesquisa">
        <input type="text" placeholder="Pesquisar" v-model="searchQuery" />
        <span class="material-icons">search</span>
      </div>
      <ul class="apos-barra">
        <li><a href="#termo">Termo</a></li>
        <li><a href="#equipe">Equipe</a></li>
        <li><a href="#envio">Envio</a></li>
        <li><a href="#devolucoes">Devoluções</a></li>
        <li class="icon-com-barra">
          <a href="#" @click.prevent="paginaAtual = 'carrinho'">
            <span class="material-icons">shopping_cart</span>
            <span v-if="carrinho.length" class="contador-carrinho">{{ carrinho.length }}</span>
          </a>
        </li>
        <li class="icon-com-barra">
          <a href="#" @click.prevent="paginaAtual = 'favoritos'">
            <span class="material-icons">favorite</span>
            <span v-if="favoritos.length" class="contador-favoritos">{{ favoritos.length }}</span>
          </a>
        </li>
        <li>
          <a href="#perfil"><span class="material-icons">person</span></a>
        </li>
      </ul>
    </nav>
    <div class="linha-azul"></div>
  </header>

  <main>
    <!--Página Inicial-->
    <section v-if="paginaAtual === 'home'">
      <section class="autor-abril" v-if="!searchQuery">
        <div class="stephenie">
          <div>
            <p><span>Autora de Abril</span></p>
          </div>
          <h1>Stephenie Meyer</h1>
          <p>
            Stephenie Meyer é a autora da série Crepúsculo, que vendeu mais de 100 milhões de cópias
            em mais de 50 países e foi traduzida para 37 línguas.
          </p>
          <button @click="adicionarAoCarrinho(livros[0])">Acessar página do livro</button>
        </div>
        <div class="img">
          <img
            src="https://http2.mlstatic.com/D_NQ_NP_905909-MLU74246720895_012024-O.webp"
            alt="Stephenie Meyer"
          />
        </div>
      </section>

      <section class="livros-section">
        <div class="botoes-informacoes" v-if="!searchQuery">
          <div class="item-info">
            <span class="material-icons">local_shipping</span>
            <span>Frete grátis para SC</span>
          </div>
          <div class="item-info">
            <span class="material-icons">star</span>
            <span>Livros recomendados</span>
          </div>
          <div class="item-info">
            <span class="material-icons">import_contacts</span>
            <span>Mais vendidos</span>
          </div>
        </div>

        <div class="container">
          <h2 class="titulo-secao">Lançamentos</h2>
          <div v-if="filteredBooks.length === 0">
            <p style="font-size: larger">Nenhum resultado encontrado.</p>
          </div>
          <div class="livros-grid">
            <div v-for="livro in filteredBooks" :key="livro.id" class="card-livro">
              <div class="livro-capa">
                <img :src="livro.imagem" alt="Capa do livro" />
              </div>
              <h3 class="livro-titulo">{{ livro.titulo }}</h3>
              <p class="livro-autor">{{ livro.autor }}</p>
              <div class="preco-favorito">
                <p class="livro-preco">R$ {{ livro.preco }}</p>
                <span class="icone-coracao" @click="adicionarAosFavoritos(livro)">
                  <span class="far fa-heart"></span>
                </span>
              </div>
              <div class="acoes">
                <button class="btn-comprar" @click="adicionarAoCarrinho(livro)">
                 <span class="material-icons">shopping_cart</span> Comprar
                 </button>
              </div>
            </div>
          </div>
        </div>
      </section>
    </section>
    <!--CARRINHO-->
    <section v-if="paginaAtual === 'carrinho'" class="pagina-carrinho">
      <h1>Carrinho</h1>
      <div v-if="carrinho.length === 0">
        <p class= "vazio" >Seu carrinho está vazio.</p>
        <button @click="paginaAtual = 'home'" class="btn-voltar">Voltar para a loja</button>
      </div>

      <div v-else>
        <div class="titulos">
          <h2>Título</h2>
          <h2>Quantidade</h2>
          <h2>Subtotal</h2>
        </div>

        <div v-for="item in carrinho" :key="item.id" class="item-carrinho">
          <!--TÍTULO-->
          <div class="coluna">
            <img :src="item.imagem" alt="Capa do livro" class="imagem-livro-carrinho" />
            <div>
              <h3 class="titulo-livro">{{ item.titulo }}</h3>
              <p class="autor-livro">{{ item.autor }}</p>
              <p>
                <span>R$ {{ item.preco.toFixed(2) }}</span>
              </p>
            </div>
          </div>
          <!--QUANTIDADE-->
          <div class="coluna-qtd">
            <div class="quantidade-area">
              <button @click="diminuirQuantidade(item)" class="btn-quantidade">-</button>
              <span>{{ item.quantidade }}</span>
              <button @click="aumentarQuantidade(item)" class="btn-quantidade">+</button>
            </div>
            <button @click="removerDoCarrinho(item.id)" class="btn-remover">
                <span class="fa-solid fa-trash-can"></span> Remover
            </button>
          </div>
          <!-- SUBTOTAL -->
          <div class="coluna-preco">
          <p><strong>R$ {{ (item.preco * item.quantidade).toFixed(2) }}</strong></p>
          </div>
          </div>
          <button @click="paginaAtual = 'home'" class="btn-voltar">Voltar para loja</button>
          <!-- CUPOM -->
          <div class="cupom">
          <input type="text" v-model="codigoCupom" placeholder="Código do cupom" />
          <button @click="aplicarCupom">Inserir Cupom</button>
          </div>
          <p v-if="mensagemCupom">{{ mensagemCupom }}</p>
          <!-- TOTAL-->
          <ul class="total">
            <li><strong>Total da Compra</strong></li>
            <li class="line">
              <p>Produtos:</p>
              <p>R$ {{ totalCarrinho.toFixed(2) }}</p>
            </li>
            <li class="line">
              <p>Frete:</p>
              <p>Grátis</p>
            </li>
            <li class="totalDesconto">
              <p>Total:</p>
              <p>R$ {{ totalCarrinho.toFixed(2) }}</p>
            </li>
            <li class="totalDesconto" v-if="totalComDesconto < totalCarrinho">
              <p>Total com desconto:</p>
              <p>R$ {{ totalComDesconto.toFixed(2) }}</p>
            </li>
            <button>Ir para o pagamento</button>
          </ul>
        </div>
  </section>
    <!-- Favoritos -->
    <section class="favoritos" v-if="paginaAtual === 'favoritos'">
      <h2>Meus Favoritos</h2>
      <div v-if="favoritos.length === 0">
        <p>Nenhum livro adicionado aos favoritos ainda.</p>
      </div>
      <ul v-else>
        <li v-for="livro in favoritos" :key="livro.id">
          <img :src="livro.imagem" alt="Capa" style="width: 100px" />
          <div>{{ livro.titulo }} - {{ livro.autor }}</div>
          <button @click="removerDosFavoritos(livro.id)" class="btn-remove">Remover dos Favoritos</button>
        </li>
      </ul>
      <button @click="paginaAtual = 'home'" class="btn-voltar">Voltar para a loja</button>
    </section>
  </main>
  <!--RODAPÉ-->
  <footer class="rodape">
    <div class="container-rodape">
      <div class="redes-sociais">
        <a href="index.html" class="logo">BeckAnd books</a>
        <div class="icones">
          <a href="#facebook"><span class="fab fa-facebook"></span></a>
          <a href="#instagram"><span class="fab fa-instagram"></span></a>
          <a href="#twitter"><span class="fab fa-twitter"></span></a>
        </div>
      </div>
      <div class="contatos">
        <p>Contatos</p>
        <p><span class="fas fa-phone-alt"></span> +55 47 99999-9999</p>
        <p><span class="fas fa-envelope"></span> beckandbooks@gmail.com</p>
        <div class="pagamento">
          <img src="https://i.ibb.co/ccfhYRbJ/paipal-1.png" alt="PayPal" class="icone-cartao" />
          <img
            src="https://i.ibb.co/ybp3bbW/Master-Card-Logo-1979-1.png"
            alt="Mastercard"
            class="icone-cartao"
          />
          <img
            src="https://i.ibb.co/bgpdtpx0/VISA-card-logo-1.png"
            alt="Visa"
            class="icone-cartao"
          />
        </div>
      </div>
    </div>
    <hr class="linha-divisoria" />
    <p class="copyright">&copy; Alguns direitos reservados | BeckAnd books 2025</p>
  </footer>
</template>

<style scoped>
/*MENU*/
header {
  font-family: 'Roboto', sans-serif;
  background-color: #fff;
}

main{
    background-color: #fff;
    min-height: calc(100vh - 200px);
}
.icon-com-barra {
  position: relative;
  display: flex;
  align-items: center;
  height: 100%;
  padding-right: 5px;
  margin-right: -10px;
}
.icon-com-barra::after {
  content: '';
  position: absolute;
  top: 10px;
  right: 0;
  width: 1px;
  height: 22px;
  background-color: #003366;
}
.icon-com-barra a span,
.apos-barra li a span {
  color: #003366;
}
.apos-barra li {
  display: flex;
  align-items: center;
  justify-content: center;
}
.apos-barra li a {
  display: flex;
  align-items: center;
  text-decoration: none;
  font-size: 15px;
  padding: 0 10px;
  color: #7a7881;
}
.apos-barra li a:hover {
  color: #003366;
}
.barra-pesquisa {
  display: flex;
  align-items: center;
  background-color: #f0f0f0;
  padding: 3px 15px;
  margin: 0.7vw 5vw 0 5vw;
}
.barra-pesquisa input {
  border: none;
  outline: none;
  background: transparent;
  color: #003366;
  font-size: 14px;
  padding: 5px 0 5px 0;
  flex-grow: 1;
}
.barra-pesquisa span {
  color: #003366;
  font-size: 18px;
  margin-left: 70px;
  cursor: pointer;
}
nav {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  flex-wrap: wrap;
  margin: 0.5vw 0 0 0;
}
nav ul {
  list-style: none;
  display: flex;
  gap: 20px;
  align-items: center;
  padding: 0;
  margin: 0;
}
nav ul li {
  padding: 1rem 1rem 0 0;
}
nav ul li a {
  text-decoration: none;
}
.logo {
  font-size: 18px;
  color: #231f2d;
}
.antes-barra {
  align-items: center;
  text-align: center;
}
.antes-barra li:first-child {
  position: relative;
}
.antes-barra li:first-child::after {
  content: '';
  position: absolute;
  top: 10px;
  right: 6px;
  width: 1px;
  height: 35px;
  background-color: #003366;
}
.slogan {
  color: #003366;
  font-size: 14px;
  line-height: 1;
  margin-left: -55px;
}
.apos-barra a {
  color: #003366;
}
.linha-azul {
  width: 100%;
  height: 1px;
  background-color: #003366;
  margin-top: 15px;
}
.contador-carrinho {
  position: absolute;
  top: 0;
  right: 0;
  background-color: white;
  color: #003366;
  border-radius: 50%;
  padding: 0 6px;
  font-size: 12px;
}
.contador-favoritos {
  position: absolute;
  top: 0;
  right: 0;
  background-color: white;
  color: #003366;
  border-radius: 50%;
  padding: 0 6px;
  font-size: 12px;
}

/*AUTOR-ABRIL*/
.autor-abril {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 5rem;
  padding: 4rem 2rem;
  margin-bottom: 2px solid #003366;
  font-family: 'Roboto', sans-serif;
}
.autor-abril .stephenie,
.autor-abril .img {
  margin: 0;
  width: 40%;
  max-width: 400px;
}

.autor-abril h1 {
  font-weight: bold;
  font-size: xxx-large;
  color: #382c2c;
}
.autor-abril p {
  color: #4d4c4c;
}
.autor-abril div.stephenie {
  margin: 0 5vw;
  width: 45%;
}
.autor-abril div.stephenie div {
  width: 130px;
  border: 2px solid #003366;
  padding: 7px 0 7px 0;
  margin: 2rem 0;
  border-radius: 8px;
  text-align: center;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}
.autor-abril div.stephenie span {
  color: #003366;
}

.autor-abril div.stephenie p {
  max-width: 500px;
}
.autor-abril button {
  font-family: 'Roboto', sans-serif;
  background-color: #003366;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 0.75rem 1.5rem;
  margin-top: 1.5rem;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s;
}
.autor-abril button:hover {
  background-color: #003366;
}
.autor-abril div.img {
  margin: 0 5vw;
  width: 45%;
}
.autor-abril div.img img {
  width: 440px;
  height: auto;
}
.autor-abril div.img p {
  font-size: 15px;
  color: #4d4c4c;
  margin-top: 8px;
  text-align: right;
}

/*CONTEÚDO*/
.livros-section {
  background-color: #fff;
  font-family: 'Roboto', sans-serif;
  border-top: 2px solid #003366;
}
.botoes-informacoes {
  display: flex;
  justify-content: space-around;
  padding: 32px 11vw;
  background: #fff;
  font-weight: 700;
  border-bottom: 2px solid #003366;
}
.botoes-informacoes span {
  font-weight: 600;
}
.item-info {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #003366;
  font-weight: 700;
  font-size: 16px;
}
.item-info span {
  font-size: 20px;
  font-weight: 700;
}
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 40px 20px;
}
.titulo-secao {
  font-size: 28px;
  margin: 0 0 2vw 0;
  font-weight: 600;
}
.livros-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 24px;
  row-gap: 75px;
}
.card-livro {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  height: 100%;
}
.livro-capa img {
  width: auto;
  height: 240px;
  object-fit: contain;
  margin-bottom: 10px;
}
.livro-titulo {
  font-size: 15px;
  font-weight: bold;
  margin-top: 10px;
  text-align: justify;
}
.livro-autor {
  color: #555;
  font-size: 13px;
  text-align: justify;
}
.livro-preco {
  margin: 8px 0;
  font-weight: bold;
  text-align: center;
  color: #003366;
  width: 100%;
}

.container h2{
    color: #003366;
}
.container h3{
    color: #003366;
}
.titulos{
    color: #003366;
}
.livro-preco{
    color: #003366;
}

.acoes {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 12px;
}
.preco-favorito {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 87%;
}
.btn-comprar {
  background-color: #003366;
  color: white;
  border: none;
  padding: 13px 80px;
  font-size: 16px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
}
.btn-comprar:hover {
  background-color: #003366;
}

/* CARRINHO */
.pagina-carrinho {
  padding: 3rem;
  font-family: 'Roboto', sans-serif;
  max-width: 100%;
}
.pagina-carrinho h1 {
  color: #003366;
  font-size: xx-large;
}
.pagina-carrinho h2 {
  font-weight: bold;
  font-size: x-large;
}
.pagina-carrinho h3 {
  font-weight: bold;
  font-size: large;
}
.vazio{
color: #003366;
}
.titulos {
  display: flex;
  gap: 25vw;
  border-bottom: 2px solid #003366;
  padding-bottom: 10px;
  margin-bottom: 20px;
  width: 100%;
  margin: 2vw 0 1.5vw 0;
}
.item-carrinho {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  border-bottom: 2px solid #ddd;
  padding-bottom: 1rem;
}
.item-carrinho p{
    color: #003366;
}
.item-carrinho h3{
    color: #003366;
}
.coluna {
  display: flex;
}
.coluna span {
  font-weight: bold;
}
.coluna-qtd {
  display: flex;
  margin: 0 15vw 0 -19vw;
  color: #003366;
}
.coluna-preco {
  margin: 0 19vw 0 -19vw;
}
.imagem-livro-carrinho {
  width: 100px;
  height: auto;
  border-radius: 4px;
  margin-right: 6px;
}
.quantidade-area {
  display: flex;
  border: 2px solid black;
  border-radius: 3px;
  padding: 5px;
}
.quantidade-area span {
  min-width: 30px;
  text-align: center;
  display: inline-block;
  font-weight: bold;
  font-size: 1rem;
}
.btn-quantidade {
  border: none;
  background-color: white;
  cursor: pointer;
}
.btn-remover {
  background-color: transparent;
  color: #ff4d4d;
  border: none;
  font-size: 1rem;
  cursor: pointer;
  font-weight: bold;
}
.btn-remover:hover {
  color: #d10000;
}
.btn-voltar {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: white;
  color: black;
  border: 2px solid black;
  border-radius: 4px;
  cursor: pointer;
  display: block;
  margin-right: auto;
  font-size: 16px;
  font-family: 'Roboto', sans-serif;
}
.total {
  margin: 2vw 65vw 0 65vw;
  border: solid 2px black;
  border-radius: 5px;
  padding: 10px 20px;
  width: 400px;
  list-style: none;
  color: #000000;
}
.total li {
  padding: 8px 0;
}
.total li.line {
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid black;
}
.total li.totalDesconto {
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid black;
}
.total strong {
  font-size: large;
  font-weight: bold;
}
.total button {
  font-family: 'Roboto', sans-serif;
  background-color: #003366;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 0.75rem 1.5rem;
  margin-top: 1.5rem;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s;
  justify-content: center;
}
.total button:hover {
  background-color: #003366;
}
.cupom input {
  font-family: 'Roboto', sans-serif;
  padding: 10px;
  border: 1px solid black;
  border-radius: 5px;
  outline: none;
  font-size: 1rem;
  width: 200px;
  margin: 0 1vw 0 0;
}
.cupom button {
  font-family: 'Roboto', sans-serif;
  background-color: #003366;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 0.75rem 1.5rem;
  margin-top: 1.5rem;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s;
  justify-content: center;
}
.cupom button:hover {
  background-color: #003366;
}

/* FAVORITOS */
.favoritos h2 {
  font-size: 35px;
  color: #003366;
  margin: 0 0 0 5vw;
}

.favoritos p {
  margin: 0 0 0 5vw;
}
.favoritos{
    color: #003366;
}
.favoritos ul {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 24px;
  margin: 2vw 5vw;
  padding: 0;
}

.favoritos ul li {
  list-style: none;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.btn-voltar{
    margin: 2px 0 0 5vw;
}
.btn-remove{
  background-color: #003366;
  color: white;
  border: none;
  padding: 8px 20px;
  margin: 0.5vw 0 0 0;
  font-size: 16px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
}

/*RODAPÉ*/
.rodape {
  margin-top: 0;
  background-color: #003366;
  color: white;
  font-size: 14px;
  font-family: 'Roboto', sans-serif;
}
.container-rodape {
  padding: 40px 10vw 20px;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  align-items: flex-start;
}
.redes-sociais .logo {
  font-size: 15px;
  margin-bottom: 10px;
  color: white;
  text-decoration: none;
}
.icones {
  display: flex;
  gap: 10px;
  width: 20px;
  height: 20px;
}
.icones span {
  color: white;
}
.contatos {
  text-align: left;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 5px;
}
.contatos p {
  margin: 4px 0;
  display: flex;
  align-items: center;
  gap: 8px;
}
.contatos span {
  font-size: 14px;
  color: white;
}
.titulo-contato {
  font-weight: bold;
  margin-bottom: 6px;
  font-size: 15px;
}
.pagamento {
  margin-top: 12px;
  display: flex;
  gap: 8px;
}
.icone-cartao {
  width: 38px;
  height: 25px;
}
.linha-divisoria {
  width: 100%;
  height: 2px;
  background-color: rgba(218, 208, 224, 0.726);
  border: none;
  margin: 20px 0;
}
.copyright {
  text-align: center;
  padding: 10px;
  font-size: 12px;
  color: rgba(218, 208, 224, 0.726);
}
</style>
