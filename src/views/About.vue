<template>
  <div class="about">
    <h1>This is an about page</h1>
  </div>
</template>


<script>
import {ref, computed, onMounted, onUnmounted} from "vue";


export default {
  name: 'About',

  setup() {
    const data = ref("Achaaaa ik ook")
    const wordCount = computed(() => data.value.length) // computed method (Import dit!)

    // Muis tracker
    const x = ref(0)
    const y = ref(0)

    // Methods zijn nu gewoon functions!
    function update(e) {
      x.value = e.pageX;
      y.value = e.pageY;
    }

    // Import de onMounted & onUnMounted hooks ook!
    onMounted(() => {
      window.addEventListener("mousemove", update);
    })

    onUnmounted(() => {
      window.addEventListener("mousemove", update);
    })

    return {data, wordCount, x, y}
  }
}
</script>

