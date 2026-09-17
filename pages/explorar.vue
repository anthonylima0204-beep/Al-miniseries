<template>
  <div class="explore-page">
    <!-- Encabezado -->
    <header class="top-header">
      <div class="brand">
        <div class="brand-icon">AL</div>
        <div>
          <h1>AL MINISERIES</h1>
          <p>Descubre nuevas historias</p>
        </div>
      </div>

      <button class="profile-button">👤</button>
    </header>

    <!-- Contenido principal -->
    <main class="content">
      <section class="intro">
        <span class="eyebrow">🎬 EXPLORA NUESTRO CATÁLOGO</span>
        <h2>Encuentra tu próxima historia favorita.</h2>
        <p>
          Explora miniseries, géneros y nuevas historias creadas para ti.
        </p>
      </section>

      <!-- Buscador -->
      <div class="search-box">
        <span>🔎</span>
        <input
          v-model="search"
          type="text"
          placeholder="Buscar miniseries..."
        />
      </div>

      <!-- Categorías -->
      <section class="categories-section">
        <h3>Categorías</h3>

        <div class="categories">
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

      <!-- Catálogo -->
      <section class="catalog-section">
        <div class="section-heading">
          <h3>Miniseries para ti</h3>
          <span>{{ filteredSeries.length }} títulos</span>
        </div>

        <div v-if="filteredSeries.length" class="series-grid">
          <article
            v-for="series in filteredSeries"
            :key="series.id"
            class="series-card"
          >
            <div
              class="poster"
              :class="series.color"
            >
              <span class="poster-icon">{{ series.icon }}</span>
              <span class="series-tag">{{ series.tag }}</span>
            </div>

            <div class="series-info">
              <h4>{{ series.title }}</h4>
              <p>{{ series.category }} · {{ series.year }}</p>

              <button class="details-button">
                Ver detalles →
              </button>
            </div>
          </article>
        </div>

        <div v-else class="empty-state">
          <span>🔍</span>
          <h3>No encontramos resultados</h3>
          <p>Prueba con otro título o categoría.</p>
        </div>
      </section>
    </main>

    <!-- Navegación inferior -->
    <nav class="bottom-navigation">
      <button @click="goHome">
        <span>⌂</span>
        Inicio
      </button>

      <button class="active">
        <span>▦</span>
        Explorar
      </button>

      <button @click="goList">
        <span>♡</span>
        Mi lista
      </button>

      <button @click="goProfile">
        <span>♙</span>
        Perfil
      </button>
    </nav>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue'

const search = ref('')
const selectedCategory = ref('Todas')

const categories = [
  'Todas',
  'Drama',
  'Romance',
  'Fantasía',
  'Comedia',
  'Suspenso',
  'Ciencia ficción'
]

const series = [
  {
    id: 1,
    title: 'Eclipsia: El Último Hechizo',
    category: 'Fantasía',
    year: '2025',
    tag: 'ORIGINAL AL',
    icon: '🌌',
    color: 'purple'
  },
  {
    id: 2,
    title: 'El Legado Perdido',
    category: 'Drama',
    year: '2025',
    tag: 'NUEVO',
    icon: '🏔️',
    color: 'green'
  },
  {
    id: 3,
    title: 'La Última Niebla',
    category: 'Ciencia ficción',
    year: '2025',
    tag: 'ESTRENO',
    icon: '🌃',
    color: 'blue'
  },
  {
    id: 4,
    title: 'Amor en Tiempos Digitales',
    category: 'Romance',
    year: '2025',
    tag: 'DESTACADA',
    icon: '❤️',
    color: 'orange'
  },
  {
    id: 5,
    title: 'El Enigma',
    category: 'Suspenso',
    year: '2025',
    tag: 'MISTERIO',
    icon: '🔐',
    color: 'dark'
  },
  {
    id: 6,
    title: 'Amigos Inesperados',
    category: 'Comedia',
    year: '2025',
    tag: 'DIVERTIDA',
    icon: '🐶',
    color: 'yellow'
  }
]

const filteredSeries = computed(() => {
  return series.filter((item) => {
    const matchesCategory =
      selectedCategory.value === 'Todas' ||
      item.category === selectedCategory.value

    const matchesSearch = item.title
      .toLowerCase()
      .includes(search.value.toLowerCase())

    return matchesCategory && matchesSearch
  })
})

function goHome() {
  navigateTo('/')
}

function goList() {
  navigateTo('/mi-lista')
}

function goProfile() {
  navigateTo('/perfil')
}
</script>

<style scoped>
* {
  box-sizing: border-box;
}

