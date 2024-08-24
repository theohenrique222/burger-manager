<template>
  <div>
    <Message :msg="msg" v-show="msg"/>
    <div>
        <form id="burger-form" @submit="createBurger">
            <div class="input-container">
                <label for="name">Nome</label>
                <input type="text" id="name" name="name" v-model="name" placeholder="Digite seu nome">
            </div>

            <div class="input-container">
                <label for="pao">Escolha o pão</label>
                <select name="pao" id="pao" v-model="pao">
                    <option>Selecione o seu pão</option>
                    <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo }}</option>
                </select>
            </div>

            <div class="input-container">
                <label for="proteina">Escolha a Proteina</label>
                <select name="proteina" id="proteina" v-model="proteina">
                    <option>Selecione a proteina</option>
                    <option v-for="proteina in proteinas" :key="proteina.id" :value="proteina.tipo">{{ proteina.tipo }}</option>
                </select>
            </div>

            <div id="opicionais-container" class="input-container">
                <label id="opicionais-title" for="opicionais">Selecione os opicionais</label>
                <div class="checkbox-container" v-for="opcional in opcionaisData" :key="opcional.id">
                    <input type="checkbox" name="opicionais" id="opicionais" v-model="opcionais" :value="opcional.tipo">
                    <span>{{ opcional.tipo }}</span>
                </div>
            </div>
            <div class="input-container">
                <input type="submit" class="submit-btn" value="Criar meu Burger">
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
            nome: null,
            pao: null,
            proteina: null,
            opcionais: [],
            status: "solicitado",
            msg: null,
        }
    },
    methods: {
        async getIngredientes() {
            const req = await fetch("http://localhost:3000/ingredientes");
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
                status: "Solicitado"
            }
            const dataJson = JSON.stringify(data);
            const req = await fetch("http://localhost:3000/burgers", {
                method: "POST",
                headers: { "Content-Type" : "aplication/json" },
                body: dataJson
            });
            const res = await req.json();
            // Inserir mensagem no sistema
            this.msg = `Pedido Nº ${res.id} realizado com sucesso!`;
            // limpar msg
            setTimeout(()=> this.msg = "", 3000)
            // Limpar os campos
            this.name = "";
            this.proteina = "";
            this.pao = "";
            this.opcionais = "";
        }
    },
    mounted(){
        this.getIngredientes()
    },
    components: {
        Message
    }
}
</script>

<style scoped>
    #burger-form {
        max-width: 400px;
        margin: 0 auto;
    }
    .input-container {
        display: flex;
        flex-direction: column;
        margin-bottom: 20px;
    }
    label {
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
    #opicionais-container {
        flex-direction: row;
        flex-wrap: wrap;
    }
    #opicionais-title {
        width: 100%;
    }
    .checkbox-container {
        display: flex;
        /* align-items: flex-start; */
        width: 50%;
        margin-bottom: 20px;
        align-items: center;
    }
    .checkbox-container span,
    .checkbox-container input {
        width: auto;
    }
    .checkbox-container span {
        margin-left: 6px;
        font-weight: bold;
    }
    .submit-btn {
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
    .submit-btn:hover {
        background-color: transparent;
        color: #222;
    }

</style>