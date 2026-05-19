<script setup>
import { computed } from "vue";
import { useRouter } from "vue-router";
import api from "../services/api";

const router = useRouter();
const userName = computed(
  () => localStorage.getItem("auth_user_name") || "User",
);

const logout = async () => {
  try {
    await api.post("/api/logout");
  } catch (error) {
    // Ignore API failures to allow local logout.
  }

  localStorage.removeItem("auth_token");
  localStorage.removeItem("auth_user_name");
  router.push("/login");
};
</script>

<template>
  <div class="auth-page">
    <div class="dashboard-container">
      <div class="dashboard-card">
        <div class="dashboard-header">
          <div>
            <p class="dashboard-label">Welcome back</p>
            <h1 class="dashboard-title">{{ userName }}</h1>
          </div>
          <button class="dashboard-logout" type="button" @click="logout">
            Logout
          </button>
        </div>

        <div class="dashboard-body">
          <p class="dashboard-text">
            You are successfully logged in. This is your dashboard.
          </p>
          <div class="dashboard-stats">
            <div class="dashboard-stat">
              <span class="dashboard-stat-label">Tasks</span>
              <span class="dashboard-stat-value">0</span>
            </div>
            <div class="dashboard-stat">
              <span class="dashboard-stat-label">Completed</span>
              <span class="dashboard-stat-value">0</span>
            </div>
            <div class="dashboard-stat">
              <span class="dashboard-stat-label">Messages</span>
              <span class="dashboard-stat-value">0</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
