<template>
  <div class="talonario">
    <section class="register" v-if="RegistroTalonario">
      <div :class="Alerta()">
        <div class="cont_alerta">
          <p class="text_alerta">{{ alerta }}</p>
        </div>
      </div>
      <h1>Talonario</h1>
      <h4>Premio</h4>
      <input
        type="text"
        placeholder="Premio"
        v-model="premio"
        class="controls"
      />
      <h4>Valor boleta</h4>
      <input
        type="text"
        placeholder="Valor boleta"
        v-model="Precio"
        class="controls"
      />
      <h4>Loteria</h4>
      <select v-model="selectedLoteria" class="selects">
        <option value="Santander">Santander</option>
        <option value="Bogota">Bogota</option>
        <option value="Dorado">Dorado</option>
      </select>
      <h4>Cantidad de boletas</h4>
      <select v-model="selectedCantidad" class="selects">
        <option value="50">50</option>
        <option value="75">75</option>
        <option value="100">100</option>
      </select>
      <h4>Fecha del sorteo</h4>
      <input type="date" v-model="fecha" class="controls" />
      <button class="custom-button2" @click="crearTalonario()">Crear</button>
    </section>
    <section class="register" v-if="RegistrarDueño">
      <h4>Registro de boleta</h4>

      <input
        type="text"
        placeholder="Nombre"
        v-model="nombre"
        class="controls"
      />

      <input
        type="text"
        placeholder="Telefono"
        v-model="telefono"
        class="controls"
      />

      <input
        type="text"
        placeholder="Direccion"
        v-model="direccion"
        class="controls"
      />

      <h4>Pagar boleta</h4>
      <select v-model="selectedBoleta" class="selects">
        <option value="Si">Si</option>
        <option value="No">No</option>
      </select>

      <button class="custom-button2" @click="registrar()">Registrar</button>
      <button class="custom-button2" @click="atras3()">Regresar</button>
    </section>
    <section class="register" v-if="informacionDueño">
      <h2>IINFORMACION</h2>
      <h3>Nombre: {{ nombre }}</h3>
      <h3>Teléfono: {{ telefono }}</h3>
      <h3>Dirección: {{ direccion }}</h3>
      <button @click="atras4()" class="custom-button2">Regresar</button>
    </section>
    <div class="container" v-if="cuerpo">
      <div class="con1">
        <section>
          <h1>INFORMACION</h1>
          <div class="margib">
            <div class="image-container">
              <img src="/taza.png" alt="imagen" class="imgs" />
            </div>
            <div class="text-container">
              <div class="texto">{{ premio_Verificado }}</div>
            </div>
          </div>
          <div class="margib">
            <div class="image-container">
              <img src="/signo-de-dolar.png" alt="imagen" class="imgs" />
            </div>
            <div class="text-container">
              <div class="texto">{{ Precio_Verificado }}</div>
            </div>
          </div>
          <div class="margib">
            <div class="image-container">
              <img src="/fiscal.png" alt="imagen" class="imgs" />
            </div>
            <div class="text-container">
              <div class="texto">{{ selectedLoteria_Verificado }}</div>
            </div>
          </div>
          <div class="margib">
            <div class="image-container">
              <img src="/calendario.png" alt="imagen" class="imgs" />
            </div>
            <div class="text-container">
              <div class="texto">{{ fecha_Verificado }}</div>
            </div>
          </div>
          <button class="custom-button2" @click="editarTalonario()">
            EDITAR
          </button>
        </section>
      </div>
      <div class="con2">
        <div
          v-for="(boleta, i) in boleta"
          :key="i"
          :class="getBoletaColor(boleta.estado)"
        >
          <div class="cont_balota" @click="comprarBoleta(i)">
            <div>
              <p>{{ boleta.item }}</p>
            </div>
          </div>
        </div>
      </div>
      <div class="con3" v-if="ocultardiv">
        <h1>ACCIONES</h1>
        <div class="button-container">
          <button class="custom-button2" @click="estadoss()">ESTADO</button>
        </div>
        <div class="button-container">
          <button class="custom-button" @click="listarBoletas()">
            <img src="/filtrar.png" alt="filtro" class="imgs2" />
            <div>LISTAR TUS BOLETAS</div>
          </button>
        </div>
        <div class="button-container">
          <button class="custom-button" @click="personalizarColores()">
            <img
              src="/configuraciones.png"
              alt="configuraciones"
              class="imgs2"
            />
            <div>PERSONALIZAR TALONARIO WEB</div>
          </button>
        </div>
        <div class="button-container">
          <button class="custom-button" @click="imprimir()">
            <img src="/descargas.png" alt="icondescargas" class="imgs2" />
            <div>GENERAR ARCHIVO DE DATOS</div>
          </button>
        </div>
      </div>
      <div v-if="mostrarFormulario" class="register">
        <h2>Personalizar Colores</h2>

        <label for="colorfondo">Fondo principal</label>
        <input
          type="color"
          id="colorfondo"
          v-model="colorfondo"
          class="controls2"
        /><br />

        <label for="color-con1">Fondo de los contenedores</label>
        <input
          type="color"
          id="color-con1"
          v-model="colorCon1"
          class="controls2"
        /><br />

        <label for="colortext">Color de los textos</label>
        <input
          type="color"
          id="colortext"
          v-model="colortext"
          class="controls2"
        /><br />

        <label for="colorfondob">Fondo de los botones</label>
        <input
          type="color"
          id="colorfondob"
          v-model="colorfondob"
          class="controls2"
        /><br />

        <button @click="aplicarColores" class="custom-button2">Aplicar</button>
      </div>
      <div class="register" v-if="mostrarestadoss">
        <div class="estado">
          <div class="estado-circulo disponible"></div>
          <p>Disponible</p>
        </div>
        <div class="estado">
          <div class="estado-circulo sin-pagar"></div>
          <p>Sin pagar</p>
        </div>
        <div class="estado">
          <div class="estado-circulo ganadora"></div>
          <p>Ganadora</p>
        </div>
        <button @click="atras5()" class="custom-button2">Regresar</button>
      </div>
    </div>
    <div class="ticket-container" v-if="balotasCompradas">
      <div>
        <table class="ticket-table">
          <thead>
            <tr>
              <th>Numero de boleta</th>
              <th>Nombre</th>
              <th>Telefono</th>
              <th>Direccion</th>
              <th>Estado de pago</th>
            </tr>
          </thead>
          <tbody>
            <tr
              class="balotas_compradas"
              v-for="(boletasCompradas, i) in boletasCompradas"
              :key="i"
            >
              <td>{{ boletasCompradas.numero }}</td>
              <td>{{ boletasCompradas.nombre }}</td>
              <td>{{ boletasCompradas.telefono }}</td>
              <td>{{ boletasCompradas.direccion }}</td>
              <td>{{ boletasCompradas.pago }}</td>
            </tr>
          </tbody>
        </table>
        <button class="back-button" @click="atras()">Regresar</button>
      </div>
    </div>
    <div class="register" v-if="cont_options_boletas">
      <button @click="DatosDueño()" class="custom-button2">
        Información dueño
      </button>

      <button @click="PagarBoleta()" v-if="pagarBoleta" class="custom-button2">
        Pagar boleta
      </button>

      <button @click="liberarBalota()" class="custom-button2">
        Liberar balota
      </button>

      <button @click="atras2()" class="custom-button2">Regresar</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import jsPDF from "jspdf";
