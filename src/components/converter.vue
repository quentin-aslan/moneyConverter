<template>
  <input ref="inputEur" class="input" type="text" v-model="inputEUR" @keyup="eurChange" @focus="deleteValue"><span class="devise">€</span>
  <input class="input" type="text" v-model="inputCAD" @keyup="dollardChange" @focus="deleteValue"><span class="devise">C$</span>
  <UsefulInformations :lastUpdate="lastUpdate" :EURtoCAD="EURtoCAD" :CADtoEUR="CADtoEUR" />
  
</template>

<script>

import UsefulInformations from "@/components/UsefulInformations";

export default {
  name: "ConverterQ",
  components: {
    UsefulInformations
  },
  props: ["devise"],
  data() {
    return {
      inputEUR: 0,
      inputCAD: 0,
      EURtoCAD: 1.36,
      CADtoEUR: 0.73,
      lastUpdate: 0
    }
  },
  mounted() {
    this.getLocalStorage();
    this.updateMoneys();
    this.$refs.inputEur.focus();
  },
  methods: {
    getLocalStorage() {
      if (localStorage.getItem("EURtoCAD")) {
        this.EURtoCAD = localStorage.getItem("EURtoCAD");
      }

      if (localStorage.getItem("CADtoEUR")) {
        this.CADtoEUR = localStorage.getItem("CADtoEUR");
      }

      if(localStorage.getItem("lastUpdate")) {
        this.lastUpdate = localStorage.getItem("lastUpdate");
      }
    },

    async updateMoneys() {
      const oneDay = 86400000;
      const diff = Date.now() - this.lastUpdate;
      console.log(`Difference: ${diff} ms 24h: ${oneDay} ms`);
      if((Date.now() - this.lastUpdate) > oneDay) {
        this.lastUpdate = Date.now();
        localStorage.setItem("lastUpdate", this.lastUpdate);
        await this.getEURtoCAD();
        await this.getCADtoEUR();
        console.log(`EUR to CAD: ${this.EURtoCAD}`);
      } else {
        console.log("Pas besoin d'update");
      }

      console.log(`EUR to CAD: ${this.EURtoCAD} CAD to EUR: ${this.CADtoEUR}`);
    },

    async getEURtoCAD() {
          const requestOptions = {
            method: 'GET',
            redirect: 'follow',
            headers: {
              "apiKey": "k0iDMF05S0DiRuEvZzgzy3j9J1BxstPG"
            }
          };
          const answer = await fetch("https://api.apilayer.com/fixer/latest?base=EUR&symbols=CAD", requestOptions);
          if (answer.status === 200) {
            const datas = await answer.json();
            this.EURtoCAD = datas.rates.CAD;
            localStorage.setItem("EURtoCAD", this.EURtoCAD);
          } else {
            console.log(answer.status);
            alert("Erreur API");
          }
    },

    async getCADtoEUR() {
      const requestOptions = {
        method: 'GET',
        redirect: 'follow',
        headers: {
          "apiKey": "k0iDMF05S0DiRuEvZzgzy3j9J1BxstPG"
        }
      };
      const answer = await fetch("https://api.apilayer.com/fixer/latest?base=CAD&symbols=EUR", requestOptions);
      if (answer.status === 200) {
        const datas = await answer.json();
        this.CADtoEUR = datas.rates.EUR;
        localStorage.setItem("CADtoEUR", this.CADtoEUR);
        console.log(`CADtoEUR: ${this.CADtoEUR}`);
      } else {
        console.log(answer.status);
        alert("Erreur API");
      }
    },


    eurChange() {
      this.formatInputValue();
      this.updateMoneys();
      const calcul = this.inputEUR * this.EURtoCAD;
      this.inputCAD = (calcul).toFixed(2);
    },
    dollardChange() {
      this.formatInputValue();
      this.updateMoneys();
      const calcul = (this.inputCAD * this.CADtoEUR);
      console.log(calcul)
      this.inputEUR = calcul.toFixed(2);
    },
    formatInputValue() {
      if(this.inputEUR) {
        if(this.inputEUR[0] === "0") {
          this.inputEUR = this.inputEUR.slice(1);
        }
        this.inputEUR.replace(",", ".");
      }

      if(this.inputCAD) {
        if(this.inputCAD[0] === "0") {
          this.inputCAD = this.inputCAD.slice(1);
        }
        this.inputCAD.replace(",", ".");
      }
    },
    deleteValue(event) {
        event.target.value = "";
    }
  }
}
</script>

<style scoped>

  .input {
    font-size: 10vw;
    border-width:0px;
    border:none;
    border-bottom: 1px solid #000;
    outline:none;
    max-width: 100%;
    text-align: center;
  }

  .devise {
    font-size: 2vw;
    font-weight: bold;
  }
</style>