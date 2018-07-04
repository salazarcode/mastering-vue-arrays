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
    <li v-for="producto in orden" :key="producto.id">
      {{producto.id}} - {{producto.nombre}} - {{producto.precio.toFixed(2)}} - {{producto.cantidad}} - {{producto.total.toFixed(2)}}
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
        elem.cantidad = 1;
        elem.total = elem.precio * elem.cantidad;
        this.orden.push(elem);
      }
    }
  },
  computed: {
    articulos: function(){
      this.orden.forEach(elem=> this.n += elem.cantidad);
      return this.n;
    },
    subtotal: function(){
      this.orden.forEach(elem => this.n1 += elem.total)
      return this.n1.toFixed(2);
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
