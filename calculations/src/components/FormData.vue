<template>
    <form @submit.prevent class="formCalculation">
        <div class="questionCalculation" v-for="question in questions" :key="question.id" :name="question.name">
                <label>{{question.paragraph}} </label>
                <input v-model="question.value" 
                class="questionInput" type="number"
                step="0.1">
        </div>
        <div class="valveType">
            <p>Клапан:<b>*</b></p>
            <select class="valveOption" v-model="selected" required>
                <option  v-for="valve in valveType" :key="valve.id" :name="valve.name" v-bind:value="{ text: valve.name, chars: valve.chars }">{{ valve.name }}</option>
            </select>
        </div>
        <button class="formCalculationButton" @click="this.selected.text? this.calculate(): null">Посчитать</button>
    </form> 
</template>
<script>
export default {
    data() {
        return{
            res: {},
            selected: {},
            coords: {}
        }
    },
    props:{
        questions:{
            type:Array
        },
        valveType:{
            type:Array
        }
    },
    methods: {
      calculate(){
        let Q = this.questions[0].value
        let V = this.questions[1].value
        let tj = this.questions[2].value
        let t0 = this.questions[3].value
        let L = this.questions[4].value
        let W = this.questions[5].value
        let Tinput = this.questions[6].value
        let Toutput = this.questions[7].value
        let TinNew = this.questions[8].value
        let ToutNew = this.questions[9].value
        let a
        if (t0 >= 0){
                a = 2.05;
        } else if(-5 <= t0  && t0<0){
                a = 1.67;
        } else if(-10 <= t0 && t0<5){
                a = 1.45;
        } else if(-15 <= t0 && t0<10){
                a = 1.29;
         } else if(-20 <= t0 && t0<15){
                a = 1.17;
        } else if(-25 <= t0 && t0<20){
                a = 1.08;
        } else if(-30 <= t0 && t0<25){
                a = 1;
        } else if(-35 <= t0 && t0<30){
                a = 0.95;
        } else if(-40 <= t0 && t0<35){   
                a = 0.9;
        } else if(-45 <= t0 && t0<40){        
                a = 0.85;
        } else if(-50 <= t0 && t0<45){ 
                a = 0.82;
        } else if(-55 <= t0 && t0<50){ 
                a = 0.8;
        } 
        this.findK(L,t0,tj,W)
        this.findQ(Q,a,V,tj,t0,this.res.K)
        this.findK(L,t0,tj,W) 
        this.findQnew(a,V,this.res.q,TinNew,ToutNew,this.res.K)
        this.findM(Q,Tinput, Toutput)
        this.res.M1= this.res.M
        this.findM (this.res.Qnew,Tinput,Toutput)
        this.res.M2= this.res.M
        this.$emit('showresults', {data: this.res})  
        this.res.valve = this.selected.chars   
      },
        findK (L,t0,tj,W){
            let K
            if (!L || !W ){
                K = 0.04
            } else {
                K = 10**(-2)*(Math.sqrt((2*9.81*L*(1-(273+t0)/(273+tj)))+W**2))
            }
            this.res.K = K
            console.log ("K=10^(-2)*Math.sqrt(2*9,806*",L,")*(1-273",t0,")/(273+",tj,")+",W,"^2))=", K)
        },
        findQ (Q,a,V,tj,t0,K){
            let q = Q/(a*V*(tj-t0)*(1+K)*10**(-6))
            this.res.q = +q.toFixed(4)  
            console.log ("q=", Q,"/(",a,"*",V,"*(",tj,"-",t0,")*(1+",K,")*10^(-6))=", q)  
        },
        findQnew(a,V,q,tj,t0,K){
            let Qnew = a*V*q*(tj-t0)*(1+K)*10**(-6)
            this.res.Qnew= +Qnew.toFixed(4) 
            console.log ("Qnew=", a,"*",V,"*",q,"*(",tj,"-",t0,")*(1+",K,")*10^(-6)=", Qnew)     
        },
        findM (Q,Tin,Tout){
            let M = Q/(Tin-Tout)
            this.res.M = +M.toFixed(4)
            console.log ("M=", Q,"/(",Tin,"-",Tout,")=",M) 
        }
 } 
}
</script>
<style>
.questionInput{
    padding: 3px;
    margin:10px;
    width: 55px;
    border: 1px solid black;
    text-align: center
}
.formCalculation{
    border: 1px solid black;
    width: 480px;
    padding:10px 20px;
    margin:0 auto
}
.formCalculationButton{
    padding: 10px;
    border-radius:5px;
    margin-top: 15px ;
    width:100%;
    background-color: lightseagreen;
    font-size: 15px;
}
.formCalculationButton:hover{
    cursor:pointer;
    background-color: lightgrey ;
}
.questionCalculation{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    -webkit-appearance: none;
}
.valveType{
    display: flex;
    justify-content: space-between;
    margin:10px 9px 0 0 
}
.valveOption{
    padding: 5px;
    border-radius: 5%;
}
b {
    color:red
}
</style>