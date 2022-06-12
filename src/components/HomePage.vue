<template>
  <div>
    <div v-if="loading">loading....</div>
    <div v-else>
      <div class="flex flex-row flex-wrap gap-3 justify-evenly p-4">
        <div
          class="bg-white border-2 border-gray-600 rounded px-5 py-2"
          v-for="data in countryData"
          :key="data"
        >
          <h1 class="font-bold text-2xl">{{ data.status }}</h1>
          <h4 class="text-xl font-bold">{{ data.patientInNumber[0] }}</h4>
        </div>
      </div>
      <div class="my-2 mx-4 md:mx-16">
        <vue-good-table :columns="columns" :rows="stateWiseData" />
      </div>
    </div>
  </div>
</template>

<script>
import "vue-good-table/dist/vue-good-table.css";
import { VueGoodTable } from "vue-good-table";
import axios from "axios";

export default {
  data() {
    return {
      ApiEndPoint: "http://localhost:8000/",
      loading: true,
      countryData: [],
      stateWiseData: [],
      columns: [
        {
          label: "S.No",
          field: "sno",
        },
        {
          label: "State Name",
          field: "stateName",
        },
        {
          label: "Active Cases",
          field: "activeCases",
          type: "number",
        },
        {
          label: "Cured Cases",
          field: "curedCases",
        },
        {
          label: "Death Cases",
          field: "deathCases",
        },
      ],
    };
  },
  async mounted() {
    try {
      await this.getCountryData();
      await this.getStateData();
    } catch (error) {
      console.log(error);
    } finally {
      this.loading = false;
    }
  },
  methods: {
    async getCountryData() {
      try {
        const res = await axios.get(this.ApiEndPoint + "country");
        this.countryData = res.data;
        console.log(this.countryData);
      } catch (error) {
        console.log(error);
      }
    },
    async getStateData() {
      try {
        const res = await axios.get(this.ApiEndPoint + "state");
        this.stateWiseData = res.data;
        console.log(this.stateWiseData);
      } catch (error) {
        console.log(error);
      }
    },
  },
  components: {
    VueGoodTable,
  },
};
</script>
