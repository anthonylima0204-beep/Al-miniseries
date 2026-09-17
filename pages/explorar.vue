<template>
  <div class="explore-page">
    <!-- ENCABEZADO -->
    <header class="top-header">
      <NuxtLink to="/" class="brand">
        <div class="brand-icon">AL</div>

        <div>
          <h1>AL MINISERIES</h1>
          <p>Descubre nuevas historias</p>
        </div>
      </NuxtLink>

      <button class="profile-button" @click="goProfile">
        <span>♙</span>
      </button>
    </header>

    <!-- CONTENIDO -->
    <main class="content">
      <section class="intro">
        <span class="eyebrow">✦ EXPLORA NUESTRO CATÁLOGO</span>

        <h2>Encuentra tu próxima historia favorita.</h2>

        <p>
          Explora miniseries, géneros y nuevas historias creadas para ti.
        </p>
      </section>

      <!-- BUSCADOR -->
      <div class="search-box">
        <span class="search-icon">⌕</span>

        <input
          v-model="search"
          type="search"
          placeholder="Buscar miniseries..."
        />

        <button
          v-if="search"
          class="clear-search"
          aria-label="Limpiar búsqueda"
          @click="search = ''"
        >
          ×
        </button>
      </div>

      <!-- CATEGORÍAS -->
      <section class="categories-section">
        <div class="section-heading">
          <h3>Categorías</h3>
          <span>{{ filteredSeries.length }} títulos</span>
        </div>

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

      <!-- CATÁLOGO -->
      <section class="catalog-section">
        <div class="section-heading">
          <h3>Miniseries para ti</h3>
          <span>{{ filteredSeries.length }} resultados</span>
        </div>

        <div v-if="filteredSeries.length" class="series-grid">
          <article
            v-for="item in filteredSeries"
            :key="item.id"
            class="series-card"
          >
            <!-- PORTADA DE LA TARJETA -->
            <button
              class="poster"
              :class="item.color"
              :aria-label="`Ver información de ${item.title}`"
              @click="openDetails(item)"
            >
              <div class="poster-decoration">
                {{ item.symbol }}
              </div>

              <div class="poster-content">
                <span class="poster-brand">AL MINISERIES</span>
                <strong>{{ item.shortTitle }}</strong>
                <small>{{ item.category }}</small>
              </div>

              <span class="series-tag">{{ item.tag }}</span>

              <span class="poster-play">▶</span>
            </button>

            <!-- INFORMACIÓN -->
            <div class="series-info">
              <div class="title-row">
                <h4>{{ item.title }}</h4>

                <button
                  class="favorite-button"
                  :class="{ favorite: isFavorite(item.id) }"
                  :aria-label="`Guardar ${item.title}`"
                  @click="toggleFavorite(item.id)"
                >
                  {{ isFavorite(item.id) ? '♥' : '♡' }}
                </button>
              </div>

              <p class="series-meta">
                {{ item.category }} · {{ item.year }} · {{ item.age }}
              </p>

              <p class="series-description">
                {{ item.description }}
              </p>

              <button
                class="details-button"
                @click="openDetails(item)"
              >
                Ver detalles <span>→</span>
              </button>
            </div>
          </article>
        </div>

        <!-- SIN RESULTADOS -->
        <div v-else class="empty-state">
          <span class="empty-icon">⌕</span>
          <h3>No encontramos resultados</h3>
          <p>Prueba con otro título o selecciona otra categoría.</p>

          <button class="reset-button" @click="resetFilters">
            Limpiar filtros
          </button>
        </div>
      </section>
    </main>

    <!-- VENTANA DE DETALLES -->
    <Transition name="modal">
      <div
        v-if="selectedSeries"
        class="modal-backdrop"
        @click.self="closeDetails"
      >
        <section class="details-modal" aria-modal="true" role="dialog">
          <button
            class="close-modal"
            aria-label="Cerrar detalles"
            @click="closeDetails"
          >
            ×
          </button>

          <div
            class="modal-cover"
            :class="selectedSeries.color"
          >
            <span>{{ selectedSeries.symbol }}</span>
            <small>AL MINISERIES ORIGINAL</small>
          </div>

          <div class="modal-content">
            <span class="modal-tag">{{ selectedSeries.tag }}</span>

            <h2>{{ selectedSeries.title }}</h2>

            <p class="modal-meta">
              {{ selectedSeries.category }} ·
              {{ selectedSeries.year }} ·
              {{ selectedSeries.age }}
            </p>

            <p>{{ selectedSeries.description }}</p>

            <div class="modal-actions">
              <button class="primary-button" @click="startWatching">
                ▶ Ver ahora
              </button>

              <button
                class="secondary-button"
                @click="toggleFavorite(selectedSeries.id)"
              >
                {{ isFavorite(selectedSeries.id) ? '♥ En mi lista' : '♡ Mi lista' }}
              </button>
            </div>
          </div>
        </section>
      </div>
    </Transition>

    <!-- NAVEGACIÓN INFERIOR -->
    <nav class="bottom-navigation">
      <NuxtLink to="/" class="nav-item">
        <span>⌂</span>
        <small>Inicio</small>
      </NuxtLink>

      <NuxtLink to="/explorar" class="nav-item active">
        <span>▦</span>
        <small>Explorar</small>
      </NuxtLink>

      <NuxtLink to="/mi-lista" class="nav-item">
        <span>♡</span>
        <small>Mi lista</small>
      </NuxtLink>

      <NuxtLink to="/perfil" class="nav-item">
        <span>♙</span>
        <small>Perfil</small>
      </NuxtLink>
    </nav>
  </div>
