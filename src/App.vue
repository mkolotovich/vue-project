<script setup>
import Form from './components/Form.vue'
import UpdateForm from './components/UpdateForm.vue'
import List from './components/List.vue'
import { ref, computed } from 'vue'

const name = ref('')
const newName = ref('')
const seller = ref('')
const newSeller = ref('')
const picked = ref('reels')
const newPicked = ref('reels')
const showProducts = ref('all')
const sortBy = ref('asc')
const page = ref(1);
const perPage = ref(20);
const isModalShown = ref(false);
const curentItem = ref('')

const pages = computed(() => {
  let data = goods.value
  if (showProducts.value !== 'all') {
    data = data.filter((el) => el.picked === showProducts.value)
  }
  const res = [];
  let j = 1;
  for (let i = 0; i < data.length; i += perPage.value) {
    if (j === 1) {
      res.push({value: j, isActive: true})
    } else {
      res.push({value: j, isActive: false})
    }
    j++
  }
  return res
})
// const goods = ref([])


function addProduct() {
  goods.value.push({ name: name.value, seller: seller.value, picked: picked.value })
  name.value = ''
  seller.value = ''
  picked.value = 'reels'
  localStorage.setItem('goods', JSON.stringify(goods.value));
}

function sortProduct() {
  if (sortBy.value === 'desc') {
    goods.value.sort(function (a, b) {
      if (a.name > b.name) {
        return 1;
      }
      if (a.name < b.name) {
        return -1;
      }
      return 0;
    })
  } else {
    goods.value.sort(function (a, b) {
      if (a.name > b.name) {
        return -1;
      }
      if (a.name < b.name) {
        return 1;
      }
      return 0;
    })
  }
}

const goods = ref([
  {"name":"a","seller":"1","picked":"reels"},
  {"name":"b","seller":"2","picked":"stories"},
  {"name":"c","seller":"3","picked":"reels"},
  {"name":"4","seller":"4","picked":"stories"},
  {"name":"5","seller":"5","picked":"reels"},
  {"name":"6","seller":"6","picked":"stories"},
  {"name":"7","seller":"7","picked":"reels"},
  {"name":"8","seller":"8","picked":"stories"},
  {"name":"9","seller":"9","picked":"reels"},
  {"name":"10","seller":"10","picked":"stories"},
  {"name":"11","seller":"11","picked":"reels"},
  {"name":"12","seller":"12","picked":"stories"},
  {"name":"13","seller":"13","picked":"reels"},
  {"name":"14","seller":"14","picked":"stories"},
  {"name":"15","seller":"15","picked":"reels"},
  {"name":"16","seller":"16","picked":"stories"},
  {"name":"17","seller":"17","picked":"stories"},
  {"name":"18","seller":"18","picked":"reels"},
  {"name":"19","seller":"19","picked":"stories"},
  {"name":"20","seller":"20","picked":"reels"},
  {"name":"21","seller":"21","picked":"stories"},
  {"name":"22","seller":"22","picked":"reels"},
  {"name":"23","seller":"23","picked":"stories"}
])

if (JSON.parse(localStorage.getItem('goods'))) {
  goods.value = JSON.parse(localStorage.getItem('goods'))
} else {
  localStorage.setItem('goods', JSON.stringify(goods.value));
}

const filteredProducts = computed(() => {
  if (goods.value.length > perPage.value) {
    const sliced = goods.value.slice((page.value - 1) * perPage.value, perPage.value * page.value)
    if (showProducts.value !== 'all') {
      return goods.value.filter((el) => el.picked === showProducts.value).slice((page.value - 1) * perPage.value, perPage.value * page.value)
    }
    return sliced
  }
  if (showProducts.value !== 'all') {
    return goods.value.filter((el) => el.picked === showProducts.value)
  }
  return goods.value
})


function removeProduct(product) {
  goods.value = goods.value.filter((t) => t !== product)
  localStorage.setItem('goods', JSON.stringify(goods.value));
}

function showProduct(product) {
  isModalShown.value = !isModalShown.value
  curentItem.value = goods.value.find((el) => el.name === product.name)
  newName.value = product.name
  newSeller.value = product.seller
  newPicked.value = product.picked
}

function editProduct(newNameValue, seller, picked) {
  curentItem.value.name = newNameValue
  curentItem.value.seller = seller
  curentItem.value.picked = picked
  localStorage.setItem('goods', JSON.stringify(goods.value));
}

function closeModal() {
  isModalShown.value = !isModalShown.value
}

function changePage(curentPage) {
  pages.value[page.value - 1].isActive = !pages.value[page.value - 1].isActive
  curentPage.isActive = !curentPage.isActive
  page.value = curentPage.value
}

function nextPage() {
  pages.value[page.value - 1].isActive = !pages.value[page.value - 1].isActive
  pages.value[page.value].isActive = !pages.value[page.value].isActive
  page.value = page.value + 1
}

function prevPage() {
  page.value = page.value - 1
  pages.value[page.value - 1].isActive = !pages.value[page.value - 1].isActive
  pages.value[page.value].isActive = !pages.value[page.value].isActive
}

</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <Form @addProduct="addProduct()" @sortProduct="sortProduct" v-model:product-name="name" v-model:seller="seller" v-model:picked="picked" v-model:show-products="showProducts" v-model:sort-by="sortBy"/>
    </div>
  </header>
  <main>
    <List
      v-for="product in filteredProducts"
      :name="product.name"
      :seller="product.seller"
      :picked="product.picked"
      @delete="removeProduct(product)"
      @edit="showProduct(product)"
	  ></List>
    <div class="d-flex">
      <button @click="prevPage()" v-if="page > 1" >Предыдущие товары</button>
      <ul class="d-flex list">
        <li v-for="item in pages">
          <button @click="changePage(item)" :class="{ active: item.isActive }">{{item.value}}</button>
        </li>
      </ul>
      <button @click="nextPage()" v-if="goods.length  % (perPage * page) < perPage && goods.length > perPage && filteredProducts.length === perPage">Cледующие товары</button>
  </div>
  </main>
  <div :class="[isModalShown ? 'd-flex' : '', 'overlay']">
    <div :class="[isModalShown ? 'd-block' : '', 'modal']">
      <UpdateForm @edit-product="editProduct" @closeModal="closeModal()" v-model:new-product-name="newName" v-model:new-seller="newSeller" v-model:new-picked="newPicked"/>
    </div>
  </div>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

.list {
  list-style: none;
}

.active {
  background: lightblue;
}

.overlay {
  background: rgba(23, 33, 57, 0.5);
  width: 100vw;
  height: 100vh;
  position: absolute;
  display: none;
}

.d-flex {
  display: flex;
}

.modal {
  background: white;
  width: 440px;
  margin: auto;
  padding-left: 40px;
  padding-right: 40px;
  box-sizing: border-box;
  display: none;
}

.d-block {
    display: block;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
