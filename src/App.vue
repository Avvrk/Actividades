<script setup>
import { ref } from "vue";
import Swal from "sweetalert2";

let actividad = ref("");
let fecha = ref("");
let check = ref("");
let arr = ref([]);

let fsDate;
let hoy = new Date();
hoy.setHours(0, 0, 0, 0);


function agg() {
  fsDate = new Date(fecha.value + "T00:00:00");

  if (actividad.value == "") {
    Swal.fire({
			text: "La actividad esta vacia",
			icon: "error",
		});
  } else if (fecha.value == "") {
    Swal.fire({
			text: "La fecha esta vacia",
			icon: "error",
		});
  } else if(fsDate < hoy) {
    Swal.fire({
			text: "Ingrese una fecha valida",
			icon: "warning",
		});
  } else {
    let checkk = check.value == true ? "Alta" : "Baja";

    arr.value.push({
      actividad: actividad.value,
      checkk,
      fecha: fecha.value
    })
    
    actividad.value = "";
    fecha.value = "";
  }
}

function ordenar() {
  arr.value.sort((a, b) => a.checkk.localeCompare(b.checkk));
}

function elim(i) {
  arr.value.splice(i, 1);
}
</script>

<template>
  <div>
    <section id="first">
      <input type="text" v-model="actividad">
      <input type="date" v-model="fecha">
    </section>
    <section id="second">
      <button id="agregar" @click="agg">+</button>
      <input type="checkbox" class="mycheck" v-model="check">
      <button id="ordenar" @click="ordenar">Ordenar</button>
    </section>
    <section id="third">
      <table>
        <thead>
          <tr>
            <th>Actividad</th>
            <th>Prioridad</th>
            <th>Fecha</th>
            <th>Opcion</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(e, i) in arr" :key="i"
            :style="e.checkk === 'Alta' ? { backgroundColor: 'crimson', color: 'white' } : { backgroundColor: 'aliceblue' }">
            <td>{{ e.actividad }}</td>
            <td>{{ e.checkk }}</td>
            <td>{{ e.fecha }}</td>
            <td>
              <button @click="elim(i)">🙅‍♂️</button>
            </td>
          </tr>
        </tbody>
      </table>
    </section>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
}

div {
  display: flex;
  flex-direction: column;
  background-color: azure;
  width: 70rem;
  height: 600px;
  padding: 30px 40px;
}

#first {
  display: flex;
  flex-direction: column;
}

#first input[type="text"] {
  width: 60%;
  padding: 15px 25px;
}

#first input[type="date"] {
  width: 15%;
  padding: 15px 25px;
}

#second {
  display: flex;
  flex-direction: row;
  justify-content: right;
  gap: 20px;
}

#second #agregar {
  padding: 3px 15px 8px 15px;
  font-size: 20px;
}

#second input[type="checkbox"] {
  cursor: pointer;
  background-color: #fff;
  color: #fff;
  background-image: url("data:image/svg+xml,%3csvg viewBox='0 0 16 16' fill='white' xmlns='http://www.w3.org/2000/svg'%3e%3cpath d='M12.207 4.793a1 1 0 010 1.414l-5 5a1 1 0 01-1.414 0l-2-2a1 1 0 011.414-1.414L6.5 9.086l4.293-4.293a1 1 0 011.414 0z'/%3e%3c/svg%3e");
  width: 24px;
  height: 24px;
  appearance: none;
  border: 2px solid #888;
  background-position: 0 -2rem;
  background-size: 100%;
  background-repeat: no-repeat;
  transition: all 0.3s ease-in-out;
}

#second input[type="checkbox"]:checked {
  background-color: rgb(75, 156, 13);
  color: rgb(75, 156, 13);
  background-position: 0 0;
}

#second input[type="checkbox"] {
  position: relative;
  top: 10px
}

#second #ordenar {
  padding: 5px 15px 10px 15px;
  font-size: 20px;
}

#third {
  margin-top: 40px;
  color: black;
  max-height: 400px;
  overflow-y: scroll;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th {
  width: 60px;
  background-color: aqua;
}

tbody button {
  background-color: transparent;
}
</style>