</template>

<script setup>
import { computed, onMounted, ref } from 'vue'

const search = ref('')
const selectedCategory = ref('Todas')
const selectedSeries = ref(null)
const favoriteIds = ref([])

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
    shortTitle: 'ECLIPSIA',
    category: 'Fantasía',
    year: '2025',
    age: '16+',
    tag: 'ORIGINAL AL',
    symbol: '✦',
    color: 'purple',
    description:
      'En un reino donde la magia se desvanece, una princesa exiliada y una hechicera rebelde deben unir sus destinos.'
  },
  {
    id: 2,
    title: 'El Legado Perdido',
    shortTitle: 'EL LEGADO',
    category: 'Drama',
    year: '2025',
    age: '13+',
    tag: 'NUEVO',
    symbol: '♜',
    color: 'green',
    description:
      'Un joven descubre un secreto familiar que lo llevará a recorrer lugares olvidados y enfrentar su pasado.'
  },
  {
    id: 3,
    title: 'La Última Niebla',
    shortTitle: 'LA NIEBLA',
    category: 'Ciencia ficción',
    year: '2025',
    age: '16+',
    tag: 'ESTRENO',
    symbol: '◈',
    color: 'blue',
    description:
      'En una ciudad cubierta por una niebla misteriosa, un grupo de sobrevivientes busca la verdad detrás del fenómeno.'
  },
  {
    id: 4,
    title: 'Amor en Tiempos Digitales',
    shortTitle: 'AMOR DIGITAL',
    category: 'Romance',
    year: '2025',
    age: '13+',
    tag: 'DESTACADA',
    symbol: '♡',
    color: 'orange',
    description:
      'Dos personas de mundos diferentes descubren que una conexión inesperada puede cambiar sus vidas para siempre.'
  },
  {
    id: 5,
    title: 'El Enigma',
    shortTitle: 'EL ENIGMA',
    category: 'Suspenso',
    year: '2025',
    age: '16+',
    tag: 'MISTERIO',
    symbol: '⌘',
    color: 'dark',
    description:
      'Una serie de pistas ocultas conduce a un investigador hacia un misterio que nadie se atreve a resolver.'
  },
  {
    id: 6,
    title: 'Amigos Inesperados',
    shortTitle: 'AMIGOS',
    category: 'Comedia',
    year: '2025',
    age: 'Todo público',
    tag: 'DIVERTIDA',
    symbol: '☀',
    color: 'yellow',
    description:
      'Una amistad poco común demuestra que las mejores historias pueden comenzar de la manera más inesperada.'
  }
]

const filteredSeries = computed(() => {
  const text = search.value.trim().toLowerCase()

  return series.filter((item) => {
    const matchesCategory =
      selectedCategory.value === 'Todas' ||
      item.category === selectedCategory.value

    const matchesSearch =
      !text ||
      item.title.toLowerCase().includes(text) ||
      item.category.toLowerCase().includes(text)

    return matchesCategory && matchesSearch
  })
})

