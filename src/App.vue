<template>
  <div>
    <NavbarComponent />
    <HeaderComponent />
    <ContentComponent />
  </div>
</template>

<script>
import NavbarComponent from './components/layout/Navbar';
import HeaderComponent from './components/layout/Header';
import ContentComponent from './components/layout/Content';
export default {
  name: "App",
  components: {
    NavbarComponent,
    HeaderComponent,
    ContentComponent
  }
}
</script>

<style>
@import url('./assets/font/fontawesome-5.15.1/css/fontawesome.min.css');
@import url('./assets/font/fontawesome-5.15.1/css/solid.min.css');

* {
    font-family: Google-Sans;
    font-size: 13px;
    box-sizing: border-box;
}

body {
    margin: 0 auto;
}

::-webkit-scrollbar {
    width: 5px;
    height: 5px;
}

::-webkit-scrollbar-track {
    background-color: #f1f1f1;
}

::-webkit-scrollbar-thumb {
    background-color: #bbb;
}

.px-1 {
    padding-left: 8px;
    padding-right: 8px;
}

.mx-1 {
    margin-left: 8px;
    margin-right: 8px;
}

.ml-1 {
    margin-left: 8px;
}

.mt-1 {
    margin-top: 8px;
}

.mt-2 {
    margin-top: 16px;
}

.mb-2 {
    margin-bottom: 16px;
}

.has-text-left {
    text-align: left;
}

.has-text-center {
    text-align: center;
}

.has-text-right {
    text-align: right;
}

.selected {
  background-color: #e5e5e5;
}

.row {
  display: flex;
  flex-wrap: wrap;
}

.col-1 {
  width: 8.33%;
}

.col-2 {
  width: 16.66%;
}

.col-3 {
  width: 25%;
}

.col-4 {
  width: 33.33%;
}

.col-5 {
  width: 41.66%;
}

.col-6 {
  width: 50%;
}

.col-7 {
  width: 58.33%;
}

.col-8 {
  width: 66.66%;
}

.col-9 {
  width: 75%;
}

.col-10 {
  width: 83.33%;
}

.col-11 {
  width: 91.66%;
}

.col-12 {
  width: 100%;
}

[class*="col-"] {
  flex: 0 0 auto;
}

@font-face {
    font-family: Google-Sans;
    src: url("./assets/font/GoogleSans-Regular.otf");
}

@font-face {
    font-family: Google-Sans-Bold;
    src: url("./assets/font/GoogleSans-Bold.otf");
}
</style>