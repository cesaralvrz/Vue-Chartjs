<template>
  <div>
  <div id="app" class="container">
    <div class="row">
      <div class="col text-center mt-5">
        <h1>Visualización COVID-19</h1>
      </div>
    </div>

    <div class="row mt-5" v-if="arrTotalDeaths.length > 0">
      <div class="col">
        <h2>Total de muertes</h2>
        <line-chart 
        :chartData="arrTotalDeaths" 
        :options="chartOptions" 
        :chartColors="deathsChartColors" 
        label="muertes">
        </line-chart>
      </div>
    </div>

    <div class="row mt-5" v-if="arrTotalDeaths.length > 0">
      <div class="col">
        <h2>Nuevas muertes</h2>
        <line-chart 
        :chartData="arrNewDeaths" 
        :options="chartOptions" 
        :chartColors="nDeathsChartColors" 
        label="muertes">
        </line-chart>
      </div>
    </div>

    <div class="row mt-5" v-if="arrTotalDeaths.length > 0">
      <div class="col">
        <h2>Total de Casos</h2>
        <line-chart 
        :chartData="arrTotalCases" 
        :options="chartOptions" 
        :chartColors="casesColors" 
        label="casos">
        </line-chart>
      </div>
    </div>

    <div class="row mt-5" v-if="arrTotalDeaths.length > 0">
      <div class="col">
        <h2>Casos diarios</h2>
        <line-chart 
        :chartData="arrNewCases" 
        :options="chartOptions"
        :chartColors="nCasesColors" 
        label="casos">
        </line-chart>
      </div>
    </div>

    <div class="row mt-5">
      <div class="col">
        <h2>Prueba de gráfica de barras</h2>
        <bar-chart></bar-chart>
      </div>
    </div>

  </div>
  </div>
</template>

<script>
// API request
import axios from 'axios';
// Format dates
import moment from 'moment';

import LineChart from './components/LineChart';
import BarChart from './components/BarChart';

export default {
  name: 'App',
  components: {
    LineChart,
    BarChart
  },
  data() {
    return {
      arrTotalDeaths: [],
      deathsChartColors: {
        borderColor: "#077187",
        pointBorderColor: "#0E1428",
        pointBackgroundColor: "#AFD6AC",
        backgroundColor: "#74A57F"
      },
      arrNewDeaths: [],
      nDeathsChartColors: {
        borderColor: "#251F47",
        pointBorderColor: "#260F26",
        pointBackgroundColor: "#858EAB",
        backgroundColor: "#858EAB"
      },
      arrTotalCases: [],
      casesColors: {
        borderColor: "#190B28",
        pointBorderColor: "#190B28",
        pointBackgroundColor: "#E55381",
        backgroundColor: "#E55381"
      },
      arrNewCases: [],
      nCasesColors: {
        borderColor: "#784F41",
        pointBorderColor: "#784F41",
        pointBackgroundColor: "#BBE5ED",
        backgroundColor: "#BBE5ED"
      },
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
      }
    }
  },
  async created(){
    const { data } = await axios.get('https://api.covid19tracking.narrativa.com/api/country/spain/region/madrid?date_from=2021-03-01&date_to=2021-04-16');
    
    console.log(data.dates)

    Object.values(data.dates).forEach(d => {

      //console.log(d.countries.Spain)

      d = d.countries.Spain

      var date = moment(d.date, "YYYY-MM-DD").format("MM/DD");

      const {
        today_deaths,
        today_new_deaths,
        today_confirmed,
        today_new_confirmed
      } = d;

      this.arrTotalDeaths.unshift({total: today_deaths, date});
      this.arrNewDeaths.unshift({date, total: today_new_deaths});
      this.arrTotalCases.unshift({date, total: today_confirmed});
      this.arrNewCases.unshift({date, total:today_new_confirmed});


      //this.arrTotalDeaths.reverse()
    })

  }

}
</script>

<style>
</style>
