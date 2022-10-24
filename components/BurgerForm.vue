<template>
    <div>
        <Message 
          :msg="msg"
          v-show="msg"
        />
        <div>
            <form id="burger-form" @submit.prevent="createBurger()">
                <div class="input-container">
                    <label for="nome">Clients Name: </label>
                    <input type="text" id="nome" name="nome" v-model="nome" placeholder="Your name...">
                </div>
                <div class="input-container">
                    <label for="pao">Choose your bread: </label>
                    <select name="pao" id="pao" v-model="pao">
                        <option value="" disabled>Select Your Bread</option>
                        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
                          {{ pao.tipo }}
                        </option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="meat">Choose your Meat: </label>
                    <select name="meat" id="meat" v-model="carne">
                        <option value="" disabled>Select Your Meat Type</option>
                        <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
                          {{ carne.tipo }}
                        </option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="pao">Select the optionals: </label>
                    
                    <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
                        <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
                        <span>{{ opcional.tipo }}</span>
                    </div>
                </div>

                <div class="input-container">
                    <button  class="submit-btn">Make my Burguer</button>
                </div>
            </form>
        </div>
    </div>
</template>

<script>
    import Message from './Message.vue'

    export default {
      name: "BurgerForm",
      data() {
        return {
          paes: null,
          carnes: null,
          opcionaisdata: null,
          nome: null,
          carne: null,
          opcionais: [],
          status: "Solicitado",
          msg: null,
        }
      },

      methods: {
        async getIngredients() {
          const req = await fetch("http://localhost:3000/ingredientes");
          const data = await req.json()

          console.log(data)
          this.paes = data.paes
          this.opcionaisdata = data.opcionais
          this.carnes = data.carnes
          
        },

        async createBurger() {
          const data = {
            nome: this.nome,
            carne: this.carne,
            pao: this.pao,
            opcionais: Array.from(this.opcionais),
            status: this.status
          }

          const dataJSON = JSON.stringify(data)

          const req = await fetch("http://localhost:3000/burgers", {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: dataJSON
          })

          const res = await req.json()

          this.msg = `Request nº ${res.id} Made Successfully`

          setTimeout(() => this.msg = "", 3000)
          
          this.nome = ""
          this.carne = ""
          this.pao = ""
        }
      },
      mounted() {
        this.getIngredients()
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
    color: #222;;
    padding: 5px 10px;
    border-left: 4px solid #fcba03;
  }
  input, select {
    padding: 15px 10px;
    width: 300px;
    outline: none;
    border: 1px solid #CCC;
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
    color:#fcba03;
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