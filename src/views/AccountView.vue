<script setup>
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
import { isTokenValid } from "@/auth";
// import { jwtDecode } from "jwt-decode";

const router = useRouter();
const isLoading = ref(false);
const protectedMessage = ref("");

const API_URL = "https://yourfinance.koyeb.app";

const checkAuth = () => {
  const token = localStorage.getItem("accessToken");
  if (isTokenValid(token)) {
    // const decoded = jwtDecode(token);
  } else {
    localStorage.removeItem("accessToken");
    router.push("/login");
  }
};

const logout = () => {
  localStorage.removeItem("accessToken");
  router.push("/login");
};

const accessProtectedRoute = async () => {
  isLoading.value = true;
  const token = localStorage.getItem("accessToken");
  if (!isTokenValid(token)) {
    protectedMessage.value = "Você deve fazer login primeiro!";
    return;
  }

  try {
    const response = await axios.get(`${API_URL}/api/v1/username`, {
      headers: {
        Authorization: `Bearer ${token}`,
      },
    });
    protectedMessage.value = response.data.message;
  } catch (error) {
    if (error.response && error.response.data) {
      protectedMessage.value =
        "Erro ao acessar a rota protegida: " + error.response.data.message;
    } else {
      protectedMessage.value =
        "Erro ao acessar a rota protegida: " + error.message;
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
  <div class="container">
    <div class="wrapper">
      <h1 class="title">Minha Conta</h1>
      <nav>
        <button @click="accessProtectedRoute" :class="{ loading: isLoading }">
          <svg
            width="28"
            height="28"
            viewBox="0 0 28 28"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fill-rule="evenodd"
              clip-rule="evenodd"
              d="M14 5a1 1 0 011 1v7h7a1 1 0 110 2h-7v7a1 1 0 11-2 0v-7H6a1 1 0 110-2h7V6a1 1 0 011-1z"
              fill="#333"
            ></path>
          </svg>
        </button>
        <button @click="logout">
          <svg
            width="28"
            height="28"
            viewBox="0 0 28 28"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fill-rule="evenodd"
              clip-rule="evenodd"
              d="M19.2929 9.29289C19.6834 8.90237 20.3166 8.90237 20.7071 9.29289L24.7071 13.2929C25.0976 13.6834 25.0976 14.3166 24.7071 14.7071L20.7071 18.7071C20.3166 19.0976 19.6834 19.0976 19.2929 18.7071C18.9024 18.3166 18.9024 17.6834 19.2929 17.2929L21.5858 15H12C11.4477 15 11 14.5523 11 14C11 13.4477 11.4477 13 12 13H21.5858L19.2929 10.7071C18.9024 10.3166 18.9024 9.68342 19.2929 9.29289Z"
              fill="#333"
            ></path>
            <path
              fill-rule="evenodd"
              clip-rule="evenodd"
              d="M5 6C5 5.44772 5.44772 5 6 5H15C15.5523 5 16 5.44772 16 6V10C16 10.5523 15.5523 11 15 11C14.4477 11 14 10.5523 14 10V7H7V21H14V18C14 17.4477 14.4477 17 15 17C15.5523 17 16 17.4477 16 18V22C16 22.5523 15.5523 23 15 23H6C5.44772 23 5 22.5523 5 22V6Z"
              fill="#333"
            ></path>
          </svg>
        </button>
      </nav>
    </div>
    <div>{{ protectedMessage }}</div>
  </div>
</template>

<style scoped>
.container {
  max-width: 50rem;
  padding: 0 1rem;
  margin: 3.75rem auto;
  font-size: 0.9rem;
  display: flex;
  flex-direction: column;
  /* gap: 1rem; */
  align-items: flex-start;
}

.wrapper {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.wrapper nav {
  display: flex;
  gap: 1rem;
}

.wrapper nav button {
  background: #eee;
  border-radius: 0.2rem;
  height: 40px;
  width: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid transparent;
  transition: 0.1s;
  cursor: pointer;
}

.wrapper nav button:hover {
  background: #fff;
  border: 1px solid rgba(0, 0, 0, 0.5);
  box-shadow: 0 0 0 3px #eee;
}

.container .btn {
  font-size: 1rem;
  cursor: pointer;
  border: none;
  border-radius: 0.4rem;
  background: #42b883;
  color: #000000;
  min-width: 8rem;
  padding: 0.5rem 1rem;
  transition: 0.1s;
}
</style>
