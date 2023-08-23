<template>
    <message :msg="msg" v-show="msg"/>
<div>    
    <form id="burger-form" @submit="createBurger">
    <div class="input-container">
      <label for="nome">Nome do Cliente:</label>
      <input v-if="mudarcor" style="background-color:  salmon ;" type="text" name="nome" id="nome" v-model="nome" placeholder="Digite o seu nome" />
      <input v-else type="text" name="nome" id="nome" v-model="nome" placeholder="Digite o seu nome"  />  
    </div>
    <div class="input-container">
      <label for="pao">Escolha o pão:</label>
      <select v-if="mudarcor" style="background-color:  salmon ;" name="pao" id="pao" v-model="pao">
        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
          {{ pao.tipo }}
        </option>
      </select>
      <select v-else name="pao" id="pao" v-model="pao">
        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
          {{ pao.tipo }}
        </option>
      </select>
    </div>
    <div class="input-container">
      <label for="carne">Escolha a carne:</label>
      <select v-if="mudarcor" style="background-color:  salmon ;" name="carne" id="carne" v-model="carne">
        <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
          {{ carne.tipo }}
        </option>
      </select>
      <select v-else name="carne" id="carne" v-model="carne">
        <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
          {{ carne.tipo }}
        </option>
      </select>
    </div>
    <div class="input-container" id="opcionais-container">
      <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
      <div class="checkbox-container" v-for="opcionaisvalue in opcionaisdata" :key="opcionaisvalue.id">
        <input  type="checkbox" name="opcionais" v-model="opcionais" :value="opcionaisvalue.tipo"/>
        <span>{{ opcionaisvalue.tipo }}</span>
      </div>
    </div>
    <div class="input-container">
      <input type="submit" class="submit-btn" value="Criar meu Burger" />
    </div>
  </form>
</div>
</template>

<script>
import message from "./Message.vue"
export default {
  name: "BurgerForm",
  components: {
    message
  },
  
  data() {
    return {
      paes: null,
      carnes: null,
      opcionaisdata: null,
      nome: null,
      pao: null,
      carne: null,
      opcionais: [],
      msg: null,
      mudarcor: false
    };
  },
  methods: {
    async getIngredientes() {
      const req = await fetch("http://localhost:3000/ingredientes");
      const data = await req.json();

      this.paes = data.paes;
      this.carnes = data.carnes;
      this.opcionaisdata = data.opcionais;
    },
    async createBurger(e){
        
        e.preventDefault();
        
        if(this.nome === null || this.pao === null || this.carne === null){
            this.mudarcor = true
            this.msg = "Preencha todos os campos obrigatórios"

            setTimeout(() => {(this.msg = ""); (this.mudarcor = false)}, 3000);

        }else{
        const data = {
            nome: this.nome,
            pao: this.pao,
            carne: this.carne,
            opcionais: Array.from(this.opcionais),
            status: "Solicitado"    
        }

        const dataJson = JSON.stringify(data);

        const req = await fetch("http://localhost:3000/burgers", {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: dataJson
        });

        const res = await req.json();

        console.log(res);

        //Limpar Campos
        this.nome = null
        this.pao = null
        this.carne = null
        this.opcionais = null

        this.msg = "Pedido Nº " + res.id + " realizado com sucesso"

        setTimeout(() => this.msg = "", 3000);
    }
        
    }
  },
  mounted() {
    this.getIngredientes();
  },
};
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
  border-left: 4px solid #fcba03;
}

input,
select {
  flex-direction: row;
  flex-wrap: wrap;
}

#opcionais-container {
    flex-direction: row;
    flex-wrap: wrap;
}
#opcionais-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
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
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.submit-btn:hover {
  background-color: transparent;
  color: #222;
}

#nome,
#pao,
#carne {
  min-height: 50px;
  font-size: 20px;
}
</style>