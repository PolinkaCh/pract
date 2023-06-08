<template>
    <div>
        <h1 class="header">Регулирование потребления тепловой энергии здания</h1>
        <calc-form :questions="questions" @showresults="showRes"/>
        <div v-if=result><result :result="result"/></div>
    </div>
    
</template>

<script>
import CalcForm from './components/FormData.vue';
import Result from './components/ResultInfo.vue';
export default{
    components:{
        CalcForm,Result
    },
    data(){
        return{
            questions:[
            {
                "id":1,
                "name": "numberHeat",
                "paragraph": "Количество теплоты с счетчика (Гкал/час) ",
                "value": ""
            },
            {
                "id":2,
                "name": "volume",
                "paragraph": "Строительный объем здания (м³) ",
                "value": ""
            },
            {
                "id":3,
                "name": "inTemp",
                "paragraph": "Фактическая температура (°С) ",
                "value": ""
            },
            {
                "id":4,
                "name": "outTemp",
                "paragraph": "Наружная температура (°С) ",
                "value": ""
            },
            {
                "id":5,
                "name": "filter",
                "paragraph": "Свободная высота здания (м)",
                "value": ""
            },
            {
                "id":6,
                "name": "wind",
                "paragraph": "Скорость ветра (м/с) ",
                "value": ""
            },
            {
                "id":7,
                "name": "tInput",
                "paragraph": "Температура в подающем трубопроводе (°С)",
                "value": ""
            },
            {
                "id":8,
                "name": "tOutput",
                "paragraph": "Температура в обратном трубопроводе (°С)",
                "value": ""
            }
            ],
            result: false
        }
    },
    methods:{
        showRes(q){
        console.log(q.data)
        this.result = q.data
        }
    }
}
</script>

<style>
*{
    margin:0;
    padding: 0;
    box-sizing:border-box;
}
.header{
    text-align: center;
    margin: 20px ;
}
</style>