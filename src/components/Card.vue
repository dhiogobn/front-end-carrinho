<template>
    <div class="card-background">
        <div class="card radius">
            <div class="background-image">
                <img :src="image" class="card-img-top img-card" alt="Nome da Carta" style="width: 200px; height: 250px;">
            </div>
            <div class="card-body ">
                <h5 class="card-title text-center">{{ title }}</h5>
                <p class="card-text text-center">{{ price }} R$</p>
                <p class="card-text text-center">Quantidade em estoque: {{ quantityRemaining }}</p>
                <div class="input-container-card">
                    <router-link :to="'/cardForm/edit/'+id">
                        <span title="Editar">
                            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil-square" viewBox="0 0 16 16">
                                <path d="M15.502 1.94a.5.5 0 0 1 0 .706L14.459 3.69l-2-2L13.502.646a.5.5 0 0 1 .707 0l1.293 1.293zm-1.75 2.456-2-2L4.939 9.21a.5.5 0 0 0-.121.196l-.805 2.414a.25.25 0 0 0 .316.316l2.414-.805a.5.5 0 0 0 .196-.12l6.813-6.814z"/>
                                <path fill-rule="evenodd" d="M1 13.5A1.5 1.5 0 0 0 2.5 15h11a1.5 1.5 0 0 0 1.5-1.5v-6a.5.5 0 0 0-1 0v6a.5.5 0 0 1-.5.5h-11a.5.5 0 0 1-.5-.5v-11a.5.5 0 0 1 .5-.5H9a.5.5 0 0 0 0-1H2.5A1.5 1.5 0 0 0 1 2.5v11z"/>
                            </svg>
                        </span>

                    </router-link>
                    <span title="Deletar Carta">
                        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16" style="color: red;" @click="handleClick(id)">
                        <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5Zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5Zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6Z"/>
                        <path d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1ZM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118ZM2.5 3h11V2h-11v1Z"/>
                        </svg>
                    </span>
                    <span title="Adicionar ao carrinho">
                        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-cart" viewBox="0 0 16 16" style="color: blue;" title="Adicionar ao carrinho" @click="addToCart(id)">
                            <path d="M0 1.5A.5.5 0 0 1 .5 1H2a.5.5 0 0 1 .485.379L2.89 3H14.5a.5.5 0 0 1 .491.592l-1.5 8A.5.5 0 0 1 13 12H4a.5.5 0 0 1-.491-.408L2.01 3.607 1.61 2H.5a.5.5 0 0 1-.5-.5zM3.102 4l1.313 7h8.17l1.313-7H3.102zM5 12a2 2 0 1 0 0 4 2 2 0 0 0 0-4zm7 0a2 2 0 1 0 0 4 2 2 0 0 0 0-4zm-7 1a1 1 0 1 1 0 2 1 1 0 0 1 0-2zm7 0a1 1 0 1 1 0 2 1 1 0 0 1 0-2z"/>
                        </svg>
                    </span>


                </div>

            </div>
        </div>
    </div>
</template>
<script lang="ts">
import axios from 'axios'
import { defineProps, defineEmits, ref} from 'vue'

export default {
    name:"Card",
    props: {
        id: Number,
        title: String,
        price: Number,
        quantityRemaining: Number,
        image: String

    },
    methods: {
        handleClick(id: any) {
            axios.delete(`http://localhost:3000/api/cards/${id}`).then(response => {
                console.log('deletado!')
            }).catch(error => {
            console.error('Ocorreu um erro ao deletar o recurso:', error);
            });
            window.location.reload();
            
        },
        addToCart(id: any) {
            axios.get(`http://localhost:3000/api/cards/${id}`).then((res) => {
                axios.post('http://localhost:3000/api/carrinho', {
                    id: res.data.id,
                    nome: res.data.nome,
                    preco: res.data.preco,
                    quantidade: res.data.quantidade,
                    image: res.data.image

                }).then((response) => {console.log(response)})
            })
        }
    }
}
</script>
<style scoped>

.input-container-card {
    display: flex;
    justify-content: center;
}

svg {
    width: 25px;
    height: 25px;
    margin-right: 10px;
    cursor: pointer;
}

.button-container {
    width: 100%;
    display: table-cell;
}

.mr-3 {
    margin-right: 10px;
}
.card-background {
    width: 300px;
    margin: 20px 20px;
}

.img-card {
    position: relative;
    left: 15%;
    margin-top: 5px;
}

.button-collor {
    background-image: linear-gradient(to right,#100A26, #6a23a4);
    color: white;
}

.button-collor:hover {
    background-image: linear-gradient(to right,#6a23a4, #100A26);
    color: white !important;
}

.text-gradient-collor {
    background-image: linear-gradient(to right,#100A26, #6a23a4);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.text-redcollor {
    color: #d31417;
}

.radius {
    border-radius: 15px;
}

.background-image {
    width: 100%;
    background-image: url("data:image/jpeg;base64,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");
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
}
</style>