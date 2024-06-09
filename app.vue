<template>
  <h1>
    💣 Lacrymap
  </h1>
  <h2>
    Carte interactive des morts causées par l'utilisation de lacrymogènes
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
  <LMap
      style="height: 350px"
      :zoom="1.5"
      :center="[47.21322, -1.559482]"
      :use-global-leaflet="true"
      :max-zoom="16"
      :min-zoom="1"
  >
    <LTileLayer
        url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
        attribution="&amp;copy; <a href=&quot;https://www.openstreetmap.org/&quot;>OpenStreetMap</a> contributors"
        layer-type="base"
        name="OpenStreetMap"
        :no-wrap="true"
    />
    <LMarkerClusterGroup
      :max-cluster-radius="25"
    >
      <LMarker
          v-for="caseData in casesData"
          :key="caseData.id"
          :lat-lng="caseData.location"
          :icon="generateIcon(caseData.deathCount, caseData.proven)"
          name="Caca"
      >
        <LTooltip>
          {{ caseData.deathCount }} mort{{ caseData.deathCount > 1 ? 's' : '' }}
        </LTooltip>
        <LPopup>
          <h2>{{ caseData.name }}</h2>
          <p> {{ caseData.date }} </p>
          <p>{{ caseData.description }}</p>
          <p>
            <a :href="caseData.source" target="_blank">Source</a>
          </p>
        </LPopup>
      </LMarker>
    </LMarkerClusterGroup>
  </LMap>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700;800&display=swap');

* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

body {
  height: 100vh;
  padding: 1rem;
  max-width: 800px;
  margin: 0 auto;
}

.leaflet-container {
  height: 500px !important;
  width: 100% !important;
}

h1 {
  font-family: 'Poppins', "Apple Color Emoji", sans-serif;
  font-size: 2.5rem;
  font-weight: 800;
  margin-bottom: 1rem;
  margin-top: 1rem;
  text-align: center;
}

h2 {
  font-family: 'Poppins', sans-serif;
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 1rem;
}

p {
  font-family: 'Poppins', sans-serif;
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

.marker-cluster {
  background-color: #5a3c5a99;
}

.marker-cluster div {
  background-color: #5a3c5a;
  color: white;
}

.leaflet-container {
  border-radius: 5px;
  box-shadow: 0 0 30px rgba(0, 0, 0, 0.05);

  .leaflet-popup-content-wrapper {
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);

    h2 {
      margin-bottom: 0.5rem;
      font-weight: 700;
      font-size: 1.3rem;
    }

    p {
      margin: 0.5rem 0;
    }

    a {
      color: #5a3c5a;
    }
  }
}
</style>

<script setup>
const casesData = ref([])
import L from 'leaflet'
import { LMarkerClusterGroup } from 'vue-leaflet-markercluster'
import 'vue-leaflet-markercluster/dist/style.css'


casesData.value = (await queryContent('cases').findOne()).cases;

const generateIcon = (weight, lighter) => {
  weight = 1 + weight / 30;
  weight = Math.min(weight, 2);
  return L.icon({
    iconUrl: lighter ? '/pin-black.png' : '/pin-grey.png',
    iconSize: [10 * weight, 12.5 * weight],
    iconAnchor: [10 * weight, 5 * weight],
    popupAnchor: [1, -34],
  });
}
</script>