<template>
  <div class="result-wrap">
    <div class="content">
      <div class="content-top">共测试</div>
      <div class="content-bottom">{{total}}&nbsp;项</div>
    </div>
    <div class="content">
      <div class="content-top">通过</div>
      <div class="content-bottom">{{result.pass}}&nbsp;项</div>
    </div>
    <div class="content">
      <div class="content-top">未通过</div>
      <div class="content-bottom">{{result.fail}}&nbsp;项</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import {computed} from 'vue'

const props = defineProps({
  result: {
    type: Object,
    default: () => ({
      fail: 0,
      pass: 0,
    })
  }
});
const total = computed(() => {
  return props.result.fail + props.result.pass;
})
</script>

<style scoped>
.result-wrap {
  width: 100%;
  height: 100%;
  padding: 32px 16px 0;
  display: flex;
  align-items: start;
  justify-content: start;
}
.content {
  width: 107px;
  height: 50px;
  padding: 6px 0;
  margin-right: 24px;
  background: url("@/assets/images/result-bg.png") no-repeat center center;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
}
.content-top {
  width: 100%;
  text-align: center;
  font-weight: 400;
  font-size: 12px;
  color: #00efff;
}
.content-bottom {
  width: 100%;
  text-align: center;
  font-weight: bold;
  font-size: 14px;
  color: #00efff;
}
</style>
