<template>
  <div class="hello" v-if="parkData">
    <select v-model="selectedFilter">
      <option v-for="(header, i) in parkData[0]" :key="i" :value="i">
       {{ i }}
      </option>
      </select>
    <table>
       <thead>
        <tr>
          <th colspan="2">The table header</th>
        </tr>
      </thead>
      <tbody>
          <tr>
              <th v-for="(header, i) in parkData[0]" :key="i" class="col-header"> {{ i }} </th>
          </tr>
          <tr v-for="(header, i) in parkDataResults" :key="i">
             <th v-for="(value, name) in header" :key="name" class="col-header"> {{ value }} </th>
          </tr>
      </tbody>
    </table>
    
  </div>
</template>

<script>

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return{
      parkData: null,
      selectedFilter: null,
    }
  },
  computed: {
    filterResults(){
      return this.parkData.filter(item => item[`${this.selectedFilter}`] === 'Yes');
    },
    parkDataResults(){
      return this.selectedFilter ? this.filterResults : this.parkData;
    },
  },
  methods: {
    async getParkData(){
      console.log('getPArkData');
      let res = await fetch('https://data.townofcary.org/api/v2/catalog/datasets/parks-and-recreation-feature-map/exports/json');
      if(!res.ok){
        throw new Error(`${res.status}`);
      }
      let data = await res.json();
      this.parkData = data;
      console.log('data ', data);
    }
  },
  mounted(){
    this.getParkData();
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

th{
  border: 1px solid black;
}
</style>
