<template>
  <v-expansion-panels v-model="panel" class="mb-3">
    <v-expansion-panel>
      <v-expansion-panel-header>体温曲线</v-expansion-panel-header>
      <v-expansion-panel-content>
        <span class="text-body-2 grey--text">诊断中的体温记录将会被绘制在这里。</span>
        <v-fade-transition>
          <line-chart  v-if="diagnosisItems.length > 0" :chart-data="chartData" height=80></line-chart>
        </v-fade-transition>
      </v-expansion-panel-content>
    </v-expansion-panel>
  </v-expansion-panels>
</template>

<script>
  import LineChart from "./LineChart.js"
  export default {
    name: 'TemperatureChart',
    components: {LineChart},
    
    props:{
      diagnosisItems:Array,
      chartData:Array
    } ,
    
    data() {
      return {
        panel: 0
      }
    },
    
    computed: {
      chartData() {
        let chartData = {
          labels: [],
          datasets: [
            {
              label: "体温",
              backgroundColor: 'rgba(0,0,0,0)',
              borderColor: 'rgba(199,0,0,0.6)',
              cubicInterpolationMode: 'monotone',
              data:[]
            }
          ]
        };
        
        this.diagnosisItems.sort((a, b) => {
          let dateA = new Date(a.time);
          let dateB = new Date(b.time);
          return dateA - dateB;
        })
        this.diagnosisItems.forEach(one => {
          if(one.time)
            chartData.labels.push(one.time.substring(5));
          chartData.datasets[0].data.push(one.temperature);
        })

        return chartData;
      }
    }
  }
</script>

<style scoped>

</style>
