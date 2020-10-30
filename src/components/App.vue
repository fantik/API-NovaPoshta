<template>
    <div>
      <h1>API NovaPoshta</h1>
      <div class="block">
        <div>
          <p>Область: </p>
          <select v-model="selectedSettlement" @change = "getCities()" style="width: 260.766px">
            <option disabled value="">Варiанти</option>
            <option v-for = "(settlement) in settlements" :value="settlement.Ref" :key="settlement.Ref">{{settlement.Description}}</option>
          </select>
        </div>
        <div>
          <p>Мiсто: </p>
          <select v-model="selectedCity" @change = "getWare()" style="width: 305.438px">
            <option disabled value="">Варiанти</option>
            <option v-for = "(citi) in cities" :value="citi.Ref" :key="citi.Ref">{{citi.Description}}</option>
          </select>
        </div>
        <div>
          <p>Пункт видачi: </p>
          <select v-model="selectedWare" style="width: 189.344px"> 
            <option disabled value="">Варiанти</option>
            <option v-for = "(ware) in wares" :value="ware.Ref" :key="ware.Ref">{{ware.Description}}</option>
          </select>
        </div>      
      </div>
    </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'

const HOST = "https://api.novaposhta.ua/v2.0/json/"
const API_KEY = "1041994827782795ee20c1576ff9cef8"

export default {
    data(){  
        return {
          settlements: [],
          selectedSettlement: '',
          cities: [],
          selectedCity: '',
          wares: [],
          selectedWare: ''
        }
    },
    mounted: function() {
      axios.post(HOST,
      {
        "apiKey": API_KEY,
        "modelName": "Address",
        "calledMethod": "getAreas",
        "methodProperties": {}
      })
      .then(response => {       
        this.settlements = response.data.data
      })
    },
    methods: {
      getCities(){
            axios.post(HOST,{
              "apiKey": API_KEY,
              "modelName": "Address",
              "calledMethod": "getCities",
              "methodProperties":{"AreaRef":this.selectedSettlement}
              })
            .then(response=>{
              this.selectedCity = ''
              this.selectedWare = ''
              this.cities=response.data.data
              })
        },
        getWare(){
          axios.post(HOST,{
              apiKey: API_KEY,
              modelName: "AddressGeneral",
              calledMethod: "getWarehouses",
              "methodProperties":{"CityRef":this.selectedCity}
          }).then(response=>{
            this.wares=response.data.data
            })
        }
    }
}
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    font-family: 'Comfortaa';
  }
  h1 {
    display: flex;
    justify-content: center;
    width: 500px;    
    margin: 100px auto;
    font-size: 40px;
    font-weight: 300;
    padding: 15px;
    text-align: center;
    border-radius: 15px;
    background-color: #f3f9eb;
    border: solid 1px #d9d9d9;
  }
  body {
    background: url('../image/background.jpg');
  }
  .block {
    width: 500px;
    margin: 00px auto;
    display: flex;
    flex-direction: column;
    justify-content: center;
    background-color: #f3f9eb;
    padding: 25px 30px;
    border-radius: 15px;
    border: solid 1px #d9d9d9;
  }
  .block > div {
    display: flex;
    font-size: 30px;
  }
  .block > div > p {
    flex-shrink: 0;
  }
  .block > div > select {
    margin-left: 20px;
    margin-top: 3px;
    flex-grow: 1;
  }
</style>