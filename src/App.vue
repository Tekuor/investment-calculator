<template>
  <div id="app">
    <div class="topnav">
      <a>Brand name</a>
    </div>
    <div id="main1">
      <div class="header__bg"></div>
      <div>
        <h1>Investment Calculator</h1>
        <p>Want to know how much my principal can grow after a duration</p>
        <div class="grid-container" style="position:relative; height:100%; width:70%;padding-top:10px;margin: auto;">
          <div style="height:100%"> 
            <div style="background-color:#FFFFFF; width:100%; height:100%; padding-top: 1px;">
              <div :class="{ active: calcOption == 'principal',inactive: calcOption != 'principal' }" style="cursor:pointer;border-radius:6px" id="option1" @click="changeOption('principal')">
                <el-row style="padding-top:10px;padding-left:10px">
                  <el-col :span="2">
                    <input type="radio" id="option" name="option" value="principal" v-model="calcOption">
                  </el-col>
                  <el-col :span="20">
                    <label for="option">I want to know how much my principal can grow after a duration.</label>
                  </el-col>
                </el-row>
              </div>

              <div :class="{ active: calcOption == 'target',inactive: calcOption != 'target' }" id="option2" style="cursor:pointer;border-radius:6px" @click="changeOption('target')">
                <el-row style="padding-top:10px;padding-left:10px">
                  <el-col :span="2">
                    <input type="radio" id="optionC" name="option" value="target" v-model="calcOption">
                  </el-col>
                  <el-col :span="20">
                    <label for="optionC">I want to know how much to invest to get a target amount after a duration.</label>
                  </el-col>
                </el-row>
              </div>

              <div id="principal" v-if="calcOption === 'principal'">
                <label for="principal" style="color:#3A3A3AB3">Principal (Amount to invest)</label><br/>
                <input type="number" min="1" step="any" id="principalValue" v-model="principal">
              </div>

              <div id="principal" v-if="calcOption === 'target'">
                <label for="principal" style="color:#3A3A3AB3">Target Amount</label><br/>
                <input type="number" min="1" step="any" id="principalValue" v-model="futureValue">
              </div>
              
              <div id="principal">
              <label for="principal" style="color:#3A3A3AB3">Period</label>
                <div>
                <input type="number" min="1" step="any" id="periodValue" v-model="period">
                </div>
              </div>

              <div id="years">
                <el-row>
                  <el-col :lg="4" style="cursor:pointer" :class="{ active: period == 1,inactiveYear: period != 1 }" id="yearOption">
                    <div v-on:click="changePeriod(1)">1 year</div>
                  </el-col>
                  <el-col :lg="4" style="cursor:pointer" :class="{ active: period == 2,inactiveYear: period != 2 }" id="yearOption">
                    <div v-on:click="changePeriod(2)">2 years</div>
                  </el-col>
                  <el-col :lg="4" style="cursor:pointer" :class="{ active: period == 3,inactiveYear: period != 3 }" id="yearOption">
                    <div v-on:click="changePeriod(3)">3 years</div>
                  </el-col>
                  <el-col :lg="4" style="cursor:pointer" :class="{ active: period == 4,inactiveYear: period != 4 }" id="yearOption">
                    <div v-on:click="changePeriod(4)">4 years</div>
                  </el-col>
                  <el-col :lg="4" style="cursor:pointer" :class="{ active: period == 5,inactiveYear: period != 5 }" id="yearOption">
                    <div v-on:click="changePeriod(5)">5 years</div>
                  </el-col>
                </el-row>
              </div>

              <div id="principal">
                <label for="principal" style="color:#3A3A3AB3">Interest</label><br/>
                <div class="percent">
                <input type="number" min="1" step="any" id="principalValue" v-model="interest">
                </div>
              </div>

              <div id="calcBtn" style="padding-bottom:20px">
                <button type="button" v-on:click="calculate()" :disabled="disableCalculate">Calculate</button>
              </div>
            </div>
          </div>
          <div  style="height:100%"> 
            <div style="background-color:#FFFFFF; width:100%; height:100%" v-if="!show && !show2">
              <img src="./assets/invest.png" width="100%" style="padding-top:20%" /> 
            </div>
            <transition name="slide-fade">
              <div style="background-color:#FFFFFF; width:100%; height:100%" v-if="show">
                <div style="color:#3F845C;text-align: center;font-weight: bold;padding-top:40px;padding-bottom:20px">GHC {{result}}</div>
                <div style="width:400px; height:400px;margin:auto">
                  <line-chart :chart-data="datacollection" :options="options"></line-chart>
                </div>
              </div>
            </transition>

            <transition name="slide-fade">
              <div class="small" style="background-color:#FFFFFF; width:100%; height:100%" v-if="show2">
                  You will need to invest <span style="text-align: center;color:#3F845C;font-weight: bold">GHC {{result}}</span> to make {{futureValue}} after {{period}} years at a rate of {{interest}}
              </div>
            </transition>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import LineChart from './LineChart.js'

