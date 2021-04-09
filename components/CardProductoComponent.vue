<template>
  <v-card
    :loading="loading"
    class="mx-auto my-12"
    max-width="374"
  >
    <template slot="progress">
      <v-progress-linear
        color="deep-purple"
        height="10"
        indeterminate
      ></v-progress-linear>
    </template>

    <v-img
      height="250"
     v-bind:src="producto.img"
      v-on:click="seleccionado"
    ></v-img>

    <v-card-title> {{producto.descripcion}} </v-card-title>

    <v-card-text>
      

      <div class="my-4 subtitle-1">
        US $ {{ producto.precio}}
      </div>

      <v-row
        align="center"
        class="mx-0"
      >
        <v-rating
          :value="4.5"
          color="amber"
          dense
          half-increments
          readonly
          size="14"
        ></v-rating>

        <div class="grey--text ml-4">
          4.5 (413)
        </div>
      </v-row>
    </v-card-text>

 
  </v-card>
</template>
<script>
  export default {
    name : 'CardProductoComponent',
    props:{
      producto: { type: Object},
      // click: { type:Function }
    },
    data: () => ({
      loading: false
    }),

    methods: {
      seleccionado () {
        this.$emit('click', this.producto);
      },
    },
  }
</script>
