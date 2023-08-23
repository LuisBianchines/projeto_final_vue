<template>
    <message :msg="msg" v-show="msg"/> 
    <div id="burger-table">
        <div>
            <div id="burger-table-heading">
                <div class="order-id">#:</div>
                <div>Cliente:</div>
                <div>Pão:</div>
                <div>Carne:</div>
                <div>Opcionais:</div>
                <div>Ações:</div>
            </div>
        </div>
        <div id="burger-table-rows">
                <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
                    <div class="order-number">{{ burger.id }}</div>
                    <div>{{ burger.nome }}</div>
                    <div>{{ burger.pao }}</div>
                    <div>{{ burger.carne }}</div>
                    <div>
                         <ul v-for="(opcional, index) in burger.opcionais" :key="index">
                             <li>{{ opcional }}</li>
                         </ul>
                    </div> 
                    <div>
                        <select name="status" id="opt" class="status" @change="updateStatus($event, burger.id)">
                            <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="burger.status == s.tipo">
                              {{ s.tipo }}
                            </option>
                        </select>
                        <button class="delete-btn" @click="deleteBurger(burger.id) ">Cancelar</button>
                    </div>           
                </div> 
        </div>
    </div>
</template>

<script>
import message from "./Message.vue"
export default {
    name: "dashboard",
    components: {
      message
    },
    data(){
        return {
            burgers : null,
            burger_id : null,
            status: [],
            msg: null
        }
    },
    methods: {
        async getBurgers(){
            const req = await fetch("http://localhost:3000/burgers");
            const data = await req.json();
            
            this.burgers = data;

            this.getStatus();
        },
        async getStatus(){
            const req = await fetch("http://localhost:3000/status");
            const data = await req.json();

            this.status = data;
        },
        async deleteBurger(id){
          const req = await fetch(`http://localhost:3000/burgers/${id}`,{
          method: "DELETE"

          });
          const res = await req.json();
          
          this.msg = `Pedido nº ${id} excluído com sucesso`

          setTimeout(() => {(this.msg = "")}, 3000);

          this.getBurgers();
        },
        async updateStatus(event, id){
          const option = event.target.value;
          const dataJson = JSON.stringify({status : option});

          const req = await fetch(`http://localhost:3000/burgers/${id}`, {
             method: "PATCH",
             headers: {"Content-Type": "application/json"},
             body: dataJson
          });
          const res = await req.json();
          
          this.msg = `Pedido nº ${id} atualizado para o status ${event.target.value}`

          setTimeout(() => {(this.msg = "")}, 5000);
        }
    },
    mounted() {
        this.getBurgers();
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
    min-height: 100%;
  }

  #burger-table-heading{
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
    border-bottom: 1px solid #ccc;

  }

  #burger-table-heading .order-id,
  .burger-table-row .order-number{
    width: 5%;
  }

  .delete-btn {
    background-color: #222;
    color:#fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
    margin-left: 10px;
  }
  
  .delete-btn:hover {
    background-color: transparent;
    color: #222;
  }

  #opt {
    min-height: 40px;
    max-height: 40px;
  }

</style>