export default {
  name: 'App',
  components: {
    LineChart
  },
  data () {
      return {
        datacollection: null,
        options: {
          legend:{
            position: 'top'
          },
          scales: {
            xAxes: [{
                gridLines: {
                  display:false
                }
            }],
            yAxes: [{
                gridLines: {
                  display:false
                },
                ticks: {
                    display: false
                } 
            }]
          },
          responsive: true
        },
        show: false,
        principal: "",
        period: '',
        interest: "",
        futureValue: "",
        result: "",
        calcOption: "principal",
        totalInterest: "",
        show2: false,
        time: "years"
      }
    },
    watch: {
    datacollection: {
      deep: true,
      handler(val) {
        console.log("check",val)
      }
    }
  },
  methods: {
      fillData () {
        this.datacollection = {
          labels: ['January', 'February'],
          datasets: [
            {
              label: 'Total Interest',
              backgroundColor: '#FFEFDA',
              data: [this.getRandomInt(), this.getRandomInt()]
            }, {
              label: 'Total contributions',
              backgroundColor: '#3F845C80',
              data: [this.getRandomInt(), this.getRandomInt()]
            }
          ]
        }
      },
      showChart(){
        this.show = !this.show
      },
      calculate(){
        if(this.calcOption === 'principal'){
          let results = [0]
          let contributions = [0]
          let labels = [0]
          for(let i = 1; i <= this.period; i++){
            const result = this.principal*Math.pow((1+(this.interest/100)),i)
            results.push(result.toFixed(2))
            labels.push(`Year ${i}`)
            this.result = result.toFixed(2)
            let totalInterest = (this.result - this.principal).toFixed(2)
            contributions.push(this.principal)
            this.totalInterest = totalInterest
          }

          this.datacollection = {
          labels: labels,
          datasets: [
            {
              label: 'Future Value',
              backgroundColor: "rgba(255, 224, 181, 0.4)",
              data: results,
              fill: true,
              lineTension: 0.1,
              borderCapStyle: 'square',
              borderDashOffset: 0.0,
              borderJoinStyle: 'miter',
              pointBorderWidth: 1,
              pointHoverRadius: 8,
              pointHoverBorderWidth: 2,
              pointRadius: 4,
              pointHitRadius: 10
            },
            {
              label: 'Contributions',
              backgroundColor: "rgba(63, 132, 92, 0.4)",
              data: contributions,
              fill: true,
              lineTension: 0.1,
              borderCapStyle: 'square',
              borderDashOffset: 0.0,
              borderJoinStyle: 'miter',
              pointBorderWidth: 1,
              pointHoverRadius: 8,
              pointHoverBorderWidth: 2,
              pointRadius: 4,
              pointHitRadius: 10
            }
          ]
        }
          this.show2 = false
          this.show = true
        }else {
          const res = this.futureValue/Math.pow((1+(this.interest/100)),this.period)
          this.result = res.toFixed(2)
          let results = [0]
          let principal = [0]
          let labels = [0]
          for(let i = 1; i <= this.period; i++){
            const result = res*Math.pow((1+(this.interest/100)),i)
            results.push(result.toFixed(2))
            labels.push(`Year ${i}`)
            console.log("interest",this.result)
            principal.push(this.result)
          }

          this.datacollection = {
          labels: labels,
          datasets: [
            {
              label: 'Future Value',
              backgroundColor: "rgba(255, 224, 181, 0.4)",
              data: results,
              fill: true,
              lineTension: 0.1,
              borderCapStyle: 'square',
              borderDashOffset: 0.0,
              borderJoinStyle: 'miter',
              pointBorderWidth: 1,
              pointHoverRadius: 8,
              pointHoverBorderWidth: 2,
              pointRadius: 4,
              pointHitRadius: 10
            },
            {
              label: 'Contributions',
              backgroundColor: "rgba(63, 132, 92, 0.4)",
              data: principal,
              fill: true,
              lineTension: 0.1,
              borderCapStyle: 'square',
              borderDashOffset: 0.0,
              borderJoinStyle: 'miter',
              pointBorderWidth: 1,
              pointHoverRadius: 8,
              pointHoverBorderWidth: 2,
              pointRadius: 4,
              pointHitRadius: 10
            }
          ]
        }
          this.show2 = false
          this.show = true
        }
      },
      changePeriod(num){
        this.period = num
      },
      changeOption(value){
        this.calcOption = value
        this.principal = ""
        this.period = ""
        this.interest = ""
        this.futureValue = ""
      }
  },
  computed:{
    disableCalculate(){
      if(this.calcOption === 'principal' && (!this.period || !this.principal || !this.interest)){
        return true
      }
      if(this.calcOption === 'target' && (!this.futureValue || !this.period || !this.interest)){
        return true
      }
      return false
    }
  }
}
</script>

