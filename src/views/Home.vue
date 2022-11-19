<template>
  <main class="container" v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate" />
    <div class="flex items-center justify-center mt-10 mb-10 gap-1">
      <SelectCountry @get-country="getCountryData" :countries="countries" />
      <button class="bg-green-700 text-white rounded p-3 px-5" v-if="stats.Country" @click="clearCountry">Clear</button>
    </div>
    <DataBoxes :stats="stats" />
  </main>
  <main class="flex flex-col align-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">Fetching data</div>
    <img :src="loadingImage" class="w-24 m-auto" alt="loading..." />
  </main>
</template>

<script>
import DataTitle from '@/components/DataTitle';
import DataBoxes from '@/components/DataBoxes';
import SelectCountry from '@/components/SelectCountry';

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    SelectCountry,
  },
  data() {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
      loadingImage: require('../assets/hourglass.gif'),
    };
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch('https://api.covid19api.com/summary');
      const data = await res.json();

      return data;
    },
    getCountryData(country) {
      this.stats = country;
      this.title = country.Country;
    },
    async clearCountry() {
      this.loading = true;
      const data = await this.fetchCovidData();
      this.title = 'Global';
      this.stats = data.Global;
      this.loading = false;
    },
  },
  async created() {
    const data = await this.fetchCovidData();
    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;

    this.loading = false;
  },
};
</script>
