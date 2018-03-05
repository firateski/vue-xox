<template>
  <div class="about">
    <div class="game-link">
      <router-link to="/">Oyuna Dön</router-link>
    </div>
    <h1>Hakkımda</h1>
    <p>{{about}}</p>
  </div>
</template>

<script>
export default {
  name: 'AboutPage',
  data() {
    return {
      about: 'Merhaba ben fırat bu da ilk Vue.js projem. Bu proje Vue.js deneme amacıyla oluşturulmuştur.',
    };
  },
};
</script>

<style scoped>

.game-link{
  margin-bottom: 40px;
}

</style>