<style>
#app {
  height: 100%
}

.active {
  background-color:#1593EF1A; 
  color:#107ECE
}

.inactive {
  background-color:#F4F4F440; 
  color:#3A3A3AB3; 
  border: 1px solid #3A3A3A1A;
}

.inactiveYear {
  border: 1px solid #70707080; 
  color:#3A3A3AB3;
}

#main1 {
  position: relative;
  height: 100%;
  overflow: hidden;
}

.header__bg {
  position: absolute;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  width: 100%;
  height: 50%;
  background-image: linear-gradient(#3F845C, #3F845C);
  transform: skewY(-7deg);
  transform-origin: top left;
  padding: 0 0 0 0;
  z-index: -100;
}

.content{;
    position: relative;
    z-index: 100;
}

/* Add a black background color to the top navigation */
.topnav {
  background-color: white;
  overflow: hidden;
}

/* Style the links inside the navigation bar */
.topnav a {
  float: left;
  color: black;
  text-align: center;
  padding: 14px 16px;
  padding-left: 8%;
  font-weight: bold;
  font-size: 13px;
}

div h1, p {
  position: relative;
  color: #FFE0B5;
  padding-left: 15%;
}

div p {
  font-size: 13px;
}

#check {
  position: relative;
  height: 70%;
  width: 100%;
  background-color: white;
  /* margin-left: 40%; */
  margin-top: 3%;
  overflow: auto;
}

#option1 {
  position: relative;
  height: 65px;
  width: 70%;
  margin: auto;
  margin-top: 5%;
  font-size: 15px;
}

#option2 {
  position: relative;
  height: 65px;
  width: 70%;
  /* background-color: #3A3A3A1A; */
  margin: auto;
  margin-top: 5%;
  font-size: 15px;
}

#principal {
  position: relative;
  width: 70%;
  margin: auto;
  margin-top: 5%;
  font-size: 15px;
}

#principalValue {
  width: 100%;
  height: 35px;
  background-color: #B9B9B91A;
  border: 0;
}

#periodValue {
  width: 100%;
  height: 35px;
  background-color: #B9B9B91A;
  border: 0;
  border-radius: 6px 0px 0px 6px;
}

#calcBtn {
  position: relative;
  width: 70%;
  margin: auto;
  margin-top: 5%;
  font-size: 15px;
}

button:active,
button:focus {
  outline-color: #fcd39a 
}

button:hover {
  cursor:pointer
}

button:disabled {
  opacity: 0.6;
  color: grey;
  pointer-events: none;
}

input:focus {
  outline-color: #b9b9b9 
}

button {
  width: 100%;
  height: 40px;
  background-color: #FFE0B5;
  border: 0;
  font-weight: 
}

#years {
  position: relative;
  /* height: 65px; */
  width: 70%;
  margin: auto;
  margin-top: 2%;
  font-size: 15px;
}

#yearOption {
  font-size:15px; 
  height:30px;
  width: 64px;
  text-align: center;
  margin-right: 10px;
  border-radius: 6px;
  padding-top: 3px;
}

/* Enter and leave animations can use different */
/* durations and timing functions.              */
.slide-fade-enter-active {
  transition: all .10s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
.grid-container {
  display: grid;
  grid-template-columns: 50% 50%;
  padding: 10px;
}
input {
  text-indent:15px;
  border-radius: 6px;
}

#option1:hover, #option2:hover {
  background-color:#1593EF1A; 
  color:#107ECE
}

#yearOption:hover {
  background-color:#1593EF1A; 
  color:#107ECE;
  border-color:#1593EF1A;
}

.percent {
  display: inline-block;
  position: relative;
  width: 100%;
}

.percent::after {
  position: absolute;
  top: 7px;
  right: .5em;
  transition: all .05s ease-in-out;
  content: '%';
}

.percent:hover::after,
.percent:focus-within::after {
  right: 1.5em;
}
</style>
