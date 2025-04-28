<script setup>
import { ref, computed } from 'vue'


const paginaAtual = ref('inicio'); 
const books = ref([
  {
    id: 1,
    cover: "https://m.media-amazon.com/images/I/618fXbK+OkL._AC_UF1000,1000_QL80_.jpg",
    title: "Crepúsculo",
    subject: "Stephenie Meyer",
    preco: "39.90",
  },
  {
    id: 2,
    cover: "https://m.media-amazon.com/images/I/7153D5oFumL._AC_UF1000,1000_QL80_.jpg",
    title: "Lua Nova",
    subject: "Stephenie Meyer",
    preco: "34.90",
  },
  {
    id: 3,
    cover: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQn8HMVXm56_FdqLrorhnR27uV3RMlyebvgYw&s",
    title: "Eclipse",
    subject: "Stephenie Meyer",
    preco: "42.00",
  },
  {
    id: 4,
    cover: "https://m.media-amazon.com/images/I/51+ljDi72mL._AC_UF1000,1000_QL80_.jpg",
    title: "Amanhecer",
    subject: "Stephenie Meyer",
    preco: "65.99",
  },
  {
    id: 5,
    cover: "https://m.media-amazon.com/images/I/815mbcFisDL.jpg",
    title: "Sol da meia-noite",
    subject: "Stephenie Meyer",
    preco: "70.99",
  },
  {
    id: 6,
    cover: "https://m.media-amazon.com/images/I/81ql6xkkliL._AC_UF1000,1000_QL80_.jpg",
    title: "A Seleção",
    subject: "Kiera Cass",
    preco: "40.99",
  },
  {
    id: 7,
    cover: "https://m.media-amazon.com/images/I/91ILPH1tDSL.jpg",
    title: "A Elite",
    subject: "Kiera Cass",
    preco: "45.99",
  },
  {
    id: 8,
    cover: "https://m.media-amazon.com/images/I/81htmSSRhXL.jpg",
    title: "A Escolha",
    subject: "Kiera Cass",
    preco: "49.99",
  },
  {
    id: 9,
    cover: "https://m.media-amazon.com/images/I/91EEn1ZVw+L._UF894,1000_QL80_.jpg",
    title: "Corrupt",
    subject: "Penelope Douglas",
    preco: "79.99",
  },
  {
    id: 10,
    cover: "https://m.media-amazon.com/images/I/91Iym6-6xML._UF894,1000_QL80_.jpg",
    title: "Hideaway",
    subject: "Penelope Douglas",
    preco: "89.99",
  },
  {
    id: 11,
    cover: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTXYnaVE0hzobDui_AAfu1Fnk9sh8P_pDesOg&s",
    title: "Kill switch",
    subject: "Penelope Douglas",
    preco: "89.99",
  },
  {
    id: 12,
    cover: "https://m.media-amazon.com/images/I/71QbHTPna7L._UF894,1000_QL80_.jpg",
    title: "Nightfall",
    subject: "Penelope Douglas",
    preco: "89.99",
  },
  {
    id: 13,
    cover: "https://m.media-amazon.com/images/I/81TjnGG0g7L.jpg",
    title: "De sangue e cinzas",
    subject: "Jennifer L.",
    preco: "59.99",
  },
  {
    id: 14,
    cover: "https://m.media-amazon.com/images/I/91SDZ2eUj+L.jpg",
    title: "Verity",
    subject: "Collen Hoover",
    preco: "59.99",
  },
  {
    id: 15,
    cover: "https://m.media-amazon.com/images/I/71Xta4Nf7uL._AC_UF1000,1000_QL80_.jpg",
    title: "Orgulho e Preconceito",
    subject: "Jane Austen",
    preco: "69.99",
  },
]);

const termoBusca = ref('');
const favoritos = ref([]);
const mostrandoFavoritos = ref(false);
const carrinho = ref([]);
const mostrandoCarrinho = ref(false);


const toggleCarrinho = (book) => {
  const index = carrinho.value.findIndex(car => car.id === book.id);
  if (index === -1) {
    carrinho.value.push(book);
  } else {
    carrinho.value.splice(index, 1);
  }
};

const mostrarCarrinho = () => {
  mostrandoCarrinho.value = true;
};

const filteredBooks = computed(() => {
  return books.value.filter((book) =>
    book.title.toLowerCase().includes(termoBusca.value.toLowerCase()) ||
    book.subject.toLowerCase().includes(termoBusca.value.toLowerCase())
  );
});

const toggleFavorito = (book) => {
  const index = favoritos.value.findIndex(fav => fav.id === book.id);
  if (index === -1) {
    favoritos.value.push(book);
  } else {
    favoritos.value.splice(index, 1);
  }
};

const mostrarFavoritos = () => {
  mostrandoFavoritos.value = true;
};

