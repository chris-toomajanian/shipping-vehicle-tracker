<template>
  <v-app>
    <v-main>
      <div class="flex-container">
        <div class="flex-item">
          <v-navigation-drawer
            v-model="drawer"
            width="380px"
            class="darken-3"
            elevation="4"
            dense
            permanent
          >
            <v-toolbar
              class="darken-3"
              flat
              style="position: sticky; top: 0; z-index: 1; margin-right: 1px;"
            >
              <v-toolbar-title>
                {{ appBarTitle }}
              </v-toolbar-title>
            </v-toolbar>
            <v-btn class="ml-3" small color="primary" @click="recenter"
              >Recenter Map <v-icon right>mdi-crosshairs-gps</v-icon></v-btn
            >
            <v-list two-line>
              <v-list-item-group active-class="primary--text">
                <v-subheader>Active Fleet</v-subheader>
                <template v-for="(truck, index) in trucks">
                  <v-list-item :key="index" @click="detectClick($event, truck)">
                    <template>
                      <v-list-item-icon>
                        <BatteryCharge :chargeLevel="truck.chargeLevel" />
                      </v-list-item-icon>
                      <v-list-item-content>
                        <v-list-item-title>{{
                          `Truck #${truck.id}`
                        }}</v-list-item-title>
                        <v-list-item-subtitle>
                          {{ convertedAddresses[index] }}
                        </v-list-item-subtitle>
                        <v-list-item-subtitle v-if="truck.chargeLevel < 0.25">
                          <v-chip
                            color="red"
                            filter
                            pill
                            x-small
                            class="white--text"
                            >Charge Level is Low!</v-chip
                          >
                        </v-list-item-subtitle>
                      </v-list-item-content>
                    </template>
                  </v-list-item>
                </template>
              </v-list-item-group>
            </v-list>
          </v-navigation-drawer>
        </div>
        <div class="flex-item">
          <Map
            :markers="trucks"
            :selected-item="selectedItem"
            :map-center="centerMap"
          />
        </div>
      </div>
    </v-main>
  </v-app>
</template>

<script>
import Map from "@/views/Map";
import BatteryCharge from "./components/BatteryCharge";

export default {
  name: "App",
  components: {
    BatteryCharge,
    Map,
  },
  data: () => ({
    appBarTitle: "Shipping Fleet Tracker",
    drawer: true,
    centerMap: { lat: 33.735092, lng: -84.409627 },
    links: [
      {
        icon: "mdi-menu",
      },
    ],
    active: null,
    selectedItem: {},
    convertedAddresses: [],
    trucks: [
      {
        id: 1,
        location: { lat: 33.705808, lng: -84.497484 },
        chargeLevel: 0.9,
        icon: {
          scaledSize: { width: 48, height: 48 },
          labelOrigin: { x: 16, y: -10 },
        },
      },
      {
        id: 2,
        location: { lat: 33.839359, lng: -84.319807 },
        chargeLevel: 0.4,
        icon: {
          scaledSize: { width: 48, height: 48 },
          labelOrigin: { x: 16, y: -10 },
        },
      },
      {
        id: 3,
        location: { lat: 33.819924, lng: -84.483244 },
        chargeLevel: 0.6,
        icon: {
          scaledSize: { width: 48, height: 48 },
          labelOrigin: { x: 16, y: -10 },
        },
      },
      {
        id: 4,
        location: { lat: 33.715919, lng: -84.250181 },
        chargeLevel: 0.2,
        icon: {
          scaledSize: { width: 48, height: 48 },
          labelOrigin: { x: 16, y: -10 },
        },
      },
    ],
  }),
  mounted() {
    this.trucks.forEach((truck) => {
      this.geoCode(truck.location);
    });
  },
  methods: {
    detectClick(e, truck) {
      this.selectedItem = truck;
      window.scroll({
        top: 0,
        left: 0,
        behavior: "smooth",
      });
    },
    recenter() {
      this.centerMap = { lat: 33.735092, lng: -84.409627 };
    },
    async geoCode(location) {
      await fetch(`https://maps.googleapis.com/maps/api/geocode/json?latlng=${location.lat},${location.lng}&key=${process.env.VUE_APP_API_KEY}
`)
        .then((response) => response.json())
        .then((data) => {
          const address = data.results[4].formatted_address;
          this.convertedAddresses.push(address);
        })
        .catch((err) => {
          console.log(err);
          const address = "Location unknown";
          this.convertedAddresses.push(address);
        });
    },
  },
};
</script>

<style lang="scss">
.flex-container {
  flex-direction: column;
  display: flex;
  flex-wrap: nowrap;

  @media (min-width: 768px) {
    flex-direction: row;
    height: 100vh;
  }

  .flex-item {
    &:first-child {
      order: 2;

      @media (min-width: 768px) {
        order: 1;
        max-width: 300px;
      }
    }

    &:last-child {
      height: 300px;

      @media (min-width: 768px) {
        height: 100%;
        width: 100%;
        order: 2;
      }
    }
  }
}
</style>
