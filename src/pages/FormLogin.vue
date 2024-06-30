<template>
  <q-page padding>
    <q-card class="login-card">
      <q-card-section>
        <div class="text-h6">Login</div>
      </q-card-section>

      <q-card-section class="q-pt-none q-pb-md">
        <q-form @submit="login">
          <q-input
            v-model="username"
            label="Username"
            outlined
            dense
            class="q-mb-sm"
          />
          <q-input
            v-model="password"
            label="Password"
            type="password"
            outlined
            dense
            class="q-mb-sm"
          />
          <q-btn
            type="submit"
            label="Login"
            color="primary"
            class="q-mt-md"
            :loading="loading"
          />
        </q-form>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import { api } from "boot/axios.js";
import { Notify } from "quasar";

export default {
  name: "FormLogin",
  data() {
    return {
      username: "",
      password: "",
      loading: false,
    };
  },
  methods: {
    async login() {
      try {
        this.loading = true;
        const response = await api.post("/login", {
          username: this.username,
          password: this.password,
        });
        // Handle successful login, e.g., store token in localStorage
        localStorage.setItem("token", response.data.token);
        alert("Login successful");
        // Redirect to dashboard or other route on successful login
        this.$router.push("/home");
      } catch (error) {
        alert(error.response.data.message || "Failed to login");
        Notify.create({
          type: "negative",
          message: error.response.data.message || "Failed to login",
        });
      } finally {
        this.loading = false;
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

.login-card {
  max-width: 400px;
  margin: auto;
}

.q-pt-none {
  padding-top: 0;
}

.q-pb-md {
  padding-bottom: 16px;
}
</style>
