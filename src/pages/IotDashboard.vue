<template>
  <q-page padding>
    <q-card>
      <q-card-section>
        <div class="text-h6">Sensor Gas</div>
      </q-card-section>

      <q-card-section
        class="q-pt-none q-pb-md q-flex q-flex-center q-items-center"
      >
        <div v-if="data" class="q-pa-md flex flex-center">
          <q-knob
            v-model="data.data_sensor"
            :min="0"
            :max="800"
            size="150px"
            color="primary"
            track-color="grey-3"
            value-color="primary"
            center-color="white"
            center-color-filled="true"
            show-value
            readonly
            class="text-primary q-ma-md"
          />
        </div>
        <div v-if="data" class="sensor-details q-ma-md">
          <p><strong>Serial Number:</strong> {{ data.serial_number }}</p>
          <p><strong>Timestamp:</strong> {{ data.time_stamp }}</p>
        </div>
        <div v-else class="q-ma-sm">
          <q-spinner color="primary" />
        </div>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import { api } from "boot/axios.js";

export default {
  name: "IotDashboard",
  data() {
    return {
      data: null,
      intervalId: null,
    };
  },
  created() {
    this.fetchData();
    this.intervalId = setInterval(this.fetchData, 2000);
  },
  beforeUnmount() {
    clearInterval(this.intervalId);
  },
  methods: {
    async fetchData() {
      try {
        const response = await api.get("http://localhost:3000/sendData");
        this.data = response.data.data;
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    },
  },
};
</script>

<style scoped>
.text-h6 {
  font-size: 1.5rem;
  text-align: center;
}

.sensor-details {
  margin-top: 20px;
  text-align: center;
}

.q-ma-md {
  margin: auto; /* Memastikan elemen tengah secara horizontal */
  padding: 16px; /* Memberikan ruang di sekitar elemen */
}

.q-ma-sm {
  margin: 8px; /* Margin lebih kecil untuk elemen-elemen kecil */
}

.q-pt-none {
  padding-top: 0; /* Menghilangkan padding atas untuk tata letak yang lebih rapat */
}

.q-pb-md {
  padding-bottom: 16px; /* Padding bawah untuk memberikan ruang ekstra */
}
</style>
