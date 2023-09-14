<template>
  <div class="container">
    <h1>Calculadora de Total</h1>
    <div class="item">
      <p>Nombre: <input v-model="nuevoArticulo.nombre" /></p>
      <p>Cantidad: <input v-model="nuevoArticulo.cantidad" type="number" /></p>
      <p>Precio Unitario: <input v-model="nuevoArticulo.precio" type="number" /></p>
      <button @click="agregarArticulo">Agregar Artículo</button>
    </div>

    <div>
      <table class="tabla">
        <thead>
          <tr>
            <th>ID</th>
            <th>Nombre</th>
            <th>Cantidad</th>
            <th>Precio Unitario</th>
            <th>Subtotal</th>
            <th>Acciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(articulo, index) in articulos" :key="index">
            <td>{{ index + 1 }}</td>
            <td>{{ articulo.nombre }}</td>
            <td>{{ articulo.cantidad }}</td>
            <td>{{ articulo.precio }}</td>
            <td>{{ articulo.cantidad * articulo.precio }}</td>
            <td>
              <button @click="eliminarArticulo(index)">Borrar</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <p>Total a Pagar: {{ totalConDescuento }}</p>
    <p v-if="tieneDescuento">Descuento: {{ porcentajeDescuento }}%</p>
    <p v-if="tieneDescuento">{{ descuentoText }}</p>
    <button @click="guardarTabla">Guardar Tabla</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      nuevoArticulo: { nombre: '', cantidad: 0, precio: 0 },
      articulos: [],
    };
  },
  computed: {
    totalSinDescuento() {
      return this.articulos.reduce((total, item) => total + item.cantidad * item.precio, 0);
    },
    totalConDescuento() {
      let descuento = 0;
      if (this.totalSinDescuento >= 240000) {
        descuento = 0.15;
      } else if (this.totalSinDescuento >= 120000) {
        descuento = 0.10;
      } else if (this.totalSinDescuento >= 60000) {
        descuento = 0.05;
      }

      return this.totalSinDescuento * (1 - descuento);
    },
    tieneDescuento() {
      return this.totalConDescuento < this.totalSinDescuento;
    },
    porcentajeDescuento() {
      if (this.tieneDescuento) {
        return ((this.totalSinDescuento - this.totalConDescuento) / this.totalSinDescuento) * 100;
      } else {
        return 0;
      }
    },
    descuentoText() {
      return this.tieneDescuento ? 'Tiene descuento.' : 'No tiene descuento.';
    },
  },
  methods: {
    agregarArticulo() {
      if (this.nuevoArticulo.nombre && this.nuevoArticulo.cantidad && this.nuevoArticulo.precio) {
        this.articulos.push({ ...this.nuevoArticulo });
        this.nuevoArticulo = { nombre: '', cantidad: 0, precio: 0 };
      }
    },
    eliminarArticulo(index) {
      if (index >= 0 && index < this.articulos.length) {
        this.articulos.splice(index, 1);
      }
    },
    guardarTabla() {
      console.log('Tabla guardada:', this.articulos);
    },
  },
};
</script>

<style scoped>
.container {
  text-align: center;
  max-width: 600px;
  margin: 0 auto;
}

.item {
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 10px;
}

.tabla {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}

.tabla th,
.tabla td {
  border: 1px solid #ccc;
  padding: 8px;
  text-align: left;
}

.tabla th {
  background-color: #f2f2f2;
}
</style>
