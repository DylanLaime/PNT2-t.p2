<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  components: { HelloWorld },
  data() {
    return {
       personas: [
        { nombre: "Daniel", apellido: "Sanchez", correo: "danielsanchez68@hotmail.com", dni: "20442873" },
        { nombre: "Juan", apellido: "Perez", correo: "j@p.gmail.com", dni: "12345678" },
        { nombre: "Ana", apellido: "Suarez", correo: "a@s.gmail.com", dni: "87654321" },
        { nombre: "Dylan", apellido: "Laime", correo: "dylan@example.com", dni: "44123456" }
      ],   // arreglo original de personas
      filtroNombre: '',     
      filtroDni: ''         
    }
  },

  computed: {
    mostrarAlerta() {

      // Normalizamos: si es undefined/null lo tratamos como cadena vacía
      const fn = this.filtroNombre ? String(this.filtroNombre).trim() : '';
      const fd = this.filtroDni ? String(this.filtroDni).trim() : '';

      // Si ambos son vacíos → no hay alerta
      if (!fn && !fd) return false;

      // Si alguno está presente y tiene longitud < 3 → mostrar
      return (fn && fn.length < 3) || (fd && fd.length < 3);
    },
    personasFiltradas() {
      // Si hay alerta (alguno con < 3) devolvemos lista vacía
      if (this.mostrarAlerta) return [];

      const fn = this.filtroNombre ? String(this.filtroNombre).trim().toLowerCase() : '';
      const fd = this.filtroDni ? String(this.filtroDni).trim() : ''; // dni compararemos como string

      // Si ambos filtros vacíos → devolvemos todo
      if (!fn && !fd) return this.personas;

      return this.personas.filter(p => {
        // Construyo texto nombre completo y lo normalizo para búsqueda case-insensitive
        const nombreCompleto = `${p.nombre} ${p.apellido}`.toLowerCase();

        // Coincidencia por nombre: si fn está vacío => true (no filtra)
        const coincideNombre = fn ? nombreCompleto.includes(fn) : true;

        // Coincidencia por dni: si fd está vacío => true
        // Convertimos dni a string para hacer includes (soporta búsquedas parciales)
        const coincideDni = fd ? String(p.dni).includes(fd) : true;

        // Excluyente: la persona debe cumplir ambos (AND) si ambos filtros están activos
        return coincideNombre && coincideDni;
      });
    }
  }
}

</script>

<template>
  <div>
    <a href="https://vite.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>

    <div class="container">
        <!-- FILTROS -->
      <input type="text" v-model.trim="filtroNombre" class="form-control" placeholder="Buscar por nombre o apellido"></input>
      <input type="text" v-model.trim="filtroDni" class="form-control" placeholder="Buscar por DNI"></input>
      
        <!-- ALERTA si no hay al menos 3 caracteres -->
      <div v-if="mostrarAlerta" class="alert alert-warning">
        Ingrese al menos 3 caracteres en alguno de los filtros.
      </div>

        <!-- LISTA DE RESULTADOS -->
      <ul class="list-group">
        <li v-for="persona in personasFiltradas" :key="persona.dni" class="list-group-item">
          {{ persona.nombre }} {{ persona.apellido }} - DNI: {{ persona.dni }}
        </li>
      </ul>
    </div>

  </div>
  <HelloWorld msg="Vite + Vue" />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
