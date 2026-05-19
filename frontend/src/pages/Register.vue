<script setup>
import { ref } from "vue";
import { RouterLink, useRouter } from "vue-router";
import api from "../services/api";
import AuthCard from "../components/AuthCard.vue";

const router = useRouter();
const form = ref({
  name: "",
  email: "",
  password: "",
});
const errorMessage = ref("");
const isSubmitting = ref(false);

const submit = async () => {
  errorMessage.value = "";
  isSubmitting.value = true;

  try {
    await api.post("/api/register", {
      name: form.value.name,
      email: form.value.email,
      password: form.value.password,
    });
    router.push("/login");
  } catch (error) {
    errorMessage.value =
      error.response?.data?.message || "Registration failed. Try again.";
  } finally {
    isSubmitting.value = false;
  }
};
</script>

<template>
  <AuthCard>
    <div class="auth-header">
      <h1 class="auth-title">Create an account</h1>
      <p class="auth-subtitle">
        Or <RouterLink to="/login">sign in</RouterLink> to your existing account
      </p>
    </div>

    <div class="auth-card">
      <form class="auth-form" @submit.prevent="submit">
        <div>
          <label class="auth-label" for="name">Full Name</label>
          <input
            id="name"
            v-model="form.name"
            type="text"
            class="auth-input"
            placeholder="Full name"
            required
          />
        </div>

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

        <button class="auth-button" type="submit" :disabled="isSubmitting">
          {{ isSubmitting ? "Signing up..." : "Sign up" }}
        </button>
      </form>
    </div>
  </AuthCard>
</template>
