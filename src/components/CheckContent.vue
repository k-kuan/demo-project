<template>
<div class="check-content">
  <CheckItem v-for="(item, index) in list" :key="index" :item="item"></CheckItem>
</div>
</template>

<script setup lang="ts">
  import {watch, ref} from 'vue';
  import CheckItem from '@/components/CheckItem.vue';
  const props = defineProps({
    list: Array,
    start: {
      type: Boolean,
      default: false
    },
    stop: {
      type: Boolean,
      default: false
    },
    reset: {
      type: Boolean,
      default: false
    }
  });
  watch(() => props.list, (value) => {
    if(value && value.length > 0) {
      startProgressForIndex(0)
    }
  })
  watch(() => props.start, (newValue) => {
    if(newValue && props.list && props.list.length > 0) {
      toggleProgress();
    }
  })
  watch(() => props.stop, (newValue) => {
    if(newValue) {
      toggleProgress();
    }
  })
  watch(() => props.reset, (newValue) => {
    if(newValue) {
      resetAll();
    }
  })
  const totalDuration = 5000; // 总持续时间（毫秒）
  const isRunning = ref(false); // 控制是否运行
  const currentIndex = ref(0); // 当前正在运行的进度条索引

  function startProgress(item, duration, callback) {
    const interval = 100; // 每次更新之间的间隔时间
    const steps = Math.floor(duration / interval); // 计算出总的步骤数
    let currentStep = 0;
    let initialValue = item.value;

    const timerId = setInterval(() => {
      if (isRunning.value) {
        if (currentStep >= steps) {
          clearInterval(timerId);
          item.value = 100; // 确保最终值为100
          if (callback) {
            callback();
          }
          return;
        }
        item.value = Math.min(100, Math.round(initialValue + (currentStep / steps) * 100));
        currentStep++;
      }
    }, interval);

    return timerId;
  }

  // 递归函数来顺序启动进度条
  function startProgressForIndex(index) {
    if (index < props.list.length && isRunning.value) {
      const item = props.list[index];
      const timerId = startProgress(item, totalDuration, () => {
        // 当前item的进度条完成后，递归调用下一个
        currentIndex.value = index + 1;
        startProgressForIndex(currentIndex.value);
      });
      item.timerId = timerId; // 保存timerId以便于清理
    }
  }
  // 切换进度条的开始/停止状态
  function toggleProgress() {
    if (isRunning.value) {
      isRunning.value = false;
      // 停止所有正在进行的进度条
      props.list.forEach(item => {
        if (item.timerId) {
          clearInterval(item.timerId);
        }
      });
    } else {
      isRunning.value = true;
      // 从当前索引开始进度条
      startProgressForIndex(currentIndex.value);
    }
  }
  // 复位所有操作并初始化数据
  function resetAll() {
    console.log('重置')
    isRunning.value = false;
    currentIndex.value = 0;
    props.list.forEach(item => {
      item.value = 0; // 重置value为0
      if (item.timerId) {
        clearInterval(item.timerId); // 清除正在进行的定时器
      }
    });
  }
</script>

<style scoped>
.check-content {
  width: 100%;
  height: 100%;
  overflow-y: auto;
}
</style>
