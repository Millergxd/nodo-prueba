<template>
  <div>
    <h1 v-show="isLoading">
      Cargando vista detallada de un personaje ...
    </h1>
    <div v-show="!isLoading&& error">
      <h1>No se encontro personaje</h1>
      <button @click="goBack()">Regresar</button>
    </div>
    <table v-show="!isLoading && !error">
      <button @click="goBack()">Ir atras</button>
      <tr>
        <th class="titles" colspan="2" style="text-align:center">Vista detalle de {{ detail.name}}</th>
      </tr>
      <tr v-show="existTheArgument(detail.imageLink)">
        <td class="titles">Imagen</td>
        <td class="information"><img v-bind:src="'https://api.got.show/'+detail.imageLink" width="128" height="auto"/></td>
      </tr>
      <tr v-show="existTheArgument(detail.house)">
        <td class="titles">Casa</td>
        <td class="information">{{detail.house}}</td>
      </tr>
      <tr v-show="existTheArgument(detail.male)">
        <td class="titles">Genero</td>
        <td class="information">{{getGender(detail.male)}}</td>
      </tr>
      <tr v-show="existTheArgument(detail.books)">
        <td class="titles">Libros</td>
        <td class="information">
          <ul v-for="book in detail.books">
            <li>{{book}}</li>
          </ul>
        </td>
      </tr>
      <tr v-show="existTheArgument(detail.titles)">
        <td class="titles">Titulos</td>
        <td class="information">
          <ul v-for="title in detail.titles">
            <li>{{title}}</li>
          </ul>
        </td>
      </tr>
      <tr v-show="existTheArgument(detail.updatedAt)">
        <td class="titles">Ultima Actualizacion</td>
        <td class="information">{{new Date(detail.updatedAt).toLocaleString('en-US',{timeZone:'America/Santiago'})}}</td>
      </tr>
      <tr v-show="existTheArgument(detail.createdAt)">
        <td class="titles">Fecha de Creacion</td>
        <td class="information">{{new Date(detail.createdAt).toLocaleString('en-US',{timeZone:'America/Santiago'})}}</td>
      </tr>
      <tr v-show="existTheArgument(detail.dateOfBirth)">
        <td class="titles">Fecha de nacimiento</td>
        <td class="information">{{detail.dateOfBirth}}</td>
      </tr>
      <tr v-show="existTheArgument(detail.dateOfDeath)">
        <td class="titles">Fecha de muerte</td>
        <td class="information">{{detail.dateOfDeath}}</td>
      </tr>
      <tr v-show="existTheArgument(detail.spouse)">
        <td class="titles">Cónyuge</td>
        <td class="information">{{detail.spouse}}</td>
      </tr>
      <tr v-show="existTheArgument(detail.culture)">
        <td class="titles">Cultura</td>
        <td class="information">{{detail.culture}}</td>
      </tr>
    </table>
  </div>
</template>


<script>
  import { getACharacter } from '../services/got.service.js'

  export default {
    name: 'list-component',

    /**
     * @description the data block represents all the local variable of this component.
     */
    data () {
      return {
        detail: [],
        isLoading: false,
        error:false
      }
    },

    /**
     * @description the create function is the first one to be execute when the component is being created (see vue js lifecycle).
     */
    created () {
      this.isLoading = true
      this.id = this.$route.params.id;    
      getACharacter(this.id)
        .then(res => {
          if("error" in res){
            this.error = true  
            this.isLoading = false
          }else{
            this.detail = res.data
            this.isLoading = false
          }
        })
    },

    /**
     * @description the methods block represents all the local methods of this component.
     */
    methods: {
      /**
       * @description define if gender is male or female
       * @param {boolean} isMale gender
       * @method getGender
       */
      getGender(isMale){
        if(isMale){
          return "Hombre"
        }else{
          return "Mujer"
        }
      },
      /**
       * @description function to go back in the history
       * @method goBack
       */
      goBack(){
        this.$router.go(-1)
      },
      /**
       * @description will verify if the argument exist
       * @param {undefined} theArgument any argument
       * @method existTheArgument
       */
      existTheArgument(theArgument){
        if(theArgument && theArgument.length > 0)
          return true
        else
          return false
      },
    }
  }
</script>
<style>
  table {
    font-family: arial, sans-serif;
    border-collapse: collapse;
    width: 100%;
  }

  td, th {
    border: 1px solid #cbcbcb;
    text-align: left;
    padding: 8px;
  }

  .titles{
    font-weight: 600;
    background-color: #dddddd;
  }
  
  .information{
    background-color: white
  }
</style>