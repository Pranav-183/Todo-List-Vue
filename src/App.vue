<template>
  <div class="top">
    <h1>Todo List ✓</h1>
    <div class="grid-options" title="Layout Options">
      <div v-for="i in [3,2,1]" :class="{ 'grid-option': true, 'selected': columns === i }" :style="'--grid-option-columns: ' + i" @click="() => changeColumns(i)" :key="i">
        <div v-for="iteration in Array.from(Array(i * 2))" class="grid-option-piece" :key="iteration"></div>
      </div>
    </div>
    <button class="newList" title="Create A New List" @click="createList">
      New List
    </button>
  </div>
  <AllTabs :tabs="tabs" @mediator:two="action" />
</template>

<script>
import AllTabs from "./components/AllTabs.vue";

const DEFAULT_TODO_LIST = [
  {
    id: 1,
    title: "Latest List",
    list: [
      { id: 1, text: "todo 1", completed: false },
      { id: 2, text: "todo 2", completed: false },
      { id: 3, text: "todo 3", completed: true },
    ],
  },
  {
    id: 2,
    title: "New List",
    list: [
      { id: 1, text: "todo 1", completed: true },
      { id: 2, text: "todo 2", completed: false },
      { id: 3, text: "todo 3", completed: true },
    ],
  },
  {
    id: 3,
    title: "First List",
    list: [
      { id: 1, text: "todo 1", completed: false },
      { id: 2, text: "todo 2", completed: true },
      { id: 3, text: "todo 3", completed: true },
    ],
  },
];

export default {
  components: { AllTabs },
  data() {
    return {
      tabs:
        JSON.parse(localStorage.getItem("todo_list_vue")) || DEFAULT_TODO_LIST,
      columns: 3
    };
  },
  mounted() {
    if (!localStorage.getItem("todo_list_vue")) {
      localStorage.setItem("todo_list_vue", JSON.stringify(DEFAULT_TODO_LIST));
    }
  },
  methods: {
    createList() {
      this.tabs.unshift({
        id: 1,
        title: `List ${this.tabs.length + 1}`,
        list: [],
      });
      this.tabs.forEach((tab, i) => {
        tab.id = i + 1;
      });
      this.save();
    },
    action(option, val, tabID, todoID) {
      const tab = this.tabs[tabID - 1];
      switch (option) {
        case "add":
          tab.list.push({
            id: tab.list.length + 1,
            text: val,
            completed: false,
          });
          this.save();
          break;
        case "editListName":
          tab.title = val;
          this.save();
          break;
        case "toggleCompleted":
          tab.list.find((todo) => todo.id === todoID).completed = val;
          this.save();
          break;
        case "deleteTodo":
          tab.list = tab.list.filter((todo) => todo.id !== todoID);
          tab.list.forEach((todo, i) => {
            todo.id = i + 1;
          });
          this.save();
          break;
        case "deleteList":
          this.tabs = this.tabs.filter((tab) => tab.id !== tabID);
          this.tabs.forEach((tab, i) => {
            tab.id = i + 1;
          });
          this.save()
          break;
      }
    },
    save() {
      localStorage.setItem("todo_list_vue", JSON.stringify(this.tabs));
    },
    changeFormWidth(w) {
      const form = document.querySelectorAll(".newTodoForm");
      form.forEach((form) => {
        form.style.width = `${w}%`;
      });
    },
    changeColumns(num) {
      const tabs = document.querySelector(".tabs");
      const tabArr = document.querySelectorAll(".tab")
      const delListBtns = document.querySelectorAll('.delList')
      tabs.style.gridTemplateColumns = `repeat(${num}, 1fr)`;

      if (num == 3) {
        tabs.style.gap = "2vw";
        tabArr.forEach((tab) => {
          tab.style.width = "30vw";
        });
        delListBtns.forEach(btn => {
          btn.style.right = '1vw'
        })
        this.changeFormWidth(100);
      } else if (num == 2) {
        tabs.style.gap = "10vw";
        tabArr.forEach((tab) => {
          tab.style.width = "40vw";
        });
        delListBtns.forEach(btn => {
          btn.style.right = '3vw'
        })
        this.changeFormWidth(90);
      } else if (num == 1) {
        tabs.style.gap = "5vw";
        tabArr.forEach((tab) => {
          tab.style.width = "60vw";
        });
        delListBtns.forEach(btn => {
          btn.style.right = '10vw'
        })
        this.changeFormWidth(80);
      }
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;500&display=swap");

* {
  margin: 0;
  font-family: "Roboto", sans-serif;
  font-weight: 400;
  overflow-x: hidden;
  user-select: none;
}
#app {
  min-height: 100vh;
  width: 100vw;
  color: white;
  background: linear-gradient(45deg, #adc2e6, #0055ff);
  display: flex;
  align-items: center;
  flex-direction: column;
}
svg {
  width: 24px;
  height: 24px;
}
.top {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  position: sticky;
  height: 20vh;
  width: 100vw;
}
h1 {
  font-size: 3rem;
  margin-top: 5vh;
  margin-bottom: 5vh;
  font-weight: 500;
  position: absolute;
  left: 50%;
  translate: -50%;
}
.grid-options {
  margin-right: 1vw;
  margin-left: 10vw;
  height: 15vh;
  width: 20vw;
  display: flex;
  justify-content: space-evenly;
  transition: 0.2s ease-in-out;
}
.grid-option {
  width: 40%;
  display: grid;
  grid-template-columns: repeat(var(--grid-option-columns), 1fr);
  place-items: center;
  cursor: pointer;
}
.grid-option:hover .grid-option-piece {
  background-color: rgba(255, 255, 255, 0.65);
}
.grid-option:nth-child(2) {
  border-left: 1px solid rgba(255, 255, 255, 0.3);
  border-right: 1px solid rgba(255, 255, 255, 0.3);
}
.grid-option:last-child {
  width: 20%;
}
.grid-option-piece {
  background-color: rgba(255, 255, 255, 0.4);
  height: 20px;
  width: 20px;
  transition: 0.2s ease-in-out;
}
.newList {
  padding: 0.75rem;
  margin-right: 1.5rem;
  border-radius: 5px;
  border: none;
  background-color: white;
  color: blue;
  font-size: 1.1rem;
  cursor: pointer;
  transition: 0.2s ease-in-out;
}
.newList:hover {
  scale: 1.05;
  color: red;
}
.todoList {
  display: flex;
  flex-direction: column;
  min-width: 400px;
}
@media (max-width: 1000px) {
  .grid-option:first-child {
    display: none;
  }
  .grid-option:nth-child(2) {
    border-left: none;
    padding-right: 5px;
  }
  .grid-option:last-child {
    margin-left: -20px;
  }
}
@media (max-width: 700px) {
  .grid-options {
    display: none;
  }
}
@media (max-width: 400px) {
  h1 {
    left: 0;
    translate: 20%;
  }
}
::-webkit-scrollbar {
  width: 12px;
}
::-webkit-scrollbar-track {
  background: #d7d7d7;
}
::-webkit-scrollbar-thumb {
  background: #a1a1a1;
}
::-webkit-scrollbar-thumb:hover {
  background: #a7a7a7;
}
::-webkit-scrollbar-track:hover {
  background: #c7c7c7;
}
</style>