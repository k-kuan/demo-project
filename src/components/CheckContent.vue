<template>
<div class="check-content">
  <CheckItem v-for="(item, index) in list" :key="index" :item="item"></CheckItem>
</div>
</template>

<script setup lang="ts">
  import {watch, ref, computed} from 'vue';
  import CheckItem from '@/components/CheckItem.vue';
  const emit = defineEmits(['result']);
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
  const computedList = computed(() => {
    const list = props.list;
    return countResults(list);
  })
  function countResults(items) {
    const result = {
      pass: 0,
      fail: 0
    };
    items.forEach(item => {
      if (item.failureResult === 'PASS') {
        result.pass++;
      } else if (item.failureResult === 'FALL') {
        result.fail++;
      }
    });
    return result;
  }
  watch(() => computedList.value, (value) => {
    if(value.fail + value.pass === props.list.length) {
      emit('result', value);
    }
  })
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
  const totalDuration = 3000; // 总持续时间（毫秒）
  const isRunning = ref(false); // 控制是否运行
  const currentIndex = ref(0); // 当前正在运行的进度条索引

  function startProgress(item, duration, callback) {
    const interval = 100; // 每次更新之间的间隔时间
    const steps = Math.floor(duration / interval); // 计算出总的步骤数
    let currentStep = 0;
    let initialValue = item.value;

    const timerId = setInterval(() => {
      if (isRunning.value) {
        if (currentStep >= steps && item.isSuccess) {
          clearInterval(timerId);
          item.value = 100; // 确保最终值为100
          item.failureResult = 'PASS'; // 设置失败结果
          if (callback) {
            callback();
          }
          return;
        }
        if (!item.isSuccess && item.value >= getRandomNumberBetween(60, 90)) { // 随机值来模拟失败
          clearInterval(timerId);
          if (item.isSuccess === false) {
            item.failureResult = 'FALL'; // 设置失败结果
          }
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
    isRunning.value = false;
    currentIndex.value = 0;
    props.list.forEach(item => {
      item.value = 0; // 重置value为0
      item.failureResult = ''; // 清除失败结果
      if (item.timerId) {
        clearInterval(item.timerId); // 清除正在进行的定时器
        delete item.timerId;
      }
    });
  }
  function getRandomNumberBetween(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
  }
</script>

<style scoped>
.check-content {
  width: 100%;
  height: 100%;
  overflow-y: auto;
}
</style>
