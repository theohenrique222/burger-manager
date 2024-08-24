<template>
  <div id="burger-table" class="content">
    <Message :msg="msg" v-show="msg"/>
    <div>
      <div id="burger-table-heading">
        <div class="order-id">Nº</div>
        <div>Cliente</div>
        <div>Pão</div>
        <div>Proteina</div>
        <div>Opcionais</div>
        <div>Ações</div>
      </div>
    </div>
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
        <div class="order-number">{{ burger.id }}</div>
        <div>{{ burger.name }}</div>
        <div>{{ burger.pao }}</div>
        <div>{{ burger.proteina }}</div>
        <div>
          <ul>
            <li v-for="(opcional, index) in burger.opcional" :key="index" >
                {{ opcional }}
            </li>
          </ul>
        </div>
        <div>
          <select name="status" class="status" @change="updateBurger($event, burger.id)">
            <option>Selecione</option>
            <option class="select-row" :value="state.tipo" v-for="state in status" :key="state.id" :selected="burger.status == state.tipo">
                {{ state.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar</button>
        </div>
      </div>
    </div>
  </div>
  <!-- <div v-else>
    <h2>Não há pedidos no momento!</h2>
  </div> -->
</template>

<script>

import Message from "./Message.vue";

export default {
    name: "Dashboard",
    data() {
        return {
            burgers: null,
            burger_id: null,
            status: [],
            msg: null
        }
    },
    components: {
        Message
    },
    methods: {
        async getPedidos() {
            const req = await fetch("http://localhost:3000/burgers")

            const data = await req.json();

            this.burgers = data;
            console.log(this.burgers)

            // resgatar status
            this.getStatus();
        },
        async getStatus() {
            const req = await fetch("http://localhost:3000/status");

            const data = await req.json();

            this.status = data;
        },
        async deleteBurger(id) {
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "DELETE"
            });
            const res = await req.json();

            //msg
            this.msg = `Pedido Nº: ${res.id} removido com sucesso!`

            setTimeout(()=> this.msg = "", 3000);

            this.getPedidos();

        },
        async updateBurger(event, id){
            const option = event.target.value;
            const dataJson = JSON.stringify({ status: option });
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "PATCH",
                headers: { "Content-type": "aplication/json" },
                body: dataJson
            });
            const res = await req.json();

            this.msg = `Pedido Nº: ${res.id} foi atualizado para ${res.status}!`

            setTimeout(()=> this.msg = "", 3000);

            console.log(res)
        }

    },
    mounted() {
        this.getPedidos();
    }
}
</script>

<style scoped>

    #burger-table {
        max-width: 1200px;
        margin: 0 auto;
    }
    #burger-table-heading,
    #burger-table-rows,
    .burger-table-row {
        display: flex;
        flex-wrap: wrap;
    }
    #burger-table-heading {
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #333;
    }
    #burger-table-heading div,
    .burger-table-row div {
        width: 19%;
    }
    .burger-table-row {
        width: 100%;
        padding: 12px;
        border-bottom: 1px solid #CCC;
    }
    #burger-table-heading .order-id,
    .burger-table-row .order-number {
        width: 5%;
    }
    select {
        padding: 12px 6px;
        margin-right: 12px;
    }
    .delete-btn {
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px 5px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }
    .delete-btn:hover {
        background: transparent;

    }
    @media (max-width: 400px) {
        
        div {
            font-size: .5rem;
            
        }
        .delete-btn {
            font-size: .5rem;
            padding: 2px 1px;
            width: 100%;
        }
        .status {
            padding: 2px 1px;
            margin-bottom: 2px;
        }
        .select-row {
            background-color: red;
            padding: 1px 1px;
        }
    }
</style>