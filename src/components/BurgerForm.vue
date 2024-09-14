<template>
  <div class="bg-gray-100 min-h-screen p-4">
    <Message :msg="msg" v-show="msg" />
    <div class="max-w-md mx-auto">
      <h1
        class="text-2xl font-bold uppercase mb-6 text-center shadow-md shadow-neutral-800 bg-gray-900 text-white py-4 px-6"
      >
        Monte o seu hamburger
      </h1>
      <form
        id="burger-form"
        @submit="createBurger"
        class="bg-white p-6 shadow-md rounded-md"
      >
        <div class="mb-4">
          <label
            for="name"
            class="block font-bold mb-2 text-gray-800 border-l-4 border-yellow-400 pl-2"
          >
            Nome
          </label>
          <input
            type="text"
            id="name"
            name="name"
            v-model="name"
            placeholder="Digite seu nome"
            required
            class="w-full p-2 border border-gray-300 rounded-md shadow-sm"
          />
        </div>

        <div class="mb-4">
          <label
            for="pao"
            class="block font-bold mb-2 text-gray-800 border-l-4 border-yellow-400 pl-2"
          >
            Escolha o pão
          </label>
          <select
            name="pao"
            id="pao"
            v-model="pao"
            required
            class="w-full p-2 border border-gray-300 rounded-md shadow-sm"
          >
            <option>Selecione o seu pão</option>
            <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
              {{ pao.tipo }}
            </option>
          </select>
        </div>

        <div class="mb-5">
          <label
            for="proteina"
            class="block font-bold mb-2 text-gray-800 border-l-4 border-yellow-400 pl-2"
          >
            Escolha a Proteina
          </label>
          <select
            name="proteina"
            id="proteina"
            v-model="proteina"
            required
            class="w-full p-2 border border-gray-300 rounded-md shadow-sm"
          >
            <option value="">Selecione a proteina</option>
            <option
              v-for="proteina in proteinas"
              :key="proteina.id"
              :value="proteina.tipo"
            >
              {{ proteina.tipo }}
            </option>
          </select>
        </div>

        <div id="opicionais-container" class="mb-5">
          <label
            id="opicionais-title"
            for="opicionais"
            class="block font-bold mb-3 text-gray-800 border-l-4 border-yellow-400 pl-2"
          >
            Selecione os opcionais
          </label>
          <div class="grid grid-cols-2 gap-4">
            <div
              class="p-1 border-b-2 border-yellow-400"
              v-for="opcional in opcionaisData"
              :key="opcional.id"
            >
              <input
                type="checkbox"
                name="opicionais"
                id="opicionais"
                v-model="opcionais"
                :value="opcional.tipo"
                class="mr-2"
              />
              <span>{{ opcional.tipo }}</span>
            </div>
          </div>
        </div>

        <div class="text-center">
          <input
            type="submit"
            class="bg-neutral-900 text-yellow-400 font-bold border-2 border-neutral-900 py-2 px-4 rounded-md cursor-pointer transition duration-500 ease-in-out hover:bg-transparent hover:text-neutral-900"
            value="Criar meu Burger"
          />
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";
export default {
  name: "BurgerForm",
  data() {
    return {
      paes: null,
      proteinas: null,
      opcionaisData: null,
      name: null,
      pao: null,
      proteina: null,
      opcionais: [],
      status: "solicitado",
      msg: null,
    };
  },
  methods: {
    async getIngredientes() {
      const req = await fetch("https://json-serve-zeta.vercel.app/ingredientes");
      const data = await req.json();

      this.paes = data.paes;
      this.proteinas = data.proteinas;
      this.opcionaisData = data.opcionais;
    },
    async createBurger(e) {
      e.preventDefault();
      const data = {
        name: this.name,
        proteina: this.proteina,
        pao: this.pao,
        opcional: Array.from(this.opcionais),
        status: "Solicitado",
      };
      const dataJson = JSON.stringify(data);
      const req = await fetch("https://json-serve-zeta.vercel.app/burgers", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });
      const res = await req.json();

      // Inserir mensagem no sistema

      this.msg = `Pedido Nº ${res.id} realizado com sucesso!`;

      // limpar msg

      setTimeout(() => (this.msg = ""), 3000);

      // Limpar os campos
      this.name = "";
      this.proteina = "";
      this.pao = "";
      this.opcionais = [];

      this.$router.push("/solicitations");
    },
  },
  mounted() {
    this.getIngredientes();
  },
  components: {
    Message,
  },
};
</script>
