<template>
  <div class="field-input" :class="{ 'has-icon': icon }">
    <label v-if="label" class="label-input">
      {{ label }}
      <template v-if="important">(<span class="important">*</span>)</template>
    </label>
    <span v-if="icon" class="input-icon-left fas" :class="'fa-' + icon"></span>
    <input
      :type="typeInput"
      class="input"
      :class="{ 'has-error': errorMsg }"
      :style="styleInput"
      :placeholder="placeholder"
      v-model="valueInput"
      @blur="$emit('blur')"
    />
    <p v-if="errorMsg" class="text-error">{{ errorMsg }}</p>
  </div>
</template>

<script>
export default {
  name: "FieldInput",
  model: {
    prop: "value",
    event: "change",
  },
  props: {

    // icon bên trái của input. Mặc định là không có.
    icon: {
      type: String,
      default: null,
    },

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

    // Từ hint text trong input.
    placeholder: {
      type: String,
      default: null,
    },

    // xác định input có tự động focus hay không.
    autoFocusInput: {
      type: Boolean,
      default: false,
    },

    // Style cho input.
    styleInput: {
      type: String,
      default: null,
    },

    // type của input. Mặc định là text. Không hỗ trợ cho radio và checkbox.
    typeInput: {
      type: String,
      default: "text",
    },

    /**
     * Giá trị khởi tạo của input.
     */
    value: {
      default: null,
    },

    /**
     * Hiển thị lỗi input khi valid.
     */
    errorMsg: {
      type: String,
      default: null,
    },
  },

  computed: {
    valueInput: {
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