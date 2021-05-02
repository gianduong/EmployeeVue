<template>
  <div class="dialog" :class="{ 'dialog-hide': !show }">
    <div class="dialog-content">
      <div class="dialog-header">
        <div class="dialog-title">THÔNG BÁO</div>
        <div class="dialog-button-close" @click="closeDialog">&#x2715;</div>
      </div>
      <div class="dialog-body">
        {{ msg }}
      </div>
      <div class="dialog-footer">
        <Button text="Ok" @click="closeDialog" />
      </div>
    </div>
  </div>
</template>

<script>
import Button from './Button';
export default {
  name: "AlertDialog",
  components: {
      Button
  },
  props: {
    /**
     * Prop xác định trạng thái của dialog thông báo.
     * true: hiện
     * false: ẩn
     */
    show: Boolean,

    /**
     * Lời nhắn của dialog thông báo.
     */
    msg: String,
  },
  methods: {
    /**
     * Hàm thực hiện đóng dialog thông báo.
     */
    closeDialog() {
      this.$emit("onClose");
    },
  },
};
</script>

<style scoped>
.dialog {
  overflow: auto;
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: #000;
  background-color: rgba(0, 0, 0, 0.4);
  display: flex;
  justify-content: center;
  align-items: center;
}

.dialog.dialog-hide {
  display: none;
}

.dialog .dialog-content {
  width: 400px;
  background-color: #fff;
  position: relative;
}

.dialog .dialog-header {
  padding: 16px;
}

.dialog .dialog-header .dialog-title {
  font-size: 15px;
  font-family: Google-Sans-Bold;
}

.dialog .dialog-header .dialog-button-close {
  font-size: 16px;
  position: absolute;
  right: 0;
  top: 0;
  height: 40px;
  width: 40px;
  line-height: 40px;
  text-align: center;
  cursor: pointer;
  border-bottom-left-radius: 4px;
}

.dialog .dialog-header .dialog-button-close:hover {
  background-color: #e5e5e5;
}

.dialog .dialog-body {
  padding: 0 24px 24px 24px;
}

.dialog .dialog-footer {
  padding-right: 24px;
  height: 60px;
  background-color: #e5e5e5;
  display: flex;
  justify-content: flex-end;
  align-items: center;
}
</style>