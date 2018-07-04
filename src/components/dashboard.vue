<template>
<div>
  
  <h1>Productos:</h1>
  <ul>
    <li v-for="producto in productos" :key="producto.id">
      {{producto.id}} - {{producto.nombre}} - {{producto.precio.toFixed(2)}} - <a href="" @click.prevent="addProductToOrder(producto.id)">Add</a>
    </li>
  </ul> 
  
  <h1>Orden:</h1>
  <ul>
    <li v-for="(producto, index) in orden" :key="producto.id">
      {{producto.id}} - 
      {{producto.nombre}} - 
      {{producto.precio.toFixed(2)}} - 
      <input id="cantidad" type="text" v-model="producto.cantidad" :data-id="producto.id" :data-index="index" @input="changeField"> -
      {{producto.total.toFixed(2)}}
    </li>
  </ul>

  <h1>Resume de orden:</h1>
  <table>
    <tr>
      <th>Artículos</th>
      <th>Subtotal</th>
      <th>Impuesto</th>
      <th>Total</th>
    </tr>
    <tr>
      <td>{{articulos}}</td>
      <td>{{subtotal}}</td>
      <td>{{impuesto}}</td>
      <td>{{total}}</td>
    </tr>
  </table>


</div>
</template>

<script>
export default {
  name: 'dashboard',
  data: function(){
    return{
      productos: [
        {id: 1, nombre: "producto a", precio: 45.0245},
        {id: 2, nombre: "producto b", precio: 12.1561},
        {id: 3, nombre: "producto c", precio: 8.515}
      ],
      orden: [],
      tax: 15,
      n: 0,
      n1: 0
    }
  },
  methods: {
    addProductToOrder: function(id){
      let index = this.orden.findIndex(elem => elem.id == id);
      if(index != -1){
        let aux = this.orden.find(elem => elem.id == id);
        aux.cantidad++;
        aux.total = aux.precio * aux.cantidad;
        this.$set(this.orden, index, aux)
      }
      else
      {
        let elem = this.productos.find(elem => elem.id == id);
        elem.cantidad = 0;
        elem.total = elem.precio * elem.cantidad;
        this.orden.push(elem);
      }
    },
    changeField: function(e){
      let propiedad = e.target.id;
      let valor = e.target.value;
      let index = e.target.dataset.index;
      let id = e.target.dataset.id;
      let elem = this.orden.find(elem => elem.id == id);
      elem[propiedad] = valor;
      this.$set(this.orden, index, elem);
    }
  },
  computed: {
    articulos: function(){
      let suma = 0;
      for(let n = 0; n < this.orden.length; n++)
      {
        suma += parseFloat(this.orden[n].cantidad);
      }
      return suma;
    },
    subtotal: function(){
      let suma = 0;
      for(let n = 0; n < this.orden.length; n++)
      {
        suma += this.orden[n].total;
      }
      return suma.toFixed(2);
    },
    impuesto: function(){
      return (this.tax * this.subtotal / 100).toFixed(2);
    },
    total: function(){
      let total = parseFloat(this.subtotal) + parseFloat(this.impuesto);
      return total.toFixed(2);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

</style>
