<template>
  <div class="flex justify-end  ">
    <q-btn push color="primary" label="Создать задачу" class="q-mt-lg">
      <q-popup-proxy class="bg-amber-1">
        <q-banner class="bg-amber-1">
          <template v-slot:avatar>
            <FormCreateTask @modelValue="(n)=>submitForm(n)" class="bg-amber-1"/>
          </template>
        </q-banner>
      </q-popup-proxy>
    </q-btn>
  </div>
  <h1></h1>
  <div class="flex wrap justify-center">
    <div class="">
    </div>
    <div v-for="category in categories"
         :key="category.id"
         @drop="onDrop($event, category.id)"
         class="droppable q-card--bordered col-xl-12 col-md-6 col-sm-4"
         :class="{unFinished_card: category.id === 0,inWorks_card: category.id === 1, finished_card: category.id === 2 }"
         @dragover.prevent
         @dragenter.prevent
    >
      <div class="flex justify-center items-center text-bold ">
        <p class="category_title q-mr-sm">
          {{ category.title }}
        </p>
        <p v-if="category.id === 0" class="count_items flex justify-center items-center">
          <span>{{ countUnfinished }}</span>
        </p>
        <p v-if="category.id === 1" class="count_items flex justify-center items-center">
          <span>{{ countInWorks }}</span>
        </p>
        <p v-if="category.id === 2" class="count_items flex justify-center items-center">
          <span>{{ countFinished }}</span>
        </p>
      </div>


      <div v-for="item in items.filter(x => x.categoryId === category.id)"
           @dragstart="onDragStart($event, item)"
           class="draggable card_task"
           :class="{unFinished: category.id === 0, finished_items: category.id ===2 }"
           draggable="true"
      >
        <div :class="{finished_items: category.id === 2}"
             v-if="category.id === 2"
        >
          <p class="text-bold flex justify-between">
            <span :class="{line_throught: category.id === 2 }">
              ID: {{ item.id }}
            </span>
            <span>
              <q-btn>
                <q-icon name="far fa-pen-to-square" @click="changeTask(item,index)"/>
                  <q-popup-edit title="Редактировать ">
                    <q-input dense autofocus
                             filled
                             label="Название"
                             hint="{{item.value.name}}"
                             v-model="changeFormTitle"
                    />
                    <q-input dense autofocus
                             filled
                             label="Цена"
                             hint="Цена"
                             v-model="changeFormPrice"
                    />
                    <q-input dense autofocus
                             filled
                             label="комментарий"
                             hint="Комментарий"
                             v-model="changeFormComment"
                    />
                    <q-btn @click.prevent="sendChangeForm(item)">Сохранить</q-btn>
            </q-popup-edit>
               </q-btn>
              <q-btn @click="delelteTask(item)">
                <q-icon name="fas fa-trash"/>
              </q-btn>
          </span>
          </p>
          <p><span class="in_works_image"><q-img :src="item.image" width="50px"/></span>
            {{ item.title }}
          </p>
          <p><span>Заверешено</span><span></span> {{ new Date() }}</p>
          <p><span>Цена</span> {{ item.price }}</p>
          <p><span>Комментарий</span>{{ changeFormComment }}</p>
        </div>

        <div class="" :class="{inWorks: category.id === 1}" v-if="category.id === 1">
          <p class="text-bold "><span>ID: {{ item.id }}</span></p>
          <p><span class="in_works_image">
            <q-img :src="item.image" width="50px"/>
          </span> {{ item.title }}
          </p>
          <p><span>Создан</span> {{ item.date }}</p>
          <p><span>Цена</span> {{ item.price }}</p>
        </div>

        <div class="" :class="{unFinished: category.id === 0}" v-if="category.id === 0">
          <p class="text-bold "><span>ID: {{ item.id }}</span></p>
          <p><span>Название</span> {{ item.title }}</p>
          <p><span>Цена</span> {{ item.price }}</p>
          <p><span>Категория</span>{{ item.category }}</p>
          <p><span>Описание</span> {{ item.description }}</p>
          <div class="flex justify-center">
            <q-btn v-if="category.id === 0" color="blue" @click="workTask($event, item)">
              Взять в работу
            </q-btn>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import {computed, onMounted, ref} from 'vue'
import axios from "axios";
import FormCreateTask from "components/formCreateTask.vue";
import {useQuasar} from 'quasar'

const $q = useQuasar()
let changeFormTitle = ref('')
let changeFormPrice = ref('')
let changeFormComment = ref('')

