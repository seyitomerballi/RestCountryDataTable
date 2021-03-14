<template>
  <div id="app" class="container">
    <img alt="Vue logo" src="./assets/logo.png" class="logo">
    <div class="row">
      <div class="col col-3">
        <input type="text" v-model="search" class="form-control" placeholder="Tüm JSON verisinde ara...">
      </div>
    </div>
    <div class="row my-2">
      <div class="col">
        <Countries @childToParent="updateSearchByData" :repositories="filteredCountries"/>
      </div>
    </div>
  </div>
</template>
<script>
import Countries from './components/Countries'
import axios from 'axios'
const restCountriesAPI = 'https://restcountries.eu/rest/v2/all'

// Filtre yapmak için - 41. satıra git
/*
  const restCountriesAPIFiltered = 'https://restcountries.eu/rest/v2/all?fields=name;capital;region;flag'
*/
export default {
  name: 'app',
  components: {
    Countries
  },
  data() {
    return {
      repositories: [],
      searchData : [],
      search: '',
      searchByName: '',
      searchByCapital: '',
      searchByRegion: ''
    }
  },
  mounted() {
    // axios.get(restCountriesAPIFiltered)
    axios.get(restCountriesAPI)
        .then((resp) => {
          this.repositories = resp.data
        })
        .catch((err) => {
          console.log(err)
        })
  },
  computed: {
    filteredCountries: function () {
      if (this.searchByName !== '') {
        return this.filteredCountriesByName(this.repositories);
      } else if (this.searchByCapital !== '') {
        return this.filteredCountriesByCapital(this.repositories);
      } else if (this.searchByRegion !== '') {
        return this.filteredCountriesByRegion(this.repositories);
      }
      return this.filteredCountriesByAll(this.repositories);
    },
  },
  methods: {
    filteredCountriesByAll: function (repo) {
      let search = this.search.trim().toLowerCase();
      return repo.filter((country) => {
        if (this.search === '') {
          return country;
        } else {
          /* Sadece tablodaki verilere göre arama yapmak için (name, capital, region, flag)
          return country.name.toLowerCase().includes(searchByName) ||
          country.capital.toLowerCase().includes(searchByName) ||
          country.region.toLowerCase().includes(searchByName) ||
          country.flag.toLowerCase().includes(searchByName)
          */
          //
          // Tüm JSON Data içinde arama
          for(let item in country){
            if(JSON.stringify(country[item]).toLowerCase().includes(search)){
              return JSON.stringify(country[item]).toLowerCase().includes(search)
            }
          }
          return this.country;
        }
      })
    },
    filteredCountriesByName: function (repo) {
      let searchByName = this.searchByName.trim().toLowerCase();
      return repo.filter((country) => {
        if (this.searchByName === '') {
          return country;
        } else {
          return country.name.toLowerCase().includes(searchByName)
        }
      })
    },
    filteredCountriesByCapital: function (repo) {
      let searchByCapital = this.searchByCapital.trim().toLowerCase();
      return repo.filter((country) => {
        if (this.searchByCapital === '') {
          return country;
        } else {
          return country.capital.toLowerCase().includes(searchByCapital)
        }
      })
    },
    filteredCountriesByRegion: function (repo) {
      let searchByRegion = this.searchByRegion.trim().toLowerCase();
      return repo.filter((country) => {
        if (this.searchByRegion === '') {
          return country;
        } else {
          return country.region.toLowerCase().includes(searchByRegion)
        }
      })
    },
    updateSearchByData(e) {
      this.searchByName = e.searchByNameData;
      this.searchByCapital = e.searchByCapitalData;
      this.searchByRegion = e.searchByRegionData;
    },
  }
}
</script>
<style>
* {
  box-sizing: border-box;
}
.container {
  max-width: 600px;
  margin: 0 auto;
}
.logo {
  display: block;
  margin: 20px auto;
  height: 100px;
}
#app {
  font-family: Century Gothic,CenturyGothic,AppleGothic,sans-serif;
  font-size: 15px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 50px;
}
</style>
