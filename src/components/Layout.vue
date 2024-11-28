<template>
  <a-layout>
    <a-layout-header>
      <div class="title">xxxxxxx系统</div>
    </a-layout-header>
    <a-layout style="height: calc(100vh - 85px);">
      <a-layout-sider>
        <slot name="sider"></slot>
      </a-layout-sider>
      <a-layout-content>
        <slot name="content"></slot>
      </a-layout-content>
    </a-layout>
  </a-layout>
</template>
<!--@contextmenu.prevent.capture-->
<script setup lang="ts">
</script>

<style scoped>
.ant-layout {
  width: 100%;
  height: 100%;
  background: #0b1621;
}
.ant-layout-header {
  height: 85px;
  background: transparent url("@/assets/images/head.png") no-repeat center center;
  display: flex;
  align-items: center;
  justify-content: center;
}
.title {
  margin-top: -17px;
  line-height: 100px;
  display: inline-block;
  font-weight: 600;
  font-style: normal;
  font-size: 32px;
  color: #00efff;
  text-shadow: 2px 2px 2px #112d5e;
}
.ant-layout-sider {
  background-color: transparent;
  flex: 0 0 500px !important;
  max-width: 500px !important;
  min-width: 500px !important;
}
</style>