import autoTable from "jspdf-autotable";

jsPDF.autoTable = autoTable;

const premio = ref("");
const premio_Verificado = ref("");
const Precio = ref("");
const Precio_Verificado = ref("");
const selectedLoteria = ref("");
const selectedLoteria_Verificado = ref("");
const selectedCantidad = ref("");
const selectedBoleta = ref("");
const fecha = ref("");
const fecha_Verificado = ref("");
const nombre = ref("");
const telefono = ref("");
const direccion = ref("");
const pago = ref("");
const RegistroTalonario = ref(true);
const RegistrarDueño = ref(false);
const cuerpo = ref(false);
const boletaSeleccionada = ref(null);
const balotasCompradas = ref(false);
const estado = ref(0);
const cont_options_boletas = ref(false);
const pagarBoleta = ref(false);
const informacionDueño = ref(false);
const estadoBoleta = ref("");
const alerta = ref("");
const colorfondo = ref("#8b8282");
const colorCon1 = ref("#9db3c9");
const colortext = ref("#ffffff");
const colorfondob = ref("#b885ae");
const mostrarFormulario = ref(false);
const ocultardiv = ref(true);
const mostrarestadoss = ref(false);

const boleta = ref([]);
const boletasCompradas = ref([]);

const ocultarAlerta = () => {
  setTimeout(() => {
    alerta.value = "";
    Alerta();
  }, 2500);
};

