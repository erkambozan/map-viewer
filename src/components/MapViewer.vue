<template>
  <div>
    <GoogleMap
        api-key="AIzaSyBgpomx-zwaOjTQbsGn48BLpxVV-_-ivfc"
        :center="center"
        :zoom="zoom"
        @click="handleMapClick"
    >
      <Marker
          v-for="location in locations"
          :key="location.id"
          :position="location.position"
          :icon="locationIcon"
      />
    </GoogleMap>
  </div>
</template>

<script>
import axios from 'axios';
import {GoogleMap, Marker} from "vue3-google-map";

export default {
  name: 'MapViewer',
  components: {GoogleMap, Marker},
  data() {
    return {
      center: {lat: 0, lng: 0},
      zoom: 10,
      locations: [],
      locationIcon: '',
    };
  },
  methods: {
    handleMapClick(event) {
      const latitude = event.latLng.lat();
      const longitude = event.latLng.lng();
      this.fetchLocations(latitude, longitude);
    },
    async fetchLocations(latitude, longitude) {
      try {
        const response = await axios.post('/location/create',  {'latitude': latitude, 'longitude': longitude}, {baseURL: 'http://localhost:8000'});
        this.locations = response.data;
      } catch (error) {
        console.error('Error fetching locations:', error);
      }
    },
  },
};
</script>
