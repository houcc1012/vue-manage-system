<template>
  <div class="page-container">
    <div style="align-self: flex-start;margin-bottom: 10px; font-size: larger; font-weight: bold;">URL编码/解码</div>
    <div style="align-self: flex-start;">在线URL编码解码工具，支持中文和特殊字符</div>
    <el-card style="width: 50%;">
      <div>输入内容</div>
      <el-input
        v-model="inputData"
        type="textarea"
        :rows="10"
        placeholder="请输入需要编码或解码的内容. . ."
      />

      <div>
        <el-button type="primary" @click="urlEncode">URL编码</el-button>
        <el-button type="primary" @click="urlDecode">URL解码</el-button>
      </div>

      <div>输出结果</div>

      <el-input
        v-model="outputData"
        type="textarea"
        :rows="10"
        placeholder="请输入需要编码或解码的内容. . ."
      />
    </el-card>
  </div>
</template>

<script lang="ts" setup>
import { ElMessage, ElNotification } from "element-plus";
import { ref } from "vue";

const inputData = ref("");
const outputData = ref("");

// 定义错误信息
const errorMessage = ref("");

const urlEncode = () => {
  try {
    outputData.value = encodeURIComponent(inputData.value); 
  } catch (error) {
    if (error instanceof Error) {
      errorMessage.value = `URL 编码失败: \n${error.message}`;
    } else {
      errorMessage.value = "URL 编码失败，请检查。";
    }
  }
}

const urlDecode = () => {
  try {
    outputData.value = decodeURIComponent(inputData.value);
  } catch (error) {
    if (error instanceof Error) {
      errorMessage.value = `URL 解码失败: \n${error.message}`;
    } else {
      errorMessage.value = "URL 解码失败，请检查。";
    }
  }
};
</script>

<style scoped>
.page-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  width: 100%;
  padding-left: 50px;
  padding-right: 50px;
}

.content-item {
  width: 50%;
  margin-right: 20px;
}
</style>