const Alerta = () => {
  if (alerta.value !== "") {
    return "alerta2";
  } else {
    return "alerta";
  }
};

const crearTalonario = () => {
  if (
    premio.value === "" ||
    Precio.value === "" ||
    selectedLoteria.value === "" ||
    selectedCantidad.value === "" ||
    fecha.value === ""
  ) {
    alerta.value = "Todos los campos son obligatorios";
    ocultarAlerta();
    return;
  } else {
    premio_Verificado.value = premio.value;
    Precio_Verificado.value = Precio.value;
    selectedLoteria_Verificado.value = selectedLoteria.value;
    fecha_Verificado.value = fecha.value;
    const cantidad = selectedCantidad.value;

    for (let i = -1; i < cantidad; i++) {
      boleta.value.push({
        item: i + 1,
        estado: estado.value,
      });
    }
    RegistroTalonario.value = false;
    cuerpo.value = true;
  }
};

const comprarBoleta = (index) => {
  boletaSeleccionada.value = index;
  if (boleta.value[index].estado === 0) {
    RegistrarDueño.value = true;
    cuerpo.value = false;
  } else if (boleta.value[index].estado === 1) {
    cont_options_boletas.value = true;
    pagarBoleta.value = false;
    cuerpo.value = false;
  } else if (boleta.value[index].estado === 2) {
    cont_options_boletas.value = true;
    pagarBoleta.value = true;
    cuerpo.value = false;
  }
};

const registrar = () => {
  if (
    nombre.value === "" ||
    telefono.value === "" ||
    direccion.value === "" ||
    selectedBoleta.value === ""
  ) {
    alerta.value = "Todos los campos son obligatorios";
    ocultarAlerta();
    return;
  } else {
    if (selectedBoleta.value === "Si") {
      estado.value = 1;
      estadoBoleta.value = "Pagada";
    } else {
      estado.value = 2;
      estadoBoleta.value = "No pagada";
    }
    boletasCompradas.value.push({
      numero: boleta.value[boletaSeleccionada.value].item,
      nombre: nombre.value,
      telefono: telefono.value,
      direccion: direccion.value,
      pago: estadoBoleta.value,
    });
    boleta.value[boletaSeleccionada.value].estado = estado.value;
    nombre.value = "";
    telefono.value = "";
    direccion.value = "";
    pago.value = "";
    selectedBoleta.value = "";
    estado.value = 0;
    RegistrarDueño.value = false;
    cuerpo.value = true;
  }
};

