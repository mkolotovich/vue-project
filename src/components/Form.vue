<script setup>
const productName = defineModel("productName")
const seller = defineModel("seller")
const picked = defineModel('picked')
const showProducts = defineModel('showProducts')
const sortBy = defineModel('sortBy')
defineEmits(['addProduct', 'sortProduct'])
</script>

<template>
  <h2>Добавить товар</h2>
  <form @submit.prevent="$emit('addProduct')">
    <div>
      <label> название товара
        <input v-model="productName" required placeholder="product name">
      </label>
    </div>
    <div>
      <label> селлер
        <input v-model="seller" required placeholder="product seller">
      </label>
    </div>
    <div>способ интеграции: 
      <input type="radio" id="one" value="reels" v-model="picked" />
      <label for="one">reels</label>

      <input type="radio" id="two" value="stories" v-model="picked" />
      <label for="two">stories</label>
    </div>
    <button>Добавить товар</button>
  </form>
  <p>Выводить товары:</p>
  <form>
    <input type="radio" id="all" value="all" v-model="showProducts" />
    <label for="all">Все</label>

    <input type="radio" id="stories" value="stories" v-model="showProducts" />
    <label for="stories">stories</label>

    <input type="radio" id="reels" value="reels" v-model="showProducts" />
    <label for="reels">reels</label>
  </form>
  <p>Сортировать товары:</p>
  <form @submit.prevent="$emit('sortProduct')">
    <input type="radio" id="asc" value="asc" v-model="sortBy" />
    <label for="asc">По возрастанию</label>

    <input type="radio" id="desc" value="desc" v-model="sortBy" />
    <label for="desc">По убыванию</label>
    <button>Отсортировать</button>
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