const acessarPaginaAutor = () => {
  paginaAtual.value = 'autor'; 
};

</script>

<template>
  <header>
    <div class="menu">
      <nav>
        <ul class="antes-barra">
          <li><a href="#beckand" class="logo">BeckAnd books</a></li>
          <li class="barrinha"></li>
          <li class="slogan">Aprecie a <br />Leitura</li>
        </ul>
        <div class="barra-pesquisa">
          <input type="text" v-model="termoBusca" placeholder="Pesquisar" />
          <i class="fa-solid fa-magnifying-glass"></i>
        </div>
        <ul class="apos-barra">
          <li><a href="#termo">Termo</a></li>
          <li><a href="#equipe">Equipe</a></li>
          <li><a href="#envio">Envio</a></li>
          <li><a href="#devolucoes">Devoluções</a></li>
          <li class="icon-com-barra">
            <a href="#carrinho">
              <i class="fa-solid fa-cart-shopping"></i>
              <span v-if="carrinho.length > 0" class="contador">{{ carrinho.length }}</span>
            </a>
          </li>
          <li class="icon-com-barra">
            <a href="#favoritos" @click="mostrarFavoritos">
              <i class="fa-solid fa-heart"></i>
              <span v-if="favoritos.length > 0" class="contador">{{ favoritos.length }}</span>
            </a>
          </li>
          <li><a href="#perfil"><i class="fa-solid fa-user"></i></a></li>
        </ul>
      </nav>
      <hr class="linha-azul" />
    </div>
  </header>

  <main>
    <section v-if="paginaAtual === 'inicio'" class="autor-abril">
      <div class="stephenie">
        <div>
          <p><span>Autora de Abril</span></p>
        </div>
        <h1>Stephenie Meyer</h1>
        <p>
          Stephenie Meyer é a autora da série Crepúsculo, que vendeu mais de 100 milhões de cópias em mais de 50 países e foi traduzida para 37 línguas.
        </p>
        <button @click="acessarPaginaAutor">Acessar página do autor</button>
      </div>
      <div class="img">
        <img src="https://http2.mlstatic.com/D_NQ_NP_905909-MLU74246720895_012024-O.webp" alt="Stephenie Meyer" />
      </div>
    </section>

      <section v-else-if="paginaAtual === 'autor'">
    <h2>Sobre Stephenie Meyer</h2>
    <p>Informações detalhadas sobre a autora...</p>
    <button @click="paginaAtual = 'inicio'">Voltar à Página Principal</button>
  </section>

    <section v-if="!mostrandoFavoritos" class="livros-section">
      <div class="botoes-informacoes">
        <div class="item-info">
          <i class="fas fa-truck"></i>
          <span>Frete grátis para SC</span>
        </div>
        <div class="item-info">
          <i class="fas fa-star"></i>
          <span>Livros recomendados</span>
        </div>
        <div class="item-info">
          <i class="fas fa-book"></i>
          <span>Mais vendidos</span>
        </div>
      </div>

      <article class="books">
        <ul>
          <li v-for="book in filteredBooks" :key="book.id">
            <h2>{{ book.title }}</h2>
            <img :src="book.cover" alt="Capa do livro" width="120" />
            <p>{{ book.subject }}</p>
            <p>{{ book.preco }}</p>
            <button @click.prevent="toggleCarrinho(book)">
              Adicionar ao Carrinho
            </button>
            <a href="#" @click.prevent="toggleFavorito(book)">
              <i class="fa-solid fa-heart"></i>
            </a>
          </li>
        </ul>
      </article>
    </section>
    <section v-if="mostrandoCarrinho" class="carrinho-section">
      <h2>Carrinho</h2>
      <ul>
        <li v-for="book in carrinho" :key="book.id">
          <h3>{{ book.title }}</h3>
          <img :src="book.cover" alt="Capa do livro" width="120" />
          <p>{{ book.subject }}</p>
          <p>{{ book.preco }}</p>
          <button @click="toggleCarrinho(book)">
          </button>
        </li>
      </ul>
      <div class="voltar-container">
        <a href="index.html" class="voltar-btn">Voltar à Página Principal</a>
      </div>
    </section>
    <section v-if="mostrandoFavoritos" class="favoritos-section">
      <h2>Seus Favoritos</h2>
      <ul>
        <li v-for="book in favoritos" :key="book.id">
          <h3>{{ book.title }}</h3>
          <img :src="book.cover" alt="Capa do livro" width="120" />
          <p>{{ book.subject }}</p>
          <p>{{ book.preco }}</p>
          <button @click="toggleFavorito(book)">
            Remover dos Favoritos
          </button>
        </li>
      </ul>
      <div class="voltar-container">
        <a href="index.html" class="voltar-btn">Voltar à Página Principal</a>
      </div>
    </section>
  </main>


  <footer class="rodape">
  <div class="container-rodape">
    <div class="redes-sociais">
      <a href="index.html" class="logo">BeckAnd books</a>
      <div class="icones">
        <i class="fab fa-facebook"></i>
        <i class="fab fa-instagram"></i>
        <i class="fab fa-twitter"></i>
      </div>
    </div>
    <div class="contatos">
      <p>Contatos</p>
      <p><i class="fas fa-phone-alt"></i> </p>
      <p><i class="fas fa-envelope"></i> beckandbooks@gmail.com</p>
      <div class="pagamento">
        <img src="https://i.ibb.co/ccfhYRbJ/paipal-1.png" alt="PayPal" class="icone-cartao" />
        <img src="https://i.ibb.co/ybp3bbW/Master-Card-Logo-1979-1.png" alt="Mastercard" class="icone-cartao" />
        <img src="https://i.ibb.co/bgpdtpx0/VISA-card-logo-1.png" alt="Visa" class="icone-cartao" />
      </div>
    </div>
  </div>
  <hr class="linha-divisoria" />
  <p class="copyright">© Alguns direitos reservados | BeckAnd 2025</p>
