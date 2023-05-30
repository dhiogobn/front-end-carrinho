<template>
  <div class="home">
      <h1 style="color: whitesmoke;">Cartas</h1>
      <div class="create-button">
        <router-link to="/cardForm">
          <button class="btn btn-success">Criar</button>
        </router-link>
      </div>
    <div class="backgroud">
      <div v-for="card in cards" :key="card.id">
        <Card :title="card.nome" :price="card.preco" :quantity-remaining="card.quantidade" :image="card.image" :id="card.id"/>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Card from '../components/Card.vue'
import {ref, onMounted} from 'vue'
import axios from 'axios';

export default defineComponent({
  name: 'HomeView',
  components: {
    Card
  },
  setup() {
    const cards = ref([]);

    onMounted(async () => {
      try {
        cards.value = []
        const response = await axios.get('http://localhost:3000/api/cards');
        cards.value = response.data;
      }catch(error) {
        console.error(error)
      }
    });

    return {
      cards
    };
  }
});


</script>

<style>

.create-button {
  width: 66%;
  display: flex;
  justify-content: end;
  margin-bottom: 10px;
}
.home {
  widows: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.backgroud {
  background-image: url("https://pbs.twimg.com/media/EWITy1SWAAAJCKF.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  width: 66%;
  min-height: 500px;
  display: flex;
  flex-wrap: wrap;
}


.text-redcollor {
    color: #d31417 !important;
}


</style>
