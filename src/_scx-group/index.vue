<template>
  <div class="scx-group">
    <transition-group name="scx-group-list" @before-leave="beforeLeave">
      <div v-for="(item,i) in list" :key="item" class="scx-group scx-group-item">
        <div class="scx-group-item-content">
          <slot :index="i" :item="item"></slot>
        </div>
        <div style="position: absolute;top: 0;right: 0;">
          <button v-if="i>0" @click="moveUp(i)">↑</button>
          <button v-if="i<list.length-1" @click="moveDown(i)">↓</button>
          <button @click="groupItemDelete(i)">X</button>
        </div>
      </div>
    </transition-group>
    <div @click="groupItemAdd()">
      <slot name="addButtonContent">
        <button>添加一条数据</button>
      </slot>
    </div>
  </div>
</template>

<script>
import './index.css'
import {computed} from "vue";

export default {
  name: "scx-group",
  props: {
    modelValue: {
      type: Array,
      required: true,
      default: [],
    },
    defaultItemValue: {
      type: Object,
      required: true,
      default: {},
    }
  },
  setup(props, ctx) {

    if (!props.modelValue) {
      ctx.emit("update:modelValue", []);
    }

    const list = computed({
      get() {
        return props.modelValue;
      },
      set(value) {
        ctx.emit("update:modelValue", value);
      }
    })

    function groupItemDelete(index) {
      list.value.splice(index, 1);
    }

    function moveUp(index) {
      if (index - 1 >= 0) {
        const temp = list.value[index];
        list.value[index] = list.value[index - 1];
        list.value[index - 1] = temp;
      }
    }

    function moveDown(index) {
      if (index + 1 <= list.value.length) {
        const temp = list.value[index];
        list.value[index] = list.value[index + 1];
        list.value[index + 1] = temp;
      }
    }

    function groupItemAdd() {
      const v = JSON.parse(JSON.stringify(props.defaultItemValue));
      list.value.push(v);
    }

    function beforeLeave(el) {
      const {marginLeft, marginTop, width, height} = window.getComputedStyle(el);
      el.style.left = `${el.offsetLeft - parseFloat(marginLeft)}px`;
      el.style.top = `${el.offsetTop - parseFloat(marginTop)}px`;
      el.style.width = width;
      el.style.height = height;
    }

    return {list, groupItemDelete, groupItemAdd, beforeLeave, moveUp, moveDown}
  }
}
</script>