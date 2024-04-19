<template>
    <div class="">
        <Message :message="feedback_message" v-show="feedback_message" />

        <div>
            <form id="burger-form" @submit="createBurger">
                <div class="input-container">
                    <label for="name">Nome do cliente:</label>
                    <input type="text" id="name" name="name" v-model="client_name" placeholder="Nome do cliente">
                </div>

                <div class="input-container">
                    <label for="bread">Escolha o pão:</label>
                    <select name="selected_bread" id="selected_bread" v-model="selected_bread">
                        <option v-for="bread in breads" :key="bread.id" :value="bread.value">{{bread.value}}</option>
                    </select>
                </div>

                <div class="input-container">
                    <label for="meat">Escolha a carne:</label>
                    <select name="selected_meat" id="selected_meat" v-model="selected_meat">
                        <option v-for="meat in meats" :key="meat.id" :value="meat.value">{{meat.value}}</option>
                    </select>
                </div>

                <div id="optional-container" class="input-container">
                    <label id="optional-title" for="optional">Selecione os opcionais:</label>
                    <div v-for="optional in optionals" :key="optional.id" class="checkbox-container">
                        <input type="checkbox" name="selected_optional" v-model="selected_optional" :value="optional.value">
                        <span>{{ optional.value }}</span>
                    </div>
                </div>

                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Criar Burger">
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import Message from "@/components/Message.vue";

export default {
    name: 'BurgerForm',
    data() {
        return {
            client_name: null,

            breads: null, 
            meats: null,
            optionals: null,

            selected_bread: null,
            selected_meat: null,
            selected_optional: [],
            feedback_message: null,
        };
    },
    components: {
        Message
    },
    methods:{
        async getIngredientsData(){
            const req = await fetch("http://localhost:3000/ingredients");
            const data = await req.json();

            this.breads = data.breads;
            this.meats = data.meats;
            this.optionals = data.optionals;
        },
        async createBurger(e){
            e.preventDefault();

            // Get data
            const data = {
                client_name: this.client_name,
                bread: this.selected_bread,
                meat: this.selected_meat,
                optionals: Array.from(this.selected_optional),
                status: "Solicitado"
            }
            const dataJson = JSON.stringify(data);

            // Send data
            const req = await fetch("http://localhost:3000/burgers", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: dataJson
            });
            const res = await req.json();

            // Set feedback message
            this.feedback_message = `Pedido Nº ${res.id} criado com sucesso!`;
            setTimeout(() => this.feedback_message = "", 3000);

            // Clear data
            this.client_name = null;
            this.selected_bread = null;
            this.selected_meat = null;
            this.selected_optional = [];
        }
    },
    mounted(){
        this.getIngredientsData();
    }
}
</script>

<style scoped>
    #burger-form {
        max-width: 400px;
        margin: auto
    }

    .input-container {
        display: flex;
        flex-direction: column;
        margin-bottom: 20px;
    }

    label{
        font-weight: bold;
        margin-bottom: 15px;
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid #FCBA03;
    }

    input, select {
        padding: 5px 10px;
        width: 300px;
    }

    #optional-container{
        flex-direction: row;
        flex-wrap: wrap
    }

    #optional-title{
       width: 100%; 
    }

    .checkbox-container{
        display: flex;
        align-items: flex-start;
        width: 50%;
        margin-bottom: 20px;
    }

    .checkbox-container span, 
    .checkbox-container input{
        width: auto;
    }

    .checkbox-container span{
        margin-left: 6px;
        font-weight: bold;
    }

    .submit-btn{
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }

    .submit-btn:hover{
        background-color: transparent;
        color: #222;
    }
</style>