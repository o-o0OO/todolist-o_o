<script setup lang="ts">
import { ref, watch } from 'vue'

interface Item {
    name: string
    price: number
    date: Date
}
const items = ref<Item[]>([
    { name: 'egg', price: 100 ,date: new Date(0)},
    { name: 'apple', price: 160, date: new Date(1)},
    { name: 'tomato', price: 10000, date: new Date(2)},
    { name: 'potato', price: 1, date: new Date(3)},
    { name: 'peach', price: 300, date: new Date(4)},
])

const kounyuu = ref<Item[]>([
])
const newItemName = ref('')
const newItemPrice = ref(0)
const goukei = ref(0)
const sort = ref('')


const sortItem = (s: string) =>{
    console.log(s);
    const compare = (a: Item, b:Item) =>{
        if(a.price > b.price){
            return 1;
        }else if(a.price == b.price){
            return 0;
        }else{
            return -1;
        }
    }
    if(s=="takai"){
        items.value.sort(compare)
        items.value.reverse()
    }else if(s=="yasui"){
        items.value.sort(compare)
    }else if(s=="jikan"){
        items.value.sort((a: Item, b: Item)=>a.date.getTime() - b.date.getTime())
    }
}

const addItem = () => {
    if (newItemName.value == '' || newItemPrice.value == 0) {
        return
    }
    items.value.push({ name: newItemName.value, price: newItemPrice.value, date:new Date()})
    sortItem(sort.value);
    newItemName.value = ''
    newItemPrice.value = 0
}

const removeItem = (i: number) => {
    kounyuu.value.push(items.value[i])
    goukei.value += items.value[i].price
    items.value.splice(i,1)
}

const allRemoveItem = () =>{
    for(let i=0; i<items.value.length; i++){
        kounyuu.value.push(items.value[i])
        goukei.value += items.value[i].price
    }
    items.value.splice(0,items.value.length)
}

watch(sort,sortItem)

</script>

<template> 
  <div>ShoppingList</div>
  <div>
    Sort: {{ sort }}
    <select v-model="sort">
        <option disabled value="">select</option>"
        <option value="takai">高い順</option>
        <option value="yasui">安い順</option>
        <option value="jikan">追加した順</option>
    </select>
</div>
  <ul>
    <li v-for="(item, index) in items" :key="index">
        <div :class="{over500:item.price>=500}">
            <div>名前: {{ item.name }}</div>
            <div>{{ item.price }}円</div>
        </div>
        <div v-if="item.price >= 10000" class="kougaku">高額商品 </div>
        <button @click="removeItem(index)">買った</button>
    </li> 
  </ul>
  <button @click="allRemoveItem">全て購入</button>
  <div> 購入履歴 </div>
  <div>合計購入金額: {{ goukei }}円</div>
  <ul>
    <li v-for="(item, index) in kounyuu" :key="index">
        <div :class="{over500:item.price>=500}">
            <div>名前: {{ item.name }}</div>
            <div>{{ item.price }}円</div>
        </div>
        <div v-if="item.price >= 10000" class="kougaku">高額商品 </div>
    </li>
  </ul>
  <div>
    <label>
        名前
        <input v-model="newItemName" type="text" />
    </label>
    <label>
        値段
        <input v-model="newItemPrice" type="number" />
    </label>
    <button @click="addItem">追加</button>
  </div>
</template>

<style>
.over500{
    color: red
}
.kougaku{
    color:yellow
}
</style>