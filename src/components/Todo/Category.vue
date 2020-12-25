<template>
  <div class="todolist__input__container">
    <input v-model="input" class="todolist__input" placeholder="New category">
    <div class="todolist__btn__container side__btn">
      <button class="todo__btn btn--add" type="button" @click="addCategoryChild()">Add Category</button>
      <button :style="[{borderBottom: '2px solid ' + categoryOptions.color,
              borderRight: '2px solid ' + categoryOptions.color}]"
              class="todo__btn btn--clear__all border__category_color animate__animated"
              type="button" @click="generateColor()">Generate color
      </button>
    </div>
  </div>
</template>

<script>
import {reactive, ref} from "vue";

export default {
  name: "Category",

  setup(props, {emit}) {
    const input = ref('');
    const categoryOptions = reactive({
      category: '',
      color: 'white'
    })

    function addCategoryChild() {
      categoryOptions.category = input.value
      // send to parent
      emit('addCategory', {...categoryOptions})
    }

    function generateColor() {
      categoryOptions.color = '#' + Math.floor(Math.random() * 2 ** 24).toString(16).padStart(6, '0'); // generates a random color
    }

    return {categoryOptions, input, generateColor, addCategoryChild}
  }
}
</script>

<style scoped>
.border__category_color {
  transition: 0.3s ease-in-out;
  border-bottom: 2px solid transparent;
}

.side__btn {
  margin-left: 5%;
  /*justify-content: space-between;*/
}
</style>
