<template>
  <!--  <div > <p>-->
  <!--    {{ selectedCar }}-->
  <!--    </p>-->
  <my-select
      v-model="selectedName"
      :firstSelect="selectedName"
      :dataForSelect="finOperatAttitudeForAdded"
  />
  <!--  {{ selectedCar }}-->
  <!--  </div>-->


</template>

<script>


import MySelect from "@/components/my-select";

export default {
  name: 'App',
  components: {MySelect},
  data() {
    return {
      finOperatAttitudeForAdded: [
        "лицо, имеющее отношение к финансовой операции (является заявителем по заявлению о государственной регистрации)",
        "представитель лица, имеющего отношение к финансовой операции (не является представителем заявителя согласно заявлению о государственной регистрации)",
      ],
      selectedName: "лицо, имеющее отношение к финансовой операции (является заявителем по заявлению о государственной регистрации)"
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
  margin-top: 10px;
}

</style>
