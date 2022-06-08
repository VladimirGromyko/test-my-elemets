<template>
  <div class="profileFormBlockFieldInput">
      <my-select
          v-model="selectedName"
          :placeholder="selected"
          :optionData="testArr"
          :disabled="false"
      />
    <br>
      <div>{{ selectedName }}</div>
<!--    </div>-->

<!--    <div class="my-class">выпадающие списки всегда отображаются вверху, единственное решение для этой проблемы - -->
<!--      скрывать выпадающие списки при отображении изображения (отображение: блок или видимость: видимый) и показывать их-->
<!--      при скрытом изображении (отображение: нет или видимость: скрыто)-->
<!--    </div>-->


  </div>


</template>

<script>


import MySelect from "@/components/mySelect";

export default {
  name: 'App',
  components: {MySelect},
  data() {
    return {
      testArr: [
        "лицо, имеющее отношение к финансовой операции (не является заявителем по заявлению о государственной регистрации)",
        "представитель лица, имеющего отношение к финансовой операции (не является представителем заявителя согласно заявлению о государственной регистрации)",
      ],
      selectedName: '',
      finOperatAttitudeForAdded: [
        ['А', 'Бваимява feg fre f', 'В', 'Г gfsdf вап gfs вавп ыва цукц кцукацукц twergfewrfe ав вапваппу пва ', 'Д'],
        ['A', 'B вап пав sdffdsg ', 'C gsdfwer trwe rtew rewerew trety dhdrt rsgt крек куеенг кке кнег н frgs d g', 'D', 'E'],
      ],
      selected: '',
      message: 'hello'
    }
  },
}


</script>

<style>
#app {
  /*font-family: Avenir, Helvetica, Arial, sans-serif;*/
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 10px;
}


.profileFormBlockFieldInput {
  display: inline-block;
  width: 400px;
  margin-left: 60px;
}

.my-select {
  max-width: 390px;
  /*height: 97px;*/
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

</style>
