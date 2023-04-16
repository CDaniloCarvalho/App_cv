<template>
  <nav>
    <div @click="isOpen = !isOpen" class="menuResponsivo"><span className="">&#9776;</span></div>
    <router-link :class="`menu ${ isOpen ? 'd-block' : 'menu-superior'}`"  to="/">Gerador CV </router-link> 
    <router-link :class="`menu ${ isOpen ? 'd-block' : 'menu-superior'}`"  to="/Help">Ajuda</router-link>
  </nav>
  <router-view/>
</template>

<script>

export default {

  data() {
    return{
      isOpen: false
    }
  },

  methods:{

  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #f6f8fa;
  background-image: url('./assets/fundo.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
  width:  100%;
  height: 100%;
  min-height: 100vh;
  padding: 0;
  margin: 0;
}

nav {
  display: flex;
  justify-items: end;
  border-bottom: solid rgba(0, 0, 0, 0.205) 1px;
  margin-bottom: 10px;
  background-color: #b4b4b53e;
  
  @media (max-width: 600px) {
    display: block; 
  }
  @media (min-width: 600px) {
    display: flex; 
  }

  a {
    font-weight: bold;
    text-decoration: none;
    color: #f1f3f4;
    
    &.router-link-exact-active {
    color: #f1f3f4;
      background-color: #8e8e8e81;
    }
  }

}

.menu {
  padding: 15px 70px;
  border: solid rgba(0, 0, 0, 0.174) 1px;
  background-color: #b4b4b53e;
 
  @media (max-width: 600px) {
    display: block; 
    width: 100%; 
    padding: 15px; 
    border: none; 
    background-color: #b4b4b53e; 
  }
}

.menuResponsivo {
  display: none;
  
  @media (max-width: 600px) {
    position: relative;
    width:100px;
    left: 0;
    justify-items: end; 
    display: block; 
    font-size: 2rem; 
    padding: 15px; 
    cursor: pointer; 
  }
}

.menuResponsivo:hover{
  background-color: #f1f3f441;
}

.menu-superior{
  display: block; 
  @media (max-width: 600px) {
    display: none; 
  }
}

router-link{
  margin-left:30px;
}
</style>
