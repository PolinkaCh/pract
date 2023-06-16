<template>
    <div class="lineInfo">
        <Line :data="chartData" :options="options"/>
    </div>
</template>
  
  <script>
  // DataPage.vue
  import { Line } from 'vue-chartjs'
  import { Chart as ChartJS, PointElement, Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale } from 'chart.js'
  
  ChartJS.register(PointElement,Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale)


  export default {
    name: 'LineChart',
    components: { Line },
    computed: {
    
      chartData() {  
        return {
            labels:  this.result.Kdiag,
            datasets: [
                {
                data: this.result.Vdiag
                }
            ]
        }
       }
    },
    props:{
        result:{
            type:Object
        }
    },
    data() {
      return {
        options: {
            maintainAspectRatio: false,
            responsive: true,
            aspectRatio: 1|1,
            plugins: {
                title: {
                    display: false,
                    text: 'График зависимости регулирования',
                    font: {
                        size: 18
                    }
                },
                legend: {
                display: false,}
            }
        }
      }
    }
  }
  </script>
  <style scoped>
  .lineInfo{
    max-width:300px;
    max-height: 350px;
	margin: 0px auto;
  }

  </style>

