<template>
    <div class="relative w-64">
      <!-- Input Field -->
      <input
        type="text"
        v-model="search"
        placeholder="Cari..."
        @focus="showDropdown = true"
        class="w-full px-4 py-2 border rounded-lg shadow-sm focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
      />
  
      <!-- Dropdown List -->
      <ul
        v-if="showDropdown"
        class="absolute w-full mt-1 bg-white border border-gray-300 rounded-lg shadow-lg max-h-48 overflow-y-auto z-10"
      >
        <li
          v-for="option in filteredOptions"
          :key="option.value"
          @click="selectOption(option)"
          class="px-4 py-2 hover:bg-blue-100 cursor-pointer transition"
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
  /* Tidak perlu tambahan style karena sudah menggunakan Tailwind */
  </style>
  