const DatosDueño = () => {
  // Obtener el índice de la boleta seleccionada
  const boletaSeleccionadaIndex = boletaSeleccionada.value;
  // Obtener los datos de la boleta seleccionada
  const boletaSeleccionadaData = boleta.value[boletaSeleccionadaIndex];

  // Obtener los datos del dueño de la boleta desde el array de boletas compradas
  const datosDueño = boletasCompradas.value.find(
    (boleta) => boleta.numero === boletaSeleccionadaData.item
  );

  // Asignar los datos del dueño de la boleta a las variables reactivas
  nombre.value = datosDueño.nombre;
  telefono.value = datosDueño.telefono;
  direccion.value = datosDueño.direccion;

  // Cambiar el estado de la aplicación para mostrar la sección correspondiente
  informacionDueño.value = true;
  cont_options_boletas.value = false;
};

const PagarBoleta = () => {
  boleta.value[boletaSeleccionada.value].estado = 1;
  cont_options_boletas.value = false;
  cuerpo.value = true;
};

const liberarBalota = () => {
  const selectedBoletaIndex = boletaSeleccionada.value;
  boleta.value[selectedBoletaIndex].estado = 0;
  cont_options_boletas.value = false;
  cuerpo.value = true;

  boletasCompradas.value = boletasCompradas.value.filter((boletaComprada) => {
    return boletaComprada.numero !== boleta.value[selectedBoletaIndex].item;
  });
};

const atras = () => {
  balotasCompradas.value = false;
  cuerpo.value = true;
};

const atras2 = () => {
  cont_options_boletas.value = false;
  cuerpo.value = true;
};

const atras3 = () => {
  nombre.value = "";
  telefono.value = "";
  direccion.value = "";
  selectedBoleta.value = "";
  estado.value = 0;
  RegistrarDueño.value = false;
  cuerpo.value = true;
};

const atras4 = () => {
  informacionDueño.value = false;
  cont_options_boletas.value = true;
};

const atras5 = () => {
  mostrarestadoss.value = false;
  ocultardiv.value = true;
};

const getBoletaColor = (estado) => {
  switch (estado) {
    case 0:
      return "boletaInicial";
    case 1:
      return "boletaPagada";
    case 2:
      return "boletaNoPagada";
  }
};

const editarTalonario = () => {
  premio.value = premio_Verificado.value;
  Precio.value = premio_Verificado.value;
  selectedLoteria.value = selectedLoteria_Verificado.value;
  selectedCantidad.value = "";
  fecha.value = fecha_Verificado.value;
  RegistroTalonario.value = true;
  cuerpo.value = false;
  boleta.value = [];
};

const listarBoletas = () => {
  balotasCompradas.value = true;
  cuerpo.value = false;
};

const aplicarColores = () => {
  document.documentElement.style.setProperty(
    "--color-fondo-con1",
    colorCon1.value
  );
  document.documentElement.style.setProperty("--color-fondo", colorfondo.value);
  document.documentElement.style.setProperty("--color-text", colortext.value);
  document.documentElement.style.setProperty(
    "--color-fondo-botones",
    colorfondob.value
  );

  mostrarFormulario.value = false; // Ocultar el formulario después de aplicar los colores
  ocultardiv.value = true;
};

const personalizarColores = () => {
  mostrarFormulario.value = true;
  ocultardiv.value = false;
};

const estadoss = () => {
  mostrarestadoss.value = true;
  ocultardiv.value = false;
};

const imprimir = () => {
  const doc = new jsPDF();

  doc.setFontSize(12);
  doc.text("Informacion del sorteo", 10, 10);

  const tableData = boletasCompradas.value.map((boleta, index) => [
    boleta.numero,
    boleta.nombre,
    boleta.telefono,
    boleta.direccion,
    boleta.pago,
  ]);

  doc.autoTable({
    head: [["Boleta", "Nombre", "Teléfono", "Dirección", "Pago"]],
    body: tableData,
    startY: 20,
  });

  doc.save("vendidas.pdf");
};
</script>

<style>
:root {
  --color-fondo: #8b8282;
  --color-fondo-con1: #9db3c9;
  --color-text: #ffffff;
  --color-fondo-botones: #b885ae;
}

