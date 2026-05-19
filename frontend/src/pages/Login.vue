<script setup>
import { ref } from "vue";
import { RouterLink, useRouter } from "vue-router";
import api from "../services/api";
import AuthCard from "../components/AuthCard.vue";

const form = ref({
  email: "",
  password: "",
});
const errorMessage = ref("");
const successMessage = ref("");
const isSubmitting = ref(false);
const router = useRouter();

const submit = async () => {
  errorMessage.value = "";
  successMessage.value = "";
  isSubmitting.value = true;

  try {
    const response = await api.post("/api/login", {
      email: form.value.email,
      password: form.value.password,
    });
    localStorage.setItem("auth_token", response.data.token);
    if (response.data.user?.name) {
      localStorage.setItem("auth_user_name", response.data.user.name);
    }
    successMessage.value = "Login successful.";
    router.push("/dashboard");
  } catch (error) {
    errorMessage.value =
      error.response?.data?.message || "Login failed. Try again.";
  } finally {
    isSubmitting.value = false;
  }
};
</script>

<template>
  <AuthCard>
    <div class="auth-header">
      <h1 class="auth-title">Success Visa LogIn</h1>
      <p class="auth-subtitle">
        Sign in to manage your tasks or
        <RouterLink to="/register">create a new account</RouterLink>
      </p>
    </div>

    <div class="auth-card">
      <form class="auth-form" @submit.prevent="submit">
        <div>
          <label class="auth-label" for="email">Email address</label>
          <input
            id="email"
            v-model="form.email"
            type="email"
            class="auth-input"
            placeholder="name@example.com"
            required
          />
        </div>

        <div>
          <label class="auth-label" for="password">Password</label>
          <input
            id="password"
            v-model="form.password"
            type="password"
            class="auth-input"
            placeholder="••••••••"
            required
          />
        </div>

        <p v-if="errorMessage" class="auth-error">
          {{ errorMessage }}
        </p>
        <p v-if="successMessage" class="auth-success">
          {{ successMessage }}
        </p>

        <button class="auth-button" type="submit" :disabled="isSubmitting">
          {{ isSubmitting ? "Signing in..." : "Sign in" }}
        </button>
      </form>
    </div>
  </AuthCard>
</template>
