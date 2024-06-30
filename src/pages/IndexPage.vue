<template>
  <q-page padding>
    <q-card>
      <q-card-section>
        <div class="text-h6">Welcome to IoT Dashboard</div>
      </q-card-section>

      <q-card-section>
        <q-btn label="Go to Dashboard" color="primary" @click="goToDashboard" />
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
export default {
  name: "IndexPage",
  methods: {
    goToDashboard() {
      this.$router.push("/dashboard");
    },
  },
};
</script>

<style scoped>
.text-h6 {
  font-size: 1.25rem;
  text-align: center;
}
</style>
