<!--
 @prettier
-->
<template>
  <div class="back">
    <div class="searchBox">
      <div class="searchBoxTitle">
        <b>搜索内容</b>
      </div>
      <div class="searchBar">
          <input id="searchText" class="searchInput" />
          <button id="searchBtn" class="searchButton">搜索</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  mounted() {},
  methods: {}
};
</script>

<style>
.searchBar {
  width: 100%;
  height: 40px;
  background: white;
  border-radius: 24px;
  box-shadow: 2px 2px 10px black;
}
.searchButton{
  margin: 0px;
  width: 60px;
  background: aqua;
  font-weight: lighter;
  font-family: sans-serif;
  font-size: 1em;
  color: #fff;
  border: 0px;
  border-radius: 24px;
}
.searchInput {
  width: 480px;
  margin: 10px 15px auto 20px;
  font-size: 16px;
  background-color: transparent;
  border: 0px;
}
.searchBoxTitle {
  margin: 0 0 25px;
  font-size: xx-large;
  font-weight: bold;
  letter-spacing: 0.2rem;
  color: white;
  font-family: sans-serif;
  text-align: center;
}
.searchBox {
  width: 600px;
  /* border: 2px solid grey; */
  margin: 200px auto;
}
.back {
  margin: 0%;
  background: url("../../picture/background.jpg") no-repeat;
  background-size: cover;
  background-position: center;
  height: 100%;
  width: 100%;
  position: fixed;
}
</style>
