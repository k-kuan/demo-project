<template>
  <div class="box-container-wrapper">
    <div class="box-container">
      <a-row v-show="!hideTitleBar" type="flex" class="title" justify="space-around" align="middle">
        <a-col flex="auto">
          <a-row type="flex" class="text" justify="space-around" align="middle">
            <a-col flex="1">
              <slot name="title">
                <div class="title1">
                  {{ title }}
                </div>
              </slot>
            </a-col>
          </a-row>
        </a-col>
      </a-row>
      <div class="box-body">
        <slot></slot>
        <slot name="body"></slot>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from 'vue';
  export default defineComponent({
    name: 'MyDashboardBox',
    inheritAttrs: true,
  });
</script>
<script lang="ts" setup>
  defineProps({
    hideTitleBar: Boolean,
    title: {
      type: String,
      default: () => '',
    },
    showClose: Boolean,
  });
</script>

<style scoped>
  .box-container-wrapper {
    height: 100%;
    width: 100%;
    padding: 8px;
  }
  .box-container {
    display: flex;
    flex-direction: column;
    height: 100%;
    width: 100%;
    background-color: rgb(20 46 67 / 60%);
    background-size: 100% 100%;
    border: 1px solid rgb(16, 40, 80);
    border-radius: 5px;
    margin: 0;
    padding: 5px 5px;
  }
  .box-body {
    flex: 1;
    overflow: hidden;
    overflow-y: auto;
  }
  .title1 {
    font-weight: 700;
    font-style: normal;
    font-size: 16px;
    color: #11d6f3;
    text-align: left;
    padding-left: 8px;
  }
  .title .text {
    height: 30px;
    line-height: 30px;
    width: 100%;
    padding: 0 5px;
    background: linear-gradient(180deg, rgb(31 54 83) 60%, rgb(31 54 83) 90%);
    border-radius: 2px;
    box-shadow: none;
  }
  .title .text .ant-col {
    height: 30px;
    line-height: 30px;
  }
  .title .header {
    height: 30px;
    width: 16px;
    background-position: -3px center, 2px center;
    background-repeat: no-repeat, no-repeat;
    background-size: auto 100%, auto 100%;
  }

  .title .right {
    display: inline-block;
    vertical-align: top;
    height: 30px;
    width: 40px;
    background-position: 0 center, 8px center, 16px center, 24px center;
    background-repeat: no-repeat;
  }

  .btn-close {
    box-sizing: border-box;
    vertical-align: top;
    height: 30px;
    line-height: 30px;
    appearance: none;
    display: inline-block;
    padding: 0;
    color: #11d6f3;
    background: transparent;
    border: 0;
    outline: 0;
    cursor: pointer;
    transition: color 0.3s;
  }
  .btn-close-x {
    display: inline-block;
    width: 30px;
    height: 30px;
    font-size: 16px;
    font-weight: 800;
    font-style: normal;
    line-height: 30px;
    text-align: center;
    text-transform: none;
    text-rendering: auto;
  }

  .btn-close:hover {
    filter: brightness(1.5);
  }
</style>
