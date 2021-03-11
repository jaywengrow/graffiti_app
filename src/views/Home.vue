<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <button v-on:click="getGraffiti()">Get Data</button>
    <line-chart :chart-data="dataCollection"></line-chart>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
import LineChart from '../components/LineChart.js'

export default {
  components: {
      LineChart
  },
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      wards: [],
      dataCollection: null
    };
  },
  created: function() {},
  mounted: function() {
    this.getGraffiti()
  },
  methods: {
    getGraffiti: function() {
      axios.get("https://data.cityofchicago.org/resource/hec5-y4x5.json").then(response => {
        this.groupIntoWards(response.data);
        this.fillChartData();
      })
    },
    wardLabels: function() {
      let labels = [];
      for(let i = 1; i <= 50; i++) {
        labels.push("Ward " + i);
      }
      return labels;
    },
    groupIntoWards: function(graffitiCalls) {
      for(const graffitiCall of graffitiCalls) {
        let ward = parseInt(graffitiCall.ward);
        if(this.wards[ward]) {
          this.wards[ward] += 1;
        } else {
          this.wards[ward] = 1
        }
      }
    },
    fillChartData: function() {
      this.dataCollection = {
          labels: this.wardLabels(),
          datasets: [
            {
              label: '# of Graffiti Calls',
              backgroundColor: '#f87979',
              data: this.wards
            }
          ]
        }
    }
  },
  
};
</script>