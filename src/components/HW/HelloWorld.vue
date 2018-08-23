<template>
  <div class="helloworld">
    <h3>Meta data</h3>
    <div v-if="msg.ODM !== undefined" class="data">
      <table>
        <tr v-for="(meta, index) in msg.ODM.$" :key="index">
          <th>{{index}}</th>
          <th>{{meta}}</th>
        </tr>
      </table>
      <h3>Patients</h3>
      <table>
        <tr v-for="(sbjct, index) in msg.ODM.ClinicalData" :key="index">
          <th>{{index}}</th>
          <table>
            <tr v-for="(sbjctmeta, index) in sbjct.$" :key="index">
              <th>{{index}}</th>
              <th>{{sbjctmeta}}</th>
            </tr>
          </table>
          <table>
            <tr v-for="(sbjctdata, index) in sbjct.SubjectData[0]" :key="index">
              <th>{{index}}</th>
              <table>
                <tr v-for="(sbjctdatadown, index) in sbjctdata" :key="index">
                  <th>{{index}}</th>
                  <table v-if="typeof(sbjctdatadown) === 'object'">
                    <tr v-for="(sbjctdatadowndown, index) in sbjctdatadown.$" :key="index">
                      <th>{{index}}</th>
                      <th>{{sbjctdatadowndown}}</th>
                    </tr>
                  </table>
                  <th v-else>{{sbjctdatadown}}</th>
                </tr>
              </table>
            </tr>
          </table>
        </tr>
      </table>
    </div>
    <div v-else>
      Nothing here
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import { isNullOrUndefined } from "util";

@Component
export default class HelloWorld extends Vue {
  @Prop() private msg!: any;
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
