<template>
  <div class="app">
    <header class="header">
      <div class="logo">
        <span class="logo-main">AL</span>
        <span class="logo-text">MINISERIES</span>
      </div>

      <button class="profile" @click="activePage = 'profile'">
        👤
      </button>
    </header>

    <main>
      <!-- INICIO -->
      <template v-if="activePage === 'home'">
        <section class="hero">
          <div class="hero-content">
            <span class="badge">🎬 AL MINISERIES</span>

            <h1>Historias que<br />te atrapan.</h1>

            <p>
              Descubre miniseries, historias originales y contenido creado
              para disfrutar capítulo tras capítulo.
            </p>

            <button class="primary-button" @click="activePage = 'explore'">
              ▶ Comenzar a ver
            </button>
          </div>
        </section>

        <section class="section">
          <div class="section-title">
            <h2>🔥 Destacadas</h2>

            <button class="see-all" @click="activePage = 'explore'">
              Ver todas ›
            </button>
          </div>

          <div class="series-grid">
            <button class="card card-one" @click="selectSeries('Próximamente')">
              <div class="card-overlay">
                <small>MINISERIE</small>
                <h3>Próximamente</h3>
              </div>
            </button>

            <button class="card card-two" @click="selectSeries('Una nueva historia')">
              <div class="card-overlay">
                <small>ORIGINAL AL</small>
                <h3>Una nueva historia</h3>
              </div>
            </button>

            <button class="card card-three" @click="selectSeries('Muy pronto')">
              <div class="card-overlay">
                <small>ESTRENO</small>
                <h3>Muy pronto</h3>
              </div>
            </button>
          </div>

          <p v-if="selectedSeries" class="selected-message">
            Has seleccionado: {{ selectedSeries }}
          </p>
        </section>

        <section class="section categories">
          <h2>Explora</h2>

          <div class="category-grid">
            <button @click="activePage = 'explore'">🎭 Drama</button>
            <button @click="activePage = 'explore'">❤️ Romance</button>
            <button @click="activePage = 'explore'">😂 Comedia</button>
            <button @click="activePage = 'explore'">🔪 Suspenso</button>
          </div>
        </section>
      </template>

      <!-- EXPLORAR -->
      <section v-else-if="activePage === 'explore'" class="page-section">
        <h1>Explorar</h1>
        <p class="page-description">
          Descubre nuevas historias y próximas miniseries de AL MINISERIES.
        </p>

        <div class="explore-card">
          <span>🎬</span>
          <div>
            <h2>Catálogo próximamente</h2>
            <p>
              Aquí aparecerán tus miniseries cuando conectemos la plataforma
              con la base de datos.
            </p>
          </div>
        </div>
      </section>

      <!-- MI LISTA -->
      <section v-else-if="activePage === 'list'" class="page-section">
        <h1>Mi lista</h1>
        <p class="page-description">
          Guarda tus miniseries favoritas en un solo lugar.
        </p>

        <div class="empty-state">
          <span>＋</span>
          <h2>Tu lista está vacía</h2>
          <p>
            Cuando guardes una miniserie, aparecerá aquí.
          </p>

          <button class="primary-button" @click="activePage = 'explore'">
            Explorar contenido
          </button>
        </div>
      </section>

      <!-- PERFIL -->
      <section v-else-if="activePage === 'profile'" class="page-section">
        <div class="profile-heading">
          <div class="large-avatar">👤</div>
          <h1>Anthony Lima</h1>
          <p>Mi perfil de AL MINISERIES</p>
        </div>

        <div class="profile-options">
          <button>⚙️ Configuración</button>
          <button>🌐 Idioma</button>
          <button>🎞️ Calidad de reproducción</button>
        </div>
      </section>
    </main>

    <!-- NAVEGACIÓN INFERIOR -->
    <nav class="bottom-nav">
      <button
        :class="{ active: activePage === 'home' }"
        @click="activePage = 'home'"
      >
        <span>⌂</span>
        Inicio
      </button>

      <button
        :class="{ active: activePage === 'explore' }"
        @click="activePage = 'explore'"
      >
        <span>🔎</span>
        Explorar
      </button>

      <button
        :class="{ active: activePage === 'list' }"
        @click="activePage = 'list'"
      >
        <span>＋</span>
        Mi lista
      </button>

      <button
        :class="{ active: activePage === 'profile' }"
        @click="activePage = 'profile'"
      >
        <span>👤</span>
        Perfil
      </button>
    </nav>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const activePage = ref('home')
const selectedSeries = ref('')

function selectSeries(seriesName) {
  selectedSeries.value = seriesName
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background: #080808;
  color: white;
  font-family: Arial, Helvetica, sans-serif;
}

button {
  border: none;
  cursor: pointer;
  color: white;
}

.app {
  min-height: 100vh;
  background: #080808;
  padding-bottom: 90px;
}

