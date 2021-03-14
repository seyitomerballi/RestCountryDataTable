<template>
  <div class="table-responsive">
    <table class="table table-hover table-sm">
      <thead>
      <tr class="border-0">
        <th>
        </th>
        <th>
          <input type="text" v-model="searchData.searchByNameData" @keyup="emitToParent(searchData)"
                 class="form-control form-control-sm"
                 placeholder="Ülke adına göre ara...">
        </th>
        <th>
          <input type="text" v-model="searchData.searchByCapitalData" @keyup="emitToParent(searchData)"
                 class="form-control form-control-sm"
                 placeholder="Başkente göre ara...">
        </th>
        <th>
          <input type="text" v-model="searchData.searchByRegionData" @keyup="emitToParent(searchData)"
                 class="form-control form-control-sm"
                 placeholder="Kıtaya göre ara...">
        </th>
        <th>
        </th>
      </tr>
      <tr>
        <th> #</th>
        <th> Name</th>
        <th> Capital</th>
        <th> Region</th>
        <th> Flag</th>
      </tr>
      </thead>
      <tbody class="text-center">
      <tr v-for="(repo, index) in repositories" :key="repo.index">
        <td class="align-middle">
          {{ index + 1 }}
        </td>
        <td class="align-middle">
          {{ repo.name }}
        </td>
        <td class="align-middle">
          {{ repo.capital }}
        </td>
        <td class="align-middle">
          {{ repo.region }}
        </td>
        <td class="align-middle">
          <img v-if="repo.flag" class="avatar" :src="repo.flag"/>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
export default {
  name: 'Countries',
  props: {
    repositories: Array,
  },
  data() {
    return {
      searchData: {
        searchByNameData: '',
        searchByCapitalData: '',
        searchByRegionData: ''
      }
    }
  },
  methods: {
    emitToParent(data) {
      this.$emit('childToParent', data)
    }
  }
}
</script>
<style scoped>
.avatar {
  max-width: 100px;
}
</style>
