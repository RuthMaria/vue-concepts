<template>
  <div class="form-todo form-group">
    <p>
      <input
        placeholder="nome"
        type="text"
        name="author"
        class="form-control"
        v-model="name"
      />
    </p>
    <p>
      <textarea
        placeholder="Comentário"
        name="message"
        class="form-control"
        v-model="message"
      ></textarea>
    </p>

    <p v-show="isError">Preencha o campo mensagem</p>

    <button @click="addComment" type="submit" class="btn btn-primary">
      Comentar
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      message: "",
      isError: false,
    };
  },

  methods: {
    addComment() {
      if (this.message.trim() === "" || this.message === "") {
        this.isError = true;
      }

      // emitindo um evento
      this.$emit("add-todo", {
        name: this.name,
        message: this.message,
      });

      this.name = "";
      this.message = "";
      this.isError = false;
    },
  },
};
</script>
