<template>
<v-container>
<v-row>
   <v-badge
          color="green"
          :content="cantidadProductos"
          overlap
         
        >
          <v-icon  @click="comprar">mdi-application</v-icon>
        </v-badge>
</v-row>
<v-row>
  <v-col md="3" lg="3" sm="3"  v-for="producto in arrProductos" :key="producto.id" >
    <CardProductoComponent :producto="producto" @click="click"/>
  </v-col>
    <VerProductoComponent :dialog="dialog" :cerrar="cerrar"
     :producto="producto" v-if="dialog" @agregar="agregar"/>

    <ComprarComponent :dialog="dialogcompra" :cerrar="cerrarCompra"
     :arrproducto="this.arrCarrito" v-if="dialogcompra"/>
</v-row>

</v-container>

</template>
<script>
import CardProductoComponent from '../components/CardProductoComponent'
import VerProductoComponent from '../components/VerProductoComponent'
import ComprarComponent from '../components/ComprarComponent'
import axios from 'axios';
import _ from 'lodash';
  export default {
    components:{
      CardProductoComponent,
      VerProductoComponent,
      ComprarComponent
    },
    data: () => ({
    cantidadProductos: 0 ,
    dialog:false,
    dialogcompra: false,
    producto: {},
    arrProductos:[],
    arrCarrito:[]


    }),
    async created(){
     const {data: {data}} = await axios.post('https://rolimapp.com:3000/productos', {
        "transaccion": "generico",
        "tipo": "4"
      });

      this.arrProductos = data;

        if(!_.isNil(localStorage.getItem('arrCarrito')))  this.arrCarrito = JSON.parse(localStorage.getItem('arrCarrito'));
      this.cantidadProductos = _.size(this.arrCarrito);
    },

methods:{
  click(producto){
    this.producto = producto;
    this.dialog = true;
  },
  cerrar(){
    this.dialog = false;
  },
  agregar(producto){

   const existeEnLista =  _.find(this.arrCarrito, function(o) { return o.id == producto.id });

  if(_.isUndefined(existeEnLista)){

    const prod = {...producto}

      this.arrCarrito.push({
            subtotal: prod.precio * 1,
            cantidad:1 ,
            ...producto
          }); 
        }
        else {
          _.map(this.arrCarrito , prod => {
            if(prod.id == producto.id){
              prod.cantidad += 1; 
              prod.subtotal = prod.precio *  prod.cantidad;
            }
          })
        }
   this.cantidadProductos = _.size(this.arrCarrito);


       localStorage.setItem('arrCarrito', JSON.stringify( this.arrCarrito))

   },

  comprar(){
    this.dialogcompra = true;
},

cerrarCompra(){
    this.dialogcompra = false;
  },

}
   
  }
</script>
