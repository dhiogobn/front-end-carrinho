<template>
  <div class="home">
      <h1 style="color: whitesmoke;">Cartas</h1>
    <div class="backgroud-cart">
      <div class="list-container">
        <table class="table table-borderless cards-list-container" v-if="cart.length > 0">
          <thead>
            <tr>
              <th>

              </th>
              <th>
                Nome
              </th>
              <th>
                Quantidade
              </th>
              <th>
                Preço
              </th>
              <th>
                Ações
              </th>
            </tr>
          </thead>
          <tbody>
            <tr  v-for="card in cart" :key="card.id" >
              <td>
                <img class="image-card" :src="card.image" alt="">
              </td>
              <td>
                {{ card.nome }}
              </td>
              <td>
                <div>
                  {{ card.quantidade }}
                  <svg xmlns="http://www.w3.org/2000/svg" width="25" height="25" fill="currentColor" class="bi bi-plus-square" viewBox="0 0 16 16" @click="addQuantidade(card.id, card)" style="cursor: pointer; margin-left: 15px;">
                      <path d="M14 1a1 1 0 0 1 1 1v12a1 1 0 0 1-1 1H2a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1h12zM2 0a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V2a2 2 0 0 0-2-2H2z"/>
                      <path d="M8 4a.5.5 0 0 1 .5.5v3h3a.5.5 0 0 1 0 1h-3v3a.5.5 0 0 1-1 0v-3h-3a.5.5 0 0 1 0-1h3v-3A.5.5 0 0 1 8 4z"/>
                  </svg>
                </div>
              </td>
              <td>
                {{ card.preco }}
              </td>
              <td title="Remover do carrinho">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16" style="color: red;cursor: pointer;" @click="removerDoCarrinho(card.id)">
                        <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5Zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5Zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6Z"/>
                        <path d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1ZM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118ZM2.5 3h11V2h-11v1Z"/>
                </svg>
              </td>

            </tr>
          </tbody>
        </table>
        <h1 v-else> Seu carrinho está vazio</h1>
        <div class="total-container">
          <span>Total: {{ total }} R$</span>
          <button class="btn btn-success spacement" @click="finalizar"> Finalizar</button>

        </div>

      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';
import axios from 'axios'

  export default defineComponent ({
    name: 'Cart',
    setup() {
      const cart = ref([])
      const total = ref(0)
      const url = 'http://localhost:3000/api/carrinho'
      // const response = axios.get(url).then((res) => cart.value = res.data)

      onMounted(async () => {
      try {
        cart.value = []
        const response = await axios.get(url);
        cart.value = response.data;
      }catch(error) {
        console.error(error)
      }
    });

      axios.get(url+'/total').then(res => total.value = res.data)

      return {
        cart,
        total
      }
    },
    methods: {
      finalizar() {
        const url = 'http://localhost:3000/api/carrinho'
        axios.post(url+'/finalizar').then((res => location.reload()))
      },
      removerDoCarrinho(id: any) {
        axios.delete(`http://localhost:3000/api/carrinho/${id}`).then((res) => location.reload())

      },
      addQuantidade(id: any, card: any) {
        axios.put(`http://localhost:3000/api/carrinho/${id}`, {
          id: card.id,
          nome: card.nome,
          quantidade: card.quantidade,
          preco: card.preco
        }).then((res) => location.reload())
      }
    }
  })

</script>

<style scoped>

.backgroud-cart {
  width: 66%;
  min-height: 500px;
  display: flex;
  flex-wrap: wrap;
  background-color: whitesmoke;
  border-radius: 5px;
  box-shadow: 5px 5px 5px;
}

.list-container {
    width: 100%;
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
}

.image-card {
  width: 75px;
  height: 100px;
  position: relative;
  bottom: 30px;
} 

.cards-list-container {
  margin-top: 100px;
  margin-left: 30px;
  width: 50%;
}

.spacement {
  margin-left: 5px;
  margin-right: 5px;
}
.total-container {
  background-color: #163283;
  width: 200px;
  height: 50px;
  color: whitesmoke;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 5px;
}

</style>
