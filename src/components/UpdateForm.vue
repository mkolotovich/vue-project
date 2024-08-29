<script setup>
const newProductName = defineModel("newProductName")
const newSeller = defineModel("newSeller")
const newPicked = defineModel('newPicked')
defineEmits(['editProduct', 'closeModal'])
</script>

<template>
  <h2>Редактировать товар</h2>
  <button @click="$emit('closeModal')">X</button>
  <form @submit.prevent="$emit('editProduct', newProductName, newSeller, newPicked)">
    <div>
      <label> название товара
        <input v-model="newProductName" required placeholder="product name">
      </label>
    </div>
    <div>
      <label> селлер
        <input v-model="newSeller" required placeholder="product seller">
      </label>
    </div>
    <div>способ интеграции: 
      <input type="radio" id="three" value="reels" v-model="newPicked" />
      <label for="three">reels</label>

      <input type="radio" id="four" value="stories" v-model="newPicked" />
      <label for="four">stories</label>
    </div>
    <button>Редактировать товар</button>
  </form>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
