<script setup>
import { ref, reactive } from "vue";

const books = ref([
  {
    id: 1,
    cover: "",
    title: "Lua Nova",
    subject: "Romance",
    preco: "34.90",
  },
  {
    id: 2,
    cover: "https://via.placeholder.com/120x180",
    title: "Crepúsculo",
    subject: "Fantasia",
    preco: "39.90",
  },
  {
    id: 3,
    cover: "https://via.placeholder.com/120x180",
    title: "Eclipse",
    subject: "Aventura",
    preco: "42.00",
  },
]);

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
        <input type="text" placeholder="Pesquisar livros..." />
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
            <li v-for="book in books" :key="book.id">
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