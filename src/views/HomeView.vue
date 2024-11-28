<script setup lang="ts">
import {ref} from 'vue'
import Layout from '@/components/Layout.vue';
import MyDashboardBox from '@/components/MyDashboardBox.vue';
import CheckList from '@/components/CheckList.vue';
import CheckContent from '@/components/CheckContent.vue';
import Result from '@/components/Result.vue';
const list = ref([]);
const isStart = ref(false);
const isStop = ref(false);
const isReset = ref(false);
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
</script>

<template>
  <Layout>
    <template #sider>
      <MyDashboardBox title="测试项">
        <CheckList @reset="handleReset" @join="handleJoin" @start="handleStart" @stop="handleStop"></CheckList>
      </MyDashboardBox>
    </template>
    <template #content>
      <div style="width: 100%;height: 100%;display: flex;flex-direction: column;">
        <div style="width: 100%;height: calc(100% - 240px);">
          <MyDashboardBox title="测试进程">
            <CheckContent :list="list" :start="isStart" :stop="isStop" :reset="isReset"></CheckContent>
          </MyDashboardBox>
        </div>
        <div style="width: 100%;height: 240px;">
          <MyDashboardBox title="测试结果">
            <Result></Result>
          </MyDashboardBox>
        </div>
      </div>
    </template>
  </Layout>
</template>
