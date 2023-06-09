<template>
    <div>
        <h1 class="header">Регулирование потребления тепловой энергии здания</h1>
        <div v-bind:class="{ active: result }">
            <calc-form :questions="questions" :valveType="valveType" @showresults="showRes"/>
            <div>
                <div v-if=result><result :result="result"/></div>
                <div v-if=result><Diagram :result="result"/></div>
            </div>
        </div>
    </div>
    
</template>

<script>
import CalcForm from './components/FormData.vue';
import Result from './components/ResultInfo.vue';
import Diagram from './components/DiagramInfo.vue'
export default{
    components:{
        CalcForm,Result, Diagram
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
            },
            {
                "id":9,
                "name": "InTempNew",
                "paragraph": "Расчетная температура в отапливаемом здании (°С)",
                "value": ""
            },
            {
                "id":10,
                "name": "OutTempNew",
                "paragraph": "Наружная температура (для искомого часа) (°С)",
                "value": ""
            }
            ],
            valveType:[
                {
                    "id":1,
                    "name":"SPL AMG2 PRO",
                    "chars":{
                        4: 20,
                        100: 30,
                        160: 40,
                    }
                },
                {
                    "id":2,
                    "name":"VF2/VF3",
                    "chars":{
                        .63: 10,
                        16: 15,
                        63: 20,
                        145: 30,
                        220: 40,
                    }
                }
            ],
            result: false
        }
    },
    methods:{
        showRes(q){
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
    margin: 10px ;
}
.active{
    display: flex;
    justify-content: space-around;
}
</style>