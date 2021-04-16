<template>
  <div id="pokemon">
    <div class="card">
      <div class="card-image">
        <figure>
          <img :src="currentImg" alt="Placeholder image"/>
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content">
            <p class="title is-4">{{ num }} - {{ name | upper }}</p>
            <p class="subtitle is-5">Type: {{ pokemon.type }}</p>
            <p class="subtitle is-5">HP: {{ pokemon.hp }}</p>
            <p class="subtitle is-6">Attack: {{ pokemon.attack }} |--| Defense: {{ pokemon.defense }}</p>
            <p class="subtitle is-6">Special Attack: {{ pokemon.specialAt }} |--|
               Special Defense: {{ pokemon.specialDef}}</p>
          </div>
        </div>

        <div class="content">
          <button id="but" class="button is-medium is-fullwidth" @click="mudarSprite()">
            Flip the pokémon (Front/Back)</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  created() {
    axios.get(this.url).then((res) => {
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.hp = res.data.stats[0].base_stat;
      this.pokemon.attack = res.data.stats[1].base_stat;
      this.pokemon.defense = res.data.stats[2].base_stat;
      this.pokemon.specialAt = res.data.stats[3].base_stat;
      this.pokemon.specialDef = res.data.stats[4].base_stat;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      this.currentImg = this.pokemon.front;
      console.log(this.pokemon);
    });
  },
  data() {
    return {
      is_front: true,
      currentImg: '',
      pokemon: {
          type: '',
          hp: Number,
          attack: Number,
          defense: Number,
          specialAt: Number,
          specialDef: Number,
          front: '',
          back: ''
      },
    };
  },
  props: {
    num: Number,
    name: String,
    url: String,
  },
  filters: {
    upper: function (value) {
      var newName = value[0].toUpperCase() + value.slice(1);
      return newName;
    },
  },
  methods: {
    mudarSprite: function(){
      if(this.is_front){
        this.is_front = false;
        this.currentImg = this.pokemon.back;
      }
      else{
        this.is_front = true;
        this.currentImg = this.pokemon.front;
      }
    }
  }
};
</script>

<style>

#pokemon{
  margin-top: 2%;
}

#but {
  background-color:goldenrod;
}

</style>