<template>
  <div class="check-item">
    <div class="name">{{item.title}}</div>
    <a-progress :percent="item.value"  trailColor="#EAE8E8FF" />
    <div style="width: 100px">
      <a-tag color="#e6a23c" v-if="item.value < 100">测试中</a-tag>
<!--      <a-tag color="#ee0a24">未通过</a-tag>-->
      <a-tag color="#07c160" v-if="item.value === 100">通过</a-tag>
    </div>
  </div>
</template>

<script setup lang="ts">
const props = defineProps({
  item: Object,
});
</script>

<style>
.check-item {
  width: 65%;
  height: 45px;
  display: flex;
  align-items: center;
  padding: 0 16px;
  font-size: 14px;
  color: #eae8e8;
}
.name {
  width: 200px;
  text-align: right;
}
.ant-progress {
  margin: 0 32px;
}
.ant-progress-text {
  color: #ffffff !important;
}
</style>
