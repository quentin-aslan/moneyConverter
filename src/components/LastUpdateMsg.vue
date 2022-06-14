<template>
  <p>
    Last update : <strong>{{getDate().toLocaleString()}}</strong> <br> Current time : <strong>{{new Date().toLocaleTimeString()}}</strong>
  </p>
</template>

<script>
export default {
  name: "LastUpdateMsg",
  props: ["lastUpdate"],
  methods: {
    getDate() {
      return new Date(parseInt(this.lastUpdate));
    }
  }
}
</script>

<style scoped>

</style>