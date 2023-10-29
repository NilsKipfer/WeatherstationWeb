<template>
  <q-page class="bg-secondary flex flex-center">
    <div class="" style="min-height: 400px; width: 80%; padding: 24px;">
      <q-banner v-for="result in  results " :key="result" style="padding: 30px;"
        class="bg-primary fit row wrap justify-around items-center content-center">
        <transition name="q-transition--scale">
          <q-card class="bg-secondary rounded-borders text-white ">
            <div>
              <div class="flex flex-center row">
                <div class="text-h4" style="padding: 10px;">{{ result["devicename"] }}</div>
                <!--<q-space />
                <q-btn flat round color="primary" icon="edit" />-->
              </div>
              <p>
                <Doughnut :data="{
                  datasets: [
                    {
                      backgroundColor: ['#28BCBC','#373838'],
                      data: [result['valuedata'], this.setChartData(result['valuedata'], result['valuekey'])],
                    }
                  ]
                }" :options="{
                  circumference: 180,
                  rotation: -90,
                  responsive: true,
                  maintainAspectRatio: false,
                }
                " />
              </p>
              <card-section>
                <div class="text-h5 flex flex-center">
                  {{ result["valuedata"] }} {{ getUnitByKey(result["valuekey"]) }}
                </div>
                <div class="text-h5 flex flex-center" style="padding: 10px;">
                  {{ result["valuekey"] }}
                </div>
              </card-section>
            </div>
          </q-card>
        </transition>
      </q-banner>
    </div>
    <q-page-scroller position="bottom-right" :scroll-offset="150" :offset="[18, 18]">
      <q-btn fab icon="keyboard_arrow_up" color="primary" />
    </q-page-scroller>
  </q-page>
</template>

<script lang="ts">
import { Chart as ChartJS, ArcElement, Tooltip, Legend } from 'chart.js'
import { Doughnut } from 'vue-chartjs'
import { defineComponent } from 'vue'
import axios from "axios";

ChartJS.register(ArcElement, Tooltip, Legend)

export default defineComponent({
  name: 'IndexPage',
  components: {
    Doughnut
  },
  data() {
    return {
      temp: "° C",
      dataset: [],
      loaded: false,
      results: [],
    }
  },
  created() {
    this.getValueData();
    this.timer = setInterval(this.getValueData, 300000);
  },
  methods: {
    async getValueData() {
      try {
        const response = await axios.get('http://192.168.0.224:5000/api/sensorValues');
        this.results = response.data;
      } catch (err) {
        console.log(err);
      }
    },
    async updateDeviceName(newDeviceName, MAC) {
      try {
        console.log(newDeviceName);
        console.log(MAC);
        await axios.put(
          `http://localhost:5000/products/${MAC}`, newDeviceName
        );
      } catch (err) {
        console.log(err);
      }
    },
    getUnitByKey(key) {
      if (key === "Temperature") {
        return "° C";
      } else if (key === "Pressure") {
        return "hPa";
      } else if (key === "Humidity") {
        return "%";
      }
    },
    cancelAutoUpdate() {
      clearInterval(this.timer);
    },
    setChartData(sensordata, key){
      let max = 0;
      let min = 0;
      if (key === "Temperature") {
        min = -10;
        max = 40;
      } else if (key === "Pressure") {
        min = 973;
        max = 1047;
      } else if (key === "Humidity") {
        max = 100;
        min = 0;
      }
      return max-min-sensordata;
    }
  },
  beforeUnmount() {
    this.cancelAutoUpdate();
  }
})
</script>
