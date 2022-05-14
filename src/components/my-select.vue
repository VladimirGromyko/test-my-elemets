<template>
  <div class="dropdown">
    <div @blur="closeDropdown" tabindex="0" ref="dropdown" @click="toggleDropdown" class="select">
      {{ value }}
    </div>
    <div class="options" :style="{'max-height': open ? '300px' : '0px'}">
      <div v-for="option in options"
           :key="option+option.index"
           @mousedown="selectOption(option)"
           class="option">
        {{ option }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "my-select",
  props: ["value"],

  data() {
    return {
      open: false,
      options: ["BMW", "Fiat", "Citroen", "Audi", "Tesla"]
    }
  },
  methods: {
    toggleDropdown() {
      this.open = !this.open;
    },
    closeDropdown() {
      this.open = false;
    },
    selectOption(option) {

      this.props.value=option
      console.log(this.props.value)
      this.$emit("input", option);
    }
  }
}
</script>

<style>
.dropdown {
  width: 200px;
  position: relative;
}

.select {
  height: 40px;
  position: absolute;
  left: 0;
  width: 100%;
  background: green;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  cursor: pointer;
}
.option {
  width: 100%;
  height: 40px;
  background: darkgreen;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.option:hover {
  background: green;
}
.options {
  overflow: hidden;
  transition: max-height 200ms;
}
</style>