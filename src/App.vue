<script setup>
import { ref, reactive, computed } from "vue";


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
    title: "Hideaway ",
    subject: "Penelope Douglas",
    preco: "89.99",
  },
  {
    id: 11,
    cover: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTXYnaVE0hzobDui_AAfu1Fnk9sh8P_pDesOg&s",
    title: "Kill switch ",
    subject: "Penelope Douglas",
    preco: "89.99",
  },
  {
    id: 12,
    cover: "https://m.media-amazon.com/images/I/71QbHTPna7L._UF894,1000_QL80_.jpg",
    title: "Nightfall ",
    subject: "Penelope Douglas",
    preco: "89.99",
  },
  {
    id: 13,
    cover: "https://m.media-amazon.com/images/I/81TjnGG0g7L.jpg",
    title: "De sangue e cinzas ",
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


const searchQuery = ref("");


const filteredBooks = computed(() => {
  const query = searchQuery.value.toLowerCase();
  return books.value.filter(book => book.title.toLowerCase().includes(query));
});

const carrinho = reactive([]);
const mostrarCarrinho = ref(false);

function adicionarAoCarrinho(book) {
  carrinho.push(book);
}
</script>

<template>
  <header>
    <nav>
      <h1>BeckAnd books</h1>

      <div class="barrapesquisa">
        <input type="text" placeholder="Pesquisar livros..." v-model="searchQuery" />
        <font-awesome-icon :icon="['fas', 'magnifying-glass']" />
      </div>

      <ul>
        <li>Termos</li>
        <li>Equipe</li>
        <li>Envio</li>
        <li>Devoluções</li>
      </ul>

      <ul>
        <li @click="mostrarCarrinho = true" style="cursor: pointer;">
          <i class="fa-solid fa-cart-shopping"></i> ({{ carrinho.length }})
        </li>
        <li><i class="fa-solid fa-heart"></i></li>
        <li><i class="fa-solid fa-user"></i></li>
      </ul>
    </nav>
  </header>

  <main>
    <template v-if="!mostrarCarrinho">
      <section>
        <p>Autor de abril</p>
        <h2>Stephenie Meyer</h2>
        <p>
          Stephenie Meyer é a autora da série Crepúsculo, que vendeu mais de 100
          milhões de cópias em mais de 50 países e foi traduzida para 37 línguas.
        </p>
        <button>Acessar página do livro</button>
      </section>

      <section>
        <ul>
          <li>
            <i class="fa-solid fa-truck"></i> Frete grátis para SC
          </li>
          <li>
            <i class="fa-solid fa-star"></i> Livros recomendados
          </li>
          <li>
            <i class="fa-solid fa-book-open"></i> Mais vendidos
          </li>
        </ul>
      </section>

      <section>
        <h3>Lançamentos</h3>
        <article class="books">
          <ul>
            <li v-for="book in filteredBooks" :key="book.id">
              <h2>{{ book.title }}</h2>
              <img :src="book.cover" alt="Capa do livro" width="120" />
              <p>{{ book.subject }}</p>
              <p>Preço: R$ {{ book.preco }}</p>
              <button @click="adicionarAoCarrinho(book)">
                Adicionar ao Carrinho
              </button>
            </li>
          </ul>
        </article>
      </section>
    </template>

    <template v-else>
      <section>
        <h2>Seu Carrinho</h2>
        <div v-if="carrinho.length === 0">
          <p>O carrinho está vazio.</p>
        </div>
        <ul v-else>
          <li v-for="(item, index) in carrinho" :key="index">
            {{ item.title }} - R$ {{ item.preco }}
          </li>
        </ul>
        <button @click="mostrarCarrinho = false">⬅ Voltar</button>
      </section>
    </template>
  </main>

  <footer>
    <div>
      <p>IFbooks</p>
      <ul>
        <li><i class="fa-brands fa-square-facebook"></i></li>
        <li><i class="fa-brands fa-square-instagram"></i></li>
        <li><i class="fa-brands fa-square-twitter"></i></li>
      </ul>
    </div>
    <div>
      <p>Contatos</p>
      <ul>
        <li>
          <i class="fa-solid fa-phone"></i> +55 47 40045263
        </li>
        <li>
          <i class="fa-solid fa-clock"></i> 8h às 23h - Seg a Sex
        </li>
        <li>
          <i class="fa-solid fa-envelope"></i> contatolalala@gmail.com
        </li>
      </ul>
    </div>
    <hr />
    <p>© Alguns direitos reservados. IFbooks 2025.</p>
  </footer>
</template>

<style scoped>
:root {
  --azul-claro: #e0f0ff;
  --azul: #0077cc;
  --azul-escuro: #003366;
  --texto: #1a1a1a;
  --fundo: #f8faff;
}


* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Segoe UI", sans-serif;
  background-color: gray;
  color: black;
}

header {
  background-color: #003366;
  color: white;
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.barrapesquisa {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background-color: white;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  margin: 1rem 0;
}

.barrapesquisa input {
  border: none;
  outline: none;
  flex-grow: 1;
  font-size: 1rem;
}


nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
}

nav h1 {
  font-size: 2rem;
  font-weight: bold;
}

nav ul {
  list-style: none;
  display: flex;
  gap: 1.5rem;
}

nav ul li {
  cursor: pointer;
  transition: 0.2s;
}

nav ul li:hover {
  color: #0077cc;
}


main {
  padding: 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

section {
  margin-bottom: 2rem;
}

section h2,
section h3 {
  color: var(--azul-escuro);
  margin-bottom: 1rem;
}

section p {
  margin-bottom: 0.5rem;
  line-height: 1.6;
}

.books ul {
  display: flex;
  gap: 2rem;
  list-style: none;
  padding: 0;
  flex-wrap: wrap;
}

.books li {
  background-color: white;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 119, 204, 0.15);
  padding: 1rem;
  width: 200px;
  text-align: center;
  transition: transform 0.2s;
}

.books li:hover {
  transform: translateY(-4px);
}

.books img {
  border-radius: 8px;
  margin-bottom: 0.5rem;
}


button {
  background-color: #003366;
  color: white;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: 0.2s;
}

button:hover {
  background-color: #003366;
}

div[v-else] {
  background-color: white;
  padding: 1.5rem;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
}

footer {
  background-color:#003366;
  color: white;
  padding: 2rem;
  margin-top: 2rem;
}

footer ul {
  list-style: none;
  display: flex;
  gap: 1rem;
  margin-top: 0.5rem;
}

footer i {
  font-size: 1.3rem;
}

footer hr {
  margin: 1rem 0;
  border: 0;
  height: 1px;
  background-color: white;
  opacity: 0.2;
}
</style>
