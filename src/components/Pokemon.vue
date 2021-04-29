<template>
   <div class="card">
      <div class="card-image">
         <figure>
            <img :src="currentImgUrl" alt="Placeholder image">
         </figure>
      </div>
      <div class="card-content">
         <div class="media">
            <div class="media-content">
               <p class="title is-4">{{ name }}</p>
               <p class="subtitle is-6">{{  }}</p>
            </div>
         </div>
         <div class="content">
            <button @click="changeSide" class="button">CHANGE SIDE</button>
         </div>
      </div>
   </div>
</template>

<script>
import axios from 'axios';

export default {
   props: {
      name:  String,
      url: String
   },

   data: function() {
      return {
         pokemon: {},
         frontSprite: true,
         currentImgUrl: ''
      }
   },

   created: function() {
      axios.get(this.url).then(data => 
      {
         this.pokemon = data.data;
         this.currentImgUrl = this.pokemon.sprites.front_default;
      });
   },

   methods: {
      changeSide: function() {
         this.frontSprite = !this.frontSprite;
         this.currentImgUrl = this.frontSprite? this.pokemon.sprites.back_default: this.pokemon.sprites.front_default;
      }
   }
}
</script>

<style scoped>
   .card {
      margin-bottom: 20px;
   }
</style>
