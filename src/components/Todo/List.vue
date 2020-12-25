<template>
  <div :style="'border-left: 4px solid ' + itemText.label[0].color" class="list__container">
    <p class="list__text">{{ itemText.item }}</p>

    <p class="list__category" :style="{borderColor: itemText.label[0].color, color: itemText.label[0].color}">
      {{itemText.label[0].category}}
    </p>
  </div>
</template>

<script>
export default {
  name: "List",
  props: {
    itemText: Object
  }
}
</script>

<style scoped>
.list__container {
  width: 100%;
  text-align: left;
  color: white;
  background: #141417;
  border-radius: 13px;
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 10px 10px 18px -14px rgba(0,0,0,0.75);
}

.list__text {
  color: white;
  margin-left:3%;
  transition: 0.3s ease-in-out;
}

.list__category {
  border: 2px solid red;
  padding: 10px;
  border-radius: 10px;
  margin-right: 4%;
  background: #18191C;
}
</style>
