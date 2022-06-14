<template>
  <p>
    Last update : <strong>{{getDate().toLocaleString()}}</strong> <br> Current time : <strong>{{new Date().toLocaleTimeString()}}</strong>
  </p>
  <p>
    1 EUR : <strong>{{EURtoCAD}} CAD</strong>
    <br>
    1 CAD : <strong>{{CADtoEUR}} EUR</strong>
  </p>
</template>

<script>
export default {
  name: "LastUpdateMsg",
  props: ["lastUpdate", "EURtoCAD", "CADtoEUR"],
  methods: {
    getDate() {
      return new Date(parseInt(this.lastUpdate));
    }
  }
}
</script>

<style scoped>

</style>