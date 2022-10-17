<template>
  <div class="listName">
    <div v-if="changeName == false" class="nameAndDel">
      <h2
        @click="() => (changeName = true)"
        title="Click To Edit Todo List Name"
      >
        {{ tab.title }}
      </h2>
      <svg viewBox="0 0 24 24" title="" class="delList" @click="delList">
        <path
          fill="currentColor"
          d="M19,4H15.5L14.5,3H9.5L8.5,4H5V6H19M6,19A2,2 0 0,0 8,21H16A2,2 0 0,0 18,19V7H6V19Z"
        />
      </svg>
    </div>
    <ChangeListName
      v-else
      :oldName="tab.title"
      @changeListName="editLastName"
      @close="() => (changeName = false)"
    />
  </div>
  <form @submit="createTodo" class="newTodoForm">
    <input
      type="text"
      placeholder="Enter new todo"
      v-model="input"
      name="input"
      :id="tab.id"
      autocomplete="off"
    />
    <button>
      <svg viewBox="0 0 24 24">
        <path
          fill="currentColor"
          d="M19,13H13V19H11V13H5V11H11V5H13V11H19V13Z"
        />
      </svg>
    </button>
  </form>
  <div class="todos">
    <div
      v-for="todo in tab.list"
      :key="todo"
      :class="{ completed: todo.completed, todo: true }"
      :id="todo.id"
    >
      <TodoComponent
        :todo="todo"
        @toggledComplete="toggleCompleted"
        @del:origin="deleteFunc"
      />
    </div>
  </div>
</template>

<script>
import TodoComponent from "./TodoComponent.vue";
import ChangeListName from "./ChangeListName.vue";

export default {
  props: ["tab"],
  emits: ["mediator:one"],
  components: { TodoComponent, ChangeListName },
  data() {
    return {
      input: "",
      changeName: false,
    };
  },
  methods: {
    addOrEdit(option, val, tabID, todoID) {
      if (val.toString().trim() !== "") this.$emit("mediator:one", option, val, tabID, todoID);
    },
    createTodo(e) {
      e.preventDefault();
      this.addOrEdit("add", this.input, this.tab.id);
      this.input = "";
      e.target.input.focus()
    },
    editLastName(newName) {
      this.addOrEdit("editListName", newName, this.tab.id);
    },
    toggleCompleted(completed, todoID) {
      this.addOrEdit("toggleCompleted", completed, this.tab.id, todoID);
    },
    deleteFunc(todoID) {
      this.addOrEdit("deleteTodo", "delTodo", this.tab.id, todoID);
    },
    delList() {
      this.addOrEdit('deleteList', "delList", this.tab.id)
    }
  },
};
</script>

<style scoped>
.listName {
  width: 100%;
  display: flex;
  justify-content: center;
  position: relative;
}
h2 {
  margin: 0.5rem 0;
  cursor: pointer;
}
.nameAndDel {
  display: flex;
  justify-content: center;
  position: relative;
  width: 100%;
}
.delList {
  position: absolute;
  right: 1vw;
  cursor: pointer;
  background-color: rgb(255, 60, 60);
  transition: .2s ease;
}
.delList:hover {
  scale: 1.05;
}
.todos {
  display: flex;
  align-items: center;
  flex-direction: column;
  width: 100%;
  margin-top: 2rem;
  margin-bottom: 3rem;
}
.todo {
  display: flex;
  height: 60px;
  width: 100%;
  background-color: #fff;
  margin: 0.25rem 0;
  transition: 0.2s ease-in-out;
}
.todo.completed {
  opacity: 0.6;
}
.todo.preDel {
  animation: fadeOut 0.3s forwards;
}
form {
  display: flex;
  width: 100%;
  justify-content: center;
  align-items: center;
}
input {
  width: 100%;
  padding: 20px;
  border: none;
  font-size: 1.2rem;
}
input:hover,
input:focus {
  outline: none;
}
button {
  border: none;
  background-color: white;
  cursor: pointer;
  height: 100%;
  aspect-ratio: 1 / 1;
  transition: 0.2s ease-in-out;
}
button:hover {
  background-color: #adc2e6;
}
button:hover svg {
  background-color: white;
  color: #adc2e6;
}
svg {
  background-color: skyblue;
  padding: 5px;
  border-radius: 5px;
  color: white;
  transition: 0.2s ease-in-out;
}
@keyframes fadeOut {
  to {
    opacity: 0;
  }
}
</style>