.explore-page {
  min-height: 100vh;
  padding-bottom: 105px;
  color: #ffffff;
  background:
    radial-gradient(circle at top right, #17245f 0%, transparent 35%),
    linear-gradient(180deg, #050b25 0%, #070b1d 55%, #030510 100%);
  font-family: Arial, Helvetica, sans-serif;
}

.top-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 22px 20px;
  border-bottom: 1px solid rgba(93, 132, 255, 0.2);
  background: rgba(5, 10, 32, 0.9);
}

.brand {
  display: flex;
  align-items: center;
  gap: 12px;
}

.brand-icon {
  display: grid;
  place-items: center;
  width: 48px;
  height: 48px;
  border: 2px solid #5c7cff;
  border-radius: 14px;
  color: #ffffff;
  font-size: 17px;
  font-weight: 900;
  background: linear-gradient(135deg, #7d18ff, #087dff);
  box-shadow: 0 0 18px rgba(74, 102, 255, 0.5);
}

.brand h1 {
  margin: 0;
  font-size: 16px;
  letter-spacing: 1px;
}

.brand p {
  margin: 5px 0 0;
  color: #91a0cf;
  font-size: 12px;
}

.profile-button {
  width: 42px;
  height: 42px;
  border: 1px solid #30447e;
  border-radius: 50%;
  color: white;
  background: #111b42;
  font-size: 20px;
}

.content {
  width: 100%;
  max-width: 1000px;
  margin: 0 auto;
  padding: 28px 20px;
}

.intro {
  margin-bottom: 25px;
}

.eyebrow {
  color: #8d9cff;
  font-size: 11px;
  font-weight: bold;
  letter-spacing: 1.4px;
}

.intro h2 {
  max-width: 600px;
  margin: 12px 0;
  font-size: clamp(30px, 7vw, 52px);
  line-height: 1.08;
}

.intro p {
  max-width: 550px;
  color: #aeb8db;
  font-size: 15px;
  line-height: 1.6;
}

.search-box {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 15px 17px;
  border: 1px solid #263d7b;
  border-radius: 15px;
  background: rgba(13, 25, 65, 0.9);
  box-shadow: 0 0 25px rgba(27, 55, 150, 0.12);
}

.search-box span {
  font-size: 20px;
}

.search-box input {
  width: 100%;
  border: none;
  outline: none;
  color: white;
  background: transparent;
  font-size: 15px;
}

.search-box input::placeholder {
  color: #8491ba;
}

.categories-section {
  margin-top: 30px;
}

.categories-section h3,
.catalog-section h3 {
  margin-bottom: 15px;
  font-size: 21px;
}

.categories {
  display: flex;
  gap: 9px;
  overflow-x: auto;
  padding-bottom: 5px;
}

.categories button {
  flex-shrink: 0;
  padding: 11px 16px;
  border: 1px solid #293d78;
  border-radius: 30px;
  color: #aebce7;
  background: #0d173b;
  cursor: pointer;
  font-size: 13px;
}

.categories button.selected {
  border-color: #6b50ff;
  color: white;
  background: linear-gradient(90deg, #7a19ef, #245fff);
  box-shadow: 0 0 16px rgba(88, 62, 255, 0.35);
}

.catalog-section {
  margin-top: 32px;
}

.section-heading {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.section-heading h3 {
  margin-bottom: 0;
}

.section-heading span {
  color: #8393c8;
  font-size: 13px;
}

.series-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 15px;
}

.series-card {
  overflow: hidden;
  border: 1px solid #24386e;
  border-radius: 16px;
  background: #0b1535;
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
}

.poster {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 175px;
  overflow: hidden;
}

.poster::before {
  position: absolute;
  inset: 0;
  content: '';
  background: linear-gradient(
    145deg,
    rgba(255, 255, 255, 0.13),
    transparent 45%,
    rgba(0, 0, 0, 0.4)
  );
}

.poster-icon {
  position: relative;
  font-size: 65px;
  filter: drop-shadow(0 5px 15px rgba(0, 0, 0, 0.4));
}

.series-tag {
  position: absolute;
  top: 10px;
  left: 10px;
  padding: 5px 8px;
  border-radius: 6px;
  color: white;
  background: rgba(8, 12, 35, 0.8);
  font-size: 9px;
  font-weight: bold;
}

.purple {
  background: linear-gradient(145deg, #7119a8, #111d68);
}

.green {
  background: linear-gradient(145deg, #315e47, #102d48);
}

.blue {
  background: linear-gradient(145deg, #145a9a, #18205f);
}

.orange {
  background: linear-gradient(145deg, #b44d36, #54204f);
}

.dark {
  background: linear-gradient(145deg, #3b465e, #090d1c);
}

.yellow {
  background: linear-gradient(145deg, #aa8236, #593b2c);
}

.series-info {
  padding: 13px;
}

.series-info h4 {
  min-height: 38px;
  margin: 0 0 6px;
  font-size: 15px;
  line-height: 1.3;
}

.series-info p {
  margin: 0 0 12px;
  color: #91a4d8;
  font-size: 12px;
}

.details-button {
  padding: 8px 0;
  color: #80a5ff;
  background: transparent;
  font-size: 12px;
  font-weight: bold;
}

.empty-state {
  padding: 50px 15px;
  text-align: center;
  color: #a6b2d5;
}

.empty-state span {
  font-size: 45px;
}

.empty-state h3 {
  margin-top: 15px;
}

.bottom-navigation {
  position: fixed;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 20;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  height: 82px;
  border-top: 1px solid #1c2c5b;
  background: rgba(4, 9, 28, 0.97);
  backdrop-filter: blur(15px);
}

.bottom-navigation button {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 7px;
  border: none;
  color: #7181ae;
  background: transparent;
  cursor: pointer;
  font-size: 11px;
}

.bottom-navigation button span {
  font-size: 24px;
}

.bottom-navigation button.active {
  color: #65cfff;
  text-shadow: 0 0 12px rgba(68, 178, 255, 0.7);
}

@media (min-width: 700px) {
  .series-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
  }

  .poster {
    height: 220px;
  }
}
</style>