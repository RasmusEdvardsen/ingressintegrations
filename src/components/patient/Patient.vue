<template>
    <div class="patient">
    <h3>Meta data</h3>
    <div class="data" v-if="data.ODM !== undefined">
      <table>
        <tr v-for="(subjectData, index) in data.ODM.ClinicalData" :key="index">
          <th>{{index}}</th>
          <th>{{subjectData}}</th>
        </tr>
      </table>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class Patient extends Vue {
  @Prop({default: {}}) private data!: any;
}
</script>

<style>
table {
    font-family: arial, sans-serif;
    border-collapse: collapse;
    width: 100%;
}

td, th {
    border: 2px solid rgba(255, 0, 0, 0.733);
    text-align: left;
    padding: 8px;
}
</style>
