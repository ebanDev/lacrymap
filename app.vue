<template>
  <h1>
    <img src="/android-chrome-192x192.png"  alt="💣"/> Lacrymap
  </h1>
  <h2>
    Carte interactive des morts causées par l'utilisation de gaz lacrymogènes
  </h2>
  <p>
    Les gaz lacrymogènes sont des armes chimiques utilisées par les forces de l'ordre pour disperser des foules. Ils
    sont censés être non-létaux, mais ils ont causé la mort de nombreuses personnes. Cette carte interactive recense les
    morts causées par l'utilisation de gaz lacrymogènes à travers le monde.
    <br>
    <br>
    Pour plus d'informations : <a href="https://mnl-syndicat.fr/article/lacrymo">Article - Sous les pavés, le gaz :
    enquête sur un scandale sanitaire d’État</a>, données disponibles sur <a href="https://github.com/ebanDev/lacrymap">GitHub</a>.
  </p>
  <span>
    <b>Morts prouvés recensées : {{ casesData.filter((caseData) => caseData.proven).reduce((acc, caseData) => acc + caseData.deathCount, 0) }}</b> —
    Évènements recensés : {{ casesData.length }}
  </span>
  <Map />
  <CasesList />
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700;800&display=swap');

* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
  font-family: 'Poppins', "Apple Color Emoji", sans-serif;
}

body {
  height: 100vh;
  padding: 1rem;
  max-width: 800px;
  margin: 1rem auto;
}

.leaflet-container {
  height: 500px !important;
  width: 100% !important;
}

h1 {
  font-size: 2.5rem;
  font-weight: 800;
  margin-top: 1rem;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;

  img {
    height: 1.75em;
    margin-right: 0.5rem;
  }
}

h2 {
  font-size: 1.5rem;
  font-weight: 600;
  margin: 1rem 0;
}

p {
  font-size: 1rem;
  margin-bottom: 1rem;
}

a {
  color: #5a3c5a;
  text-decoration: none;
  font-weight: 600;
  transition: 0.2s;

  &:hover {
    color: #5a3c5a;
    font-weight: 700;
    text-decoration: underline;
  }
}

li {
  margin: 0.5rem 0;
  margin-left: 1rem;
}

b {
  font-weight: 700;
  color: #5a3c5a;
}

span:not(.leaflet-container span) {
  font-family: 'Poppins', sans-serif;
  display: block;
  font-weight: 500;
  margin: 1rem 0;
  text-align: center;
}
</style>

<script setup>
const casesData = ref([])

casesData.value = (await queryContent('cases').findOne()).cases;

useHead({
  title: 'Lacrymap - Carte interactive des morts causées par l\'utilisation de gaz lacrymogènes',
  meta: [
    {
      name: 'description',
      content: 'Carte interactive des morts causées par l\'utilisation de gaz lacrymogènes à travers le monde.'
    },
    {
      name: 'keywords',
      content: 'lacrymap, gaz lacrymogènes, morts, carte interactive'
    }
  ],
  link: [
    {
      rel: 'icon',
      href: '/favicon.ico'
    }
  ]
})
</script>