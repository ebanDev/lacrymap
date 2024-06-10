<template>
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
    iconUrl: lighter ? '/pin-black.png' : '/pin-gray.png',
    iconSize: [10 * weight, 12.5 * weight],
    iconAnchor: [10 * weight, 5 * weight],
    popupAnchor: [1, -34],
  });
}
</script>