<template>
  <div class="mask" @click.self="$emit('close')">
    <div class="pop-box">
      <h3>{{ category.name }}</h3>
      <div class="word-flip" :class="{ flipped }" @click="flip">
        <div class="flip-inner">
          <div class="flip-front">{{ currentWord?.en }}</div>
          <div class="flip-back">{{ currentWord?.cn }}</div>
        </div>
      </div>
      <div class="btn-group">
        <button class="btn" @click="speak">🔊 发音朗读</button>
        <button class="btn" @click="markDone">✅ 已掌握</button>
        <button class="btn" @click="nextWord">➡️ 下一句</button>
        <button class="btn" @click="$emit('close')">❌ 关闭</button>
      </div>
      <div class="knowledge-toggle-bar">
        <button class="btn" @click="showKnowledge = !showKnowledge" style="font-size:13px; padding:6px 20px;">
          {{ showKnowledge ? '收起旅行介绍' : '展开旅行介绍' }}
        </button>
      </div>
      <div v-if="showKnowledge" class="knowledge" v-html="category.content"></div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const props = defineProps({ category: Object });
const emit = defineEmits(['close', 'update']);

const words = ref(props.category.list || []);
const index = ref(0);
const flipped = ref(false);
const mastered = ref(props.category.masteredCount || 0);
const showKnowledge = ref(false);

const currentWord = computed(() => words.value[index.value]);

const flip = () => { flipped.value = !flipped.value; };

const speak = () => {
  if (currentWord.value) {
    const u = new SpeechSynthesisUtterance(currentWord.value.en);
    u.lang = 'en-US';
    u.rate = 0.85;
    speechSynthesis.speak(u);
  }
};

const markDone = () => {
  if (mastered.value < props.category.total) {
    mastered.value++;
    const stored = JSON.parse(localStorage.getItem('travel-progress') || '{}');
    stored[props.category.id] = mastered.value;
    localStorage.setItem('travel-progress', JSON.stringify(stored));
    emit('update', { ...props.category, masteredCount: mastered.value });
  }
  nextWord();
};

const nextWord = () => {
  flipped.value = false;
  index.value = (index.value + 1) % words.value.length;
};
</script>
