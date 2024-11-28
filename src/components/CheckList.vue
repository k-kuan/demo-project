<template>
<div class="check-list-wrap">
  <div class="top">
    <a-button type="primary" @click="onReset">初始化</a-button>
    <a-button type="primary" :disabled="isStart || isStop" @click="onJoinTest">加入测试</a-button>
    <a-button type="primary" :disabled="isStart" @click="onStartTest">开始测试</a-button>
    <a-button type="primary" :disabled="isStop || (!isStart)" @click="onStop">停止测试</a-button>
  </div>
  <div class="bottom">
    <div style="width: 100%;height: 100%;" ref="containerRef">
      <div class="flex" style="height: 50px;">
        <div class="flex">
          <a-checkbox
            v-model:checked="state.checkAll"
            :indeterminate="state.indeterminate"
            @change="onCheckAllChange"
          >
            全选/反选
          </a-checkbox>
          <div style="margin-left: 8px;color: #ffffff">
          共有&nbsp;{{ list.length }}&nbsp;记录，已选中&nbsp;{{ checkedCount }}&nbsp;项</div
          >
        </div>
      </div>
      <div style="width: 100%;height: calc(100% - 50px);overflow-y: auto;overflow-x: hidden;">
        <div style="width: 100%;height: 100%;">
          <template v-for="(item, index) in list" :key="item.id">
            <div class="flex" style="padding: 16px 0;">
              <div
                class="item-content"
                @click="toggleChecked(item)"
              >
                <a-checkbox :checked="item.checked" />
                <div style="margin-left: 16px;margin-right: 16px;">{{ index + 1 }}、</div>
                <div style="margin-right: 16px;">
                  {{item.title}}
                </div>
              </div>
            </div>
          </template>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script setup lang="ts">
  import {reactive, ref, watch, computed} from 'vue';
  const emit = defineEmits(['join', 'start', 'stop', 'reset']);
  const isStart = ref(false);
  const isStop = ref(false);
  const list = ref([
    {
      id: 1,
      checked: false,
      title: '测试项一',
      value: 0,
    },
    {
      id: 2,
      checked: false,
      title: '测试项二',
      value: 0,
    },
    {
      id: 3,
      checked: false,
      title: '测试项三',
      value: 0,
    },
  ]);
  const state = reactive({
    indeterminate: false,
    checkAll: false,
  });
  const checkedCount = computed(() => {
    return list.value.filter((item: any) => item.checked).length;
  });
  const onCheckAllChange = (e: any) => {
    state.indeterminate = false;
    list.value.forEach((item: any) => {
      item.checked = e.target.checked;
    });
  };
  watch(
    () => list.value,
    (val) => {
      if (val && val.length) {
        state.indeterminate = checkMixedStatus(val);
        state.checkAll = val.every((item: any) => item.checked);
      } else {
        state.indeterminate = false;
        state.checkAll = false;
      }
    },
    {
      deep: true,
    },
  );
  const checkMixedStatus = (items: any) => {
    // 检查数组是否为空或只有一个元素
    if (items.length <= 1) {
      return false; // 如果数组为空或只有一个元素，直接返回 false
    }
    // 使用 Set 来存储不同的 checked 值
    const uniqueChecks = new Set(items.map((item) => item.checked));
    // 如果 Set 的大小为 1，说明所有 checked 值都相同
    if (uniqueChecks.size === 1) {
      return false; // 所有 checked 都相同
    }
    // 否则，存在不同的 checked 值
    return true; // 存在混合的 checked 值
  };
  const toggleChecked = (item: any) => {
    item.checked = !item.checked;
  };

  const onReset = () => {
    emit('reset', true);
    isStart.value = false;
    isStop.value = false;
    state.indeterminate = false;
    state.checkAll = false;
    list.value = list.value.map((item: any) => {
      item.checked = false;
      item.value = 0;
      delete item.timerId;
      return item;
    })
    console.log(list.value);
    setTimeout(() => {
      emit('join', []);
      emit('reset', false);
      emit('start', false);
      emit('stop', false);
    }, 500)
  }
  const onJoinTest = () => {
    const checkedList = list.value.filter((item: any) => item.checked);
    emit('join', JSON.parse(JSON.stringify(checkedList)));
  }
  const onStartTest = () => {
    emit('start', true);
    emit('stop', false);
    isStart.value = true;
    isStop.value = false;
  }
  const onStop = () => {
    emit('stop', true);
    emit('start', false);
    isStart.value = false;
    isStop.value = true;
  }
</script>

<style scoped>
.check-list-wrap {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.top {
  width: 100%;
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: space-around;
}
.bottom {
  width: 100%;
  height: calc(100% - 80px);
  padding: 0 16px 16px;
}
.flex {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.ant-btn-primary {
  background: linear-gradient(180deg, rgb(31 54 83) 60%, rgb(31 54 83) 90%);
  border: 1px solid #017db3;
  font-size: 12px;
  padding: 0 20px;
  color: #ffffff;
}
.ant-btn-primary[disabled],.ant-btn-primary[disabled]:hover {
  color: #00000075 !important;
  border-color: #d9d9d9 !important;
  background: #f5f5f5 !important;
}
.item-content {
  width: 100%;
  display: flex;
  align-items: center;
  color: #ffffff;
  font-size: 14px;
  cursor: pointer;
}
.ant-checkbox-wrapper {
  color: #ffffff;
}
</style>
