<template>
  <div>
    <div v-if="bloque1">
      <section class="register" id="register">
        <h1>Crear Talonario</h1>
        <h4>Premio</h4>
        <input
          type="text"
          class="controls"
          placeholder="Premio..."
          v-model.trim="premio"
        />
        <h4>Valor boleta</h4>
        <input
          type="text"
          class="controls"
          placeholder="Valor boleta..."
          v-model.trim="valorb"
        />
        <h4>Loteria</h4>
        <select name="loteria" class="selects" v-model.trim="tipodeloteria">
          <option value="laperla">La perla</option>
          <option value="eldorado">El dorado</option>
          <option value="santander">Santander</option>
        </select>
        <h4>Numero de boletas</h4>
        <select name="numboletas" class="selects" v-model.trim="numboletas">
          <option value="50">50</option>
          <option value="75">75</option>
          <option value="100">100</option>
        </select>
        <h4>Ingrese la fecha del sorteo</h4>
        <input
          type="date"
          class="controls"
          placeholder="Fecha..."
          v-model.trim="date"
        />
        <button class="Registrar" @click="Registrar()">Crear</button>
      </section>
    </div>
    <div class="container" v-else>
      <div class="con1">
        <section>
          <h1>INFORMACION</h1>
          <div class="margib">
            <img src="/taza.png" alt="imagen" class="imgs"/>
            <div class="texto">Tu texto aquí</div>
          </div>
        </section>
      </div>
      <div class="con2">
        <section>
          <h1>TALONARIO</h1>
          <div v-if="numboletas && !bloque1">
            <button
              v-for="n in parseInt(numboletas)"
              :key="n"
              class="boleta-button"
              @click="handleBoletaClick(n)"
            >
              {{ ` ${n}` }}
            </button>
          </div>
        </section>
      </div>
      <div class="con3">
        <section>
          <h1>ACCIONES</h1>
          <div class="margib">
            <h1>Estado</h1>
          </div>
        </section>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

let premio = ref("");
let valorb = ref("");
let tipodeloteria = ref("");
let numboletas = ref("");
let date = ref("");
let registros = ref([]);
const bloque1 = ref(true);

function Registrar() {
  const Registros = {
    premio: premio.value,
    valorb: valorb.value,
    tipodeloteria: tipodeloteria.value,
    numboletas: numboletas.value,
    date: date.value,
  };
  registros.value.push(Registros);
  console.log(registros);
  bloque1.value = false;
}

function handleBoletaClick(boletaNumber) {
  // Lógica que se ejecutará cuando se haga clic en una boleta.
  console.log(`Clic en Boleta ${boletaNumber}`);
  // Puedes agregar más lógica aquí según tus necesidades.
}
</script>

<style>
body {
  background-color: #8b8282;
}

.register {
  width: 20%;
  background: #6584a2;
  padding: 30px;
  margin: auto;
  margin-top: 30px;
  margin-bottom: 30px;
  border-radius: 4px;
  font-family: "calibri";
  color: white;
  box-shadow: 7px 13px 37px #000;
  text-align: center;
}

.controls {
  width: 100%;
  background: #a2adb7;
  padding: 10px;
  border-radius: 4px;
  border: 1px solid #1f53c5;
  font-family: "calibri";
  font-size: 18px;
  color: white;
}

.h4 {
  margin: 2px;
}

.selects {
  width: 100%;
  background: #a2adb7;
  border: 1px solid #1f53c5;
  border-radius: 4px;
  padding: 10px;
  color: white;
  font-size: 16px;
}

.Registrar {
  background-color: #675a5a;
  margin-top: 12px;
  cursor: pointer;
  color: white;
  width: 30%;
  height: 20%;
}

h4 {
  margin-bottom: 3%;
  margin-top: 3%;
}

.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(4, auto);
}

.con1 {
  width: 250px;
  background: #6584a2;
  padding: 30px;
  margin: auto;
  margin-top: 30px;
  margin-bottom: 30px;
  border-radius: 4px;
  font-family: "calibri";
  color: white;
  box-shadow: 7px 13px 20px #000;
  text-align: center;
}

.con2 {
  width: 550px;
  background: #6584a2;
  padding: 30px;
  margin: auto;
  margin-top: 30px;
  margin-bottom: 30px;
  border-radius: 4px;
  font-family: "calibri";
  color: white;
  box-shadow: 7px 13px 20px #000;
  text-align: center;
  overflow: auto;
}

.con3 {
  width: 250px;
  background: #6584a2;
  padding: 30px;
  margin: auto;
  margin-top: 30px;
  margin-bottom: 30px;
  border-radius: 4px;
  font-family: "calibri";
  color: white;
  box-shadow: 7px 13px 20px #000;
  text-align: center;
}

.margib {
  border: 2px solid #333;
  border-radius: 10px;
}

.boleta-button {
  border-radius: 20px;
  margin: 5px;
  width: 40px;
  height: 40px;
}

.imgs{
  width: 30px;
  height: 30px;
}



</style>
