<template>
    <form @submit.prevent class="formCalculation">
        <div class="questionCalculation" v-for="question in questions" :key="question.id" :name="question.name">
                <label>{{question.paragraph}} </label>
                <input v-model="question.value" 
                class="questionInput" type="number"
                step="0.1">
        </div>
        <button class="formCalculationButton" @click="calculate">Посчитать</button>
    </form> 
</template>
<script>
import axios from 'axios'
export default {
    data() {
        return{
            res: {},
            coords: {}
        }
    },
    props:{
        questions:{
            type:Array
        }
    },
    methods: {
      async calculate(){
        this.getDate()
       // await this.findWeather()
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
      },
        findK (L,t0,tj,W){
            let K = 10**(-2)*(Math.sqrt((2*9.81*L*(1-(273+t0)/(273+tj)))+W**2))
            //let K = 0.04
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
        },

        async findWeather(){
        try {
            const pos = await new Promise((resolve, reject) => {
            navigator.geolocation.getCurrentPosition(resolve, reject);
            });
            const latitude  = pos.coords.latitude;
            const longitude = pos.coords.longitude;
            this.coords = {latitude, longitude} 
        } 
        catch (error) {
            console.log('Невозможно получить ваше местоположение', error);
        } 
        await axios.get ('http://api.openweathermap.org/data/2.5/forecast?lat='+`${this.coords.latitude}`+'&lon='+`${this.coords.longitude}`+'&units=metric&appid=ef66adfcbc80be689c655a64edf759f0',
            {
                headers: {'Content-Type': 'application/json'}
            })
            .then ((response)=>{      
                this.res.tempFuture = response.data.list[this.res.i].main.temp
                this.res.windFuture = response.data.list[this.res.i].wind.speed
                console.log(response.data)
            })
            .catch((error) => { 
            console.log ('Невозможно определить прогноз погоды', error)
            })      
        }, 
        getDate () {
            let date = new Date()
                let current = new Date().getHours()
                date.setHours(current+5)   
                let next = date.getHours() 
                let i
                if (12<=next && next<15){
                    i = 0
                } else if (15<=next && next<18){
                    i = 1
                } else if (18<=next && next<21){
                    i = 2
                } else if (21<=next && next<24){
                    i = 3
                } else if (0<=next && next<3){
                    i = 4
                } else if (3<=next && next<6){
                    i = 5
                } else if (6<=next && next<9){
                    i = 6
                } else {
                    i = 7
                }
                this.res.i = i
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
    padding:25px;
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
</style>