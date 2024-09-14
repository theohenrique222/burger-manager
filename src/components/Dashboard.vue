<template>
    <div class="max-w-6xl mx-auto p-4 bg-white shadow-xl rounded-md">
        <h1 class="lg:text-2xl text-base font-bold uppercase mb-6 text-center shadow-md shadow-neutral-800 bg-gray-800 text-white py-4 px-6">
          Gerenciador de pedidos
        </h1>
        <Message :msg="msg" v-show="msg" />
      <div class="mb-4">
        <div class="flex font-bold lg:text-base text-xs text-center border-b-2 border-gray-800 pb-2">
          <div class="flex-1">Nº</div>
          <div class="flex-1">Cliente</div>
          <div class="flex-1">Pão</div>
          <div class="flex-1">Proteina</div>
          <div class="flex-1">Opcionais</div>
          <div class="flex-1">Ações</div>
        </div>
      </div>
      <div class="rounded-md text-center">
        <div
          class="flex items-center lg:text-base text-[.5rem] text-center border-b-2 border-gray-300 p-4"
          v-for="burger in burgers"
          :key="burger.id"
        >
          <div class="flex-1">{{ burger.id }}</div>
          <div class="flex-1">{{ burger.name }}</div>
          <div class="flex-1">{{ burger.pao }}</div>
          <div class="flex-1">{{ burger.proteina }}</div>
          <div class="flex-1">
            <ul class="list-inside text-start">
              <li v-for="(opcional, index) in burger.opcional" :key="index">
                {{ opcional }}
              </li>
            </ul>
          </div>
          <div class="flex-1 lg:flex items-center justify-center lg:space-x-2 space-y-1">
            <select
              name="status"
              class="px-2 py-2 border border-gray-300 rounded-md shadow-sm"
              @change="updateBurger($event, burger.id)"
            >
              <option value="" disabled selected hidden>Selecione</option>
              <option
                v-for="state in status"
                :key="state.id"
                :value="state.tipo"
                :selected="burger.status == state.tipo"
              >
                {{ state.tipo }}
              </option>
            </select>
            <button
              class="bg-gray-800 text-yellow-400 font-bold border-2 border-gray-800 py-2 px-2 rounded-md cursor-pointer transition-colors duration-500 hover:bg-transparent hover:text-gray-800"
              @click="deleteBurger(burger.id)"
            >
              Cancelar
            </button>
          </div>
        </div>
      </div>
    </div>
  </template>

  

<script>
import Message from "./Message.vue";
import axios from 'axios';

axios.defaults.baseURL = 'https://json-serve-zeta.vercel.app';

export default {
    name: "Dashboard",
    data() {
        return {
            burgers: null,
            burger_id: null,
            status: [],
            msg: null
        };
    },
    components: {
        Message
    },
    methods: {
        async getPedidos() {
            try {
                const response = await axios.get('/burgers');
                this.burgers = response.data;
                console.log('Burgers:', this.burgers);

                // resgatar status
                this.getStatus();
            } catch (error) {
                console.error('Erro ao obter pedidos:', error.message);
            }
        },
        async getStatus() {
            try {
                const response = await axios.get('/status');
                this.status = response.data;
                console.log('Status:', this.status);
            } catch (error) {
                console.error('Erro ao obter status:', error.message);
            }
        },
        async deleteBurger(id) {
            try {
                const response = await axios.delete(`/ingredientes/${id}`);
                this.msg = `Pedido Nº: ${response.data.id} removido com sucesso!`;
                console.log('Delete response:', response.data);

                setTimeout(() => this.msg = "", 3000);

                this.getPedidos();
            } catch (error) {
                console.error('Erro ao deletar pedido:', error.message);
            }
        },
        async updateBurger(event, id) {
            const option = event.target.value;
            const dataJson = { status: option };
            try {
                const response = await axios.patch(`/burgers/${id}`, dataJson, {
                    headers: { "Content-Type": "application/json" }
                });
                this.msg = `Pedido Nº: ${response.data.id} foi atualizado para ${response.data.status}!`;
                console.log('Update response:', response.data);

                setTimeout(() => this.msg = "", 3000);
            } catch (error) {
                console.error('Erro ao atualizar pedido:', error.message);
            }
        }
    },
    mounted() {
        this.getPedidos();
    }
}
</script>
