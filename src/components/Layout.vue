<template>
  <Layout>
    <LayoutHeader>Header</LayoutHeader>
    <Layout>
      <LayoutSider>Sider</LayoutSider>
      <LayoutContent>Content</LayoutContent>
    </Layout>
  </Layout>
</template>
<!--@contextmenu.prevent.capture-->
<script setup lang="ts">
  import { Layout, LayoutHeader, LayoutSider, LayoutContent } from 'ant-design-vue'
</script>

<style scoped>
.ant-layout {
  width: 100%;
  height: 100%;
  background: #0b1621;
}
.ant-layout-header {
  height: 100px;
  background: transparent url("@/assets/images/head.png") no-repeat center center;
}
.ant-layout-sider {
  background-color: transparent;
  flex: 0 0 400px !important;
  max-width: 400px !important;
  min-width: 400px !important;
}
</style>
