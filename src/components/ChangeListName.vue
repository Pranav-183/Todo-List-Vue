<template>
  <div class="modal">
    <div class="old">
      <label for="oldName">Old Name: </label>
      <input type="text" name="oldName" :value="oldName" readonly />
    </div>
    <form @submit="submit">
      <div class="new">
        <label>New Name: </label>
        <input type="text" placeholder="New Todo List Name" autofocus v-model="newName" />
      </div>
      <div class="btns">
        <button class="cancel" type="button" @click="close">Cancel</button>
        <button class="change" type="submit" @click="submit">Change</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  props: ["oldName"],
  emits: ['changeListName', 'close'],
  data() {
    return {
      newName: "",
    };
  },
  methods: {
    submit(e) {
      e.preventDefault()
      if (this.newName === '') return this.close()
      this.$emit('changeListName', this.newName)
      this.close()
    },
    close() {
      this.$emit('close')
    }
  }
};
</script>

<style scoped>
.modal {
  display: flex;
  flex-direction: column;
  width: 100%;
  gap: .5rem;
}
.old, .new {
  display: flex;
  width: 100%;
  gap: .5rem;
  align-items: center;
  justify-content: flex-end;
}
.old > input {
  cursor: not-allowed;
}
.old > input:focus {
  outline: none;
}
.new > input:focus {
  outline: none;
  border: 2px solid orange;
}
.btns {
  display: flex;
  width: 100%;
  justify-content: center;
  margin: 1rem 0;
  gap: 1rem;
}
.btns > button {
  border: none;
  padding: .75rem;
  cursor: pointer;
  transition: .2s ease;
}
.btns > button:hover {
  opacity: .9;
}
.cancel {
  background-color: rgb(3, 201, 3);
}
.change {
  background-color: red;
}
label {
  font-size: 1.2rem;
  font-family: monospace;
}
input {
  width: 70%;
  padding: 0.5rem;
  font-size: 1rem;
}
</style>