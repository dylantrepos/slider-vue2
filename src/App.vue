<template>
  <div id="app">
    <CarouselComponent>
      <CarouselSlide v-for="n in slides" :key="n" :index="n - 1">
       <div style="position: absolute; left: 0; right: 0; text-align: center; top: 50%; color: #FFF; font-size: 16px"> Slide numéro {{ n }}</div>
        <img :src="'https://picsum.photos/630/300?random=' + n" width="100%"> 
      </CarouselSlide>
    </CarouselComponent>
    <button @click="addSlide">Ajouter un slide</button>
    <button @click="removeSlide">Supprimer une slide</button>
  </div>
</template>

<script>
import CarouselComponent from './components/carousel/CarouselComponent.vue';
import CarouselSlide from './components/carousel/CarouselSlide.vue';

export default { 
  name: 'App',
  data() {
    return {
      slides: 5,
    }
  },
  components: {
    CarouselComponent,
    CarouselSlide,
  },
  methods: {
    addSlide() {
      this.slides++
    },
    removeSlide() {
      if(this.slides > 1) this.slides--;
    }
  }
}
</script>

<style src="./components/todos.css">

</style>