<script setup>
import { RouterView, RouterLink } from "vue-router";
</script>

<template>
  <div id="app">
    <header>
      <div class="wrapper">
        <nav>
          <RouterLink to="/">YourFinance</RouterLink>
          <RouterLink to="/login">
            <span>Login / Criar</span>
            <img src="@/assets/user.svg" alt="User" />
          </RouterLink>
        </nav>
      </div>
    </header>

    <main>
      <RouterView />
    </main>

    <footer>
      <div class="wrapper">
        <RouterLink to="/">YourFinance</RouterLink>
        <span> YourFinance. Alguns direitos reservados. </span>
      </div>
    </footer>
  </div>
</template>

<style scoped>
#app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

header {
  width: 100%;
  border-bottom: 2px solid rgba(0, 0, 0, 0.1);
  position: fixed;
  top: 0;
  z-index: 1000;
  background: #fff;
}

.wrapper {
  max-width: 50rem;
  padding: 0 1rem;
  margin: 0 auto;
  font-size: 0.9rem;
}

nav {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
}

nav a {
  display: flex;
  gap: 0.5rem;
}

main {
  flex: 1;
  padding-top: 3.75rem;
  padding-bottom: 5rem;
}

footer {
  width: 100%;
  background: #42b883;
  padding: 2rem 1rem;
  text-align: center;
  color: #000;
}

footer .wrapper {
  display: flex;
  flex-direction: column;
}
</style>
