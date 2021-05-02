<template>
  <div @click="$emit('click')">
    <div
      v-if="!onlyIcon"
      class="btn"
      :class="[{ 'has-icon': iconLeft }, color ? 'btn-' + color : '']"
      :style="styleBtn"
    >
      <div v-if="iconLeft" class="btn-icon fas" :class="'fa-' + iconLeft"></div>
      <div class="btn-text">{{ text }}</div>
    </div>

    <div v-else class="btn-only-icon" :style="styleBtn">
      <div class="fas" :class="'fa-' + icon"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Button",
  props: {
    /**
     * Style tự định nghĩa cho button.
     */
    styleBtn: {
      type: String,
      default: null
    },

    /**
     * Button text
     */

    /**
     * Màu nền của button
     * Mặc định là primary
     * Hỗ trợ: primary (màu chính), secondary (màu phụ) và null (không màu)
     */
    color: {
      type: String,
      default: "primary",
    },

    // Text của button text. Mặc định là null.
    text: {
      type: String,
      default: null,
    },

    /**
     * icon left của button text. Giá trị của prop là class fontawesome 5 solid. Vd: icon search thì sẽ là search.
     */
    iconLeft: {
      type: String,
      default: null,
    },

    /**
     * Xác định loại button
     * true: button chỉ có icon
     * false: button text (icon có thể có).
     */
    onlyIcon: {
      type: Boolean,
      default: false,
    },

    // icon của button chỉ có icon theo fontawesome.
    icon: {
      type: String,
      default: null,
    },
  },
};
</script>

<style scoped>
.btn {
  border-radius: 4px;
  height: 40px;
  line-height: 40px;
  padding-left: 24px;
  padding-right: 24px;
  display: inline-block;
  cursor: pointer;
  text-decoration: none;
}

.btn.btn-primary {
  background-color: #019160;
  color: #fff;
}

.btn.btn-primary:hover {
  background-color: #2fbe8e;
}

/* Button có icon */

.btn.has-icon {
  padding-left: 16px;
  padding-right: 16px;
}

.btn.has-icon .btn-icon {
  display: inline-block;
}

.btn.has-icon .btn-text {
  display: inline-block;
  margin-left: 8px;
}

/* Button chỉ có icon */

.btn-only-icon {
  height: 40px;
  line-height: 40px;
  border: 1px solid #bbbbbb;
  padding-left: 12px;
  padding-right: 12px;
  border-radius: 4px;
  cursor: pointer;
  text-decoration: none;
}

.btn-only-icon:hover {
  background-color: #e5e5e5;
}
</style>