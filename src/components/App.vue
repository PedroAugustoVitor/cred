<script>
import 'bootstrap'

export default {
  data: function () {
    return {
      rates: {
        receivable: [0, 1.0799, 1.0922, 1.0984, 1.1045, 1.11, 1.1178, 1.12384, 1.1268, 1.128, 1.1298, 1.1348, 1.1398, 1.17, 1.175, 1.18, 1.185, 1.19, 1.2],
        limit: [0, 0.07398833225298596, 0.08441677348471, 0.08958485069191602, 0.09461294703485701, 0.09909909909909898, 0.10538557881553001, 0.11019362186788195, 0.11253106141285096, 0.11347517730496404, 0.11488759072402199, 0.11878745153331005, 0.12265309703456695, 0.145299145299145, 0.14893617021276606, 0.15254237288135597, 0.15611814345991604, 0.15966386554621803, 0.16666666666666696]
      },
      value: 1000,
      term: 1,
      limit: false,
      result: {
        willCharge: 0,
        willReceive: 0,
        portion: 0
      }
    }
  },
  mounted() {
    this.process()
  },
  methods: {
    process: function () {
      this.limit ? this.result = {
        willCharge: this.formatter(this.value),
        willReceive: this.formatter(this.value - (this.value * this.rates.limit[this.term])),
        portion: this.formatter(this.value / this.term)
      } : this.result = {
        willCharge: this.formatter(this.value * this.rates.receivable[this.term]),
        willReceive: this.formatter(this.value),
        portion: this.formatter(this.value * this.rates.receivable[this.term] / this.term)
      }
    },
    formatter: function (e) {
      return e.toLocaleString("pt-BR", {
        style: "currency",
        currency: "BRL"
      })
    },
    copy: function () {
      let e = `Simulador SertãoCred\nSerá cobrado no cartão: ${this.result.willCharge}.\nVocê receberá: ${this.result.willReceive}.\nSer${this.term > 1 ? "ão" : "á"} ${this.term} parcela${this.term > 1 ? "s" : ""} de ${this.result.portion}`;
      navigator.clipboard.writeText(e)
    }
  }
}
</script>

<template>
  <div class="text-bg-dark">
    <div class="col-6 offset-3">
      <div class="col-4 offset-4 pt-5">
        <img alt="Logo" class="img-fluid" src="../img/logo.jpeg">
      </div>
      <div class="mb-3">
        <label for="exampleFormControlInput1" class="form-label">Valor: </label>
        <input type="number" class="form-control" v-model="value" @change="process">
      </div>
      <div class="col-12">
        <label class="form-label">Número de parcelas: </label>
        <input type="range" class="form-range" min="1" max="18" v-model="term" @change="process">
        <h2 class="text-center">{{ term }}</h2>
      </div>
      <div class="form-check form-switch">
        <input class="form-check-input" type="checkbox" role="switch" v-model="limit" @change="process">
        <label class="form-check-label">Calcular {{limit?"a partir do limite":"valor à receber"}}</label>
      </div>
      <div class="jumbotron">
        <h1 class="display-6">Detalhes da simulação</h1>
        <p class="lead">Será cobrado no cartão: R$: {{ result.willCharge }}</p>
        <p class="lead">Você receberá: R$: {{ result.willReceive }}</p>
        <hr class="my-4">
        <p>Ser{{ parseInt(term) === 1 ? "á" : "ão" }} {{ term }} parcela{{ parseInt(term) === 1 ? "" : "s" }} de R$:
          {{ result.portion }}</p>
      </div>
      <p class="lead">
        <a class="btn btn-outline-primary btn-lg" role="button" @click="copy">Copiar</a>
      </p>
    </div>
  </div>
</template>

<style scoped>

</style>
