<template>
  <div class="result-wrap">
    <div class="content">
      共检测{{result.fail + result.pass}}项，通过{{result.pass}}项，未通过{{result.fail}}项
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps({
  result: {
    type: Object,
    default: () => ({
      fail: 0,
      pass: 0,
    })
  }
});
</script>

<style scoped>
.result-wrap {
  width: 100%;
  height: 100%;
  padding: 16px;
}
.content {
  font-size: 14px;
  color: #ffffff;
  line-height: 24px;
}
</style>
