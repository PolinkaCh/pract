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
        this.sort
        return {
            labels:  this.sort.k,
            datasets: [
                {
                data: this.sort.l
                }
            ]
        }
       },
       sort(){
        const dataLabel = Object.keys(this.result.valve)
        const dataValues = Object.values (this.result.valve)
        let merged = dataValues.map((value,i)=>{
            return{"value": dataValues[i], "key":dataLabel[i]}
        })
        const sort = merged.sort(function(a,b){
            return a.key-b.key
        })
        const k =[];
        const l =[]; 
        let i = 0
        for (i; i<sort.length;i++){
            k.push(sort[i].key)
            l.push(sort[i].value)
        }
        console.log(sort, k,l)
        return {k,l}
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
            plugins: {
                title: {
                    display: true,
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
    max-height: max-content;
	margin: 0 auto;
  }
  </style>

