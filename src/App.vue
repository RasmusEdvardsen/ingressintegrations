<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <br>
    <input @keyup.enter="getSpecific(toSearch)" type="text" v-model="toSearch">
    <button v-on:click="getSpecific(toSearch)">Search Patients</button>
    <component :is="currComp" v-bind="currProp"></component>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import HelloWorld from "./components/HW/HelloWorld.vue";
import Patient from "./components/patient/Patient.vue";
import xml2js from "xml2js";
import { isNullOrUndefined } from "util";

@Component({
  components: {
    HelloWorld,
    Patient,
  },
})
export default class App extends Vue {
  public data: any = {};
  public patientData: any = {};
  public currComp: string = "HelloWorld";
  private toSearch: string = "";
  public mounted() {
    this.fetchstuff("https://edvardsenfunctions.azurewebsites.net"
                   + "/api/fetch?code=SNgzI0bGqIZ6GuPvAJRIarCMIcF7UgIyz2cag4tTbrTEZJocZxVT0g=="
                   , this.data);
  }
  get currProp() {
    switch (this.currComp) {
      case "Patient":
        return {data: this.patientData};
      case "HelloWorld":
      default:
        return {msg: this.data};
    }
  }
  private getSpecific(subject: string): any {
    const obj: any = this.data.ODM.ClinicalData.find((el: any) => el.SubjectData[0].$.SubjectKey === subject);
    if (!isNullOrUndefined(obj)) {
      this.currComp = "Patient";
      fetch(`https://ingressazurefunctions.azurewebsites.net/api/SubjectsGet/${subject}`)
        .then((rsp) => {
            rsp.json()
                .then((data: any) => {
                  this.patientData = data;
                  console.log(data + " ");
                });
        })
        .catch((err) => console.log(err));
    } else {
      this.patientData = { error: true, msg: "notfound" };
      this.currComp = "HelloWorld";
    }
  }
  private formatXML(s: string): string {
    let noescapes: string = s.replace(/\\r|\\n/g, "").replace(/\\\"/g, "\"");
    noescapes = noescapes.startsWith("\"") ? noescapes.slice(1) : noescapes;
    noescapes = noescapes.endsWith("\"") ? noescapes.slice(0, noescapes.length - 1) : noescapes;
    return noescapes;
  }
  private fetchstuff(url: string, toSet: string): void {
    fetch(url)
    .then((rsp) => {
        rsp.text()
            .then((data) => {
                const formattedXML: string = this.formatXML(data);
                xml2js.parseString(formattedXML, (err, res) => {
                  switch (toSet) {
                    case "patientData":
                      this.patientData = res;
                      break;
                    case "data":
                    default:
                      this.data = res;
                      break;
                  }
                });
            });
    })
    .catch((err) => console.log(err));
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
