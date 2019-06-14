<template>
  <div id="app">
    <div class="container">
      <DataTable :columns="columns" url="http://app.planete-ecole.sf4/api/dictations" />
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import DataTable from './components/DataTable';

  const token = null;
  const instance = axios.create({
    baseURL: 'http://app.planete-ecole.sf4/api',
    // headers: {
    //   'Authorization': `Bearer ${token}`
    // }
  });

  export default {
    components: { DataTable },
    data() {
      return {
        columns: [
          { name: 'name',       header: 'Exercice', sortable: true },
          { name: 'updated_at', header: 'Date',     sortable: true },
        ],
        axios: instance,
      }
    }
  }
</script>
