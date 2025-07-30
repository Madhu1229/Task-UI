<template>
  <div class="w-full p-1">
    <div class="overflow-x-auto">
      <!-- Time Display -->
      <h2 class="text-center">{{ currentTime }}</h2>

      <!-- Card Grid -->
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
        <div
          v-for="row in rows"
          :key="row.deviceKey"
          class="flex justify-center"
        >
          <div
            class="bg-white shadow rounded-lg mb-2 text-xl text-dark font-bold border-3 device-card p-1 cursor-pointer"
            @click="openChart(row.deviceKey)"
          >
            <!-- Location Title -->
            <h3 class="text-center mb-0">{{ row.location }}</h3>

            <!-- Pond SVG + Fill Percentage -->
            <div class="flex flex-row justify-between items-center mt-1">
              <img
                :id="`pond-${row.deviceKey}`"
                class="pond-icon"
                :src="require('@/assets/images/diagrams/pond.png')"
                alt="pond"
              />
              <div class="flex flex-col justify-center items-center">
                <div>
                  Fill Percentage<br />{{ row.fillPercentage }}
                </div>
              </div>
            </div>

            <!-- Battery & Wi-Fi + Timestamp -->
            <div class="flex flex-row justify-between items-center mt-1">
              <div class="flex flex-col justify-center items-center">
                <img
                  class="w-8"
                  :src="appIcons['battery-' + row.batteryStatus]"
                  alt="battery status"
                />
                <p class="mb-0">{{ row.battery }}</p>
              </div>
              <div class="flex flex-col justify-center items-center">
                <img
                  class="w-8"
                  :src="appIcons['wifi-' + row.communicationStatus]"
                  alt="wifi status"
                />
                <p class="mb-0">{{ row.time }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import appIcons from "@/@leecom/app-icons";
import InlineSvg from "vue-inline-svg";
import pondData from "@/assets/data/pond-status.json";

export default {
  components: { InlineSvg },
  data() {
    return {
      rows: pondData.results,
      appIcons,
      currentTime: "loading…"
    };
  },
  methods: {
    updateTime() {
      this.currentTime = this.$moment().format("HH:mm:ss  DD MMMM YYYY");
    },
    openChart(deviceKey) {
      this.$router.push({ name: "tanks", query: { deviceKey } });
    }
  },
  created() {
    this.updateTime();
    setInterval(this.updateTime, 1000);
  }
};
</script>

<style scoped>
.device-card {
  border-color: #000000;
  max-width: 300px;
  width: 100%;
  height: 260px; 
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.pond-icon {
  max-height: 78px;
}
</style>
