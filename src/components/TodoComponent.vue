<template>
  <span :class="{ completed }">{{ todo.text }}</span>
  <svg viewBox="0 0 24 24" id="complete" @click="toggleComplete">
    <path
      fill="currentColor"
      d="M21,7L9,19L3.5,13.5L4.91,12.09L9,16.17L19.59,5.59L21,7Z"
    />
  </svg>
  <svg viewBox="0 0 24 24" id="del" @click="delFunc">
    <path
      fill="currentColor"
      d="M19,4H15.5L14.5,3H9.5L8.5,4H5V6H19M6,19A2,2 0 0,0 8,21H16A2,2 0 0,0 18,19V7H6V19Z"
    />
  </svg>
</template>

<script>
export default {
  props: ["todo"],
  emits: ["toggledComplete", "del:origin"],
  data() {
    return {
      completed: this.todo.completed,
    };
  },
  methods: {
    toggleComplete() {
      this.completed = !this.completed;
      this.$emit("toggledComplete", this.completed, this.todo.id);
    },
    fallEnded() {
      this.$emit("del:origin", this.todo.id);
    },
    delFunc(e) {
      let todo = e.target.id === 'del' ? e.target.parentElement : e.target.parentElement.parentElement
      todo.classList.add("preDel");
      todo.addEventListener("animationend", this.fallEnded);
    },
  },
};
</script>

<style scoped>
span {
  background-color: #fff;
  color: black;
  display: flex;
  align-items: center;
  padding-left: 1rem;
  white-space: nowrap;
  overflow-x: auto;
  transition: 0.2s ease-in-out;
  font-size: 1.3rem;
}
::-webkit-scrollbar {
  height: 8px;
}
span.completed {
  text-decoration: line-through;
}
svg {
  padding: 10px;
  height: 40px;
  min-width: 25px;
  cursor: pointer;
}
#complete {
  margin-left: auto;
  background-color: rgb(4, 186, 4);
}
#del {
  background-color: orangered;
}
</style>