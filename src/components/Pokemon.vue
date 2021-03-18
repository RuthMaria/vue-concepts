<template>
  <div>
    <h1>{{ num }} {{ name | upper }}</h1>
    <small>{{ url }}</small
    ><br />
    <img :src="currentImg" alt="" /><br />
    <small>{{ pokemon.type }}</small
    ><br />
    <small>{{ pokemon.back }}</small>
    <button class="button is-medium is-fullwidth" @click="changeSprite">
      Mudar sprint
    </button>
    ><br />
  </div>
</template>

<script>
import axios from "axios";

export default {
  created: function () {
    axios.get(this.url).then((res) => {
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      this.currentImg = this.pokemon.front;
    });
  },

  data() {
    return {
      currentImg: "",
      isfront: true,
      pokemon: {
        type: "",
        front: "",
        back: "",
      },
    };
  },

  methods: {
    changeSprite: function () {
      if (this.isfront) {
        this.isfront = false;
        this.currentImg = this.pokemon.back;
      } else {
        this.isfront = true;
        this.currentImg = this.pokemon.front;
      }
    },
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
};
</script>

<style>
</style>
