<template>
  <div class="wrapper">
    <div class="dropdown"
         :class="{'dropdown-disabled': disabled}"
         tabindex="0"
         ref="dropdown"
         @click="toggleDropdown"
         @blur="closeDropdown"
    >
      <span :class="viewPlaceholder">{{ choice.value }}</span>
      <div class="arrow-up" :class="[{ 'arrow-down': isOpenSelect }]"/>
    </div>
    <div class="options" :class="isOpenSelect ? 'options-visible': 'options-hidden' ">
      <div class="block-triangle">
        <div class="select-options">
          <div v-for="(option, index) in options"
               :key="option+index"
               @click="selectOption(option, index)"
               :class="!option.selectedValue ? 'option': 'option option-plus' ">
            {{ option.value }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MySelect",

  props: {
    modelValue: {type: String || Number},
    placeholder: {default: ''},
    optionData: {type: Array, required: true},
    disabled: Boolean,
  },
  data() {
    return {
      isOpenSelect: false,
      options: [],
      choice: {},
      previousChoice: {},
      firstModel: this.modelValue,
    }
  },
  methods: {
    toggleDropdown() {
      if (!this.disabled) this.isOpenSelect = !this.isOpenSelect;
    },
    closeDropdown() {
      this.isOpenSelect = false;
    },
    selectOption(option, index) {
      this.options[this.previousChoice.index].selectedValue = false
      this.options[index].selectedValue = true
      this.choice = {...option, selectedValue: true}
      this.previousChoice = {...this.choice, index}
      this.updateInput(this.choice.value)
    },
    updateInput(value) {
      this.$emit("update:modelValue", value);
    },
    getPlaceholder() {
      let value = this.modelValue, placeholder = this.placeholder
      if (!this.modelValue && this.modelValue === '' ||
          typeof (this.modelValue) !== "string" &&
          typeof (this.modelValue) !== "number") {
        value = ''
      }
      if (!this.placeholder || typeof (this.placeholder) !== "string" && typeof (this.placeholder) !== "number") {
        placeholder = 'Выберите из списка'
      }
      if (value) placeholder = value
      if (this.disabled) placeholder = value
      return placeholder;
    },
  },
  computed: {
    viewPlaceholder() {
      const value = this.getPlaceholder()
      let choiceAppearance
      if (this.disabled) choiceAppearance = ''
      else {
        if (this.modelValue) {
          if (value !== this.firstModel && this.firstModel === this.modelValue) choiceAppearance = 'choice-empty'
          else choiceAppearance = 'choice'
        } else if (value === this.choice.value) choiceAppearance = 'choice-empty'
        else choiceAppearance = 'choice'
      }
      return choiceAppearance
    }
  },

  mounted() {
    const placeholder = this.getPlaceholder()
    this.optionData.length && this.optionData.forEach(element => {
      if (typeof (element) !== "string" && typeof (element) !== "number") {
        element = ''
      }
      const selectedValue = element === placeholder;
      element && this.options.push({value: element, selectedValue})
    })
    let choiceIndex = 0;
    const choice = this.options.length && this.options.filter((el, index) => {
      if (el.selectedValue) choiceIndex = index
      return el.selectedValue
    })
    this.choice = choice[0]
        ? choice[0]
        : {value: placeholder, selectedValue: false}
    this.previousChoice = {...this.choice, index: choiceIndex}
  }
}
</script>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
}

.dropdown {
  box-sizing: border-box;
  position: relative;
  display: flex;
  flex-direction: column;
  min-height: 35px;
  color: #606266;
  cursor: pointer;
  border: 1px solid #dcdfe6;
  line-height: 1.2;
  font-size: 1rem;
  border-radius: 4px;
  padding: 10px 25px 10px 10px;
}

.dropdown-disabled {
  cursor: not-allowed;
  color: black;
  background-color: #f5f7fa;
}

.dropdown:hover {
  border: 1px solid #b4bccc;
  transition: all 500ms ease;
}

.dropdown:focus {
  border-color: #409eff;
}

.choice {
  color: #7272e5;
}

.choice-empty {
  color: #c1c1c5;
}

.arrow-up {
  display: inline-block;
  width: 10px;
  height: 10px;
  position: absolute;
  right: 10px;
  top: 50%;
  transform: translateY(-50%) rotate(45deg);
  border-bottom: 2px solid #c0c4cc;
  border-right: 2px solid #c0c4cc;
}

.arrow-down {
  transform: rotate(225deg);
}

.options {
  position: relative;
  height: 0;
}

.options-visible {
  z-index: 3;
  border-radius: 4px;
  visibility: visible;
  opacity: 1;
  transition: visibility 0.3s linear, opacity 0.3s linear;
}

.options-hidden {
  z-index: 3;
  visibility: hidden;
  opacity: 0;
  transition: visibility 0.3s linear, opacity 0.3s linear;
}

.select-options {
  border: 1px solid #E4E7ED;
  border-radius: 4px;
  padding: 5px 0;
  box-shadow: 0 2px 12px 0 rgb(0 0 0 / 10%);
}

.option {
  white-space: normal;
  color: #606266;
  line-height: 1.2;
  cursor: pointer;
  font-size: 0.85rem;
  padding: 6px 10px;
}

.option:hover {
  background: #F5F7FA;
}

.option-plus {
  color: #409EFF;
  font-weight: 700;
}

.block-triangle {
  position: absolute;
  padding-top: 12px;
  background-color: #FFF;
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