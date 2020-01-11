<template>
  <div>
    <form @submit="addTodo">
      <input type="text" v-model="title" name="title" placeholder="Add Todo" />
      <input type="submit" value="Submit" class="btn" />
    </form>
  </div>
</template>


<script>
export default {
  name: "AddTodo",
  data() {
    return {
      title: ""
    };
  },
  methods: {
    addTodo(e) {
      e.preventDefault();
      const newTodo = {
        id: 4,
        title: this.title,
        completed: false
      };
      // send via emit
      this.$emit("add-todo", newTodo);
      this.title = "";
    }
  }
};
</script>


<style scoped>
form {
  display: flex;
  margin-bottom: 20px;
}
input[type="text"] {
  flex: 10;
  padding: 5px;
  text-align: center;
}
input[type="submit"] {
  flex: 2;
}
</style>