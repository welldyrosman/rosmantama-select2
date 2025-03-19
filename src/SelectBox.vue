<template>
    <div class="select-box">
      <input type="text" v-model="search" placeholder="Cari..." @focus="showDropdown = true" />
      <ul v-if="showDropdown">
        <li v-for="option in filteredOptions" :key="option.value" @click="selectOption(option)">
          {{ option.label }}
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      options: Array,
      multiple: Boolean,
    },
    data() {
      return {
        search: "",
        selected: this.multiple ? [] : null,
        showDropdown: false,
      };
    },
    computed: {
      filteredOptions() {
        return this.options.filter((opt) => opt.label.toLowerCase().includes(this.search.toLowerCase()));
      },
    },
    methods: {
      selectOption(option) {
        if (this.multiple) {
          if (!this.selected.includes(option)) {
            this.selected.push(option);
          }
        } else {
          this.selected = option;
          this.showDropdown = false;
        }
        this.$emit("update:modelValue", this.selected);
      },
    },
  };
  </script>
  
  <style>
  .select-box {
    position: relative;
    width: 200px;
  }
  .select-box input {
    width: 100%;
    padding: 8px;
  }
  .select-box ul {
    position: absolute;
    width: 100%;
    background: white;
    list-style: none;
    padding: 0;
    border: 1px solid #ccc;
  }
  .select-box li {
    padding: 8px;
    cursor: pointer;
  }
  .select-box li:hover {
    background: #f0f0f0;
  }
  </style>
  