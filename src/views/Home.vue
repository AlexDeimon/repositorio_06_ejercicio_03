<template>
  <div class="home">
    <div class="div">
      <!-- eslint-disable -->
      <img src="https://img.icons8.com/ios-filled/40/ffff00/pacman.png" alt="pacman">
      <h2 class="titulo2">Productos</h2>
      <img src="https://img.icons8.com/external-glyph-chroma-amoghdesign/40/external-casper-halloween-glyph-chroma-amoghdesign.png" alt="ghost">
    </div>
    <fieldset>
      <legend>Buscar productos</legend>
      <button v-on:click="mostrarProductos">Ver productos</button>
      <div class="overflow">
        <table id="productos" class="productos">
          <thead>
            <tr>
              <th>Producto</th>
              <th>Precio</th>
              <th>Cantidad disponible</th>
              <th>Descripción</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="producto in productos_list" v-bind:key="producto">
              <td>{{producto.producto}}</td>
              <td>{{producto.precio}}</td>
              <td>{{producto.stock}}</td>
              <td>{{producto.descripcion}}</td>
            </tr>
          </tbody>
        </table>  
      </div>
    </fieldset>
    <div class="divForm">
      <form v-on:submit.prevent="crearProducto">
        <fieldset class="fieldset">
          <legend>Agregar Productos</legend>
          <div>
            <label for="nombre_producto">Producto:</label>
            <input required type="text" id="nombre_producto" v-model='producto.producto'>
            <label for="precio_producto">Precio:</label>
            <input required type="number" id="precio_producto" step="100" v-model='producto.precio'>
            <label for="stock_producto">Cantidad disponible:</label>
            <input required type="number" id="stock_producto" v-model='producto.stock'>
            <label for="descripcion_producto">Descripción:</label>
            <textarea required id="descripcion_producto" cols="30" rows="10" v-model='producto.descripcion'></textarea>
            <button type="submit">Agregar a inventario</button>
          </div>
        </fieldset>
      </form>
      <form v-on:submit.prevent="actualizarProducto">
        <fieldset class="fieldset">
          <legend>Actualizar Producto</legend>
          <div>
            <label for="nombre_producto_update">Producto:</label>
            <input required type="search" id="nombre_producto_update" v-model='producto_update.producto'>
            <button v-on:click.prevent="buscarProducto">Buscar producto</button><br>
            <label for="precio_producto_update">Precio:</label>
            <input required type="number" id="precio_producto_update" step="100" v-model='producto_update.precio'>
            <label for="stock_producto_update">Cantidad disponible:</label>
            <input required type="number" id="stock_producto_update" v-model='producto_update.stock'>
            <label for="descripcion_producto_update">Descripción:</label>
            <textarea required id="descripcion_producto_update" cols="30" rows="10" v-model='producto_update.descripcion'></textarea>
            <button type="submit">Actualizar producto</button>
          </div>
        </fieldset>
      </form>
      <form v-on:submit.prevent="eliminarProducto">
        <fieldset class="fieldset">
          <legend>Eliminar Producto</legend>
          <div>
            <label for="nombre_producto_delete">Producto:</label>
            <input required type="search" id="nombre_producto_delete" v-model='producto_delete.producto'>
            <button type="submit">Eliminar</button>
          </div>
        </fieldset>
      </form>  
    </div>   
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "Home",
  data: function () {
    return {
      producto: {
        producto: "",
        precio: "",
        stock: "",
        descripcion: "",
      },
      producto_update: {
        producto: "",
        precio: "",
        stock: "",
        descripcion: "",
      },
      producto_delete: {
        producto: "",
      },
      productos_list: [],
    };
  },
  methods: {
    limpiarCampos() {
      location.reload();
    },
    mostrarProductos() {
      var productos = this.productos;
      axios({
        method: "GET",
        url: "https://project-ing-web-default-rtdb.firebaseio.com/productos.json",
      })
        .then((response) => {
          this.productos_list = response.data;
          console.log(response, productos);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    crearProducto() {
      console.log("response", this.producto);
      var producto = this.producto;
      axios({
        method: "PUT",
        url: `https://project-ing-web-default-rtdb.firebaseio.com/productos/${producto.producto}.json`,
        data: {
          producto: producto.producto,
          precio: producto.precio,
          stock: producto.stock,
          descripcion: producto.descripcion,
        },
      })
        .then((response) => {
          /* eslint-disable */
          this
          console.log(response, producto);
          alert("Se ha añadido el producto");
          this.limpiarCampos();
        })
        .catch(function (error) {
          console.log(error);
          alert("El producto ya existe");
        });
    },
    buscarProducto() {
      console.log("response", this.producto_update);
      var producto_update = this.producto_update;
      axios({
        method: "GET",
        url: `https://project-ing-web-default-rtdb.firebaseio.com/productos/${producto_update.producto}.json`,
      })
        .then((response) => {
          this.producto_update = response.data;
          console.log(response, producto_update);
        })
        .catch(function (error) {
          console.log(error);
          alert("El producto no existe");
        });
      console.log(this.producto_update);
    },
    actualizarProducto() {
      console.log("response", this.producto_update);
      var producto_update = this.producto_update;
      axios({
        method: "PATCH",
        url: `https://project-ing-web-default-rtdb.firebaseio.com/productos/${producto_update.producto}.json`,
        data: {
          precio: producto_update.precio,
          stock: producto_update.stock,
          descripcion: producto_update.descripcion,
        },
      })
        .then((response) => {
          this
          console.log(response, producto_update);
          alert("Se ha actualizado el producto");
          this.limpiarCampos();
        })
        .catch(function (error) {
          console.log(error);
          alert("El producto no existe");
        });
    },
    eliminarProducto() {
      console.log("response", this.producto_delete);
      var producto_delete = this.producto_delete;
      axios({
        method: "DELETE",
        url: `https://project-ing-web-default-rtdb.firebaseio.com/productos/${producto_delete.producto}.json`,
      })
        .then((response) => {
          this
          console.log(response, producto_delete);
          alert("El producto se ha eliminado");
           this.limpiarCampos();
        })
        .catch(function (error) {
          console.log(error);
          alert("El producto no existe");
        });
    },
  },
};
</script>
<style>
fieldset {
  background-color: rgba(0, 0, 0, 0.664);
  box-shadow: 5px 5px 20px white;
  font-size: large;
  width: 90vw;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 20px;
}

legend {
  color: #ffff00;
  font-size: x-large;
  font-weight: bold;
}

label {
  color: #ffffff;
}

input {
  height: 25px;
  border-radius: 5px;
}

button {
  margin: 10px;
  padding: 10px;
  font-size: medium;
  color: #ffffff;
  background-color: #d40000;
  border: 1px solid #ffffff;
  border-radius: 10px;
}

@media all and (max-width: 340px) {
  .fieldset {
    min-width: 200px;
    max-width: 220px;
  }
}

@media all and (min-width: 320px) and (max-width: 480px) {
  .fieldset {
    min-width: 320px;
    max-width: 320px;
  }
}

@media all and (min-width: 480px) {
  .fieldset {
    min-width: 450px;
    max-width: 450px;
  }
}

.fieldset {
  display: inline-block;
  position: relative;
}

.divForm {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  flex-wrap: wrap;
  row-gap: 20px;
}

button:hover {
  cursor: pointer;
  color: yellow;
  border-color: yellow;
}

.fieldset div {
  display: grid;
  grid-template-columns: 45% 55%;
  row-gap: 8px;
}

.home .div {
  text-align: center;
}

.overflow {
  max-width: 90vw;
  overflow: auto;
  max-height: 50vh;
  height: 50vh;
  background-color: white;
}

.productos {
  width: 100%;
  border-collapse: collapse;
}

.productos th,
.productos td {
  padding: 10px;
  border: 1px solid black;
  color: black;
  min-width: 200px;
}

.productos thead,
.productos tbody tr:nth-child(even) {
  background-color: #ffffff;
}

.productos tbody tr:nth-child(odd) {
  background-color: #c5c5c5;
}
</style>
