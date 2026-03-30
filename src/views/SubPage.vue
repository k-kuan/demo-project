<script setup lang="ts">
import {ref} from 'vue'
import Layout from '@/components/Layout.vue';
import MyDashboardBox from '@/components/MyDashboardBox.vue';
import CheckList2 from '@/components/CheckList2.vue';
import CheckContent from '@/components/CheckContent.vue';
import Result from '@/components/Result.vue';
const list = ref([]);
const isStart = ref(false);
const isStop = ref(false);
const isReset = ref(false);
const isReStart = ref(false);
const resultObj = ref({
  fail: 0,
  pass: 0,
})
const isFinish = ref(false);
const handleReset = (item) => {
  isReset.value = item;
}
const handleJoin = (item) => {
  list.value = item;
}
const handleStart = (item) => {
  isStart.value = item;
}
const handleStop = (item) => {
  isStop.value = item;
}
const handleReStart = (item) => {
  isReStart.value = item;
}
const handleResult = (item) => {
  resultObj.value = item;
}
const handleFinish = (item) => {
  isFinish.value = item;
}
</script>

<template>
  <Layout title="时序测量及显控系统2">
    <template #sider>
      <MyDashboardBox title="测试项">
        <CheckList2 @reset="handleReset"
                   @join="handleJoin"
                   @start="handleStart"
                   @stop="handleStop"
                   @re-start="handleReStart"
                   :finish="isFinish"></CheckList2>
      </MyDashboardBox>
    </template>
    <template #content>
      <div style="width: 100%;height: 100%;display: flex;flex-direction: column;">
        <div style="width: 100%;height: calc(100% - 240px);">
          <MyDashboardBox title="测试进程">
            <CheckContent :list="list"
                          :start="isStart"
                          :stop="isStop"
                          :reset="isReset"
                          :re-start="isReStart"
                          @result="handleResult"
                          @finish="handleFinish"></CheckContent>
          </MyDashboardBox>
        </div>
        <div style="width: 100%;height: 240px;">
          <MyDashboardBox title="测试结果">
            <Result :result="resultObj"></Result>
          </MyDashboardBox>
        </div>
      </div>
    </template>
  </Layout>
</template>
