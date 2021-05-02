<template>
  <div class="field-input">
    <label v-if="label" class="label-input">
      {{ label }}
      <template v-if="important">(<span class="important">*</span>)</template>
    </label>
    <select class="input" :style="styleCombobox" v-model="valueCombobox">
      <option v-for="(o, i) in option" :key="i" :value="o.value">
        {{ o.text }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  name: "Combobox",
  model: {
    prop: "value",
    event: "change",
  },
  props: {
    // Tên label của input.
    label: {
      type: String,
      default: null,
    },

    /**
     * Xác định field input có bắt buộc không.
     * true: có
     * false: không.
     */
    important: {
      type: Boolean,
      default: false,
    },

    // style cho combobox
    styleCombobox: {
      type: String,
      default: null,
    },

    // Các option của combobox.
    option: Array,

    /**
     * Giá trị khởi tạo của combobox.
     */
    value: {
      default: null,
    },
  },
  computed: {
    valueCombobox: {
      get() {
        return this.value;
      },
      set(val) {
        this.$emit("change", val);
      },
    },
  },
};
</script>

<style scoped>
.field-input .label-input {
  display: block;
  margin-bottom: 4px;
}

.field-input .label-input .important {
  color: red;
}

.field-input .input {
  border: 1px solid #bbb;
  outline: none;
  border-radius: 5px;
  height: 40px;
  padding-left: 16px;
  font-size: 13px;
  width: 100%;
}

.field-input .input::placeholder {
  color: #bbbbbb;
  font-size: 12px;
}

.field-input.has-icon {
  position: relative;
}

.field-input.has-icon .input-icon-left {
  position: absolute;
  left: 16px;
  top: 12px;
  font-size: 16px;
  color: #bbbbbb;
}

.field-input.has-icon .input {
  padding-left: 40px;
}

.field-input .input:focus {
  border-color: #019160;
}

.field-input .input.has-error {
  border-color: red;
}

.field-input .text-error {
  margin: 0;
  color: red;
}
</style>