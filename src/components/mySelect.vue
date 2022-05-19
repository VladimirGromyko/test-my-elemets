<template>
  <label>
    <input
        :value="modelValue"
        @input="updateInput"
        type="text"
        style="display: none"
    />

    <div v-if="disabled"
         class="dropdown-disabled">
      <div class="choice"
           tabindex="0"
           ref="dropdown"
           style="background-color: #F5F7FA">
        <div class="choice-value">
          {{ myChoice.value }}
        </div>
        <div class="choice-select">
          <div class="arrow-down-disabled"></div>
        </div>
      </div>
    </div>
    <div v-else
         class="dropdown">
      <div class="choice"
           @blur="closeDropdown"
           tabindex="0"
           ref="dropdown"
           @click="toggleDropdown">
        <div class="choice-value">
          {{ myChoice.value }}
        </div>
        <div class="choice-select">
          <div :class="arrowDirectionUp ? 'arrow-up' : 'arrow-down' "></div>
        </div>
      </div>

      <div :class="isOpenSelect ? 'options-visible': 'options-hidden' ">
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
  </label>
</template>

<script>
export default {
  name: "my-select",

  props: {
    modelValue: String,
    title: {},
    dataForSelect: Array,
    disabled: Boolean,
  },
  emits: ['update:modelValue'],

  data() {
    return {
      isOpenSelect: false,
      options: [],
      myChoice: {},
      myPreviousChoice: {},
      arrowDirectionUp: false,
    }
  },
  methods: {
    toggleDropdown() {
      this.isOpenSelect = !this.isOpenSelect;
      this.arrowDirectionUp = !this.arrowDirectionUp
    },
    closeDropdown() {
      this.isOpenSelect = false;
      this.arrowDirectionUp = false
    },
    selectOption(option, index) {
      this.options[this.myPreviousChoice.index].selectedValue = false
      this.options[index].selectedValue = true
      this.myChoice = {...option, selectedValue: true}
      this.myPreviousChoice = {...this.myChoice, index}
      this.updateInput(this.myChoice.value)
    },
    updateInput(value) {
      this.$emit("update:modelValue", value)
    }
  },
  mounted() {
    this.dataForSelect.forEach(element => {
      let selectedValue = (element === this.title)
      this.options.push({value: element, selectedValue})
    })
    let myChoice, myChoiceIndex;
    myChoice = this.options.filter((element, index) => {
      element.selectedValue && (myChoiceIndex = index)
      return element.selectedValue === true
    })
    this.myChoice = myChoice[0] ? myChoice[0]
        : {value: this.title.toString(), selectedValue: true}
    myChoiceIndex===undefined && (myChoiceIndex=0)
    this.myPreviousChoice = {...this.myChoice, index: myChoiceIndex}
  }
}
</script>

<style>
.dropdown {
  width: 394px;
  height: auto;
  left: -7px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.dropdown-disabled {
  width: 394px;
  height: auto;
  left: -7px;
  position: relative;
  display: flex;
  flex-direction: column;
  cursor: not-allowed;
}

.choice {
  display: flex;
  flex-direction: row;
  border: 1px solid #DCDFE6;
  border-radius: 4px;
  height: auto;
  margin: 0 7px 0 7px;
}

.choice:hover {
  border: 1px solid #b4bccc;
  transition: all 500ms ease;
}

.choice-value {
  flex-basis: 350px;
  height: auto;
  color: #606266;
  text-align: start;
  font-size: 14px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  border-radius: 4px;
  line-height: 1.2;
  padding: 10px;
}

.choice-select {
  display: flex;
  align-items: center;
  justify-content: center;
  height: auto;
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
  top: -3px;
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
  top: 3px;
}

.arrow-down-disabled {
  width: 10px;
  padding: 5px;
  position: relative;
  border-radius: 5px;
}

.arrow-down-disabled::before, .arrow-down-disabled::after {
  content: '';
  position: absolute;
  left: 0;
  top: 5px;
  border: 7px solid transparent;
  border-top: 7px solid #C0C4CC;
}

.arrow-down-disabled::after {
  border-top: 7px solid #F5F7FA;
  top: 3px;
}

.options-visible {
  overflow: hidden;
  border-radius: 4px;
  height: auto;
  visibility: visible;
  opacity: 1;
  transition: visibility 0.3s linear, opacity 0.3s linear;
}

.options-hidden {
  visibility: hidden;
  opacity: 0;
  transition: visibility 0.3s linear, opacity 0.3s linear;
}

.select-options {
  border: 1px solid #E4E7ED;
  border-radius: 4px;
  padding: 5px 1px;
  box-shadow: 0 2px 12px 0 rgb(0 0 0 / 10%);
}

.option {
  width: auto;
  height: auto;
  white-space: normal;
  color: #606266;
  text-align: left;
  line-height: 1.2;
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
  line-height: 1.2;
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
  background-color: #FFF;
  /*position: relative;*/
  text-align: left;
  position: absolute;
  z-index: 3;
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