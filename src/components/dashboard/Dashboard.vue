<template>
  <div class="dashboard">
    <div class="row row-equal">
      <div class="flex xs12 md6 lg6">
        <div class="va-card">
          <div class="va-card__header">
            <div class="va-card__header-inner">Fluentbit Data</div>
          </div>
          <div
            class="va-card__body va-card__body--padding-top"
            v-for="content in data"
            :key="content"
          >
            <p>Version: {{content.version}}</p>
            <p>Edition: {{content.edition}}</p>
            <div class="markup-tables">
              <va-card class="mb-2">
                <table class="va-table">
                  <thead>
                    <tr>
                      <th>Flags</th>
                    </tr>
                  </thead>
                  <tr v-for="flag in content.flags" :key="flag">
                    <td>{{ flag}}</td>
                  </tr>
                </table>
              </va-card>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- <dashboard-charts /> -->
    <!-- <dashboard-info-block /> -->
    <div class="row">
      <!-- <div class="flex xs12">
        <dashboard-table />
      </div> -->
    </div>
    <div class="row row-equal">
      <!-- <div class="flex xs12 lg6">
        <dashboard-tabs @submit="addAddressToMap" />
      </div> -->
      <!-- <div class="flex xs12 lg6">
        <dashboard-map ref="dashboardMap" />
      </div> -->
      <div class="flex xs12 md6 lg6">
        <div class="va-card">
          <div v-for="detail in chart" :key="detail">
            <!-- <h3>Cncf</h3>
            cncf: {{chart.input.cncf}}
            <p>cncf-record: {{chart.input.cncf.records}}</p>
            <p>cncf-bytes: {{chart.input.cncf.bytes}}</p>

            <h3>CPU</h3>
            CPU: {{chart.input["cpu.1"]}} -->
          </div>

          <h2>Input Data</h2>
          Input: {{chart.input}}
          <h2>Filter Data</h2>
          Filter: {{chart.filter}}
          <h2>Output Data</h2>
          Output: {{chart.output}}
          <h2>all data</h2>
          {{chart}}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import DashboardCharts from "./DashboardCharts"
// import DashboardInfoBlock from './DashboardInfoBlock'
// import DashboardTable from './DashboardTable'
// import DashboardTabs from './DashboardTabs'
// import DashboardMap from './DashboardMap'
import LineChart from "./chart.vue";
import axios from "axios";

export default {
  name: "dashboard",
  components: {
    DashboardCharts,
    // DashboardInfoBlock,
    // DashboardTable,
    // DashboardTabs,
    // DashboardMap,
    LineChart
  },
  data() {
    return {
      data: [],
      chart: []
    };
  },
  mounted() {
    const api = "http://127.0.0.1:2020";
    axios.get(api).then(response => (this.data = response.data));

    axios
      .get("http://127.0.0.1:2020/api/v1/metrics")
      .then(response => {

        var inputplugin = Object.keys(response.data.input);

        // var parserPlugin = Object.keys(response.data.parser);

        var outputPlugin = Object.keys(response.data.output);

        console.log(inputplugin,outputPlugin)

        inputplugin.map( val => {
          console.log(response.data.input[val])
        })
        outputPlugin.map( val => {
          console.log(response.data.output[val])
        })
      })
      .catch(err => console.log(err));
  }
};
</script>

<style lang="scss">
.row-equal .flex {
  .va-card {
    height: 100%;
  }
}

.dashboard {
  .va-card {
    margin-bottom: 0 !important;
  }
}
</style>
