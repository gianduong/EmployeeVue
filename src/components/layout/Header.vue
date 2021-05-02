<template>
  <div class="header">
    <div class="header-left">
      <div class="header-left-text">Nhà hàng biển đông</div>
      <div class="header-left-icon fas fa-chevron-down"></div>
    </div>
    <div class="header-right">
      <div class="header-right-notify fas fa-bell"></div>
      <div class="header-right-avatar"></div>
      <div class="header-right-name">Dương Bằng Huân</div>
      <div class="header-right-option fas fa-ellipsis-h"></div>
    </div>
  </div>
</template>

<style scoped>
.header {
    position: absolute;
    left: 226px;
    top: 0;
    right: 0;
    height: 48px;
    padding-right: 16px;
    padding-left: 16px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 2px solid #e5e5e5;
}

.header .header-left {
    display: flex;
    align-items: center;
    cursor: pointer;
}

.header .header-left-text {
    font-family: Google-Sans-Bold;
}

.header .header-left-icon {
    margin-left: 10px;
}

.header .header-right {
    display: flex;
    align-items: center;
}

.header .header-right-notify {
    margin-right: 16px;
}

.header .header-right-avatar {
    background-image: url("../../assets/icon/avatar-default.png");
    background-position: center;
    background-size: contain;
    background-repeat: no-repeat;
    width: 24px;
    height: 24px;
}

.header .header-right-name {
    margin-left: 16px;
    margin-right: 16px;
    font-family: Google-Sans-Bold;
}

.header .header-right-option {}
</style>