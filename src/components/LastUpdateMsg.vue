<template>
  <p>
    Last update : <strong>{{new Date(parseInt(dateUpdate)).toLocaleString()}}</strong> <br> Current : <strong>{{new Date().toLocaleTimeString()}}</strong>
  </p>
</template>

<script>
export default {
  name: "LastUpdateMsg",
  props: ["dateUpdate"],
}
</script>

<style scoped>

</style>