<template>
  <input ref="inputEur" class="input" type="text" v-model="inputEUR" @keyup="eurChange" @focus="deleteValue"><span class="devise">€</span>
  <input class="input" type="text" v-model="inputCA" @keyup="dollardChange" @focus="deleteValue"><span class="devise">C$</span>

  <p>
    Last update : <strong>{{new Date(parseInt(this.dateUpdate)).toLocaleString()}}</strong> Current : <strong>{{new Date().toLocaleTimeString()}}</strong>
  </p>
</template>

<script>
export default {
  name: "ConverterQ",
  props: ["devise"],
  data() {
    return {
      inputEUR: 0,
      inputCA: 0,
      EURtoCA: 1.36,
      CAtoEUR: 0.73,
      dateUpdate: 0
    }
  },
  mounted() {
    this.getLocalStorage();
    this.updateMoneys();
    this.$refs.inputEur.focus();
  },
  methods: {
    getLocalStorage() {
      if (localStorage.getItem("EURtoCA")) {
        this.EURtoCA = localStorage.getItem("EURtoCA");
      }

      if (localStorage.getItem("CAtoEUR")) {
        this.CAtoEUR = localStorage.getItem("CAtoEUR");
      }

      if(localStorage.getItem("dateUpdate")) {
        this.dateUpdate = localStorage.getItem("dateUpdate");
      }
    },

    async updateMoneys() {
      const oneDay = 86400000;
      const diff = Date.now() - this.dateUpdate;
      console.log(`Difference: ${diff} ms 24h: ${oneDay} ms`);
      if((Date.now() - this.dateUpdate) > oneDay) {
        console.log("Update !");
        this.dateUpdate = Date.now();
        localStorage.setItem("dateUpdate", this.dateUpdate);
        await this.getEURtoCA();
        await this.getCAtoEUR();
        console.log(`EUR to CA: ${this.EURtoCA}`);
      } else {
        console.log("Pas besoin d'update");
      }

      console.log(`EUR to CA: ${this.EURtoCA} CA to EUR: ${this.CAtoEUR}`);
    },

    async getEURtoCA() {
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
            this.EURtoCA = datas.rates.CAD;
            localStorage.setItem("EURtoCA", this.EURtoCA);
          } else {
            console.log(answer.status);
            alert("Erreur API");
          }
    },

    async getCAtoEUR() {
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
        this.CAtoEUR = datas.rates.EUR;
        localStorage.setItem("CAtoEUR", this.CAtoEUR);
        console.log(`CAtoEUR: ${this.CAtoEUR}`);
      } else {
        console.log(answer.status);
        alert("Erreur API");
      }
    },
    eurChange() {
      this.formatInputValue();
      this.updateMoneys();
      const calcul = this.inputEUR * this.EURtoCA;
      console.log(calcul);
      this.inputCA = (calcul).toFixed(2);
    },
    dollardChange() {
      this.formatInputValue();
      this.updateMoneys();
      const calcul = (this.inputCA * this.CAtoEUR);
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

      if(this.inputCA) {
        if(this.inputCA[0] === "0") {
          this.inputCA = this.inputCA.slice(1);
        }
        this.inputCA.replace(",", ".");
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