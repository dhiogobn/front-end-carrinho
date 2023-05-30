<template>
    <div class="home">        
        <h1 v-if="!idUrl" style="color: whitesmoke;">Criar carta</h1>
        <h1 v-if="idUrl" style="color: whitesmoke;">Editar Carta</h1>
        <div class="card-background">
            <form @submit.prevent="submitForm">
                <div class="input-container">
                    <div class="mb-3 input-container-position">
                        <label for="nome" class="form-label">Nome: </label>
                        <input type="text" class="form-control size-adjust" id="nome" v-model="cardForm.nome" v-validate="'required'" placeholder="Mago Negro">
                    </div>
                    <div class="mb-3 input-container-position">
                        <label for="preco" class="form-label">Preço: </label>
                        <input type="text" class="form-control size-adjust" id="preco" v-model="cardForm.preco" v-validate="'required'" placeholder="15">
                    </div>
                    <div class="mb-3 input-container-position">
                        <label for="quantidade" class="form-label">Quantidade: </label>
                        <input type="text" class="form-control size-adjust" id="quantidade" v-model="cardForm.quantidade" v-validate="'required'" placeholder="12">
                    </div>
                    <div class="mb-3 input-container-position">
                        <label for="imagem" class="form-label">Imagem: </label>
                        <input type="text" class="form-control size-adjust" id="imagem" v-model="cardForm.image" v-validate="'required'" placeholder="https://img.mypcards.com/img/3/496/yugioh_gld1_en039/yugioh_gld1_en039_en.jpg">
                    </div>

                    <button class="btn btn-primary" style=" width: 75px;
                                                            position: relative;
                                                            left: 41%;
                                                            margin-top: 50px;
                                                            margin-bottom: 50px;"
                                                             type="submit">salvar</button>
                                                            
                </div>
            </form>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { useForm, useField } from 'vee-validate';
import axios from 'axios';
import { useRoute } from 'vue-router';

export default defineComponent({
    name: "CardForm",
    methods: {
        redirecionarParaOutraRota() {
            // Usando o método de roteamento this.$router.push()
            this.$router.push('/');
        }
    },
    setup() {
        const cardForm = ref({
            id: null,
            nome: '',
            preco: '',
            quantidade: '',
            image: ''
        })

        const url = 'http://localhost:3000/api/cards'
        const router = useRoute();
        const idUrl = router.params.id;

        const { handleSubmit, resetForm, errors } = useForm();

        const submitForm = handleSubmit((values) => {
        console.log(idUrl)
        if(!idUrl){
            console.log('entrou no post')
            axios.post(url, {
            nome: cardForm.value.nome,
            preco: cardForm.value.preco,
            quantidade: cardForm.value.quantidade,
            image: cardForm.value.image
        })
        .then(function (response) {
            
        })
        .catch(function (error) {
            window.alert(error)
        });

        }else {
            axios.put(url+`/${idUrl}`, {
                nome: cardForm.value.nome,
                preco: cardForm.value.preco,
                quantidade: cardForm.value.quantidade,
                image: cardForm.value.image
            }).then((res) => {
                
            })
        }
        window.history.back();
        });

        useField('nome')
        useField('preco')
        useField('quantidade')
        useField('image')

        return {
            cardForm,
            submitForm,
            errors,
            idUrl
        }
    }
})
</script>

<style scoped>

.size-adjust {
    width: 50%;
}

.input-container-position {
    position: relative;
    left: 20%;
}

.input-container {
    margin-top: 8%;
    margin-left: 8%;
    display: flex;
    flex-direction: column;
    justify-content: center;
}
.card-background {
    width: 66%;
    margin: 20px 20px;
    min-height: 300px;
    background-color: aliceblue;
    border-radius: 5px;
}

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