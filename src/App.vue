<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'

</script>

<template>
  <header>
    

    <div class="wrapper">
      <img alt="Vue logo" class="logo" src="@/assets/books_img.png" width="125" height="125" />
      <HelloWorld msg="読書管理DB" />

      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/input">Input</RouterLink>
        <RouterLink to="/list">List</RouterLink>
        <RouterLink to="/updata">Updata</RouterLink>
        <RouterLink to="/database">Database</RouterLink>
      </nav>
    </div>
  </header><br>


  <RouterView />


</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;

}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
     display: flex;
    place-items: center;
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: center;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
