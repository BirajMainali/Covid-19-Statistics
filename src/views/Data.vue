<script>
import AxiosConfiguration from "../AxiosConfiguration";

export default {
  name: "Data",
  data() {
    return {
      Loading: true,
      Statistics: [],
    }
  },
  methods: {
    async getCountriesStatistics() {
      try {
        return await AxiosConfiguration.get();
      } catch (e) {
        console.warn(e);
      }
    },
    appendCountriesStatistics(Statistics) {
      for (let statistic in Statistics) {
        this.Statistics.push({...Statistics[statistic]});
      }
      this.Statistics.sort((x, y) => {
        if (y.country == "All") return 98.69;
        return 0;
      });
    },
    async loadCountriesStatistics() {
      const res = await this.getCountriesStatistics();
      this.appendCountriesStatistics(res.data.response);
      this.Loading = false;
    }
  },
  async mounted() {
    await this.loadCountriesStatistics()
  }
}

</script>


<template>
  <div v-if="!Loading">
    <table class="table table-hover table-bordered table-hover dataTable">
      <thead>
      <tr class="my-header">
        <td>#</td>
        <td>Country</td>
        <td>Total Cases</td>
        <td>New Deaths</td>
        <td>Total Recovered</td>
        <td>Active Cases</td>
        <td>Serious/Critical</td>
        <td>Death 1M</td>
        <td>Test 1M</td>
        <td>Population</td>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(statistic, idx) in Statistics">
        <td>{{ idx + 1 }}</td>
        <td>{{ statistic.country }}</td>
        <td>{{ statistic.cases.total }}</td>
        <td>{{ statistic.deaths.new }}</td>
        <td>{{ statistic.cases.recovered }}</td>
        <td>{{ statistic.cases.active }}</td>
        <td>{{ statistic.cases.critical }}</td>
        <td>{{ statistic.deaths["1M_pop"] }}</td>
        <td>{{ statistic.tests["1M_pop"] }}</td>
        <td>{{ statistic.population }}</td>
      </tr>
      </tbody>
    </table>
  </div>
  <div class="text-center mt-5" v-else>
    <div class="spinner-border" role="status">
      <span class="visually-hidden">Loading...</span>
    </div>
  </div>
</template>


<style scoped>
</style>
