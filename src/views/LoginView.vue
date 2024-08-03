<script setup>
import { onMounted, ref } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
import { isTokenValid } from "../auth";

const router = useRouter();

const username = ref("");
const password = ref("");
const errorMessage = ref("");
const usernameError = ref("");
const passwordError = ref("");
const isLoading = ref(false);

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

const login = async () => {
  isLoading.value = true;
  validateUsername();
  validatePassword();
  if (usernameError.value || passwordError.value) {
    isLoading.value = false;
    return;
  }

  try {
    const response = await axios.post(`${API_URL}/api/v1/login`, {
      username: username.value,
      password: password.value,
    });

    const token = response.data.access_token;
    localStorage.setItem("accessToken", token);

    if (isTokenValid(token)) {
      router.push("/conta");
    } else {
      errorMessage.value = "Token inválido.";
    }
  } catch (error) {
    if (error.response && error.response.data) {
      errorMessage.value = error.response.data.message;
    } else {
      errorMessage.value = "Erro ao fazer login. Tente novamente mais tarde.";
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
    <h1 class="title">Login</h1>
    <form @submit.prevent="login">
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
          {{ isLoading ? "Carregando..." : "Entrar" }}
        </button>
      </div>
    </form>
    <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
    <div class="register">
      <h3>Cadastre-se</h3>
      <p>Ainda não possui conta? Cadastre-se no site.</p>
      <RouterLink to="/login/criar">Cadastro</RouterLink>
    </div>
  </div>
</template>

<style scoped>
.register {
  margin-top: 2rem;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  gap: 1rem;
  flex-wrap: wrap;
  align-items: flex-start;
}

.register h3 {
  font-family: "Spectral", Georgia;
  font-size: 1.75rem;
  position: relative;
}

.register h3::after {
  content: "";
  display: block;
  width: 3.75rem;
  height: 0.5rem;
  background: #e2e2e2;
  position: absolute;
  bottom: 1px;
  border-radius: 0.2rem;
  z-index: -1;
}

.register p {
  font-size: 0.9rem;
}

.register a {
  font-size: 0.9rem;
  border-radius: 0.4rem;
  background: #42b883;
  color: #000000;
  padding: 0.75rem 0.9rem;
  transition: 0.1s;
}

.register a:hover {
  box-shadow: 0 0 0 3px #a0f8d0, 0 0 0 4px #42b883;
}
</style>
