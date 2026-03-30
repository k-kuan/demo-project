<template>
  <div class="check-item">
    <div class="check-item-child">
      <div class="name">{{item.title}}</div>
      <a-progress :percent="item.value" strokeColor="#00efff"  trailColor="#1B9CFF40" />
      <div style="width: 100px">
        <a-tag color="#00efff" style="color: #1a1919" v-if="!item.failureResult && item.value < 100">测试中</a-tag>
        <a-tag color="#ee0a24" v-if="item.failureResult === 'FALL' && item.value < 100">未通过</a-tag>
        <a-tag color="#45eba5" style="color: #163a5f;" v-if="item.failureResult === 'PASS' && item.value === 100">通过</a-tag>
      </div>
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
  width: 70%;
  height: 45px;
  padding: 10px 0;
  font-size: 14px;
  color: #00efff;
}
.check-item-child {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  background: linear-gradient(90deg, #00317335 0%, #1B9CFF40 52%, #00317335 100%);
}
.name {
  width: 300px;
  text-align: right;
}
.ant-progress {
  margin: 0 32px;
}
.ant-progress-text {
  color: #00efff !important;
}
</style>
