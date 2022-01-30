<template>
  <div class="hello" id="printMe">
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul>
    <div>
    <!-- SOURCE -->

    <!-- OUTPUT -->
    <button @click="print">Print Button</button>
    <ag-grid-vue
    style="width: 500px; height: 200px"
    class="ag-theme-alpine"
    :columnDefs="columnDefs"
    @grid-ready="onGridReady"
    :rowData="rowData"
  >
  </ag-grid-vue>
  </div>
    <table>
    <thead>
      <tr>
        <th>Name</th>
        <th>Age</th>
        <th>Breed</th>
        <th>Gender</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="cat in cats" v-bind:key="cat">
        <td>{{cat.name}}</td>
        <td>{{cat.age}}</td>
        <td>{{cat.breed}}</td>
        <td>{{cat.gender}}</td>
      </tr>
    </tbody>
  </table>
  
  </div>
</template>

<script>

import Vue from 'vue';
import VueHtmlToPaper from 'vue-html-to-paper';
// ECMA 6 - using the system import method
import "ag-grid-community/dist/styles/ag-grid.css";
import "ag-grid-community/dist/styles/ag-theme-alpine.css";
import { AgGridVue } from "ag-grid-vue";

export default {
  name: 'HelloWorld',
  data: function () {
  return {
    gridApi: null,
      columnApi: null,
      defaultColDef: { sortable: true },
    columnDefs: null,
      rowData: null,
    cats:[]
  }
},
beforeMount() {
  window.setPrinterFriendly = function setPrinterFriendly(api) {
  const eGridDiv = document.querySelector('#myGrid');
  eGridDiv.style.height = '';
  api.setDomLayout('print');
};

window.setNormal = function setNormal(api) {
  const eGridDiv = document.querySelector('#myGrid');
  eGridDiv.style.width = '700px';
  eGridDiv.style.height = '200px';
  api.setDomLayout(null);
};

    this.columnDefs = [
      { field: "make", filter: 'agTextColumnFilter', sortable: true },
      { field: "model" , filter: true, sortable: true},
      { field: "price", filter: 'agNumberColumnFilter', sortable: true },
    ];

    this.rowData = [
      { make: "Toyota", model: "Celica", price: 35000 },
      { make: "Ford", model: "Mondeo", price: 32000 },
      { make: "Porsche", model: "Boxter", price: 72000 },
    ];
  },
components: {
    AgGridVue,
  }
  ,
  created:
  
  function() {
    const options = {
  name: '_blank',
  specs: [
    'fullscreen=yes',
    'titlebar=yes',
    'scrollbars=yes'
  ],
  styles: [
    'https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css',
    'https://unpkg.com/kidlat-css/css/kidlat.css'
  ]
}
    Vue.use(VueHtmlToPaper, options);
    // or using the defaults with no stylesheet
    fetch('https://www.raymondcamden.com/.netlify/functions/get-cats')
    .then(res => res.json())
    .then(res => {
      this.cats = res;
    })
  },
  methods: {
    onGridReady(params) {
      this.gridApi = params.api;
      this.gridColumnApi = params.columnApi;
    },
    onBtPrint() {
      const api = this.gridApi;
      setPrinterFriendly(api);
      setTimeout(function () {
        print();
        setNormal(api);
      }, 2000);
    },
    print () {
      // Pass the element id here
      this.$htmlToPaper('printMe');
    }
  },
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