onMounted(() => {
  const savedFavorites = localStorage.getItem('al-miniseries-favorites')

  if (savedFavorites) {
    try {
      favoriteIds.value = JSON.parse(savedFavorites)
    } catch {
      favoriteIds.value = []
    }
  }
})

function isFavorite(id) {
  return favoriteIds.value.includes(id)
}

function toggleFavorite(id) {
  if (isFavorite(id)) {
    favoriteIds.value = favoriteIds.value.filter((item) => item !== id)
  } else {
    favoriteIds.value.push(id)
  }

  if (import.meta.client) {
    localStorage.setItem(
      'al-miniseries-favorites',
      JSON.stringify(favoriteIds.value)
    )
  }
}

function openDetails(item) {
  selectedSeries.value = item
}

function closeDetails() {
  selectedSeries.value = null
}

function resetFilters() {
  search.value = ''
  selectedCategory.value = 'Todas'
}

function startWatching() {
  closeDetails()
  navigateTo('/')
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
    radial-gradient(circle at 100% 0%, #202b75 0%, transparent 32%),
    radial-gradient(circle at 0% 45%, #251052 0%, transparent 30%),
    linear-gradient(180deg, #050b25 0%, #070b1d 60%, #030510 100%);
  font-family: Arial, Helvetica, sans-serif;
}

.top-header {
  position: sticky;
  top: 0;
  z-index: 15;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 18px 20px;
  border-bottom: 1px solid rgba(91, 126, 255, 0.25);
  background: rgba(4, 9, 30, 0.94);
  backdrop-filter: blur(18px);
}

.brand {
  display: flex;
  align-items: center;
  gap: 12px;
  color: white;
  text-decoration: none;
}

.brand-icon {
  display: grid;
  place-items: center;
  width: 48px;
  height: 48px;
  border: 2px solid #684dff;
  border-radius: 15px;
  color: #83edff;
  font-size: 18px;
  font-weight: 900;
  background: linear-gradient(135deg, #26106c, #102b82);
  box-shadow: 0 0 22px rgba(91, 64, 255, 0.6);
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
  display: grid;
  place-items: center;
  width: 44px;
  height: 44px;
  border: 1px solid #38539b;
  border-radius: 50%;
  color: #6ed5ff;
  background: #101b46;
  font-size: 25px;
  cursor: pointer;
}

.content {
  width: 100%;
  max-width: 1100px;
  margin: 0 auto;
  padding: 30px 20px;
}

.intro {
  margin-bottom: 25px;
}

.eyebrow {
  color: #8e9fff;
  font-size: 11px;
  font-weight: bold;
  letter-spacing: 1.4px;
}

.intro h2 {
  max-width: 680px;
  margin: 14px 0;
  font-size: clamp(30px, 7vw, 54px);
  line-height: 1.08;
  letter-spacing: -1.5px;
}

.intro p {
  max-width: 600px;
  color: #aeb8db;
  font-size: 15px;
  line-height: 1.7;
}

.search-box {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 15px 17px;
  border: 1px solid #30488c;
  border-radius: 16px;
  background: rgba(13, 25, 65, 0.9);
  box-shadow: 0 0 30px rgba(27, 55, 150, 0.16);
}

.search-icon {
  color: #8baaff;
  font-size: 27px;
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

.clear-search {
  border: none;
  color: #b5c6ff;
  background: transparent;
  font-size: 24px;
  cursor: pointer;
}

.categories-section,
.catalog-section {
  margin-top: 32px;
}

.section-heading {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 15px;
}

.section-heading h3 {
  margin: 0 0 15px;
  font-size: 22px;
}

.section-heading span {
  color: #8393c8;
  font-size: 13px;
}

.categories {
  display: flex;
  gap: 9px;
  overflow-x: auto;
  padding: 4px 0 10px;
  scrollbar-width: none;
}

.categories::-webkit-scrollbar {
  display: none;
}

.categories button {
  flex-shrink: 0;
  padding: 11px 17px;
  border: 1px solid #293d78;
  border-radius: 30px;
  color: #aebce7;
  background: #0d173b;
  cursor: pointer;
  font-size: 13px;
  transition: 0.2s ease;
}

.categories button:hover {
  border-color: #6c74ff;
}

.categories button.selected {
  border-color: #714cff;
  color: white;
  background: linear-gradient(90deg, #7a19ef, #245fff);
  box-shadow: 0 0 18px rgba(88, 62, 255, 0.4);
}

.series-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 17px;
}

.series-card {
  overflow: hidden;
  border: 1px solid #24386e;
  border-radius: 18px;
  background: linear-gradient(180deg, #0d1b47, #080f2b);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.22);
  transition: transform 0.2s ease, border-color 0.2s ease;
}

.series-card:hover {
  transform: translateY(-3px);
  border-color: #596dff;
}

.poster {
  position: relative;
  display: flex;
  align-items: flex-end;
  width: 100%;
  height: 210px;
  overflow: hidden;
  border: none;
  color: white;
  text-align: left;
  cursor: pointer;
}

.poster::after {
  position: absolute;
  inset: 0;
  content: '';
  background: linear-gradient(
    180deg,
    transparent 20%,
    rgba(2, 5, 22, 0.15) 40%,
    rgba(2, 5, 22, 0.9) 100%
  );
}

.poster-decoration {
  position: absolute;
  top: 15px;
  right: 16px;
  color: rgba(255, 255, 255, 0.35);
  font-size: 80px;
  text-shadow: 0 0 30px rgba(255, 255, 255, 0.3);
}

.poster-content {
  position: relative;
  z-index: 2;
  display: flex;
  flex-direction: column;
  gap: 7px;
  padding: 15px;
}

.poster-brand {
  color: #b9c8ff;
  font-size: 9px;
  letter-spacing: 1.5px;
}

.poster-content strong {
  max-width: 170px;
  font-size: 25px;
  line-height: 1;
  letter-spacing: -0.5px;
}

.poster-content small {
  color: #d1d9ff;
  font-size: 11px;
}

.series-tag {
  position: absolute;
  z-index: 3;
  top: 12px;
  left: 12px;
  padding: 6px 9px;
  border: 1px solid rgba(255, 255, 255, 0.25);
  border-radius: 7px;
  color: white;
  background: rgba(6, 10, 35, 0.75);
  font-size: 9px;
  font-weight: bold;
}

.poster-play {
  position: absolute;
  z-index: 3;
  right: 13px;
  bottom: 13px;
  display: grid;
  place-items: center;
  width: 35px;
  height: 35px;
  border: 1px solid rgba(255, 255, 255, 0.5);
  border-radius: 50%;
  background: rgba(4, 8, 30, 0.6);
  font-size: 13px;
}

.purple {
  background:
    radial-gradient(circle at 75% 25%, #d24cc6, transparent 28%),
    linear-gradient(145deg, #40147e, #101d68 75%);
}

.green {
  background:
    radial-gradient(circle at 75% 25%, #70ad83, transparent 25%),
    linear-gradient(145deg, #244d45, #102b48 75%);
}

.blue {
  background:
    radial-gradient(circle at 75% 25%, #5e9cff, transparent 28%),
    linear-gradient(145deg, #123d87, #101743 75%);
}

.orange {
  background:
    radial-gradient(circle at 75% 25%, #ff9c6d, transparent 28%),
    linear-gradient(145deg, #913b58, #351b50 75%);
}

.dark {
  background:
    radial-gradient(circle at 75% 25%, #7586b3, transparent 25%),
    linear-gradient(145deg, #303c5d, #090d1c 75%);
}

.yellow {
  background:
    radial-gradient(circle at 75% 25%, #f2d26d, transparent 28%),
    linear-gradient(145deg, #82612b, #35233b 75%);
}

.series-info {
  padding: 14px;
}

.title-row {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 8px;
}

.series-info h4 {
  min-height: 40px;
  margin: 0 0 7px;
  font-size: 15px;
  line-height: 1.3;
}

.favorite-button {
  flex-shrink: 0;
  border: none;
  color: #a5b5e9;
  background: transparent;
  font-size: 24px;
  cursor: pointer;
}

.favorite-button.favorite {
  color: #d45cff;
  text-shadow: 0 0 12px rgba(212, 92, 255, 0.7);
}

.series-meta {
  margin: 0 0 10px;
  color: #91a4d8;
  font-size: 11px;
}

.series-description {
  display: -webkit-box;
  min-height: 42px;
  margin: 0 0 12px;
  overflow: hidden;
  color: #9eadd6;
  font-size: 12px;
  line-height: 1.5;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3;
}

.details-button {
  padding: 8px 0;
  border: none;
  color: #83b2ff;
  background: transparent;
  font-size: 12px;
  font-weight: bold;
  cursor: pointer;
}

.details-button span {
  margin-left: 5px;
}

.empty-state {
  padding: 65px 15px;
  text-align: center;
  color: #a6b2d5;
}

.empty-icon {
  color: #8b9eff;
  font-size: 60px;
}

.empty-state h3 {
  margin: 15px 0 8px;
  color: white;
}

.empty-state p {
  font-size: 14px;
}

.reset-button {
  margin-top: 15px;
  padding: 12px 18px;
  border: 1px solid #586bff;
  border-radius: 25px;
  color: white;
  background: #18265e;
  cursor: pointer;
}

.bottom-navigation {
  position: fixed;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 20;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  height: 86px;
  border-top: 1px solid #1c2c5b;
  background: rgba(4, 9, 28, 0.97);
  backdrop-filter: blur(18px);
}

.nav-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 7px;
  color: #7181ae;
  text-decoration: none;
  font-size: 12px;
}

.nav-item span {
  font-size: 26px;
}

.nav-item.active {
  color: #65d9ff;
  text-shadow: 0 0 15px rgba(68, 178, 255, 0.7);
}

.modal-backdrop {
  position: fixed;
  inset: 0;
  z-index: 50;
  display: grid;
  place-items: center;
  padding: 20px;
  background: rgba(0, 0, 15, 0.82);
  backdrop-filter: blur(8px);
}

.details-modal {
  position: relative;
  width: 100%;
  max-width: 480px;
  max-height: 90vh;
  overflow: auto;
  border: 1px solid #526dff;
  border-radius: 22px;
  background: #0a1233;
  box-shadow: 0 0 60px rgba(70, 70, 255, 0.35);
}

.close-modal {
  position: absolute;
  z-index: 3;
  top: 12px;
  right: 12px;
  width: 35px;
  height: 35px;
  border: 1px solid rgba(255, 255, 255, 0.35);
  border-radius: 50%;
  color: white;
  background: rgba(5, 8, 25, 0.75);
  font-size: 25px;
  cursor: pointer;
}

.modal-cover {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 220px;
  overflow: hidden;
}

.modal-cover span {
  color: rgba(255, 255, 255, 0.5);
  font-size: 120px;
}

.modal-cover small {
  position: absolute;
  bottom: 15px;
  left: 20px;
  color: #d1dcff;
  font-size: 10px;
  letter-spacing: 2px;
}

.modal-content {
  padding: 24px;
}

.modal-tag {
  color: #9caeff;
  font-size: 11px;
  font-weight: bold;
  letter-spacing: 1px;
}

.modal-content h2 {
  margin: 10px 0;
  font-size: 28px;
  line-height: 1.1;
}

.modal-meta {
  color: #91a4d8;
  font-size: 13px;
}

.modal-content > p:not(.modal-meta) {
  color: #b5c2e4;
  font-size: 14px;
  line-height: 1.7;
}

.modal-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 22px;
}

.primary-button,
.secondary-button {
  padding: 13px 17px;
  border-radius: 25px;
  font-weight: bold;
  cursor: pointer;
}

.primary-button {
  border: none;
  color: white;
  background: linear-gradient(90deg, #a400ff, #176eff);
  box-shadow: 0 0 20px rgba(109, 50, 255, 0.35);
}

.secondary-button {
  border: 1px solid #5269bd;
  color: #c0ceff;
  background: #111e4a;
}

.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.2s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}

@media (min-width: 700px) {
  .series-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
  }

  .poster {
    height: 235px;
  }
}

@media (max-width: 420px) {
  .content {
    padding: 25px 14px;
  }

  .series-grid {
    gap: 10px;
  }

  .series-info {
    padding: 11px;
  }

  .series-info h4 {
    font-size: 13px;
  }

  .series-description {
    font-size: 11px;
  }

  .poster {
    height: 175px;
  }

  .poster-content strong {
    font-size: 21px;
  }
}
</style>