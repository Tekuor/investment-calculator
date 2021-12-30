<template>
  <div id="wrapper">
    <div class="content">
      <div class="heading">
        <h1 class="text-primary">Investment Calculator</h1>
        <p class="text-primary">
          Want to know how much my principal can grow after a duration
        </p>
      </div>

      <b-card class="mb-2 card">
        <b-row>
          <b-col md="5" sm="12">
            <div
              :class="{
                active: calcOption == 'principal',
                inactive: calcOption != 'principal',
              }"
              style="cursor:pointer;border-radius:6px;padding:1em;"
              @click="changeOption('principal')"
            >
              <b-row style="padding: 20px 10px 15px 10px">
                <b-col :cols="1">
                  <input
                    style="cursor:pointer"
                    type="radio"
                    id="option"
                    name="option"
                    value="principal"
                    v-model="calcOption"
                  />
                </b-col>
                <b-col :cols="11">
                  <label for="option" style="cursor:pointer"
                    >I want to know how much my principal can grow after a
                    duration.</label
                  >
                </b-col>
              </b-row>
            </div>

            <div class="mt-3">
              <div
                :class="{
                  active: calcOption == 'target',
                  inactive: calcOption != 'target',
                }"
                id="option2"
                style="cursor:pointer;border-radius:6px"
                @click="changeOption('target')"
              >
                <b-row style="padding: 20px 10px 15px 10px;">
                  <b-col :cols="1">
                    <input
                      style="cursor:pointer"
                      type="radio"
                      id="optionC"
                      name="option"
                      value="target"
                      v-model="calcOption"
                    />
                  </b-col>
                  <b-col :cols="11">
                    <label for="optionC" style="cursor:pointer"
                      >I want to know how much to invest to get a target amount
                      after a duration.</label
                    >
                  </b-col>
                </b-row>
              </div>
            </div>

            <div class="my-3" v-if="calcOption !== 'target'">
              <label for="principal" style="color:#3A3A3AB3;"
                >Principal (Amount to invest)</label
              >
              <b-form-input
                v-model="principal"
                id="principal"
                type="number"
              ></b-form-input>
            </div>

            <div class="my-3" v-else>
              <label for="principal" style="color:#3A3A3AB3"
                >Target Amount</label
              ><br />
              <b-form-input
                type="number"
                min="1"
                step="any"
                id="principalValue"
                v-model="futureValue"
              ></b-form-input>
            </div>

            <div class="my-1">
              <label for="period" style="color:#3A3A3AB3;">Period</label>
              <b-form-input
                id="period"
                type="number"
                v-model="period"
              ></b-form-input>
              <b-row class="mt-2">
                <b-col sm="2" class="mr-2">
                  <b-badge
                    style="width:70px;height:23px;text-align:center;line-height:16px;"
                    v-on:click="changePeriod(1)"
                    :class="{
                      active: selectedPeriod(1),
                      inactive: !selectedPeriod(1),
                    }"
                    href="#"
                    >1 year</b-badge
                  >
                </b-col>

                <b-col sm="2" class="mr-2">
                  <b-badge
                    style="width:70px;height:23px;text-align:center;line-height:16px;"
                    v-on:click="changePeriod(2)"
                    :class="{
                      active: selectedPeriod(2),
                      inactive: !selectedPeriod(2),
                    }"
                    href="#"
                    >2 years</b-badge
                  >
                </b-col>

                <b-col sm="2" class="mr-2">
                  <b-badge
                    style="width:70px;height:23px;text-align:center;line-height:16px;"
                    v-on:click="changePeriod(3)"
                    :class="{
                      active: selectedPeriod(3),
                      inactive: !selectedPeriod(3),
                    }"
                    href="#"
                    >3 years</b-badge
                  >
                </b-col>

                <b-col sm="2" class="mr-2">
                  <b-badge
                    style="width:70px;height:23px;text-align:center;line-height:16px;"
                    v-on:click="changePeriod(4)"
                    :class="{
                      active: selectedPeriod(4),
                      inactive: !selectedPeriod(4),
                    }"
                    href="#"
                    >4 years</b-badge
                  >
                </b-col>

                <b-col sm="2" class="mr-2">
                  <b-badge
                    style="width:70px;height:23px;text-align:center;line-height:16px;"
                    v-on:click="changePeriod(5)"
                    :class="{
                      active: selectedPeriod(5),
                      inactive: !selectedPeriod(5),
                    }"
                    href="#"
                    >5 years</b-badge
                  >
                </b-col>
              </b-row>
            </div>

            <div class="my-3">
              <label for="interest" style="color:#3A3A3AB3;">Interest</label>
              <div class="percent">
                <b-form-input
                  type="number"
                  id="principalValue"
                  v-model="interest"
                ></b-form-input>
              </div>
              <b-row class="mt-2">
                <b-col cols="2" class="mr-2">
                  <b-badge
                    style="width:70px;height:23px;text-align:center;line-height:16px;"
                    href="#"
                    v-on:click="changeInterest(5)"
                    :class="{
                      active: selectedInterest(5),
                      inactive: !selectedInterest(5),
                    }"
                    >5%</b-badge
                  >
                </b-col>

                <b-col cols="2" class="mr-2">
                  <b-badge
                    style="width:70px;height:23px;text-align:center;line-height:16px;"
                    href="#"
                    v-on:click="changeInterest(10)"
                    :class="{
                      active: selectedInterest(10),
                      inactive: !selectedInterest(10),
                    }"
                    >10%</b-badge
                  >
                </b-col>

                <b-col cols="2" class="mr-2">
                  <b-badge
                    style="width:70px;height:23px;text-align:center;line-height:16px;"
                    href="#"
                    v-on:click="changeInterest(15)"
                    :class="{
                      active: selectedInterest(15),
                      inactive: !selectedInterest(15),
                    }"
                    >15%</b-badge
                  >
                </b-col>

                <b-col cols="2" class="mr-2">
                  <b-badge
                    style="width:70px;height:23px;text-align:center;line-height:16px;"
                    href="#"
                    v-on:click="changeInterest(20)"
                    :class="{
                      active: selectedInterest(20),
                      inactive: !selectedInterest(20),
                    }"
                    >20%</b-badge
                  >
                </b-col>

                <b-col cols="2">
                  <b-badge
                    style="width:70px;height:23px;text-align:center;line-height:16px;"
                    href="#"
                    v-on:click="changeInterest(25)"
                    :class="{
                      active: selectedInterest(25),
                      inactive: !selectedInterest(25),
                    }"
                    >25%</b-badge
                  >
                </b-col>
              </b-row>
            </div>

            <div class="my-1">
              <b-button
                :disabled="disableCalculate"
                block
                class="calc"
                @click="calculate"
                style="color:#3A3A3AB3;"
                >Calculate</b-button
              >
            </div>
          </b-col>
          <b-col md="6" sm="12">
            <img
              v-if="!show"
              src="./assets/invest.png"
              width="100%"
              style="padding-top:20%"
              class="pl-md-6"
            />
            <div v-else>
              <div
                v-if="calcOption === 'target'"
                class="small text-center pl-md-6 pl-xs-4"
                style="background-color:#FFFFFF; width:100%; height:100%"
              >
                You will need to invest
                <span style="text-align: center;color:#3F845C;font-weight: bold"
                  >GHC {{ result }}</span
                >
                to make GHC {{ futureValue }} after {{ period }} years at a rate
                of {{ interest }}%
              </div>
              <div
                v-else
                class="small text-center pl-md-6 pl-xs-4"
                style="background-color:#FFFFFF; width:100%; height:100%"
              >
                You will make
                <span style="text-align: center;color:#3F845C;font-weight: bold"
                  >GHC {{ result }}</span
                >
                with a principal of GHC {{ principal }} after {{ period }} years
                at a rate of {{ interest }}%
              </div>
              <line-chart
                class="pl-md-6 pt-md-6 pl-xs-4 pt-xs-4"
                :chart-data="datacollection"
                :options="options"
              ></line-chart>
            </div>
          </b-col>
        </b-row>
      </b-card>
    </div>
    <div class="background"></div>
  </div>
