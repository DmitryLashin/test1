<template>
  <Profile/>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component'
import HelloWorld from './components/HelloWorld.vue'
import Profile from './components/Profile.vue'

@Options({
  components: {
    Profile,
    HelloWorld
  }
})

export default class App extends Vue {
  public myVariable = 'omg'
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
