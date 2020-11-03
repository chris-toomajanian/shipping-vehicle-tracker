<template>
  <span>
    <v-icon :color="batteryChargeColor">
      {{ `mdi-battery-${batteryChargeIcon}` }}
    </v-icon>
    <br />
    <span>{{ chargeLevel | percent }}</span>
  </span>
</template>

<script>
export default {
  name: "BatteryCharge",
  props: {
    chargeLevel: {
      type: Number,
      required: true,
    },
  },
  computed: {
    batteryChargeColor() {
      return this.$props.chargeLevel > 0.35 ? "green" : "red";
    },
    batteryChargeIcon() {
      return this.$props.chargeLevel > 0.5 ? "70" : "30";
    },
  },
  filters: {
    percent(val) {
      return val * 100 + "%";
    },
  },
};
</script>
