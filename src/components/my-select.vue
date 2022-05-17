<template>
  <div class="dropdown">
    <div class="choice"
         @blur="closeDropdown"
         tabindex="0"
         ref="dropdown"
         @click="toggleDropdown">
      <div class="choice-value">
        {{ myChoice.value }}
      </div>
      <div class="choice-select">
        <div :class="arrowDirection ? 'arrow-up' : 'arrow-down' "></div>
      </div>
    </div>

    <div class="options" :style="{'max-height': open ? '300px' : '0px'}">
      <div class="block-triangle">
        <div class="select-options">
          <div v-for="(option, index) in options"
               :key="option+index"
               @mousedown="selectOption(option, index)"
               :class="!option.selectedValue ? 'option': 'option-plus' ">
            {{ option.value }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "my-select",
  props: {
    firstSelect: String,
    dataForSelect: []
  },

  data() {
    return {
      open: false,
      options: [],
      myChoice: {},
      myPreviousChoice: {value: this.firstSelect, selectedValue: true, index: 0},
      arrow: ['arrow-up', 'arrow-down'],
      arrowDirection: false
    }
  },
  methods: {
    toggleDropdown() {
      this.open = !this.open;
      this.arrowDirection = !this.arrowDirection
    },
    closeDropdown() {
      this.open = false;
      this.arrowDirection = !this.arrowDirection
    },
    selectOption(option, index) {
      this.options[this.myPreviousChoice.index].selectedValue = false
      this.options[index].selectedValue = true
      this.myChoice = {...option, selectedValue: true}
      this.myPreviousChoice = {...this.myChoice, index}
      this.$emit("input", option);
    },
  },
  mounted() {
    this.myChoice = {value: this.firstSelect, selectedValue: true}
    this.dataForSelect.forEach(element => {
      let selectedValue
      if (element=== this.firstSelect) {
        selectedValue=true
      } else selectedValue=false
      this.options.push({value:element, selectedValue})
    })
  }
}
</script>

<style>
.dropdown {
  width: 380px;
  height: auto;
  position: relative;
  display: flex;
  flex-direction: column;
}

.choice {
  display: flex;
  flex-direction: row;
  border: 1px solid #DCDFE6;
  border-radius: 4px;
  height: auto;
  margin: 0 7px 0 7px;
}

.choice-value {
  flex-basis: 350px;
  height: auto;
  color: #606266;
  text-align: start;
  font-size: 14px;
  background-color: #FFF;
  background-image: none;
  border-radius: 4px;
  line-height: 1.2;
  padding: 10px;
}

.choice-select {
  display: flex;
  align-items: center;
  justify-content: center;
  height: auto;
  background-color: #FFF;
  border-radius: 5px;
  flex-basis: auto;
}

.arrow-up {
  width: 10px;
  background-color: #FFF;
  padding: 5px;
  position: relative;
  border-radius: 5px;
}

.arrow-up::before, .arrow-up::after {
  content: '';
  position: absolute;
  left: 0;
  top: -5px;
  border: 7px solid transparent;
  border-bottom: 7px solid #C0C4CC;;
}

.arrow-up::after {
  border-bottom: 7px solid #FFF;
  top: -4px;
}

.arrow-down {
  width: 10px;
  background-color: #FFF;
  padding: 5px;
  position: relative;
  border-radius: 5px;
}

.arrow-down::before, .arrow-down::after {
  content: '';
  position: absolute;
  left: 0;
  top: 5px;
  border: 7px solid transparent;
  border-top: 7px solid #C0C4CC;;
}

.arrow-down::after {
  border-top: 7px solid #FFF;
  top: 4px;
}

.select-options {
  border: 1px solid #E4E7ED;
  border-radius: 4px;
  padding: 5px 1px;
  box-shadow: 0 2px 12px 0 rgb(0 0 0 / 10%);
}

.options {
  overflow: hidden;
  transition: max-height 300ms;
  border-radius: 4px;
  height: auto;
}

.option {
  width: auto;
  height: auto;
  white-space: normal;
  color: #606266;
  text-align: left;
  display: flex;
  align-items: center;
  cursor: pointer;
  font-family: "Times New Roman";
  padding: 6px 10px;
}

.option:hover {
  background: #F5F7FA;
}

.option-plus {
  width: auto;
  height: auto;
  white-space: normal;
  color: #409EFF;
  font-size: 14px;
  font-family: "Times New Roman";
  font-weight: 700;
  text-align: left;
  display: flex;
  align-items: center;
  cursor: pointer;
  padding: 6px 10px;
}

.option-plus:hover {
  background: #F5F7FA;
}

.block-triangle {
  padding: 12px 7px 7px 7px;
  position: relative;
  text-align: left;
}

.block-triangle::before, .block-triangle::after {
  content: '';
  position: absolute;
  left: 35px;
  top: 0;
  border: 6px solid transparent;
  border-bottom: 6px solid #E4E7ED;
}

.block-triangle::after {
  border-bottom: 6px solid white;
  top: 1px;
}

</style>