<template>
  <div class="app">
    <!-- ENCABEZADO -->
    <header class="topbar">
      <div class="brand">
        <div class="brand-icon">AL</div>

        <div>
          <h1>AL MINISERIES</h1>
          <p>Historias que te atrapan</p>
        </div>
      </div>

      <button class="avatar-button" @click="activePage = 'profile'">
        👤
      </button>
    </header>

    <main class="content">
      <!-- PANTALLA PRINCIPAL -->
      <template v-if="activePage === 'home'">
        <!-- SERIE PRINCIPAL -->
        <section class="featured">
          <div class="featured-background"></div>

          <div class="featured-content">
            <span class="premiere">✦ ÚLTIMO ESTRENO</span>

            <div class="rating">
              ⭐ 4.9 · 2025 · 16+
            </div>

            <h2>Eclipsia:<br />El Último Hechizo</h2>

            <div class="tags">
              <span>Fantasía</span>
              <span>Romance</span>
              <span>LGBTQ+</span>
            </div>

            <div class="language">
              ◉ ES Latino · EN Original
            </div>

            <p>
              En un reino donde la magia se desvanece, una princesa exiliada
              y una hechicera rebelde deben unir sus destinos.
            </p>

            <div class="featured-actions">
              <button class="watch-button" @click="openSeries">
                ▶ Ver ahora
              </button>

              <button
                class="favorite-button"
                @click="toggleFavorite"
                :class="{ favorite: isFavorite }"
              >
                {{ isFavorite ? '♥' : '♡' }}
              </button>
            </div>
          </div>
        </section>

        <!-- CONTINÚA VIENDO -->
        <section class="section">
          <div class="section-heading">
            <div>
              <h2>◷ Continúa viendo</h2>
              <p>Retoma tus historias favoritas</p>
            </div>

            <button @click="activePage = 'explore'">Ver todos ›</button>
          </div>

          <div class="continue-grid">
            <article class="continue-card">
              <div class="poster poster-one">
                <span class="progress-label">47% · E3</span>
              </div>

              <div class="card-info">
                <h3>Amigos Inesperados</h3>
                <p>T1 · E3 | 21 min</p>

                <div class="progress">
                  <span style="width: 47%"></span>
                </div>
              </div>
            </article>

            <article class="continue-card">
              <div class="poster poster-two">
                <span class="progress-label">22% · E1</span>
              </div>

              <div class="card-info">
                <h3>El Enigma</h3>
                <p>T1 · E1 | 16 min</p>

                <div class="progress">
                  <span style="width: 22%"></span>
                </div>
              </div>
            </article>
          </div>
        </section>

        <!-- BANNER DEL CREADOR -->
        <section class="creator-banner">
          <div class="creator-symbol">✧</div>

          <div class="creator-text">
            <h2>¿Listo para crear tu propia miniserie?</h2>
            <p>Usa la inteligencia artificial y da vida a tus ideas.</p>
          </div>

          <button @click="activePage = 'creator'">
            Crear con IA →
          </button>
        </section>

        <!-- CATEGORÍAS -->
        <section class="section">
          <div class="section-heading">
            <div>
              <h2>Explora categorías</h2>
              <p>Encuentra tu próxima historia</p>
            </div>
          </div>

          <div class="category-grid">
            <button>🎭 Drama</button>
            <button>✨ Fantasía</button>
            <button>❤️ Romance</button>
            <button>🚀 Ciencia ficción</button>
          </div>
        </section>
      </template>

      <!-- EXPLORAR -->
      <section v-else-if="activePage === 'explore'" class="page">
        <h2>Explorar</h2>
        <p class="page-subtitle">
          Descubre nuevas historias en AL MINISERIES.
        </p>

        <div class="empty-panel">
          <span>🔎</span>
          <h3>Catálogo de miniseries</h3>
          <p>
            Aquí aparecerán los estrenos, recomendaciones y categorías
            conectados a nuestra base de datos.
          </p>
        </div>
      </section>

      <!-- MI LISTA -->
      <section v-else-if="activePage === 'list'" class="page">
        <h2>Mi lista</h2>
        <p class="page-subtitle">
          Tus miniseries guardadas.
        </p>

        <div class="empty-panel">
          <span>♡</span>
          <h3>Aún no tienes favoritos</h3>
          <p>
            Guarda tus miniseries favoritas y aparecerán aquí.
          </p>

          <button class="watch-button" @click="activePage = 'explore'">
            Explorar contenido
          </button>
        </div>
      </section>

      <!-- PERFIL -->
      <section v-else-if="activePage === 'profile'" class="page">
        <div class="profile-header">
          <div class="profile-avatar">👤</div>
          <h2>Anthony Lima</h2>
          <p>Mi perfil de AL MINISERIES</p>
        </div>

        <div class="settings-list">
          <button>⚙️ Configuración <span>›</span></button>
          <button>🌐 Idioma <span>›</span></button>
          <button>🎞️ Calidad de reproducción <span>›</span></button>
          <button>🔔 Notificaciones <span>›</span></button>
        </div>
      </section>

      <!-- PANEL DE CREADOR -->
      <section v-else-if="activePage === 'creator'" class="page creator-page">
        <span class="creator-badge">✦ ÁREA DE CREADOR</span>

        <h2>Crea tus propias miniseries con IA</h2>

        <p class="page-subtitle">
          Convierte tus ideas en historias, personajes y escenas.
        </p>

        <div class="creator-tools">
          <button>
            📄
            <strong>Generar guion</strong>
            <small>Crear una historia con IA</small>
          </button>

          <button>
            👤
            <strong>Crear personajes</strong>
            <small>Diseñar protagonistas</small>
          </button>

          <button>
            🖼️
            <strong>Generar escenas</strong>
            <small>Visualizar tus ideas</small>
          </button>

          <button>
            🎬
            <strong>Editar proyecto</strong>
            <small>Administrar tu miniserie</small>
          </button>
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
        <span>▦</span>
        Estrenos
      </button>

      <button
        :class="{ active: activePage === 'list' }"
        @click="activePage = 'list'"
      >
        <span>♡</span>
        Mi lista
      </button>

      <button
        :class="{ active: activePage === 'explore' }"
        @click="activePage = 'explore'"
      >
        <span>⌕</span>
        Buscar
      </button>

      <button
        :class="{ active: activePage === 'creator' }"
        @click="activePage = 'creator'"
      >
        <span>✧</span>
        Creador
      </button>

      <button
        :class="{ active: activePage === 'profile' }"
        @click="activePage = 'profile'"
      >
        <span>♙</span>
        Perfil
      </button>
    </nav>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const activePage = ref('home')
