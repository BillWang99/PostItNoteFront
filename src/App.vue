<script >
  import HeaderView from './components/HeaderView.vue';
  import MainView from './components/MainView.vue';
  import TrashCanView from './components/TrashCanView.vue';
  export default{
    name:'App',
    components:{
      HeaderView,
      MainView,
      TrashCanView,
    },
    data(){
      return {

      }
    }
  }
</script>

<template>
  <header>
    <HeaderView/>
  </header>

  <main>
    <MainView />
    <TrashCanView/>
  </main>

  <main>

  </main>
</template>

<style scoped>
  .main{
    width:100%;
  }
</style>