.header {
  height: 70px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 22px;
  background: #090909;
}

.logo {
  display: flex;
  align-items: center;
  gap: 8px;
}

.logo-main {
  font-size: 28px;
  font-weight: 900;
  letter-spacing: -2px;
}

.logo-text {
  font-size: 12px;
  font-weight: bold;
  letter-spacing: 2px;
  opacity: 0.8;
}

.profile {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: #1b1b1b;
  font-size: 18px;
}

main {
  min-height: calc(100vh - 145px);
}

.hero {
  min-height: 430px;
  display: flex;
  align-items: flex-end;
  padding: 35px 22px;
  background:
    linear-gradient(
      to top,
      #080808 0%,
      rgba(8, 8, 8, 0.65) 45%,
      rgba(8, 8, 8, 0.15) 100%
    ),
    linear-gradient(135deg, #42145c, #111111 55%, #111);
}

.hero-content {
  max-width: 600px;
}

.badge {
  display: inline-block;
  margin-bottom: 15px;
  font-size: 11px;
  font-weight: bold;
  letter-spacing: 1.5px;
  opacity: 0.85;
}

.hero h1 {
  font-size: 48px;
  line-height: 0.98;
  margin-bottom: 18px;
  letter-spacing: -2px;
}

.hero p {
  color: #cfcfcf;
  line-height: 1.5;
  font-size: 15px;
  max-width: 430px;
  margin-bottom: 25px;
}

.primary-button {
  background: white;
  color: black;
  padding: 14px 22px;
  border-radius: 8px;
  font-weight: bold;
  font-size: 14px;
}

.section {
  padding: 28px 20px 0;
}

.section-title {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 16px;
}

.section h2 {
  font-size: 21px;
}

.see-all {
  background: transparent;
  color: #aaa;
  font-size: 13px;
}

.series-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
}

.card {
  height: 190px;
  border-radius: 10px;
  overflow: hidden;
  position: relative;
  background: linear-gradient(145deg, #32104a, #111);
  text-align: left;
  padding: 0;
}

.card-two {
  background: linear-gradient(145deg, #173c52, #111);
}

.card-three {
  background: linear-gradient(145deg, #542219, #111);
}

.card-overlay {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 12px;
  background: linear-gradient(transparent, rgba(0, 0, 0, 0.9));
}

.card-overlay small {
  font-size: 8px;
  color: #ccc;
}

.card-overlay h3 {
  font-size: 13px;
  margin-top: 4px;
}

.selected-message {
  margin-top: 15px;
  color: #bbb;
  font-size: 13px;
}

.categories {
  padding-bottom: 20px;
}

.category-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
  margin-top: 15px;
}

.category-grid button {
  padding: 16px;
  border-radius: 10px;
  background: #171717;
  text-align: left;
  font-size: 14px;
}

.page-section {
  padding: 35px 22px;
}

.page-section h1 {
  font-size: 34px;
  margin-bottom: 12px;
}

.page-description {
  color: #aaa;
  line-height: 1.5;
  margin-bottom: 25px;
}

.explore-card {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 20px;
  border-radius: 12px;
  background: #171717;
}

.explore-card > span {
  font-size: 35px;
}

.explore-card h2 {
  font-size: 18px;
  margin-bottom: 8px;
}

.explore-card p,
.empty-state p {
  color: #aaa;
  line-height: 1.5;
  font-size: 14px;
}

.empty-state {
  text-align: center;
  padding: 60px 20px;
  background: #111;
  border-radius: 15px;
}

.empty-state > span {
  display: block;
  font-size: 40px;
  margin-bottom: 15px;
}

.empty-state h2 {
  margin-bottom: 10px;
}

.empty-state .primary-button {
  margin-top: 25px;
}

.profile-heading {
  text-align: center;
  padding: 20px 0 30px;
}

.large-avatar {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 85px;
  height: 85px;
  margin: 0 auto 15px;
  border-radius: 50%;
  background: #242424;
  font-size: 35px;
}

.profile-heading h1 {
  margin-bottom: 8px;
}

.profile-heading p {
  color: #aaa;
}

.profile-options {
  display: grid;
  gap: 10px;
}

.profile-options button {
  background: #171717;
  border-radius: 10px;
  padding: 18px;
  text-align: left;
  font-size: 14px;
}

.bottom-nav {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: 75px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  background: rgba(12, 12, 12, 0.96);
  border-top: 1px solid #222;
  z-index: 20;
}

.bottom-nav button {
  background: transparent;
  color: #777;
  font-size: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 5px;
}

.bottom-nav button span {
  font-size: 21px;
}

.bottom-nav .active {
  color: white;
}

@media (max-width: 600px) {
  .series-grid {
    grid-template-columns: repeat(3, 1fr);
  }

  .card {
    height: 170px;
  }

  .hero h1 {
    font-size: 43px;
  }
}
</style>