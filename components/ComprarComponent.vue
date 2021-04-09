<template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      max-width="900"
    >


 <v-card
    max-width="900"
    class="mx-auto"
  >
 

    <v-list three-line>
      <template v-for="item in arrproducto" >
        <!-- <v-subheader
          v-if="item.header"
          :key="item.header"
          v-text="item.header"
        ></v-subheader> -->

        <!-- <v-divider
          v-else-if="item.divider"
          :key="index"
          :inset="item.inset"
        ></v-divider> -->

        <v-list-item
          :key="item.descripcion"
        >
          <v-list-item-avatar>
            <v-img :src="item.img"></v-img>
          </v-list-item-avatar>

          <v-list-item-content>
            <v-row >
              <v-col md="6" lg="6">
                <v-list-item-title v-html="item.descripcion"></v-list-item-title>
                <v-list-item-subtitle v-html="item.precio"></v-list-item-subtitle>
              </v-col>
               <v-col md="2" lg="2">
                    <v-btn
                      color="green darken-1"
                      text
                      @click="aumentar( item)"
                    >
                      +
                    </v-btn>
              
               <v-col md="2" lg="2">
                    <v-btn
                      color="green darken-1"
                      text
                      @click="disminuir(item)"
                    >
                      -
                    </v-btn>
              </v-col>

              </v-col>
               <v-col md="2" lg="2">
                  <label> {{ item.cantidad}}  </label>
                
              </v-col>
               
               <v-col md="2" lg="2">
                  <label> {{ item.subtotal}}  </label>
                
              </v-col>
              
            </v-row>
            
          
                

          </v-list-item-content>
        </v-list-item>
        
        
      </template>
    </v-list>


        <v-row>
            <v-col></v-col>
              <v-col></v-col>
            <v-col>
             TOTAL: $  {{ totalCompra }}
            </v-col>


        </v-row>

      <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="green darken-1"
            text
            @click="comprar"
          >
            Comprar
          </v-btn>
          <v-btn
            color="red darken-1"
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
  import axios from 'axios';
  import _ from 'lodash'
  export default {
      components:{
          CardProductoComponent
      },
      name: "ComprarComponent",
      props:{
          dialog: { type: Boolean , default: false},
          cerrar:  { type: Function },
          arrproducto: { type: Array },
        //   agregar: { type: Function }
      },
    data () {
      return {
        
      }
    },
    computed:{
      totalCompra(){
          return _.sumBy(this.arrproducto, function(o) { return o.subtotal; });  
      }
    },
    created(){
      console.log(this.arrproducto)
    },
    methods:{
      aumentar( item){
        item.cantidad  +=1;
        item.subtotal = item.cantidad *item.precio
        
      },

      disminuir( item){
        if(item.cantidad > 0 ) item.cantidad -=1;
        item.subtotal = item.cantidad *item.precio
      },

      async comprar(){
        const {data} = await axios.post('https://rolimapp.com:3000/usuarios', 
        {
          "transaccion": "autenticarUsuario",
          "datosUsuario": {
              "email": "danielsilvaorrego@gmail.com",
              "password": "89e495e7941cf9e40e6980d14a16bf023ccd4c91"
          }
        });


  localStorage.setItem('token', data.token)

        if(_.has(data, 'plan')){
        //vaidar la compra
        }
        else{
          //cargar modal
          this.$router.push('/inspire')
        }



        
      }
     
       
    }
  }
</script>