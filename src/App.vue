<template>
  <div class="app-container">
    <header class="header">
      <h1>🇹🇭 泰国旅行英语 100 句</h1>
      <button class="theme-btn" @click="toggleTheme">{{ themeBtnText }}</button>
    </header>
    <CategoryList />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import CategoryList from './components/CategoryList.vue';

const themeBtnText = ref('切换暗黑模式');
const toggleTheme = () => {
  const root = document.documentElement;
  if (root.getAttribute('data-theme') === 'dark') {
    root.removeAttribute('data-theme');
    themeBtnText.value = '切换暗黑模式';
  } else {
    root.setAttribute('data-theme', 'dark');
    themeBtnText.value = '切换浅色模式';
  }
};
</script>
