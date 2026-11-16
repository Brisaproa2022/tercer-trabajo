
<script setup>
import { ref } from "vue"

const ciudad = ref("")
const temperatura = ref("")
const humedad = ref("")
const viento = ref("")
const descripcion = ref("")
const cargando = ref(false)
const error = ref("")

async function buscarClima() {
  if (ciudad.value === "") {
    error.value = "Ingresa una ciudad"
    return
  }

  cargando.value = true
  error.value = ""

  try {
    const respuesta = await fetch(
      "https://geocoding-api.open-meteo.com/v1/search?name=" + ciudad.value + "&count=1&language=es&format=json"
    )

    const datos = await respuesta.json()

    if (!datos.results) {
      error.value = "No se encontro la ciudad"
      cargando.value = false
      return
    }

    const latitud = datos.results[0].latitude
    const longitud = datos.results[0].longitude

    const respuestaClima = await fetch(
      "https://api.open-meteo.com/v1/forecast?latitude=" + latitud + "&longitude=" + longitud + "&current=temperature_2m,relative_humidity_2m,wind_speed_10m,weather_code"
    )

    const datosClima = await respuestaClima.json()

    temperatura.value = datosClima.current.temperature_2m
    humedad.value = datosClima.current.relative_humidity_2m
    viento.value = datosClima.current.wind_speed_10m

    descripcion.value = "Clima actual"

  } catch (errorConsulta) {
    error.value = "No se pudo consultar el clima"
  }

  cargando.value = false
}
</script>

<template>
  <div class="pagina">

    <h1>Mi Clima</h1>
    <p>Consulta el clima de tu ciudad</p>

    <div class="buscador">
      <input
        v-model="ciudad"
        type="text"
        placeholder="Ingresa una ciudad"
      >

      <button @click="buscarClima">
        Buscar
      </button>
    </div>

    <p v-if="cargando">Buscando clima...</p>

    <p v-if="error">{{ error }}</p>

    <div v-if="temperatura !== ''" class="resultado">

      <h2>{{ ciudad }}</h2>

      <h1>{{ temperatura }} °C</h1>

      <p>{{ descripcion }}</p>

      <p>Humedad: {{ humedad }} %</p>

      <p>Viento: {{ viento }} km/h</p>

    </div>

  </div>
</template>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #eaf4ff;
}

.pagina {
  text-align: center;
  padding: 50px 20px;
}

h1 {
  color: #2563a6;
}

.buscador {
  margin-top: 30px;
}

input {
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  padding: 12px 20px;
  margin-left: 8px;
  background-color: #2563a6;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.resultado {
  background-color: white;
  width: 300px;
  margin: 30px auto;
  padding: 20px;
  border-radius: 12px;
}
</style>
