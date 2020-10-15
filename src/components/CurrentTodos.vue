<template>
  <div class="current-todos">
    <h3>Todos</h3>
    <ul>
      <li v-for="(todo, index) in todos" :key="index" class="todo d-flex align-items-center">
        <div class="todoVal d-flex">
          
          <div v-if="isEdit && editTodoVal.indexEdit === index">
            <b-input-group>
              <b-form-input :value="todo" v-model="editTodoVal.val"></b-form-input>
              <b-input-group-append>
                <b-button variant="outline-success" @click="update(editTodoVal.val, index)">ok</b-button>
                <b-button variant="danger" @click="update(null, -1)">X</b-button>
              </b-input-group-append>
            </b-input-group>
          </div>
          <div v-else class="d-flex align-items-center">{{ todo }}</div>
        </div>

        <div class="btn-div d-flex justit">
          <b-button v-if="!isEdit || editTodoVal.indexEdit !== index" variant="info" @click="editTodo(todo, index)"
            >Edit</b-button
          >
          <b-button class="mx-2" variant="success" @click="completedTodo(todo, index)">
            Complete
          </b-button>
          <b-button variant="danger" @click="removeTodo(index)"
            >Remove</b-button
          >
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  computed: {
    todos() {
      return this.$store.state.todos;
    }
  },
  data() {
    return {
      isEdit: false,
      editTodoVal: {
        indexEdit: -1,
        val: null
      }
    }
  },
  methods: {
    editTodo(todo, index) {
      this.editTodoVal.indexEdit = index
      this.editTodoVal.val = todo
      this.isEdit = true
    },
    update(todo, index) {
      if (!todo) {
        this.isEdit = false
        this.editTodoVal = {
          indexEdit: -1,
          val: null
        }
        return
      }
      this.isEdit = false
      this.$store.commit("updateTodo", {todo, index});
    },
    completedTodo(todo, index) {
      this.$store.commit("completeTodo", { todo, index });
    },
    removeTodo(index) {
      this.$store.commit("removeTodo", index);
    }
  }
};
</script>

<style>
.current-todos {
  margin: 30px 0;
}
.todo {
  display: flex;
  justify-content: space-between;
  border: 1px solid #333;
  padding: 5px;
  border-radius: 5px;
  justify-items: center;
}
</style>
