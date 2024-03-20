<template>
  <!-- <div v-if="screenWidthDesktop">
    <MainMenuSaraiva :n1menu="menu"  />
  </div>
  <div v-if="screenWidthMob">
    <MainMenuMob :n1menu="menu"  />
  </div> -->
  <div>
    <MainMenuSaraiva :n1menu="menu"  />
  </div>
</template>

<script>
import MainMenuSaraiva from './components/saraiva/MainMenuSaraiva.vue'
// import MainMenuMob from './components/saraiva/MainMenuMob.vue'
import menujson from './api/saraiva.json';

export default {
  name: 'App',
  components: {
    MainMenuSaraiva,
    // MainMenuMob
  },
  data() {
    return {
      screenWidthDesktop: window.innerWidth > 1200,
      screenWidthMob: window.innerWidth < 1200
    }
  },
  computed: {
    menu(){
      return menujson.menu;
    },
  },
  created(){
    console.log(typeof this.menu)
    console.log(window.innerWidth)
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
body {
  background: #fff;
  margin: 0;
}
</style>