/* Массив с категориями */
const items = ref([])
const categories = ref([
  {
    id: 0,
    title: 'Не обработанные '
  },
  {
    id: 1,
    title: 'В работе'
  },
  {
    id: 2,
    title: 'завершенные'
  }
])
/* Создание Колонок по категориям  */

/* Создание всех карточек товаров и занесение их в массив  */
async function getProducts() {
  try {
    const res = await axios.get('https://fakestoreapi.com/products?limit=10')
    res.data.forEach((item) => {
      items.value.push({
        id: item.id,
        categoryId: 0,
        title: item.title,
        category: item.category,
        price: item.price,
        image: item.image,
        description: item.description,
        date: new Date()
      })
    })
  } catch (err) {
    console.log(err)
  }
}

onMounted(() => {
  getProducts()
})
/* Формирование колонок */

const countUnfinished = computed({
  // getter
  get() {
    let sum = 0
    items.value.forEach((item) => {
      if (item.categoryId === 0) {
        sum += 1
      }
    })
    countUnfinished.value = sum
    return sum
  },
  // setter
  set(newValue) {

  }
})


const countInWorks = computed({
  // getter
  get() {
    let sum = 0
    items.value.forEach((item) => {
      if (item.categoryId === 1) {
        sum += 1
        console.log(sum)
      }
    })
    countUnfinished.value = sum
    return sum
  },
  // setter
  set(newValue) {

  }
})


const countFinished = computed({
  // getter
  get() {
    let sum = 0
    items.value.forEach((item) => {
      if (item.categoryId === 2) {
        sum += 1
        console.log(sum)
      }
    })
    countUnfinished.value = sum
    return sum
  },
  // setter
  set(newValue) {

  }
})


/* Функции для Drag and DROP  */
function onDragStart(e, item) {
  e.dataTransfer.dropEffect = 'move'
  e.dataTransfer.effectAllowed = 'move'
  e.dataTransfer.setData('itemId', item.id.toString())
}

function onDrop(e, categoryId) {
  const itemId = parseInt(e.dataTransfer.getData('itemId'))
  items.value = items.value.map(x => {
    if (x.id == itemId)
      x.categoryId = categoryId
    return x
  })
}

/* функция для определенияв какую колонку занести  */
function workTask(e, item) {
  items.value = items.value.map(x => {
    if (x.id === item.id)
      x.categoryId = 1
    return x
  })
}

/* Удаление карточки товара  */
function delelteTask(item) {
  let itemId = items.value.indexOf(item, 0)
  console.log(itemId)
  items.value.splice(itemId, 1)
}

/* Создание новой карточки и занесение ее в массив */
const submitForm = (n) => {
  console.log(n.value.name.value)
  items.value.push({
    id: Math.random().toString().slice(2, 10),
    categoryId: 0,
    title: n.value.name.value,
    price: n.value.price.value,
    phone: n.value.phone.value,
    pay: n.value.pay.value,
    date: n.value.date.value
  })
}

/* Заполнение Формы редактирования карточки */
const changeTask = async (item, index) => {
  console.log(item.title)
  changeFormTitle.value = item.title
  changeFormPrice.value = item.price
}

/* Изменения карточки товара */
let sendChangeForm = (item) => {
  let itemId = items.value.indexOf(item, 0)
  console.log(items.value[itemId].id)
  if (itemId !== -1) {
    items.value[itemId] = {
      id: items.value[itemId].id,
      categoryId: 2,
      image: items.value[itemId].image,
      title: changeFormTitle.value,
      price: changeFormPrice.value
    }
  }
}

</script>

<style scoped>
.category_title {
  font-size: 20px;
  font-weight: bold;
}

.count_items {
  width: 50px;
  background: white;
  border-radius: 50%;
  font-size: 20px;
}

.line_throught {
  text-decoration: line-through;
}

.in_works_image {
  max-width: 10px;
  border-radius: 50%;
}

.card_task span {
  font-weight: bold;
}

.unFinished_items {
  color: #000000;

}

.unFinished_card {
  background: rgba(95, 95, 243, 0.63);
  max-width: 33%;
}

.inWorks_items {


}

.inWorks_card {
  background: #F3A100FC;
  max-width: 33%;
}


.finished_items {
  background: #fa1d42 !important;
  color: white;

}

.finished_card {
  background: #FA6A6AA0;
  max-width: 33%;

}


.droppable {
  padding: 15px;
  border-radius: 5px;
  margin-bottom: 10px;
}

.droppable h4 {
  color: white;
}

.draggable {
  background: white;
  padding: 5px;
  border-radius: 5px;
  margin-bottom: 5px;
}

.draggable h5 {
  margin: 0;
}
</style>
