<template>
  <div class="home">
    <Header />
    <div class="modal" v-if="isItemAddVis">
      <div class="modal_block">
        <div class="close" @click="hideAddItemModal">
          <img src="@/assets/close.svg" alt="">
        </div>
        <h4>Добавить покупку</h4>
        <form @submit.prevent="addItem">
          <input v-focus v-model="input_content" class="add-item-input" type="text" placeholder="Название">
          <input type="submit" class="btn btn-lg" value="Добавить" />
        </form>
      </div>
    </div>
    <div class="modal" v-if="isItemSetDone">
      <div class="modal_block">
        <div class="close" @click="hideAddItemModal">
          <img src="@/assets/close.svg" alt="">
        </div>
        <h4>Укажите цену за товар</h4>
        <form @submit.prevent="addPrice">
          <input v-focus v-model="input_contentAdd" class="add-item-input" type="text" placeholder="Цена">
          <input type="submit" class="btn btn-lg" value="Готово" />
        </form>
      </div>
    </div>



    <section class="list__wrapper" v-if="listItems.length !== 0">
      <div v-for="item in listItems_asc" :key="item.id" :class="`list-item ${item.isDone && 'item-done'}`">
        <div class="btns">
          <span>
            <button @click="itemDone(item)" class="btn btn-sm done" v-if="item.price === null">
              <img src="@/assets/done.svg" alt="done">
            </button>
            <span v-else class="text price">
              {{ item.price }}
              <svg width="12" height="15" viewBox="0 0 12 15" fill="none" xmlns="http://www.w3.org/2000/svg">
                <g clip-path="url(#clip0_1_103)">
                  <path
                    d="M2.8125 0.9375C2.29395 0.9375 1.875 1.35645 1.875 1.875V7.5H0.9375C0.418945 7.5 0 7.91895 0 8.4375C0 8.95605 0.418945 9.375 0.9375 9.375H1.875V10.3125H0.9375C0.418945 10.3125 0 10.7314 0 11.25C0 11.7686 0.418945 12.1875 0.9375 12.1875H1.875V13.125C1.875 13.6436 2.29395 14.0625 2.8125 14.0625C3.33105 14.0625 3.75 13.6436 3.75 13.125V12.1875H8.4375C8.95605 12.1875 9.375 11.7686 9.375 11.25C9.375 10.7314 8.95605 10.3125 8.4375 10.3125H3.75V9.375H7.03125C9.36035 9.375 11.25 7.48535 11.25 5.15625C11.25 2.82715 9.36035 0.9375 7.03125 0.9375H2.8125ZM7.03125 7.5H3.75V2.8125H7.03125C8.32617 2.8125 9.375 3.86133 9.375 5.15625C9.375 6.45117 8.32617 7.5 7.03125 7.5Z"
                    fill="#0B0A0A" />
                </g>
                <defs>
                  <clipPath id="clip0_1_103">
                    <rect width="11.25" height="15" fill="white" />
                  </clipPath>
                </defs>
              </svg>
            </span>
          </span>
          <span>
            <!-- <button class="btn btn-sm">
              <img src="@/assets/pencil.svg" alt="edit">
            </button> -->
            <button @click="removeItem(item)" class="btn btn-sm">
              <img src="@/assets/del.svg" alt="del">
            </button>
          </span>
        </div>
        <div class="list-item-content">
          <span class="text">{{ item.content }}</span>
        </div>
      </div>
    </section>

    <Empty v-else />

    <AddBtn @click="showAddItemModal" />
    <!-- <CalcBtn /> -->

  </div>
</template>

<script setup>
import { ref, onMounted, computed, watch } from 'vue'

import Header from '@/components/Header.vue';
import CalcBtn from '@/components/CalcBtn.vue';
import AddBtn from '@/components/AddBtn.vue';
import Empty from '@/components/Empty.vue';

const listItems = ref([])
const input_content = ref('')
const input_contentAdd = ref('')

const content = ref('')
const price = ref(null)
const isItemAddVis = ref(false)
const isItemSetDone = ref(false)

const listItems_asc = computed(() => listItems.value.sort((a, b) => {
  return b.id - a.id
}))


const addItem = () => {
  if (input_content.value.trim() === '') {
    return
  }

  listItems.value.push({
    content: input_content.value,
    price: null,
    isDone: false,
    id: new Date().getTime()
  })
  input_content.value = ''
  hideAddItemModal()
}

const removeItem = item => {
  listItems.value = listItems.value.filter(t => t !== item)
  console.log(item);
}

watch(listItems, newVal => {
  localStorage.setItem('listItems', JSON.stringify(newVal))
}, { deep: true })

onMounted(() => {
  listItems.value = JSON.parse(localStorage.getItem('listItems')) || []
})


const itemDone = (item) => {
  showAddPriceItemModal()
  item.price = input_contentAdd
  item.isDone = true
}
const addPrice = () => {
  hideAddItemModal()
}


function showAddItemModal() {
  isItemAddVis.value = true
}
function showAddPriceItemModal() {
  isItemSetDone.value = true
}
function hideAddItemModal() {
  isItemAddVis.value = false
  isItemSetDone.value = false
}




content.value = 'масло'

</script>

<style scoped lang="scss">
.home {
  padding: 0 20px;
}

.modal_block {
  position: relative;
  height: auto;
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 30px;

  .close {
    position: absolute;
    right: 10px;
    top: 10px;
  }

  form {
    display: flex;
    flex-direction: column;
    align-items: center;

    width: 100%;
    gap: 30px;

    input[type="text"] {
      width: 100%;
    }
  }
}

.item-done {
  filter: blur(1px);

  .list-item-content {
    text-decoration: line-through;
    opacity: .8;
  }
}

.list__wrapper {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.list-item {
  border-radius: 6px;
  padding: 9px;
}

.price {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-family: 'Oswald', sans-serif;
}

.btns {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;

  span:nth-last-child(1) {
    display: flex;
    justify-content: space-between;
    gap: 5px;
  }
}

.done {
  padding: 8px 10px;
  padding-bottom: 5px;
}</style>
