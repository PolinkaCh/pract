<template>
    <div class="result">
        <h2>Результат:</h2>
        <table class = "resultTable">
            <tr><th>Фактическая удельная отопительная характеристика здания:</th><td>q = {{ result.q }}</td></tr>
            <tr><th>Новое значение количества теплоты:</th><td>Q = {{ result.Qnew }}</td></tr>
            <tr><th>Расход теплоносителя в 1-м случае:</th><td>M1 = {{ result.M1 }}</td></tr>
            <tr><th>Расход теплоносителя во 2-ом случае:</th><td>M2 = {{ result.M2 }}</td></tr>
            <tr><th>Пропускная способность в первом случае: </th><td>{{ result.valvefir }}</td></tr>
            <tr><th>Пропускная способность во втором случае:</th><td> {{ result.valvesec }}</td></tr>
            <tr><th>Процент открытия клапана во втором случае</th><td> {{ result.final }}%</td></tr>
        </table>
    </div>  
</template>
<script>
export default {
    props:{
        result:{
            type:Object
        }
    },
}
</script>
<style>
.result{
    text-align: center;
    margin: 0px 35px;
    font-size: 20px
}
.result h2{
    margin-bottom:5px
}
.resultTable, th, td{
    border: 1px solid black;
    border-collapse: collapse;
    padding: 10px;
    margin:0 auto
}
th{
    width: 30vw
}
td{
    width: 15vw
}
</style>