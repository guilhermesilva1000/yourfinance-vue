<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";
import { isTokenValid } from "../auth";

const username = ref("");
const password = ref("");
const errorMessage = ref("");
const usernameError = ref("");
const passwordError = ref("");
const isLoading = ref(false);
const router = useRouter();

const API_URL = "https://yourfinance.koyeb.app";

const checkAuth = () => {
  const token = localStorage.getItem("accessToken");
  if (isTokenValid(token)) {
    router.push("/conta");
  }
};

const validateUsername = () => {
  if (!username.value) {
    usernameError.value = "Preencha um valor.";
  } else {
    usernameError.value = "";
  }
};

const validatePassword = () => {
  if (!password.value) {
    passwordError.value = "Preencha um valor.";
  } else {
    passwordError.value = "";
  }
};

const register = async () => {
  isLoading.value = true;
  validateUsername();
  validatePassword();
  if (usernameError.value || passwordError.value) {
    isLoading.value = false;
    return;
  }
  try {
    await axios.post(`${API_URL}/api/v1/login/criar`, {
      username: username.value,
      password: password.value,
    });

    router.push("/login");
  } catch (error) {
    if (error.response && error.response.data) {
      errorMessage.value = error.response.data.message;
    } else {
      errorMessage.value = "Erro ao registrar. Tente novamente mais tarde.";
    }
  } finally {
    isLoading.value = false;
  }
};

onMounted(() => {
  checkAuth();
});
</script>

<template>
  <div class="content">
    <h1 class="title">Cadastre-se</h1>
    <form @submit.prevent="register">
      <div class="form-group">
        <label for="username">Usuário</label>
        <input type="text" id="username" v-model="username" name="username" />
        <p v-if="usernameError" class="error-input">{{ usernameError }}</p>
      </div>
      <div class="form-group">
        <label for="password">Senha</label>
        <input
          type="password"
          id="password"
          v-model="password"
          name="password"
        />
        <p v-if="passwordError" class="error-input">{{ passwordError }}</p>
      </div>
      <div class="form-group">
        <button
          type="submit"
          :disabled="isLoading"
          :class="{ loading: isLoading }"
        >
          {{ isLoading ? "Carregando..." : "Cadastrar" }}
        </button>
      </div>
    </form>
    <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
  </div>
</template>

<style scoped></style>
