<template>
  <div class="todolist__wrapper">

    <div class="todolist__container">
      <div class="todolist__header">
        <h2 class="header__text">Todo List</h2>
      </div>

      <div class="todolist__body">
        <transition class="todolist_list__container"
                    enter-active-class="animate__animated animate__bounceIn"
                    leave-active-class="animate__animated animate__bounceOut">
          <Category @addCategory="pushCategory"/>
        </transition>

        <div class="todolist__input__container">
          <input v-model="input" class="todolist__input" placeholder="New item">

          <select v-if="categoryList.length > 0" v-model="selectCategory"
                  :style="[selectCategory.category !== '' ?
                  {borderBottom: '2px solid ' + selectCategory.color, borderRight: '2px solid ' + selectCategory.color}
                  : {borderBottom: '2px solid transparent', borderRight: '2px solid transparent'}]"
                  class="todolist__input todolist__select animate__animated animate__bounceIn">
            <option disabled value="">Please select one</option>
            <option v-for="(category, index) in categoryList" :key="category+index"
                    :selected="selectCategory.category === category" :value="category">
              {{ category.category }}
            </option>
          </select>
        </div>

        <div class="todolist_list__container">
          <div class="todolist__btn__container">
            <button class="todo__btn btn--add" type="button" @click="addToList">Add</button>
            <button class="todo__btn btn--clear" type="button" @click="shuffleItems">Shuffle</button>
            <button class="todo__btn btn--clear__all " type="button" @click="clearList">Clear All</button>
          </div>

          <transition enter-active-class="animate__animated animate__bounceIn"
                      leave-active-class="animate__animated animate__bounceOut">
            <p v-if="errorMessage !== ''">{{ errorMessage }}</p>
          </transition>
        </div>

        <div class="todolist_list__container todo__list__items">
          <transition-group
              enter-active-class="animate__animated animate__zoomIn"
              leave-active-class="animate__animated  animate__zoomOut"
              mode="out-in"
              name="list">
            <List v-for="(item, index) in list" :class="index" :key="item.item"
                  :itemText="item" class="list__item"
                  @click="removeFromList(list.indexOf(item))">{{index}}</List>
          </transition-group>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import {ref} from "vue";
import Category from "@/components/Todo/Category";
import List from "@/components/Todo/List";


export default {
  name: 'TodoList',
  components: {
    List, Category
  },

  setup() {
    const input = ref('')
    const list = ref([])
    const categoryList = ref([])
    const selectCategory = ref([]);
    const errorMessage = ref('');


    function addToList() {
      if (input.value !== '') {
        errorMessage.value = ''
        console.log(selectCategory.value);
        list.value.push({item: input.value, label: [selectCategory.value]});
        input.value = ''
      } else {
        errorMessage.value = 'Input is empty'
      }
    }

    function pushCategory(value) {
      categoryList.value.push(value)
      selectCategory.value = []
      selectCategory.value.push(value)
    }

    function shuffleItems() {
      if(list.value.length === 0) {
        return errorMessage.value = 'Add more items to shuffle'
      }

      errorMessage.value = ''
      let randomNumbers = list.value;
      for (let i = randomNumbers.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [randomNumbers[i], randomNumbers[j]] = [randomNumbers[j], randomNumbers[i]];
      }
      list.value = [];               //Maakt de items array leeg
      list.value = randomNumbers;    //En voeg de geshuffelde nummers toe aan items
    }

    function removeFromList(index) {
      list.value.splice(index, 1)
    }

    function clearList() {
      list.value = []
    }

    return {
      list, input, categoryList, selectCategory, errorMessage,
      shuffleItems, clearList, addToList, removeFromList, pushCategory
    }
  }
}
</script>

<style>
.todolist__wrapper {
  margin-top: 30px;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.todolist__container {
  width: 60%;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  border-radius: 15px;
}

.todolist__header {
  text-align: left;
  background: #141417;
  border-radius: 15px;
  z-index: 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header__text {
  margin-left: 5.5%;
  color: white;
}

.todolist__body {
  background: #18191C;
  margin-top: -15px;
  padding: 40px 10px 10px 10px;
  border-bottom-left-radius: 15px;
  border-bottom-right-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.todo__category--activate {
  border-radius: 15px;
  padding: 0 5px;
  cursor: pointer;
  width: 30px;
  height: 30px;
  font-size: 24px;
  font-weight: 500;
  background: #18191C;
  transition: 0.3s ease-in-out;
}

.todo__category--activate:hover {
  color: #18191C;
  background: #2c3e50;
  transition: 0.3s ease-in-out;
}

.todolist_list__hide__category {
  margin-right: 5.5%;
}

.todolist__input__container {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  width: 94%;
  margin-bottom: 30px;
}


.todolist__input {
  background: #141417;
  padding: 13px 13px;
  border-radius: 12px;
  border: 2px solid transparent;
  color: #757575;
  width: 50%;
  max-width: 300px;
}

.todolist__select {
  color: white;
  margin-left: 5%;
  width: 20%;
}

.todolist__btn__container {
  display: flex;
  justify-content: flex-start;
  width: 40%;
}

.todo__btn {
  margin-right: 5px;
  padding: 13px 13px;
  border-radius: 8px;
  border: 1px solid transparent;
  color: white;
  font-size: 12px;
  background: #141417;;
}

.todolist_list__container {
  width: 94%;
}

.todolist_list__container:not(:first-child) {
  margin-bottom: 30px;
}

.todo__list__items {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  justify-content: flex-start;
}

.list__item {
  min-width: 350px;
  max-width: 100px;
  transition: 0.3s ease-in-out;
  margin: 10px;
}
</style>
