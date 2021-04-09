<template>
  <v-data-table
    v-model="selected"
    :headers="headers"
    :items="desserts"
    item-key="name"
    show-select
    class="elevation-1"
  >
    <template v-slot:top>
     <v-btn depressed>
      Grabar
    </v-btn>
    </template>
  </v-data-table>
</template>

<script>
import axios from 'axios';
import _ from 'lodash'
  export default {
    data () {
      return {
        singleSelect: false,
        selected: [],
        headers: [
          { text: 'Codigo', value: 'codigo' },
          {
            text: 'nombre',
            align: 'start',
            value: 'nombre',
          },
          { text: 'Descripcion', value: 'descripcion' },
          { text: 'Valor Plan', value: 'plan' }
        ],
        desserts: [
          
        ],
      }
    },
    
    async created(){

      const headers = {
         'Content-Type': 'application/json',
          'x-token': localStorage.getItem('token')
      }

      const data = { "transaccion": "consultarPlanes"  };

      const dataResponse = await axios.post('https://rolimapp.com:3000/planes',
      data, { headers: headers  } );
 
       this.desserts = dataResponse.data.retorno;
 console.log(this.desserts)
 
 }
 }
</script>