body {
  background-color: var(--color-fondo);
}

.register {
  width: 300px;
  background-color: var(--color-fondo-con1);
  padding: 30px;
  margin: auto;
  margin-top: 30px;
  margin-bottom: 30px;
  border-radius: 4px;
  font-family: "calibri";
  color: var(--color-text);
  box-shadow: 7px 13px 37px #000;
  text-align: center;
}

.controls {
  width: 90%;
  background: #a2adb7;
  padding: 10px;
  border-radius: 4px;
  border: 1px solid #1f53c5;
  font-family: "calibri";
  font-size: 18px;
  color: white;
}

.controls2 {
  margin-left: 10px;
}

.selects {
  width: 97%;
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
  text-align: center;
  align-items: center;
  justify-content: center;
}

.con1 {
  width: 250px;
  height: 70vh;
  background-color: var(--color-fondo-con1);
  padding: 30px;
  margin: auto;
  margin-top: 80px;
  border-radius: 4px;
  font-family: "calibri";
  color: var(--color-text);
  box-shadow: 7px 13px 20px #000;
  text-align: center;
}

.margib {
  margin-top: 20px;
  display: flex;
  align-items: center;
}

.con2 {
  width: 600px;
  height: 75vh;
  background-color: var(--color-fondo-con1);
  padding: 30px;
  margin: auto;
  margin-top: 30px;
  margin-bottom: 30px;
  border-radius: 4px;
  font-family: "calibri";
  color: var(--color-text);
  box-shadow: 7px 13px 20px #000;
  text-align: center;
  overflow: auto;
  display: grid;
  grid-template-columns: repeat(10, 1fr);
  gap: 5px;
}

.boletaInicial {
  background-color: #e6e6e6;
  width: 47px;
  height: 47px;
  border-radius: 47px;
  text-align: center;
  color: black;
}

.boletaPagada {
  background-color: #aaffaa;
  width: 47px;
  height: 47px;
  border-radius: 47px;
  text-align: center;
  color: black;
}

.boletaNoPagada {
  background-color: #ffaaaa;
  width: 47px;
  height: 47px;
  border-radius: 47px;
  text-align: center;
  color: black;
}

.con3 {
  width: 250px;
  height: 70vh;
  background-color: var(--color-fondo-con1);
  padding: 30px;
  margin: auto;
  margin-top: 90px;
  border-radius: 4px;
  font-family: "calibri";
  color: var(--color-text);
  box-shadow: 7px 13px 20px #000;
}

.button-container {
  display: flex;
  align-items: center;
  text-align: center;
  margin-bottom: 10px;
}

.custom-button {
  background-color: var(--color-fondo-botones);
  color: var(--color-text);
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
  transition: background-color 0.3s;
  width: 100%;
  display: flex;
  align-items: center;
}

.custom-button2 {
  background-color: var(--color-fondo-botones);
  color: var(--color-text);
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
  transition: background-color 0.3s;
  width: 100%;
  align-items: center;
}

.imgs {
  width: 50px;
  height: 50px;
}

.imgs2 {
  width: 25px;
  height: 25px;
}

.image-container {
  margin-right: 20px;
}

/* table  */

.ticket-container {
  margin: 20px;
}

.ticket-table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
}

.ticket-table th,
.ticket-table td {
  border: 1px solid #2a2929;
  padding: 8px;
  text-align: left;
}

.ticket-table th {
  background-color: #59c0c7;
}

.ticket-table tr {
  background-color: #9fa7a7;
}

.back-button {
  padding: 10px 15px;
  background-color: var(--color-fondo-botones);
  color: var(--color-text);
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.back-button:hover {
  background-color: #959d96;
}

.estado {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 10px;
}

.estado-circulo {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  margin-bottom: 10px;
}

.disponible {
  background-color: #aaffaa;
}

.sin-pagar {
  background-color: #ffaaaa;
}

.ganadora {
  background-color: yellow;
}
</style>
