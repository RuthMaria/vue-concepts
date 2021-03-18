<template>
  <div class="container">
    <h1>Comentários</h1>
    <hr />

    <div v-for="(poke, index) in pokemons" :key="index">
      <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
    </div>

    <FormTodo @add-todo="addComment($event)"></FormTodo>

    <div class="list-group">
      <p
        v-if="comments.length <= 0"
        :class="{ comment: !isDark, comentClean: isDark }"
      >
        {{ pharse | pharseUpcase }}
      </p>
      <div
        v-else
        class="list-group-item"
        v-for="(comment, index) in allComments"
        :key="index"
      >
        <span class="comment__author"
          >Autor: <strong>{{ comment.name }}</strong></span
        >
        <p>{{ comment.message }}</p>
        <div>
          <a href="#" title="Excluir" @click.prevent="removeComment(index)"
            >Excluir</a
          >
        </div>
      </div>
    </div>
    <hr />
  </div>
</template>

<script>
import FormTodo from "./FormTodo";
import axios from "axios";
import Pokemon from "./Pokemon";

export default {
  components: {
    FormTodo,
    Pokemon,
  },

  // igual ao useEffect
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => (this.pokemons = res.data.results));
  },

  data() {
    return {
      comments: [],
      isDark: true,
      pharse: "Sem comentários...",
      pokemons: [],
    };
  },

  methods: {
    addComment(comment) {
      this.comments.push(comment);
    },

    removeComment: function (index) {
      this.comments.splice(index, 1);
    },
  },

  // sempre retorna um valor. São 'variáveis' que retornam valores. Dados formatados.
  computed: {
    allComments() {
      return this.comments.map((comment) => ({
        ...comment,
        name: comment.name.trim() === "" ? "Anônimo" : comment.name,
      }));
    },
  },

  //precisa receber um valor e retorna um valor
  filters: {
    pharseUpcase: function (value) {
      return value.toUpperCase();
    },
  },

  // fica assistindo as mudanças
  watch: {
    comments(val) {
      this.val = val + 1;
    },
  },
};
</script>

<style scoped>
.comment {
  font-weight: 600;
}
</style>
