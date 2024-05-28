<template>
  <section class="section">
    <h2>Wähle deine Zahlen:</h2>

    <div class="cards">
      <div v-for="(n, index) in 49" :key="index" class="card" :data-card="index + 1">
        {{ index + 1 }}
      </div>
    </div>
  </section>

  <hr>

  <section class="section">
    <h2>Deine gewählten Zahlen:</h2>

    <div class="cards selectedCards">
      <div v-for="(item) in sortedSelectedCards" class="card">
        {{ item.dataset.card }}
      </div>
    </div>
  </section>
</template>

<style scoped lang="scss">
h2 {

  margin-top: 0;
}
.section {

  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  align-items: center;
  margin: 5rem auto;
  max-width: 50rem;
}

.cards {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 1rem;
}
.card {

  padding: 2rem;
  width: 2rem;
  height: 2rem;

  font-size: 1rem;
  line-height: 2rem;
  text-align: center;

  border-radius: .5rem;
  border: .125rem solid black;
  background-color: yellow;

  cursor: pointer;

  &.active {

    background-color: lightgray;
    opacity: .5;
  }
}
</style>

<script setup lang="ts">
import { onMounted, ref } from 'vue';

let cards: NodeListOf<HTMLElement> | null = null;
const selectedCards = ref<HTMLElement[]>([]);
const sortedSelectedCards = ref<HTMLElement[]>([]);

onMounted(() => {
  cards = document.querySelectorAll('[data-card]');

  if (!cards) return
  cards.forEach((card: HTMLElement) => {
    card.addEventListener('click', () => {
      if (selectedCards.value.includes(card)) {
        removeSelection(card);
      } else {
        addSelection(card);
      };
    })
  })
})

function addSelection(card: HTMLElement) {
  if (selectedCards.value.length >= 6) return
  selectedCards.value.push(card);
  card.classList.add('active')
  sortedSelectedCards.value = [...selectedCards.value].sort((a, b) => parseInt(a.dataset.card!) - parseInt(b.dataset.card!));
}

function removeSelection(card: HTMLElement) {
  const index = selectedCards.value.indexOf(card);
  
  if (index !== -1) {
    selectedCards.value.splice(index, 1);
  }
  card.classList.remove('active');
  sortedSelectedCards.value = [...selectedCards.value].sort((a, b) => parseInt(a.dataset.card!) - parseInt(b.dataset.card!));
}
</script>
