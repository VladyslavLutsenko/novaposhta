<template>
  <div class="hello">
    <div>
      <p>Введіть населений пункт</p>
      <input type="text" v-model="searchString">
    </div>
    
    <!-- <div v-for="state in foundState" :key="state.Ref">
      <h4 @click="selectState(state)" class="pointer">{{state.Description}}</h4>

    </div> -->
    <template v-if="!cityIsSelected">
      <div v-for="city in foundCity" :key="city.Ref">
        <h4 @click="selectCity(city)" class="pointer">{{city.Description}}</h4>

      </div>
    </template>
    <div v-for="warehouse in warehouseInfo" :key="warehouse.Ref">
      <h5 class="warehouse">{{warehouse.Description}}</h5>

    </div>
    
    
  </div>
</template>

<script>
export default {
  name: 'NovaPoshta',
  data(){
    return {
      url: 'https://api.novaposhta.ua/v2.0/json/',
      key: "9a557481f95094531372a9d1b55222c8",
      stateInfo: [],
      cityInfo: [],
      warehouseInfo: [],
      searchString: "",
      foundState:[],
      foundCity:[],
      cityIsSelected: false,
      selectedCity: {}
    }
  },
  mounted: function(){
    this.getStates();
    this.getCities();

  },
  watch: {
    searchString: function(val){
      this.warehouseInfo=[];
      if (val) {
        this.findCity(val);
      }
      else{
        this.foundCity=[];
      }
      this.cityIsSelected=false;
    },
    selectedCity: function () {
      this.getWarehouses(this.selectedCity.Ref);
    }
  },
  methods: {
    getStates(){
      var body={
        "apiKey": this.key,
        "modelName": "Address",
        "calledMethod": "getAreas",
        "methodProperties": {}
      }
      this.axios.post(this.url, body).then((response) =>{
        this.stateInfo=response.data.data;
        console.log(this.stateInfo);
      })
    },
    getCities(){
      var body={
        "modelName": "Address",
        "calledMethod": "getCities",
        "methodProperties": {
          
        },
        "apiKey": this.key
      }
      this.axios.post(this.url, body).then((response) =>{
        console.log(response.data);
        this.cityInfo=response.data.data;
        console.log(this.cityInfo);
      })
    },
    getWarehouses(ref){
      var body={
        "apiKey": this.key,
        "modelName": "AddressGeneral",
        "calledMethod": "getWarehouses",
        "methodProperties": {
            "Language": "ru",
            "CityRef" : ref
        }
      }
      this.axios.post(this.url, body).then((response) =>{
        this.warehouseInfo=response.data.data;
      })

    },
    findState(val){
      this.foundState=[];
      this.stateInfo.forEach(element => {
        if (element.Description.toUpperCase().includes(val.toUpperCase())) {
          this.foundState.push(element);
        }
      });
    },
    findCity(val){
      this.foundCity=[];
      
      this.cityInfo.forEach(element => {
        var temp;
        for (let i = 0; i < val.length; i++) {
          if (element.Description[i].toUpperCase()==val[i].toUpperCase()) {
            temp=true;
          }
          else{
            temp=false;
            break;
          }
        }
        if (temp) {
          this.foundCity.push(element);
        }
      });
    },
    selectState(state){
      this.searchString=state.Description;
    },
    selectCity(city){
      this.selectedCity=city;
      console.log(this.selectedCity);
      this.searchString=city.Description;
      setTimeout(()=>{
        this.cityIsSelected=true;
      }, 10);
    }
  
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.pointer {
  cursor: pointer;
}
.warehouse{
  text-align: center;
  color: rgb(63, 63, 63);
}
</style>
