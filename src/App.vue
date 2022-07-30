<template>
<div class="bg-stone-100 h-screen w-full">
  <div class="flex justify-between items-center w-full p-6 max-w-screen-sm md:max-w-screen-md lg:max-w-screen-lg mx-auto">
    <div>
      <h1 class="text-3xl font-bold drop-shadow-md">YourStake</h1>
    </div>
    <div>
    <input type="text" v-model="search" placeholder="Search by name or ticker" class="border shadow-inner py-2 px-4 rounded-xl" />
  </div>
  </div>

  <div class="">
    <p class="max-w-prose text-center py-2 px-2 italic mx-auto">Use this tool to find out what companies are involved in activities that are bad for the environment. You can use the search box above and the options below.</p>
  </div>
  
  <div class="w-full md:w-2/3 lg:w-1/2 max-w-screen-sm md:max-w-screen-md lg:max-w-screen-lg mx-auto">
    
    <div class="p-4">
    <fieldset class="border p-4 bg-stone-50 rounded-xl drop-shadow-md w-full">
      <legend class="drop-shadow-md">Check For:</legend>
      <div>
        <div class="space-x-1">
        <input type="checkbox" id="animals-include" name="filters" value="true" v-model="animalInclude">
        <label for="animals-include">Animal Testing</label>
        </div>
        <div class="space-x-1">
        <input type="checkbox" id="nuclear-include" name="filters" value="true" v-model="nuclearInclude">
        <label for="nuclear-include">Nuclear Weapons</label>
        </div>
        <div class="space-x-1">
        <input type="checkbox" id="coal-include" name="filters" value="true" v-model="coalInclude">
        <label for="coal-include">Coal Power</label>
        </div>
        <div class="space-x-1">
        <input type="checkbox" id="rainforest-include" name="filters" value="true" v-model="rainforestInclude">
        <label for="rainforest-include">Rainforest Destruction</label>
        </div>
        <div>
          <button @click="clearFilters" class="px-2 py-1 border-b mt-4 shadow-md hover:shadow-none">Clear Filters</button>
        </div>
      </div>
    </fieldset>
    </div>

  </div>
  <div>
    <ul>
      <li v-for="company in filteredCompanies">
        <div class="w-full md:w-2/3 lg:w-1/2 max-w-screen-sm md:max-w-screen-md lg:max-w-screen-lg mx-auto">
            <div class="border p-4 m-4 rounded-xl drop-shadow-md bg-stone-50">
            <div class="text-2xl font-bold">
              {{company['Company Name']}}
            </div>
            <div class="text-xl">
              {{company['Ticker Symbol']}}
            </div>
            <div v-show=false>
                {{count = 0}}
            </div>
            <div v-for="i in company">
              <div v-if="i === true" v-show=false>
                  {{count = count + 1}}
              </div>
            </div>
            <p>Issue Count: {{count}}</p>
            <div>
              <p v-if="company['Animal Testing'] === true">Animal Testing</p>
            </div>
            <div>
              <p v-if="company['Nuclear Weapons'] === true">Nuclear Weapons</p>
            </div>
            <div>
              <p v-if="company['Coal Power'] === true">Coal Power</p>
            </div>
            <div>
              <p v-if="company['Rainforest Destruction'] === true">Rainforest Destruction</p>
            </div>
          </div>
        </div>
      </li>
    </ul>
  </div>
</div>
</template>

<script>
import CompanyData from './assets/data.json';

export default {
  data() {
    return {
      companies: CompanyData,
      search: '',
      animalInclude: '',
      nuclearInclude: '',
      coalInclude: '',
      rainforestInclude: '',
    }
  },
  computed: {
    filteredCompanies: function() {
    var vm = this;
    var animals = vm.animalInclude;
    var nuclear = vm.nuclearInclude;
    var coal = vm.coalInclude;
    var rainforest = vm.rainforestInclude;

    return vm.companies.filter((company) => {
      return company['Company Name'].toLowerCase().match(vm.search.toLowerCase()) || company['Ticker Symbol'].toLowerCase().match(vm.search.toLowerCase())
    })
    .filter((company) => {
      return company['Animal Testing'].toString().match(animals)
    })
    .filter((company) => {
      return company['Nuclear Weapons'].toString().match(nuclear)
    })
    .filter((company) => {
      return company['Coal Power'].toString().match(coal)
    })
    .filter((company) => {
      return company['Rainforest Destruction'].toString().match(rainforest)
    });
    }
  },
  methods: {
    clearFilters() {
      this.animalInclude = ''
      this.nuclearInclude = ''
      this.coalInclude = ''
      this.rainforestInclude = ''
    }
  },
  name: 'App',
  components: {
  },
}
</script>