const isFavorite = ref(false)

function toggleFavorite() {
  isFavorite.value = !isFavorite.value
}

function openSeries() {
  alert('La sección de reproducción se conectará próximamente.')
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

:root {
  color-scheme: dark;
}

body {
  background: #050b24;
  color: #ffffff;
  font-family: Arial, Helvetica, sans-serif;
}

button {
  border: 0;
  cursor: pointer;
  color: white;
  font-family: inherit;
}

.app {
  min-height: 100vh;
  padding-bottom: 100px;
  background:
    radial-gradient(circle at 20% 0%, #151b56 0%, transparent 35%),
    linear-gradient(180deg, #07143c 0%, #050b24 55%, #030718 100%);
}

.topbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 18px 20px;
  background: rgba(4, 10, 31, 0.92);
  border-bottom: 1px solid rgba(92, 125, 255, 0.25);
}

.brand {
  display: flex;
  align-items: center;
  gap: 12px;
}

.brand-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 48px;
  height: 48px;
  border: 2px solid #6c4dff;
  border-radius: 15px;
  color: #68eaff;
  font-size: 19px;
  font-weight: 900;
  background: linear-gradient(145deg, #2921a0, #10183f);
  box-shadow: 0 0 18px rgba(76, 81, 255, 0.65);
}

.brand h1 {
  font-size: 18px;
  letter-spacing: 0.5px;
}

.brand p {
  margin-top: 4px;
  color: #9caee8;
  font-size: 11px;
}

.avatar-button {
  width: 43px;
  height: 43px;
  border: 1px solid #5367b9;
  border-radius: 50%;
  background: #101d4b;
  font-size: 21px;
}

.content {
  max-width: 850px;
  margin: 0 auto;
}

.featured {
  position: relative;
  min-height: 550px;
  margin: 16px;
  overflow: hidden;
  border: 1px solid #324cff;
  border-radius: 24px;
  background: #0b1644;
  box-shadow: 0 0 28px rgba(55, 70, 255, 0.25);
}

.featured-background {
  position: absolute;
  inset: 0;
  background:
    linear-gradient(180deg, transparent 10%, rgba(5, 9, 31, 0.3) 35%, #071039 90%),
    radial-gradient(circle at 70% 20%, #e77ac9 0%, #5334b5 17%, transparent 42%),
    linear-gradient(135deg, #111b65, #182b70 45%, #120b3c);
}

.featured-background::after {
  content: '✦';
  position: absolute;
  top: 65px;
  right: 40px;
  color: rgba(154, 210, 255, 0.5);
  font-size: 110px;
}

.featured-content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  min-height: 550px;
  padding: 25px;
}

.premiere,
.creator-badge {
  display: inline-block;
  width: fit-content;
  padding: 8px 12px;
  border-radius: 20px;
  background: linear-gradient(90deg, #a400ff, #315bff);
  font-size: 11px;
  font-weight: bold;
  letter-spacing: 0.4px;
}

.rating {
  margin-top: 14px;
  color: #a9caff;
  font-size: 12px;
}

.featured h2 {
  margin-top: 12px;
  font-size: clamp(32px, 8vw, 54px);
  line-height: 1.02;
  letter-spacing: -1.5px;
}

.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-top: 17px;
}

.tags span,
.language {
  padding: 8px 11px;
  border: 1px solid #354d9c;
  border-radius: 18px;
  background: rgba(13, 25, 73, 0.85);
  color: #c9d8ff;
  font-size: 11px;
}

.language {
  width: fit-content;
  margin-top: 10px;
  color: #7bdfff;
}

.featured p {
  max-width: 510px;
  margin-top: 16px;
  color: #d1d9f6;
  font-size: 14px;
  line-height: 1.55;
}

.featured-actions {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-top: 22px;
}

.watch-button {
  padding: 14px 22px;
  border-radius: 25px;
  background: linear-gradient(90deg, #bd00ff, #1d8dff);
  box-shadow: 0 0 18px rgba(75, 86, 255, 0.45);
  font-weight: bold;
}

.favorite-button {
  width: 48px;
  height: 48px;
  border: 1px solid #6376c8;
  border-radius: 50%;
  background: rgba(8, 19, 61, 0.9);
  font-size: 27px;
}

.favorite-button.favorite {
  color: #ff68df;
}

.section {
  padding: 14px 20px;
}

.section-heading {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  margin-bottom: 15px;
}

.section-heading h2 {
  font-size: 21px;
}

.section-heading p {
  margin-top: 5px;
  color: #8195d0;
  font-size: 12px;
}

.section-heading button {
  background: transparent;
  color: #8ab6ff;
  white-space: nowrap;
}

.continue-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 12px;
}

.continue-card {
  overflow: hidden;
  border: 1px solid #263d91;
  border-radius: 15px;
  background: #0b1745;
}

.poster {
  position: relative;
  height: 135px;
}

.poster-one {
  background:
    radial-gradient(circle at 50% 30%, #d9bd70, transparent 13%),
    linear-gradient(135deg, #477b32, #182e26 55%, #0b1026);
}

.poster-two {
  background:
    linear-gradient(135deg, #56657a, transparent 35%),
    linear-gradient(145deg, #27364a, #080d1e);
}

.progress-label {
  position: absolute;
  top: 10px;
  left: 10px;
  padding: 5px 8px;
  border-radius: 10px;
  background: rgba(2, 8, 27, 0.85);
  font-size: 10px;
}

.card-info {
  padding: 12px;
}

.card-info h3 {
  font-size: 13px;
}

.card-info p {
  margin-top: 6px;
  color: #8ca5df;
  font-size: 11px;
}

.progress {
  height: 4px;
  margin-top: 12px;
  overflow: hidden;
  border-radius: 5px;
  background: #26345f;
}

.progress span {
  display: block;
  height: 100%;
  background: linear-gradient(90deg, #b900ff, #1aaaff);
}

.creator-banner {
  display: flex;
  align-items: center;
  gap: 13px;
  margin: 18px 20px;
  padding: 18px;
  border: 1px solid #714cff;
  border-radius: 17px;
  background: linear-gradient(100deg, #4512a0, #123b9d);
  box-shadow: 0 0 22px rgba(81, 48, 255, 0.25);
}

.creator-symbol {
  color: #b7f3ff;
  font-size: 35px;
}

.creator-text {
  flex: 1;
}

.creator-text h2 {
  font-size: 14px;
}

.creator-text p {
  margin-top: 6px;
  color: #c8d5ff;
  font-size: 11px;
}

.creator-banner button {
  padding: 11px 13px;
  border-radius: 20px;
  background: #237cff;
  font-size: 11px;
  font-weight: bold;
}

.category-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
}

.category-grid button {
  padding: 18px;
  border: 1px solid #263d91;
  border-radius: 13px;
  background: #0b1745;
  text-align: left;
}

.page {
  min-height: 75vh;
  padding: 30px 20px;
}

.page h2 {
  font-size: 32px;
}

.page-subtitle {
  margin-top: 10px;
  color: #91a5df;
  line-height: 1.5;
}

.empty-panel {
  margin-top: 30px;
  padding: 35px 20px;
  border: 1px solid #293f91;
  border-radius: 18px;
  background: #0b1745;
  text-align: center;
}

.empty-panel > span {
  display: block;
  margin-bottom: 15px;
  color: #8d9cff;
  font-size: 42px;
}

.empty-panel h3 {
  font-size: 20px;
}

.empty-panel p {
  margin: 12px 0 25px;
  color: #91a5df;
  line-height: 1.5;
}

.profile-header {
  padding: 20px 0 30px;
  text-align: center;
}

.profile-avatar {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 95px;
  height: 95px;
  margin: 0 auto 18px;
  border: 2px solid #5c70ff;
  border-radius: 50%;
  background: #111e50;
  font-size: 42px;
}

.profile-header h2 {
  font-size: 27px;
}

.profile-header p {
  margin-top: 8px;
  color: #91a5df;
}

.settings-list {
  display: grid;
  gap: 10px;
}

.settings-list button {
  display: flex;
  justify-content: space-between;
  padding: 18px;
  border: 1px solid #263d91;
  border-radius: 12px;
  background: #0b1745;
  text-align: left;
}

.creator-page h2 {
  margin-top: 20px;
  font-size: 30px;
}

.creator-tools {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 12px;
  margin-top: 25px;
}

.creator-tools button {
  display: flex;
  flex-direction: column;
  gap: 9px;
  padding: 20px 15px;
  border: 1px solid #394cc0;
  border-radius: 15px;
  background: linear-gradient(145deg, #14265d, #0b1740);
  text-align: left;
}

.creator-tools button:first-child,
.creator-tools button:nth-child(4) {
  border-color: #873cff;
}

.creator-tools strong {
  font-size: 14px;
}

.creator-tools small {
  color: #91a5df;
  font-size: 11px;
}

.bottom-nav {
  position: fixed;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 20;
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  height: 82px;
  border-top: 1px solid #263b86;
  background: rgba(4, 10, 34, 0.97);
  backdrop-filter: blur(15px);
}

.bottom-nav button {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 7px;
  background: transparent;
  color: #7184b8;
  font-size: 10px;
}

.bottom-nav button span {
  font-size: 23px;
}

.bottom-nav button.active {
  color: #6de7ff;
  text-shadow: 0 0 12px rgba(72, 220, 255, 0.7);
}

@media (max-width: 600px) {
  .featured {
    margin: 10px;
  }

  .featured-content {
    padding: 20px;
  }

  .creator-banner {
    align-items: flex-start;
  }

  .creator-banner button {
    display: none;
  }

  .bottom-nav {
    height: 76px;
  }

  .bottom-nav button {
    font-size: 9px;
  }

  .bottom-nav button span {
    font-size: 20px;
  }
}
</style>