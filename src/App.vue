<template>
  <div class="app-con">
    <div class="brand">
      <div class="logo">
        <img :src="icon" />
        <div class="bg"></div>
      </div>
      <div class="hello">{{ title }}</div>
      <div class="edit">编辑项目下的 src/App.vue 开始开发吧</div>
      <div class="records-count">
        当前视图共条
        <b>{{ typeof count === "undefined" ? "?" : count }}</b> 记录
      </div>
    </div>
    <div class="playground">
      <div class="con">
        <div class="block">
          <div class="header">env 映射信息</div>
          <div class="content env">
            {{ JSON.stringify(env, null, 2) }}
          </div>
        </div>
        <div class="block">
          <div class="header">config 视图信息</div>
          <div class="content variables">
            {{ JSON.stringify({ appId, worksheetId, viewId }, null, 2) }}
          </div>
        </div>
        <CallMdFunctionVue :addLog="addLog" />
        <Logs :logs="logs" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { config, env, api } from "mdye";
import CallMdFunctionVue from "./components/CallMdFunction.vue";
import Logs from "./components/Logs.vue";
import icon from "./icon.svg";
const { appId, worksheetId, viewId } = config;
const { getFilterRowsTotalNum } = api;
const title = ref("Hello Vue!");
const count = ref(undefined);
const logs = ref([]);
onMounted(() => {
  addLog("hello world!");
  loadRecords();
});
async function loadRecords() {
  addLog("正在获取记录数量...");
  count.value = await getFilterRowsTotalNum({ worksheetId, viewId });
  addLog(`当前视图共${count.value}条记录`);
}
async function addLog(content) {
  const time = new Date();
  logs.value.push({
    time,
    timeStr: `[${[time.getHours(), time.getMinutes(), time.getSeconds()]
      .map((num) => String(num).padStart(2, 0))
      .join(":")}]`,
    content,
  });
}
</script>

<style>
</style>