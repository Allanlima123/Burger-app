<template>
  <!-- <Message :msg="msg"/> -->

  <section>
    <form id="burger--form" @submit.prevent="createBurger">
      <div class="input--container">
        <label for="name">Nome do Cliente: </label>
        <input
          type="text"
          name="name"
          id="name"
          v-model="name"
          placeholder="Digiet o seu Nome"
        />
      </div>

      <div class="input--container">
        <label for="bread">Escolha o Pão: </label>
        <select name="bread" id="bread" v-model="bread">
          <option hidden disabled selected value="">Selecione o seu Pão</option>

          <option
            v-for="{ id, tipo } in paes"
            :value="tipo"
            :key="id"
            :name="tipo"
          >
            {{ tipo }}
          </option>
        </select>
      </div>

      <div class="input--container">
        <label for="meat">Escolha sua Carne: </label>
        <select name="meat" id="meat" v-model="meat">
          <option selected hidden disabled value=" ">Selecione a Carne</option>
          <option
            v-for="{ id, tipo } in carnes"
            :key="id"
            :value="tipo"
            :name="tipo"
          >
            {{ tipo }}
          </option>
        </select>
      </div>

      <div class="input--container">
        <label id="optional--title">Selecione os Opcionais: </label>
        <fieldset class="optional--groups">
          <div
            class="checkbox-container"
            v-for="{ id, tipo } in opcionaisdata"
            :key="id"
          >
            <input
              type="checkbox"
              name="opcionais"
              v-model="opcionais"
              :value="tipo"
            />
            <span>{{ tipo }}</span>
          </div>
        </fieldset>
      </div>

      <input type="submit" value="Crair meu Burger!" class="submit--btn" />
    </form>
  </section>
</template>

<script>
// import Message from "./Message.vue";
import swal from "sweetalert";
const axios = require("axios")

export default {
  name: "BurgerForm",

  data() {
    return {
      paes: null,
      carnes: null,
      opcionaisdata: null,
      name: null,
      bread: null,
      meat: null,
      opcionais: [],
      // msg: null,
    };
  },
  
  methods: {
    async getIngredientes() {
      // const data = await (await fetch("http://localhost:3000/ingredientes")).json();
      const dataResult = await axios({
        method : 'GET',
        url : "http://localhost:3000/ingredientes",
      });

      const { paes, carnes, opcionais } = dataResult.data
      
      this.paes = paes;
      this.carnes = carnes;
      this.opcionaisdata = opcionais;
    },

    async createBurger() {
      const data = {
        name: this.name,
        meat: this.meat,
        bread: this.bread,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado",
      };

      const dataJson = JSON.stringify(data);

      const req = axios({
        method : 'POST',
        headers : { "Content-type": "application/json" },
        url : "http://localhost:3000/burgers",
        data : dataJson
      }) 
      // const req = await fetch("http://localhost:3000/burgers", {
      //   method: "POST",
      //   headers: { "Content-type": "application/json" },
      //   body: dataJson,
      // });

      const res = await req;

      swal({
        title: `Pedido N° ${res.data.id}`,
        text: `Realizado com sucesso`,
        icon: "success",
        button: false,
        timer: 3000,
      });
      // this.msg = `Pedido N° ${res.id} realizado com sucesso`;

      // setTimeout(() => (this.msg = ""), 3000);

      this.name = "";
      this.meat = "";
      this.bread = "";
      this.opcionais = [];
    }
  },

  mounted() {
      this.getIngredientes();
  },

  // components: {
  //   Message,
  // },
};
</script>


<style scoped>
#burger--form {
  max-width: 400px;
  margin: 0 auto;
}

.input--container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

.input--container label {
  font-weight: bold;
  color: #222;
  margin-bottom: 15px;
  padding: 5px 10px;
  border-left: 4px solid #fcda03;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
}

.optional--groups {
  display: flex;
  flex-wrap: wrap;
  border: none;
}

#optional--title {
  width: 100%;
}

.optional--groups .checkbox-container {
  flex: 1 1 49%;
  margin: 0 0 20px 1px;
  display: flex;
  align-items: center;
}

.optional--groups .checkbox-container input,
.optional--groups .checkbox-container span {
  width: auto;
}

.optional--groups .checkbox-container span {
  font-weight: bold;
  margin-left: 8px;
}

.submit--btn {
  background: #222;
  color: #fcda03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
  border-radius: 5px;
}

.submit--btn:hover {
  background: transparent;
  color: #222;
  border-radius: 5px;
}
</style>