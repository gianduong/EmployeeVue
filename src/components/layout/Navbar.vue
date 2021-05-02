<template>
  <div class="navbar">
    <div class="logo-box">
      <div class="toggle fas fa-th"></div>
      <div class="logo"></div>
    </div>

    <div class="navbar-content">
      <div class="navbar-item">
        <div class="navbar-item-icon fas fa-chart-pie"></div>
        <div class="navbar-item-text">Tổng quan</div>
      </div>
      <div class="navbar-item">
        <div class="navbar-item-icon fas fa-chart-bar"></div>
        <div class="navbar-item-text">Báo cáo</div>
      </div>
      <div class="navbar-item">
        <div class="navbar-item-icon fas fa-chart-pie"></div>
        <div class="navbar-item-text">Mua hàng</div>
      </div>
      <router-link to="/employee" class="navbar-item" :class="{active: $route.name == 'employee'}">
        <div class="navbar-item-icon fas fa-users"></div>
        <div class="navbar-item-text">Danh sách nhân viên</div>
      </router-link>
      <div class="navbar-item">
        <div class="navbar-item-icon fas fa-cog"></div>
        <div class="navbar-item-text">Thiết lập hệ thống</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Navbar",
}
</script>

<style scoped>
.navbar {
    width: 226px;
    height: 100vh;
    border-right: 2px solid #e5e5e5;
}


/* Css cho logo */

.navbar .logo-box {
    height: 48px;
    display: flex;
    align-items: center;
    padding-left: 16px;
}

.navbar .logo-box .toggle {
    font-size: 24px;
    flex-shrink: 1;
    cursor: pointer;
}

.navbar .logo-box .logo {
    margin-left: 10px;
    height: 30px;
    width: 150px;
    flex-shrink: 1;
    background-image: url("../../assets/img/cukcuk-logo.png");
    background-position: left;
    background-repeat: no-repeat;
    background-size: contain;
    cursor: pointer;
}


/* css cho navbar menu */

.navbar .navbar-content .navbar-item {
    height: 44px;
    padding-left: 16px;
    display: flex;
    align-items: center;
    cursor: pointer;
    text-decoration: none;
}

.navbar .navbar-content .navbar-item:active,
.navbar .navbar-content .navbar-item.active {
    background-color: #019160;
    color: #fff;
}

.navbar .navbar-content .navbar-item:hover:not(.active) {
    background-color: #e5e5e5;
    color: #000;
}

.navbar .navbar-content .navbar-item-icon {
    font-size: 16px;
    line-height: 20px;
    height: 20px;
    width: 20px;
    margin-right: 10px;
}

.navbar .navbar-content .navbar-item-text {}
</style>