</template>

<script>
import LineChart from "./LineChart.js";

export default {
  name: "App",
  components: {
    LineChart,
  },
  data() {
    return {
      datacollection: null,
      options: {
        legend: {
          position: "top",
        },
        scales: {
          xAxes: [
            {
              gridLines: {
                display: false,
              },
            },
          ],
          yAxes: [
            {
              gridLines: {
                display: false,
              },
              ticks: {
                display: false,
              },
            },
          ],
        },
        responsive: true,
      },
      show: false,
      principal: "",
      period: "",
      interest: "",
      futureValue: "",
      result: "",
      calcOption: "principal",
      totalInterest: "",
      show2: false,
      time: "years",
    };
  },
  watch: {
    datacollection: {
      deep: true,
      handler(val) {
        console.log("check", val);
      },
    },
  },
  methods: {
    fillData() {
      this.datacollection = {
        labels: ["January", "February"],
        datasets: [
          {
            label: "Total Interest",
            backgroundColor: "#FFEFDA",
            data: [this.getRandomInt(), this.getRandomInt()],
          },
          {
            label: "Total contributions",
            backgroundColor: "#3F845C80",
            data: [this.getRandomInt(), this.getRandomInt()],
          },
        ],
      };
    },
    showChart() {
      this.show = !this.show;
    },
    calculate() {
      if (this.calcOption === "principal") {
        let results = [0];
        let contributions = [0];
        let labels = [0];
        for (let i = 1; i <= this.period; i++) {
          const result = this.principal * Math.pow(1 + this.interest / 100, i);
          results.push(result.toFixed(2));
          labels.push(`Year ${i}`);
          this.result = result.toFixed(2);
          let totalInterest = (this.result - this.principal).toFixed(2);
          contributions.push(this.principal);
          this.totalInterest = totalInterest;
        }

        this.datacollection = {
          labels: labels,
          datasets: [
            {
              label: "Future Value",
              backgroundColor: "rgba(255, 224, 181, 0.4)",
              data: results,
              fill: true,
              lineTension: 0.1,
              borderCapStyle: "square",
              borderDashOffset: 0.0,
              borderJoinStyle: "miter",
              pointBorderWidth: 1,
              pointHoverRadius: 8,
              pointHoverBorderWidth: 2,
              pointRadius: 4,
              pointHitRadius: 10,
            },
            {
              label: "Contributions",
              backgroundColor: "rgba(63, 132, 92, 0.4)",
              data: contributions,
              fill: true,
              lineTension: 0.1,
              borderCapStyle: "square",
              borderDashOffset: 0.0,
              borderJoinStyle: "miter",
              pointBorderWidth: 1,
              pointHoverRadius: 8,
              pointHoverBorderWidth: 2,
              pointRadius: 4,
              pointHitRadius: 10,
            },
          ],
        };
        this.show2 = false;
        this.show = true;
      } else {
        const res =
          this.futureValue / Math.pow(1 + this.interest / 100, this.period);
        this.result = res.toFixed(2);
        let results = [0];
        let principal = [0];
        let labels = [0];
        for (let i = 1; i <= this.period; i++) {
          const result = res * Math.pow(1 + this.interest / 100, i);
          results.push(result.toFixed(2));
          labels.push(`Year ${i}`);
          principal.push(this.result);
        }

        this.datacollection = {
          labels: labels,
          datasets: [
            {
              label: "Future Value",
              backgroundColor: "rgba(255, 224, 181, 0.4)",
              data: results,
              fill: true,
              lineTension: 0.1,
              borderCapStyle: "square",
              borderDashOffset: 0.0,
              borderJoinStyle: "miter",
              pointBorderWidth: 1,
              pointHoverRadius: 8,
              pointHoverBorderWidth: 2,
              pointRadius: 4,
              pointHitRadius: 10,
            },
            {
              label: "Contributions",
              backgroundColor: "rgba(63, 132, 92, 0.4)",
              data: principal,
              fill: true,
              lineTension: 0.1,
              borderCapStyle: "square",
              borderDashOffset: 0.0,
              borderJoinStyle: "miter",
              pointBorderWidth: 1,
              pointHoverRadius: 8,
              pointHoverBorderWidth: 2,
              pointRadius: 4,
              pointHitRadius: 10,
            },
          ],
        };
        this.show2 = false;
        this.show = true;
      }
    },
    changePeriod(num) {
      this.period = num;
    },
    changeOption(value) {
      this.calcOption = value;
      this.principal = "";
      this.period = "";
      this.interest = "";
      this.futureValue = "";
    },
    selectedPeriod(year) {
      return this.period == year;
    },
    changeInterest(num) {
      this.interest = num;
    },
    selectedInterest(interest) {
      return this.interest == interest;
    },
  },
  computed: {
    disableCalculate() {
      if (
        this.calcOption === "principal" &&
        (!this.period || !this.principal || !this.interest)
      ) {
        return true;
      }
      if (
        this.calcOption === "target" &&
        (!this.futureValue || !this.period || !this.interest)
      ) {
        return true;
      }
      return false;
    },
  },
};
</script>

