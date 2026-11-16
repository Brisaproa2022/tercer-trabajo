<template>
  <div class="inicio">
    <h1>Bienvenido a Mi Clima</h1>
    <p>Consulta el estado del tiempo en tu ciudad.</p>

    <router-link to="/clima">
      <button>Consultar clima</button>
    </router-link>
  </div>
</template>

<style scoped>
.inicio {
  text-align: center;
  padding: 80px 20px;
}

h1 {
  color: #2563a6;
}

button {
  background-color: #2563a6;
  color: white;
  padding: 12px 25px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
</style>