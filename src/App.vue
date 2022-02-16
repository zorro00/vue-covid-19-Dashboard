<template>
  <div id="app" class="container">
    <div class="row mt-5">
      <div class="col text-center">
        <h1>COVID-19 Data Visualization</h1>
      </div>
    </div>

    <div class="row mt-5" v-if="arrPositive.length > 0">
      <div class="col">
        <h2>Positive</h2>
        <line-chart
          :char-data="arrPositive"
          :options="chartOptions"
          label="Positive"
          :chartColors="positiveChartColors"
        ></line-chart>
      </div>
    </div>

    <div class="row mt-5" v-if="arrHospitalized.length > 0">
      <div class="col">
        <h2>Hospitalized</h2>
        <line-chart
          :char-data="arrHospitalized"
          :options="chartOptions"
          label="Hospitalized"
          :chartColors="hospitalizedChartColors"
        ></line-chart>
      </div>
    </div>

    <div class="row mt-5" v-if="arrInIcu.length > 0">
      <div class="col">
        <h2>In ICU</h2>
        <line-chart
          :char-data="arrInIcu"
          :options="chartOptions"
          label="In ICU"
          :chartColors="inIcuChartColors"
        ></line-chart>
      </div>
    </div>

    <div class="row mt-5" v-if="arrOnVentilators.length > 0">
      <div class="col">
        <h2>On Ventilators</h2>
        <line-chart
          :char-data="arrOnVentilators"
          :options="chartOptions"
          label="On Ventilators"
          :chartColors="OnVentilatorsChartColors"
        ></line-chart>
      </div>
    </div>

    <div class="row mt-5" v-if="arrRecovered.length > 0">
      <div class="col">
        <h2>Recovered</h2>
        <line-chart
          :char-data="arrRecovered"
          :options="chartOptions"
          label="Recovered"
          :chartColors="recoveredChartColors"
        ></line-chart>
      </div>
    </div>

    <div class="row mt-5" v-if="arrDeaths.length > 0">
      <div class="col">
        <h2>Deaths</h2>
        <line-chart
          :char-data="arrDeaths"
          :options="chartOptions"
          label="Deaths"
          :chartColors="deathsChartColors"
        ></line-chart>
      </div>
    </div>


  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
import LineChart from "./components/LineChart";

export default {
  name: "App",
  components: {
    LineChart,
  },
  data() {
    return {
      arrInIcu: [],
      arrDeaths: [],
      arrPositive: [],
      arrRecovered: [],
      arrHospitalized: [],
      arrOnVentilators: [],
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
      },
      positiveChartColors:{
        borderColor: '#077187',
        pointBackgroundColor: '#AFD6AC',
        pointBorderColor: '#0E1428',
        backgroundColor: '#74A57F',
      } ,
      OnVentilatorsChartColors:{
        borderColor: '#ffc857',
        pointBackgroundColor: '#e9724c',
        pointBorderColor: '#c5283d',
        backgroundColor: '#481d24',
      }, 
      hospitalizedChartColors:{
        borderColor: '#606c38',
        pointBackgroundColor: '#283618',
        pointBorderColor: '#fefae0',
        backgroundColor: '#dda15e',
      } ,
      recoveredChartColors:{
        borderColor: '#6b2737',
        pointBackgroundColor: '#e08e45',
        pointBorderColor: '#f8f4a6',
        backgroundColor: '#3943b7',
      } ,
      deathsChartColors:{
        borderColor: '#96acb7',
        pointBackgroundColor: '#40376e',
        pointBorderColor: '#48233c',
        backgroundColor: '#36558f',
      }, 
      inIcuChartColors:{
        borderColor: '#0d1821',
        pointBackgroundColor: '#b4cded',
        pointBorderColor: '#bfcc94',
        backgroundColor: '#f0f4ef',
      } 
    };
  },
  async created() {
    const { data } = await axios.get(
      "https://api.covidtracking.com/v1/states/current.json"
    );
    data.forEach((d) => {
      const date = moment(d.date, "YYYYMMDD").format("MM/DD");
      const {
        positive,
        inIcuCurrently,
        hospitalizedCurrently,
        onVentilatorCurrently,
        recovered,
        death,
      } = d;

      this.arrDeaths.push({ date, total: death });
      this.arrPositive.push({ date, total: positive });
      this.arrRecovered.push({ date, total: recovered });
      this.arrInIcu.push({ date, total: inIcuCurrently });
      this.arrHospitalized.push({ date, total: hospitalizedCurrently });
      this.arrOnVentilators.push({ date, total: onVentilatorCurrently });
    });
  },
};
</script>

<style></style>