</footer>
</template>

<style scoped>
body {
  font-family: 'Poppins', sans-serif;
}
.autor-abril {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 5rem;
  padding: 4rem 2rem;
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
  margin: 0;
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
  font-family: 'Poppins', sans-serif;
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
  background-color:#003366;
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
  background-color:#003366;
}

.icon-com-barra a i,
.apos-barra li a i {
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
  color: #7b7881;
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

.barra-pesquisa i {
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

.livros-section {
  background-color: #fff;
}

.botoes-informacoes {
  display: flex;
  justify-content: space-around;
  padding: 32px 11vw;
  border-bottom: 1px solid #ccc;
  background: #fff;
  font-weight: 700;
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

.item-info i {
  font-size: 20px;
  font-weight: 700;
}

.books {
  padding: 4rem 2rem;
  display: flex;
  justify-content: center;
}

.books ul {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
  gap: 2rem;
  list-style: none;
  padding: 0;
  margin: 0;
  width: 100%;
  max-width: 1100px;
}

.books li {
  text-align: center;
  padding: 1rem;
  border-radius: 12px;

  transition: transform 0.2s ease-in-out;

}



.books h2 {
  font-size: 1.1rem;
  color: #003366;
  margin: 0.5rem 0;
}

.books p {
  color: #555;
  margin: 0.25rem 0;
  font-size: 0.95rem;
}

.books button {
  margin-top: 0.5rem;
  padding: 0.5rem 1rem;
  font-size: 0.9rem;
  background-color: #003366;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.books button:hover {
  background-color: #002244;
}
.favoritos-section {
  padding: 20px;
  background-color: #f4f4f9;
  border-radius: 8px;
  margin-top: 20px;
}

.favoritos-section h2 {
  font-size: 2rem;
  font-weight: bold;
  text-align: center;
  color: #333;
  margin-bottom: 20px;
}

.favoritos-section ul {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
  list-style-type: none;
  padding: 0;
}

.favoritos-section li {
  background-color: #fff;
  padding: 15px;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.favoritos-section li:hover {
  transform: translateY(-5px);
}

.favoritos-section li img {
  width: 100%;
  height: 200px;
  object-fit: cover;
  border-radius: 5px;
  margin-bottom: 15px;
}

.favoritos-section li h3 {
  font-size: 1.25rem;
  font-weight: 600;
  color: #333;
  margin-bottom: 10px;
}

.favoritos-section li p {
  font-size: 1rem;
  color: #666;
  margin-bottom: 10px;
}

.favoritos-section li button {
  background-color: #003366;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s;
  display: block;
  margin-left: auto;
  margin-right: auto;
}

.favoritos-section li button:hover {
  background-color: #003366;
}

.favoritos-section li button:focus {
  outline: none;
}
/* Estilo para o botão de voltar */
.voltar-container {
  text-align: center;
  margin-top: 30px;
}

.voltar-btn {
  background-color: #003366;
  color: white;
  padding: 12px 25px;
  border-radius: 5px;
  text-decoration: none;
  font-size: 1.2rem;
  display: inline-block;
  transition: background-color 0.3s;
}
button {
  background-color: #003366;
  color: white;
  border: none;
  padding: 8px 16px;
  font-size: 14px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #002244;
}

button:focus {
  outline: none;
}

.voltar-btn:hover {
  background-color:#003366;
}

.voltar-btn:focus {
  outline: none;
}

.rodape {
  margin: 10vw 0 0 0;
  background-color: #003366;
  color: white;
  font-size: 14px;
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
}
.icones i {
  width: 20px;
  height: 20px;
  background-color: white;
  color: #003366;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 16px;
  border-radius: 2px;
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
.contatos i {
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