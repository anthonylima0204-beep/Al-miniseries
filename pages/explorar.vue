

<template>
  <div class="explore-page">
    <header class="header">
      <NuxtLink to="/" class="logo">
        <strong>AL</strong>
        <span>MINISERIES</span>
      </NuxtLink>

      <NuxtLink to="/" class="back-button">←</NuxtLink>
    </header>

    <main class="content">
      <section class="intro">
        <span class="eyebrow">AL MINISERIES</span>
        <h1>Explora nuevas historias</h1>
        <p>
          Encuentra miniseries, historias originales y contenido de tus géneros
          favoritos.
        </p>
      </section>

      <section class="search-section">
        <div class="search-box">
          <span>⌕</span>
          <input
            v-model="search"
            type="search"
            placeholder="Buscar miniseries..."
          />
        </div>
      </section>

      <section class="categories">
        <h2>Categorías</h2>

        <div class="category-list">
          <button
            v-for="category in categories"
            :key="category"
            :class="{ selected: selectedCategory === category }"
            @click="selectedCategory = category"
          >
            {{ category }}
          </button>
        </div>
      </section>

      <section class="series-section">
        <div class="section-heading">
          <h2>
            {{ selectedCategory === 'Todos' ? 'Todas las historias' : selectedCategory }}
          </h2>
          <span>{{ filteredSeries.length }} títulos</span>
        </div>

        <div v-if="filteredSeries.length" class="series-grid">
          <article
            v-for="series in filteredSeries"
            :key="series.id"
            class="series-card"
          >
            <div class="poster" :class="series.color">
              <span class="poster-label">{{ series.type }}</span>
              <div class="poster-content">
                <small>AL MINISERIES</small>
                <h3>{{ series.title }}</h3>
                <p>{{ series.genre }}</p>
              </div>
            </div>

            <div class="series-info">
              <h3>{{ series.title }}</h3>
              <p>{{ series.description }}</p>
              <span class="genre">{{ series.genre }}</span>
            </div>
          </article>
        </div>

        <div v-else class="empty-state">
          <span>🔎</span>
          <h3>No encontramos historias</h3>
          <p>Prueba con otro nombre o selecciona otra categoría.</p>
        </div>
      </section>
    </main>

    <nav class="bottom-nav">
      <NuxtLink to="/">
        <span>⌂</span>
        Inicio
      </NuxtLink>

      <NuxtLink to="/explorar" class="active">
        <span>▦</span>
        Explorar
      </NuxtLink>

      <NuxtLink to="/">
        <span>＋</span>
        Mi lista
      </NuxtLink>

      <NuxtLink to="/">
        <span>♙</span>
        Perfil
      </NuxtLink>
    </nav>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue'

const search = ref('')
const selectedCategory = ref('Todos')

const categories = [
  'Todos',
  'Drama',
  'Romance',
  'Comedia',
  'Fantasía',
  'Suspenso',
  'Ciencia ficción'
]

const series = [
  {
    id: 1,
    title: 'Eclipsia: El Último Hechizo',
    genre: 'Fantasía',
    type: 'ORIGINAL AL',
    description: 'Una historia de magia, secretos y destinos cruzados.',
    color: 'purple'
  },
  {
    id: 2,
    title: 'El Enigma',
    genre: 'Suspenso',
    type: 'NUEVO',
    description: 'Un misterio que cambiará la vida de sus protagonistas.',
    color: 'blue'
  },
  {
    id: 3,
    title: 'Amor en Tiempos Digitales',
    genre: 'Romance',
    type: 'DESTACADA',
    description: 'Dos corazones que se encuentran en un mundo conectado.',
    color: 'pink'
  },
  {
    id: 4,
    title: 'El Legado Perdido',
    genre: 'Drama',
    type: 'MINISERIE',
    description: 'Una familia, un secreto y una verdad escondida.',
    color: 'green'
  },
  {
    id: 5,
    title: 'La Última Nebulosa',
    genre: 'Ciencia ficción',
    type: 'ESTRENO',
    description: 'Una aventura más allá de las estrellas.',
    color: 'cyan'
  },
  {
    id: 6,
    title: 'Amigos Inesperados',
    genre: 'Comedia',
    type: 'ORIGINAL AL',
    description: 'Una amistad inesperada llena de momentos divertidos.',
    color: 'orange'
  }
]

const filteredSeries = computed(() => {
  return series.filter((item) => {
    const matchesCategory =
      selectedCategory.value === 'Todos' ||
      item.genre === selectedCategory.value

    const matchesSearch = item.title
      .toLowerCase()
      .includes(search.value.toLowerCase())

    return matchesCategory && matchesSearch
  })
})
</script>

<style scoped>
* {
  box-sizing: border-box;
}

