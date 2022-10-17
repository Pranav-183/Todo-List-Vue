<template>
  <div class="tabs">
     <div v-for="tab in tabs" :key="tab" class="tab">
      <OneTab :tab="tab" @mediator:one="emitMediator" />
     </div>
  </div>
</template>

<script>
import OneTab from './OneTab.vue'
export default {
   props: ['tabs'],
   emits: ['mediator:two'],
   components: {OneTab},
   methods: {
      emitMediator(option, val, tabID, todoID) {
         this.$emit('mediator:two', option, val, tabID, todoID)
      }
   }
}
</script>

<style>
.tabs {
   display: grid;
   grid-template-columns: repeat(3, 1fr);
   gap: 2vw;
}
.tab {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 30vw;
}
@media (max-width: 1000px) {
   .tabs {
      grid-template-columns: repeat(2, 1fr);
      gap: 10vw;
   }
   .tab {
      width: 40vw;
   }
}
@media (max-width: 800px) {
   .top > h1 {
      font-size: 2rem;
   }
   .tabs {
      grid-template-columns: repeat(1, 1fr);
   }
   .tab {
      width: 70vw;
   }
}
</style>