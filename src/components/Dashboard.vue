<template>
    <div class="burger-table">
        <Message :message="feedback_message" v-show="feedback_message" />

        <div>
            <div id="burger-table-heading">
                <div class="order-id">#</div>
                <div>Cliente</div>
                <div>Pão</div>
                <div>Carne</div>
                <div>Opcionais</div>
                <div>Ações</div>
            </div>
        </div>

        <div id="burger-table-rows">
            <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
                <div class="order-number">{{ burger.id }}</div>
                <div>{{ burger.client_name }}</div>
                <div>{{ burger.bread }}</div>
                <div>{{ burger.meat }}</div>
                <div>
                    <ul>
                        <li v-for="(optional, index) in burger.optionals" :key="index">
                            {{ optional }}
                        </li>
                    </ul>
                </div>
                <div>
                    <select name="status" class="status" @change="updateBurger($event, burger.id)">
                        <option :value="status_row.value" v-for="status_row in status" :key="status_row.id" :selected="burger.status == status_row.value">
                            {{ status_row.value }}
                        </option>
                    </select>
                    <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar</button>
                </div>
            </div>
        
        </div>
    </div>
</template>

<script>
import Message from "@/components/Message.vue";

export default {
    name: 'Dashboard',
    data() {
        return {
            burgers: null,
            burger_id: null,
            status: [],
            feedback_message: null
        };
    },
    components: {
        Message
    },
    methods: {
        async getOrders(){
            // Get burgers
            const req_burgers = await fetch("http://localhost:3000/burgers");
            const burger_data = await req_burgers.json();
            this.burgers = burger_data;
            
            // Get status
            const req_status = await fetch("http://localhost:3000/status");
            const status_data = await req_status.json();
            this.status = status_data;
        }, 
        async deleteBurger(burger_id){
            const req = await fetch(`http://localhost:3000/burgers/${burger_id}`, {
                method: 'DELETE'
            });
            const res = await req.json();

            this.getOrders();

            // Set feedback message
            this.feedback_message = `Pedido Nº ${burger_id} removido com sucesso!`;
            setTimeout(() => this.feedback_message = "", 3000);
        },
        async updateBurger(event, id){
            const option = event.target.value;
            const dataJson = JSON.stringify({
                status: option
            });

            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: 'PATCH',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: dataJson
            });
            const res = await req.json();
            this.getOrders();

            // Set feedback message
            this.feedback_message = `Pedido Nº ${res.id} foi atualizado para ${res.status}!`;
            setTimeout(() => this.feedback_message = "", 3000);
        }
    },
    mounted(){
        this.getOrders();
    }
}
</script>

<style scoped>
    #burger-table{
        max-width: 1200px;
        margin: 0 auto;
    }

    #burger-table-heading,
    #burger-table-rows,
    .burger-table-row{
        display: flex;
        flex-wrap: wrap;
    }

    #burger-table-heading{
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #333;
    }

    #burger-table-heading div,
    .burger-table-row div{
        width: 19%;
    }

    .burger-table-row{
        width: 100%;
        padding: 12px;
        border-bottom: 1px solid #ccc;
    }

    #burger-table-heading .order-id,
    .burger-table-row .order-number{
        width: 5%;
    }

    select {
        padding: 12px 6px;
        margin-right: 12px;
    }

    .delete-btn{
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

    .delete-btn:hover{
        background-color: transparent;
        color: #222;
    }
</style>