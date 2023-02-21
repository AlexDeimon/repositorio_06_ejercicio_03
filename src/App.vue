<template>
  <header class="header">
    <nav id="nav" class="nav">
      <Logo/>
      <div id="toggle-menu" class="toggle-menu">
        <label for="toggle-menu_checkbox">
          <img src="https://cdn.icon-icons.com/icons2/903/PNG/512/menu_icon-icons.com_69502.png" alt="icono menu">
        </label>
      </div>
    </nav>
    <input type="checkbox" class="toggle-menu_checkbox" id="toggle-menu_checkbox">
    <ul class="nav-menu" id="nav-menu">
      <li class="nav-menu_item"><router-link to="/" class="nav-menu_link">Productos</router-link></li>
      <li class="nav-menu_item"><router-link to="/about" class="nav-menu_link">Sobre el trabajo</router-link></li>
    </ul>
  </header>
  <main>
    <router-view />
  </main>
  <footer class="footer">
    <Logo/>
    <a href="https://github.com/AlexDeimon" class="link resaltado">❤️ Diego Sandoval</a>
  </footer>
</template>
<script>
import Logo from "@/components/Logo.vue";
export default {
  name: "App",
  components: {
    Logo,
  },
};
</script>
<style>
body {
  margin: 0;
}

h1,
h2 {
  margin-top: 0;
}

main {
  position: relative;
  background-image: url("assets/GZ02.jpg");
  background-size: cover;
  background-position: center;
  min-height: 80vh;
}

.header {
  position: relative;
  background-color: black;
  height: 90px;
}

.nav-menu {
  list-style: none;
  display: flex;
  flex-direction: column;
  align-items: center;
  transform: translateY(-180%);
  transition: transform 0.5s;
  background-color: black;
  width: 100%;
  padding-left: 0;
  font-size: large;
}

@media screen and (min-width: 768px) {
  .nav-menu {
    flex-direction: row;
    transform: translateY(0%);
    width: auto;
  }
  .toggle-menu {
    display: none;
  }
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .toggle-menu_checkbox:checked + .nav-menu {
    transform: translateY(100%);
  }
}

.nav-menu_item {
  padding: 1em;
}

.toggle-menu {
  width: 40px;
  position: absolute;
  top: 1.5rem;
  right: 1.5rem;
  cursor: pointer;
  filter: invert(1);
}

.toggle-menu img {
  max-width: 100%;
}

.toggle-menu_checkbox {
  display: none;
}

.toggle-menu_checkbox:checked + .nav-menu {
  position: relative;
  transform: translateY(-20%);
  z-index: 10;
}

.footer {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: black;
  font-size: large;
}

.titulo2 {
  padding: 10px;
  display: inline-block;
  text-align: center;
  color: #ffff00;
  font-size: xx-large;
  letter-spacing: 10px;
  margin-bottom: 0;
  margin-top: 10px;
}

.header a {
  font-weight: bold;
  color: white;
}

.header a.router-link-exact-active {
  color: red;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
}
</style>
