<template>
  <div class="container mt-5">
    <div class="row justify-content-center">
      <div class="col-md-8">
        <div class="card">
          <div class="card-header">Home</div>

          <div class="card-body">

              <form id="searchForm" @submit.prevent="loadData">
                  <!-- text -->
                  <p>
                    <input type="date" id="startDate" v-model="startDate">
                  </p>

                  <p>
                    <input type="date" id="endDate" v-model="endDate">
                  </p>

                  <button class="btn btn-primary" >Submit</button>
              </form>
              

              <div v-if="this.results">
                  <ul>
                    <li v-for="(result, key) in this.results"  v-bind:key="result">
                      Date: {{ key }}
                      <ul>
                        <li v-for="item in result"  v-bind:key="item">
                          Name : {{ item.name }}
                          ID : {{ item.id }}
                        </li>
                      </ul>
                    </li>
                  </ul>
              </div>

              <div v-if="!this.results">
                  <p>No records found</p>
              </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<!-- Vue js JavaScript -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

<!-- Axios js JavaScript -->
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>


<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.9.4/Chart.js"></script>

<script>

    const axios = require('axios').default;

export default {

  data: function(){
      return {
          results: {},
          startDate:null,
          endDate:null,
          nasaApiKey: '10LgANZ6AsSpKbthTAA6l3xVMYhPJDnnkpwKcffc',
          url: null,
      }
  },

  mounted(){
      //this.loadData();
  },

  methods: {
    loadData: function(){

      if(this.startDate == null || this.endDate == null){
          alert("Start date and End date can not be null.");
          return;
      }

      if(this.startDate > this.endDate){
          alert("Start date can not be greater than End date.");
          return;
      }

      const start = new Date(this.startDate);
      const end = new Date(this.endDate);
      const diffTime = Math.abs(end - start);
      const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24)); 
      if(diffDays > 7){
          alert("Date difference can not be greater than 7 days.");
          return;
      }

      this.url = 'https://api.nasa.gov/neo/rest/v1/feed?start_date='+ this.startDate +'&end_date='+ this.endDate +'&api_key=' + this.nasaApiKey;

        axios.get(this.url).then((response) => {
          this.results = response.data.near_earth_objects;
          console.log(this.results);
        })
        .catch((error) => {
          console.log(error);
        });
    }
  }
    
    
}
</script>