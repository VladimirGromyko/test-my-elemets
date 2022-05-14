<template>
  <div > <p>
    {{ selectedCar }}
    </p>
    <my-select
        v-model="selectedCar"
        :selectedCar="selectedCar"
    />{{ selectedCar }}
  </div>




</template>

<script>


import MySelect from "@/components/my-select";

export default {
  name: 'App',
  components: {MySelect},
  data() {
    return {
      selectedCar: "BMW"
    }
  }
}


</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>
