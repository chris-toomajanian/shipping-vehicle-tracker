<template>
  <GmapMap
    v-if="mapCenter"
    :center="mapCenter"
    :zoom="10"
    map-type-id="roadmap"
    style="width: 100%; height: 100%"
    ref="gmap"
    :options="{
      zoomControl: true,
      mapTypeControl: false,
      scaleControl: false,
      streetViewControl: false,
      rotateControl: false,
      fullscreenControl: false,
      disableDefaultUi: false,
    }"
  >
    <GmapMarker
      :key="index"
      v-for="(m, index) in markers"
      :position="m.location"
      :clickable="true"
      :draggable="false"
      :icon="truckIcon"
      :label="m.label"
    ></GmapMarker>
  </GmapMap>
</template>

<script>
import { gmapApi } from "vue2-google-maps";

export default {
  name: "Map",
  props: {
    markers: {
      type: Array,
      required: true,
    },
    selectedItem: {
      type: Object,
    },
    mapCenter: {
      type: Object,
      required: true,
      default: () => ({
        lat: 33.735092,
        lng: -84.409627,
      }),
    },
  },
  mounted() {
    this.setBounds();
  },
  methods: {
    setBounds() {
      this.$refs.gmap.$mapPromise.then((map) => {
        var bounds = new this.google.maps.LatLngBounds();
        this.markers.forEach((marker) => {
          bounds.extend(
            new this.google.maps.LatLng(
              marker.location.lat,
              marker.location.lng
            )
          );
        });
        map.fitBounds(bounds, { right: 50, left: 50 });
      });
    },
  },
  computed: {
    google: gmapApi,
    truckIcon() {
      return require("@/assets/truckicon.png");
    },
  },
  watch: {
    selectedItem() {
      let selectedMarker = this.markers.find((m) => {
        return m.id === this.$props.selectedItem.id;
      });
      this.$refs.gmap.$mapPromise.then((map) => {
        map.panTo(selectedMarker.location);
        map.setZoom(14);
      });
    },
    mapCenter() {
      this.setBounds();
    },
  },
};
</script>

<style lang="scss" scoped>
.vue-map-container,
.vue-map-container .vue-map {
  width: 100%;
  height: 100%;
}
</style>
