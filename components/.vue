<template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      max-width="500"
    >
      <v-card>
        <v-card-title class="headline">
         
        </v-card-title>

        <!-- <v-card-text>
          Let Google help apps determine location. This means sending anonymous location data to Google, even when no apps are running.
        </v-card-text> -->
        
        <CardProductoComponent :producto="producto"/>
        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn
            color="green darken-1"
            text
            @click="agregar"
          >
            Agregar
          </v-btn>

          <v-btn
            color="green darken-1"
            text
            @click="cerrar"
          >
            Cerrar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>
<script>
  import CardProductoComponent from "../components/CardProductoComponent"
  import {CANTIDAD} from '../globals/global'
  export default {
      components:{
          CardProductoComponent
      },
      name: "VerProductoComponent",
      props:{
          dialog: { type: Boolean , default: false},
          cerrar:  { type: Function },
          producto: { type: Object },
        //   agregar: { type: Function }
      },
    data () {
      return {
        // dialog:false
      }
    },
    methods:{
        agregar(){
            this.$emit('agregar', this.producto);
        }
    }
  }
</script>