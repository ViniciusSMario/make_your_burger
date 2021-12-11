<template>
    <div>
        <h1>Monte o seu Burger:</h1>
        <Mensagem :msg="msg" v-show="msg"/>
        <form id="burger-form" @submit="createBurger">
            <div class="input-container">
                <label for="name">Nome do Cliente</label>
                <input type="text" id="name" v-model="name" name="name" placeholder="Digite Seu Nome">
            </div>
            <div class="input-container">
                <label for="pao">Escolha o Pão:</label>
                <select name="pao" id="pao" v-model="pao">
                    <option value="">Selecione Seu Pão</option>
                    <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{pao.tipo}}</option>
                </select>
            </div>
            <div class="input-container">
                <label for="carne">Escolha a carne:</label>
                <select name="carne" id="carne" v-model="carne">
                    <option value="">Selecione a Carne</option>
                    <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{carne.tipo}}</option>
                </select>
            </div>
            <div class="input-container optionals-container">
                <label id="optionals-title" for="opcionais">Selecione os opcionais:</label>
                <div class="checkbox-container" v-for="opcional in opcionaisdata"  :key="opcional.id">
                    <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
                    <span>{{opcional.tipo}}</span>
                </div>
            </div>
            <div class="input-container">
                <input class="submit_btn" type="submit" value="Criar meu Burger">
            </div>
        </form>
    </div>

</template>

<script>
import Mensagem from './Mensagem.vue'
export default {
    name: "BurgerForm",
    components:{
        Mensagem
    },
    data(){
        return{
            opcionaisdata: null,
            paes: null,
            carnes: null,
            nome: null,
            pao: null,
            carne: null,
            opcionais:[],
            msg: null
        }
    },
    methods:{
        async getIngredientes(){
            const req = await fetch("http://localhost:3000/ingredientes")
            const data = await req.json();

            this.paes = data.paes;
            this.carnes = data.carnes;
            this.opcionaisdata = data.opcionais;
        },

        async createBurger(e){
            e.preventDefault();

            const data = {
                nome: this.name,
                carne: this.carne,
                pao: this.pao,
                opcionais: Array.from(this.opcionais),
                status: "Solicitado"
            }

            const dataJson = JSON.stringify(data)

            const req = await fetch('http://localhost:3000/burgers', {
                method: "POST",
                headers: {"Content-Type": "application/json"},
                body: dataJson
            });

            const res = await req.json();

            //colocar msg de sistema
            this.msg = `Pedido Nº ${res.id} Realizado com sucesso!`

            //limpar msg
            setTimeout(() => this.msg = null, 3000)

            //limpar campos
            this.nome = "";
            this.carne = "";
            this.pao = "";
            this.opcionais = "";
        }
    },
    mounted(){
        this.getIngredientes()
    }
}
</script>

<style scoped>
    #burger-form{
        display: flex;
        justify-content: center;
        flex-direction: column;
        max-width: 400px;
        margin: 0 auto;
    }

    .input-container{
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

    input, select{
        padding: 5px 10px;
        width: 300px;
    }

    .optionals-container{
        flex-direction: row;
        flex-wrap: wrap;
    }

    #optionals-title{
        width: 100%;
    }

    .checkbox-container{
        display: flex;
        align-items: flex-start;
        width: 50%;
        margin-bottom: 30px;
    }

    .checkbox-container span,
    .checkbox-container input {
        width: auto;
    }

    .checkbox-container span{
        margin-left: 6px;
        font-weight: bold;
    }

    .submit_btn{
        background-color: #222;
        color: #FCBA03;
        padding: 10px;
        font-weight: bold;
        font-size: 16px;
        cursor: pointer;
        border: 2px solid #222;
        margin: 0 auto;
        transition: .5s;
    }

     .submit_btn:hover{
         background-color: transparent;
         color:#222 ;
     }
</style>