<template>
  <scx-group v-model="data" :default-item-value="{name:'',age:'',c:[]}">
    <template #default="{item,index}">
      <div class="test-input-wrapper">
        Name
        <input v-model="item.name"/>
        Age:
        <input v-model="item.age"/>
      </div>
      <h3>H3H3H3H3</h3>
      <scx-group v-model="item.c" :default-item-value="{b:'',d:''}">
        <template #default="h">
          <div class="test-input-wrapper">
            B
            <input v-model="h.item.b"/>
            D:
            <input v-model="h.item.d"/>
          </div>
        </template>
      </scx-group>
    </template>
  </scx-group>
  {{ data }}
</template>

<script setup>
import ScxGroup from './_scx-group/index.vue'
import {ref} from "vue";

const data = ref([{
  name: '', age: '', c: [
    {b: '', d: ''},
    {b: '', d: ''},
    {b: '', d: ''},
    {b: '', d: ''},
    {b: '', d: ''},
    {b: '', d: ''},
    {b: '', d: ''},
  ]
}, {
  name: '', age: '', c: [
    {b: '', d: ''},
    {b: '', d: ''},
    {b: '', d: ''},
    {b: '', d: ''},
    {b: '', d: ''},
    {b: '', d: ''},
    {b: '', d: ''},
  ]
}]);
</script>
<style scoped>
.test-input-wrapper {
  display: flex;
  justify-content: center;
}
</style>