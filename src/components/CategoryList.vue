<template>
  <div class="grid">
    <div v-for="cat in categories" :key="cat.id" class="item-card" @click="openCategory(cat)">
      <h3>{{ cat.name }}</h3>
      <p class="item-desc">{{ cat.desc }}</p>
      <div class="item-row">
        <span>共 {{ cat.total }} 句</span>
        <span>已掌握 {{ cat.masteredCount }}</span>
      </div>
      <div class="bar-base"><div class="bar-fill" :style="{width: (cat.total > 0 ? cat.masteredCount/cat.total*100 : 0) + '%'}"></div></div>
    </div>
  </div>
  <WordPopup v-if="currentCat" :category="currentCat" @close="currentCat=null" @update="updateCategory" />
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { travelData } from '../data.js';
import WordPopup from './WordPopup.vue';

const categories = ref([]);
const currentCat = ref(null);

const loadCategories = () => {
  const storedProgress = JSON.parse(localStorage.getItem('travel-progress') || '{}');
  categories.value = travelData.map(cat => ({
    ...cat,
    masteredCount: storedProgress[cat.id] || 0
  }));
};

const openCategory = (cat) => {
  currentCat.value = { ...cat };
};

const updateCategory = (updated) => {
  const idx = categories.value.findIndex(c => c.id === updated.id);
  if (idx !== -1) categories.value[idx] = updated;
};

onMounted(loadCategories);
</script>
