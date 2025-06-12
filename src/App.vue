<script setup lang="ts">
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import ClickCounter from './components/ClickCounter.vue';
import ItemList from './components/ItemList.vue';

defineProps<{
  msg: string
}>()
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
      <h1>{{ msg }}</h1>
      <ClickCounter />
      <ItemList />
    </div>
  </header>

  <main>
    <TheWelcome />
  </main>
</template>

<style scoped>
a {
  color: #42b983;
}
</style>
