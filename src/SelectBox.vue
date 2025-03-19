<template>
    <div class="relative w-64">
      <!-- Selected Items (Menampilkan pilihan yang dipilih) -->
      <div v-if="multiple ? selected.length > 0 : selected" class="selected-box">
        <span v-if="multiple" v-for="option in selected" :key="option.value" class="badge">
          {{ option.label }}
        </span>
        <span v-else>{{ selected?.label }}</span>
      </div>
  
      <!-- Search Box -->
      <input
        type="text"
        v-model="search"
        placeholder="Search..."
        @focus="showDropdown = true"
        class="form-control"
      />
  
      <!-- Dropdown List -->
      <ul
        v-if="showDropdown"
        class="border border-gray-300 shadow-lg max-h-48 overflow-y-auto bg-white absolute w-full"
      >
        <li
          v-for="option in filteredOptions"
          :key="option.value"
          @click="selectOption(option)"
          class="px-4 py-2 cursor-pointer hover:bg-gray-200"
        >
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
      modelValue: [Object, Array]
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
        return this.options.filter((opt) =>
          opt.label.toLowerCase().includes(this.search.toLowerCase())
        );
      }
    },
    methods: {
      selectOption(option) {
        if (this.multiple) {
          if (!this.selected.some(item => item.value === option.value)) {
            this.selected.push(option);
          }
        } else {
          this.selected = option;
          this.showDropdown = false;
        }
        this.search = ""; // Reset search box setelah memilih
        this.$emit("update:modelValue", this.selected);
      }
    }
  };
  </script>
  
  <style>
  .selected-box {
    background: #f3f4f6;
    padding: 5px;
    margin-bottom: 5px;
    border-radius: 5px;
  }
  
  .badge {
    background: #3182ce;
    color: white;
    padding: 2px 6px;
    border-radius: 4px;
    margin-right: 4px;
  }
  </style>
  