<template>
  <div class="explore-page">
    <header class="header">
      <div class="logo">
        <strong>AL</strong>
        <span>MINISERIES</span>
      </div>

      <NuxtLink to="/" class="back-button">←</NuxtLink>
    </header>

    <main class="content">
      <section class="intro">
        <p class="eyebrow">AL MINISERIES</p>
        <h1>Explora nuevas historias</h1>
        <p>Encuentra miniseries de todos tus géneros favoritos.</p>
      </section>

      <div class="search-box">
        <span>🔎</span>
        <input
          v-model="search"
          type="search"
          placeholder="Buscar miniseries..."
        />
      </div>

      <section class="categories">
        <button
          v-for="category in categories"
          :key="category"
          :class="{ selected: selectedCategory === category }"
          @click="selectedCategory = category"
        >
          {{ category }}
        </button>
      </section>

      <section class="section">
        <h2>🎬 Miniseries disponibles</h2>

        <div v-if="filteredSeries.length" class="series-grid">
          <article
            v-for="series in filteredSeries"
            :key="series.title"
            class="series-card"
          >
            <div class="poster" :class="series.color">
              <span>{{ series.emoji }}</span>
            </div>

            <div class="series-info">
              <h3>{{ series.title }}</h3>
              <p>{{ series.category }}</p>

              <button @click="showDetails(series)">
                Ver detalles
              </button>
            </div>
          </article>
        </div>

        <p v-else class="empty">
          No encontramos resultados.
        </p>
      </section>

      <section v-if="selectedSeries" class="details">
        <button class="close" @click="selectedSeries = null">✕</button>

        <h2>{{ selectedSeries.title }}</h2>
        <p>{{ selectedSeries.description }}</p>

        <button class="watch-button" @click="goHome">
          Volver al inicio
        </button>
      </section>
    </main>

    <nav class="bottom-nav">
      <NuxtLink to="/">
        <span>⌂</span>
        Inicio
      </NuxtLink>

      <NuxtLink to="/explorar" class="active">
        <span>🔎</span>
        Explorar
      </NuxtLink>

      <NuxtLink to="/mi-lista">
        <span>＋</span>
        Mi lista
      </NuxtLink>

      <NuxtLink to="/perfil">
        <span>👤</span>
        Perfil
      </NuxtLink>
    </nav>
  </div>
</template>

<script setup