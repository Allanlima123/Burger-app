<template>
  <!-- <Message :msg="msg" v-show="msg" /> -->
  <section id="burger--table">
    <table>
      <thead id="burger--table--heading">
        <tr>
          <th class="order--id">#:</th>
          <th>Cliente:</th>
          <th>Paõ:</th>
          <th>Carne:</th>
          <th>Opcionais:</th>
          <th>Ações:</th>
        </tr>
      </thead>

      <tbody id="burger--table--rows">
        <tr
          class="burger--table-row"
          v-for="{ id, name, bread, meat, opcionais, status } in burgers"
          :key="id"
        >
          <td class="order--number">{{ id }}</td>
          <td>{{ name }}</td>
          <td>{{ bread }}</td>
          <td>{{ meat }}</td>
          <td>
            <ul>
              <li v-for="(opcional, index) in opcionais" :key="index">
                {{ opcional }}
              </li>
            </ul>
          </td>

          <td>
            <select
              name="status"
              class="status"
              @change="updateBurger($event, id)"
            >
              <option value="" selected hidden disabled>Selecione</option>
              <option
                v-for="{ tipo, id } in statusData.status"
                :value="tipo"
                :key="id"
                :selected="status == tipo"
                :name="tipo"
              >
                {{ tipo }}
              </option>
            </select>

            <button class="delete--btn" @click="deleteBurger(id)">
              Cancelar
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </section>
</template>

<script setup>
import { ref, reactive, onMounted } from "vue";
import axios from "axios";
import swal from "sweetalert";

const burgers = ref(null);
const statusData = reactive({
  status: [],
});

const getPedidos = async () => {
  const dataFetch = await axios({
    method: "GET",
    url: "http://localhost:3000/burgers",
  });

  const { data } = dataFetch;

  burgers.value = data;
  getStatus();
};

const getStatus = async () => {
  const statusFetch = await axios("http://localhost:3000/status");
  statusData.status = statusFetch.data;
};

const deleteBurger = async (id) => {
  await axios({
    method: "DELETE",
    url: `http://localhost:3000/burgers/${id}`,
  });

  swal({
    title: `Pedido N° ${id}`,
    text: `Removido com sucesso`,
    icon: "success",
    button: false,
    timer: 3000,
  });

  getPedidos();
};

const updateBurger = async (event, id) => {
  const option = event.target.value;

  const dataJson = JSON.stringify({ status: option });

  const res = await axios(`http://localhost:3000/burgers/${id}`, {
    method: "PATCH",
    headers: { "Content-Type": "application/json" },
    data: dataJson,
  });

  // const req = await fetch(`http://localhost:3000/burgers/${id}`, {
  //   method: "PATCH",
  //   headers: { "Content-Type": "application/json" },
  //   body: dataJson,
  // });

  const { data } = res;

  swal({
    title: `Pedido N° ${data.id}`,
    text: `Foi atualizado para ${data.status}`,
    icon: "success",
    button: false,
    timer: 3000,
  });
};

onMounted(() => {
  getPedidos();
});
// });

// export default {
//   name: "Dashboard",

//   data() {
//     return {
//       burgers: null,
//       burger_id: null,
//       status: [],
//       msg: null,
//     };
//   },

// methods: {
//   async getPedidos() {
//     const data = await (await fetch("http://localhost:3000/burgers")).json();

//     this.burgers = data;

//     this.getStatus();
//   },

// async getStatus() {
//   const data = await (await fetch("http://localhost:3000/status")).json();

//   this.status = data;
// },

// async deleteBurger(id) {
//   const req = await fetch(`http://localhost:3000/burgers/${id}`, {
//     method: "DELETE",
//   });

//   const res = await req.json();

//   this.msg = `Pedido removido com sucesso`;

//   setTimeout(() => (this.msg = ""), 3000);

//   this.getPedidos();
// },

//   async updateBurger(event, id) {
//     const option = event.target.value;

//     const dataJson = JSON.stringify({ status: option });

//     const req = await fetch(`http://localhost:3000/burgers/${id}`, {
//       method: "PATCH",
//       headers: { "Content-Type": "application/json" },
//       body: dataJson,
//     });

//     const res = await req.json();

//     this.msg = `Pedido N° ${res.id} foi atualizado para ${res.status}`;

//     setTimeout(() => (this.msg = ""), 2000);
//   },
// },

//   onmounted() {
//     this.getPedidos();
//   },

//   components: {
//     Message,
//   },
// };
</script>

<style scoped>
#burger--table {
  max-width: 1440px;
  margin: 0 auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

#burger--table table #burger--table--heading {
  border-bottom: 2px solid #222;
}

#burger--table table #burger--table--heading tr th {
  width: 120px;
}

#burger--table table #burger--table--heading tr th,
#burger--table table #burger--table--rows tr td {
  text-align: left;
  padding: 15px 0 15px 10px;
}

#burger--table table #burger--table--rows tr {
  border-bottom: 3px solid rgba(0, 0, 0, 0.1);
}

#burger--table table #burger--table--rows tr td {
  padding-top: 0;
  color: rgba(0, 0, 0, 0.8);
  font-size: 15px;
}

#burger--table table #burger--table--rows tr td ul {
  margin-top: 5px;
}

#burger--table table #burger--table--rows tr td ul li {
  margin-top: 5px;
}

#burger--table table #burger--table--rows tr td .status {
  width: 150px;
  padding: 15px 0;
  text-align: center;
  font-weight: bold;
  border-radius: 5px;
}

.delete--btn {
  background: #222;
  color: #fcda03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin-left: 10px;
  cursor: pointer;
  transition: 0.5s;
  margin-top: 15px;
}

.delete--btn:hover {
  background: transparent;
  color: #222;
  border-radius: 5px;
}

.order--number,
.order--id {
  width: 5%;
}
</style>