.explore-page {
  min-height: 100vh;
  padding-bottom: 90px;
  color: #ffffff;
  background:
    radial-gradient(circle at top right, #17134b 0%, transparent 35%),
    #050817;
  font-family: Arial, Helvetica, sans-serif;
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 22px 24px;
  border-bottom: 1px solid rgba(100, 130, 255, 0.2);
  background: rgba(5, 8, 23, 0.95);
}

.logo {
  display: flex;
  align-items: center;
  gap: 10px;
  color: white;
  text-decoration: none;
}

.logo strong {
  color: #70eaff;
  font-size: 30px;
  letter-spacing: -2px;
}

.logo span {
  font-size: 15px;
  font-weight: 700;
  letter-spacing: 4px;
}

.back-button {
  color: #8f9bca;
  font-size: 30px;
  text-decoration: none;
}

.content {
  width: min(1100px, 100%);
  margin: auto;
  padding: 28px 22px;
}

.intro {
  margin-bottom: 25px;
}

.eyebrow {
  color: #8c78ff;
  font-size: 12px;
  font-weight: bold;
  letter-spacing: 3px;
}

h1 {
  margin: 12px 0;
  font-size: clamp(32px, 6vw, 54px);
  line-height: 1.05;
}

.intro p {
  max-width: 600px;
  color: #aeb7d5;
  font-size: 16px;
  line-height: 1.6;
}

.search-box {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 15px 18px;
  border: 1px solid #29366d;
  border-radius: 15px;
  background: #0c1330;
}

.search-box span {
  color: #76dfff;
  font-size: 28px;
}

.search-box input {
  width: 100%;
  border: none;
  outline: none;
  color: white;
  background: transparent;
  font-size: 16px;
}

.search-box input::placeholder {
  color: #7e89b0;
}

.categories {
  margin-top: 32px;
}

h2 {
  margin-bottom: 17px;
  font-size: 24px;
}

.category-list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.category-list button {
  padding: 11px 17px;
  border: 1px solid #2d3d79;
  border-radius: 30px;
  color: #aeb9e0;
  background: #0c1433;
  cursor: pointer;
  transition: 0.2s;
}

.category-list button.selected,
.category-list button:hover {
  border-color: #7b55ff;
  color: white;
  background: linear-gradient(90deg, #7a18ff, #176dff);
}

.series-section {
  margin-top: 35px;
}

.section-heading {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 15px;
}

.section-heading span {
  color: #7e91cb;
  font-size: 14px;
}

.series-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 18px;
}

.series-card {
  overflow: hidden;
  border: 1px solid #23356e;
  border-radius: 17px;
  background: #0b1432;
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.2);
}

.poster {
  position: relative;
  display: flex;
  min-height: 245px;
  align-items: flex-end;
  padding: 18px;
  overflow: hidden;
}

.poster::before {
  position: absolute;
  inset: 0;
  content: '';
  background: linear-gradient(transparent 25%, rgba(3, 5, 20, 0.95));
}

.poster-label,
.poster-content {
  position: relative;
  z-index: 1;
}

.poster-label {
  position: absolute;
  top: 14px;
  left: 14px;
  padding: 6px 9px;
  border-radius: 7px;
  color: white;
  background: rgba(20, 10, 70, 0.75);
  font-size: 10px;
  font-weight: bold;
}

.poster-content small {
  color: #a9d9ff;
  font-size: 10px;
  letter-spacing: 2px;
}

.poster-content h3 {
  margin: 8px 0;
  font-size: 25px;
  line-height: 1.05;
}

.poster-content p {
  margin: 0;
  color: #d3dafa;
  font-size: 13px;
}

.purple {
  background: linear-gradient(145deg, #7b19b5, #11154b 75%);
}

.blue {
  background: linear-gradient(145deg, #147ca4, #10172e 75%);
}

.pink {
  background: linear-gradient(145deg, #c64b7d, #31164b 75%);
}

.green {
  background: linear-gradient(145deg, #28795b, #102d36 75%);
}

.cyan {
  background: linear-gradient(145deg, #155cba, #10163d 75%);
}

.orange {
  background: linear-gradient(145deg, #bd6a35, #321a31 75%);
}

.series-info {
  padding: 15px;
}

.series-info h3 {
  margin: 0 0 8px;
  font-size: 17px;
}

.series-info p {
  min-height: 38px;
  margin: 0 0 12px;
  color: #8e9cc4;
  font-size: 13px;
  line-height: 1.4;
}

.genre {
  display: inline-block;
  padding: 5px 9px;
  border-radius: 7px;
  color: #83dfff;
  background: #132758;
  font-size: 11px;
}

.empty-state {
  padding: 55px 15px;
  text-align: center;
  color: #9aa8d0;
}

.empty-state span {
  font-size: 40px;
}

.empty-state h3 {
  color: white;
}

.bottom-nav {
  position: fixed;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 10;
  display: flex;
  justify-content: space-around;
  padding: 13px 8px 15px;
  border-top: 1px solid #1c2d5d;
  background: rgba(5, 8, 23, 0.97);
  backdrop-filter: blur(12px);
}

.bottom-nav a {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5px;
  color: #7785ad;
  font-size: 12px;
  text-decoration: none;
}

.bottom-nav a span {
  font-size: 24px;
}

.bottom-nav a.active,
.bottom-nav a:hover {
  color: #65eaff;
}

@media (max-width: 500px) {
  .header {
    padding: 18px;
  }

  .logo span {
    font-size: 12px;
    letter-spacing: 2px;
  }

  .content {
    padding: 25px 15px;
  }

  .series-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 12px;
  }

  .poster {
    min-height: 190px;
    padding: 12px;
  }

  .poster-content h3 {
    font-size: 19px;
  }

  .series-info {
    padding: 11px;
  }

  .series-info h3 {
    font-size: 14px;
  }

  .series-info p {
    font-size: 12px;
  }
}
</style>