<style lang="scss">
@import "~bootstrap/scss/bootstrap.scss";
@import "~bootstrap-vue/dist/bootstrap-vue.css";

#wrapper {
  width: 100%;
  height: 100%;
}

.content {
  position: relative;
  z-index: 100;
}

.background {
  color: #999999;
  position: absolute;
  top: 0;
  left: 0;
  z-index: -100;
  top: 0;
  bottom: 0;
  right: 0;
  width: 100%;
  height: 50%;
  background-image: linear-gradient(#3f845c, #3f845c);
  transform: skewY(-7deg);
  transform-origin: top left;
  padding: 0 0 0 0;
}

.heading {
  margin-left: 10%;
}

.card {
  width: 80%;
  margin-left: 10%;
  margin-right: 10%;
}

.active {
  background-color: #1593ef1a;
  color: #107ece;
}

.inactive {
  background-color: #f4f4f440;
  color: #3a3a3ab3;
  border: 1px solid #3a3a3a1a;
}

.percent {
  display: inline-block;
  position: relative;
  width: 100%;
}

.percent::after {
  position: absolute;
  top: 7px;
  right: 0.5em;
  transition: all 0.05s ease-in-out;
  content: "%";
}

.percent:hover::after,
.percent:focus-within::after {
  right: 1.5em;
}

/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}

.calc {
  background-color: $yellow;
  border: 0;
}

.calc:hover,
.calc:focus .calc:active {
  background-color: darken($yellow, 5%);
  box-shadow: none;
}

$hoverColor: #1593ef1a;

a.badge-secondary:hover,
a.badge-secondary:focus,
a.badge-secondary.focus {
  color: #107ece;
  background-color: darken($hoverColor, 15%);
  box-shadow: none;
}

.center {
  margin-top: 14%;
  margin-left: 10%;
}

.btn-secondary.disabled,
.btn-secondary:disabled,
.btn-secondary.active,
.btn-secondary:active {
  background-color: lighten($yellow, 5%);
}

.btn-secondary:not(:disabled):not(.disabled):active,
.btn-secondary:not(:disabled):not(.disabled).active,
.show > .btn-secondary.dropdown-toggle {
  background-color: darken($yellow, 5%);
  box-shadow: none;
  border-style: none;
}

.btn-secondary:focus,
.btn-secondary.focus {
  background-color: darken($yellow, 5%);
  box-shadow: none;
  border-style: none;
}
</style>
