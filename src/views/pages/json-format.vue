<template>
  <div>
    <el-card>
      <template #header>
        <h3>JSON 格式化工具</h3>
      </template>
      <div class="content-container">
        <div class="content-item">
          <div
            style="
              display: flex;
              flex-direction: row;
              justify-content: flex-start;
            "
          >
            <el-text style="width: 100%">输入JSON</el-text>
            <el-button
              type="info"
              plain
              style="justify-self: flex-end"
              @click="clearInput"
              >清空</el-button
            >
          </div>
          <el-input
            v-model="inputJson"
            type="textarea"
            :rows="20"
            placeholder="在此输入JSON文本. . ."
            resize="none"
          />
          <el-button type="primary"  style="width: 100%;margin-top: 10px; padding: 15px; " @click="formatJson"
            >格式化</el-button
          >
        </div>

        <div class="content-item">
          <div
            style="
              display: flex;
              flex-direction: row;
              justify-content: flex-start;
            "
          >
            <el-text style="width: 100%">格式化结果</el-text>
            <el-button
              type="info"
              plain
              style="justify-self: end"
              @click="compressFormattedJson"
              >{{ canCompressed ? "压缩" : "展开" }}</el-button
            >
            <el-button
              type="info"
              plain
              style="justify-self: end"
              @click="copyFormattedJson"
            >
              <el-icon><CopyDocument /></el-icon>复制</el-button
            >
          </div>
          <el-input v-if="!errorMessage"
            v-model="formattedJson"
            type="textarea"
            :rows="20"
            placeholder=""
          />
          <div v-if="errorMessage">
          <el-alert
            :title="errorMessage"
            type="error"
            :message="errorMessage"
            show-icon
            :closable="false"
          />
        </div>
        </div>
      </div>
    </el-card>
  </div>
</template>

<script lang="ts" setup>
import { ElMessage, ElNotification } from "element-plus";
import { ref } from "vue";

// 定义输入的 JSON 字符串
const inputJson = ref("");
// 定义格式化后的 JSON 字符串
const formattedJson = ref("");
// 定义错误信息
const errorMessage = ref("");
// 是否可以压缩
const canCompressed = ref(true);

// 格式化 JSON 的函数
const formatJson = () => {
  try {
    errorMessage.value = "";
    canCompressed.value = true;
    // 尝试解析输入的 JSON 字符串
    const parsedJson = JSON.parse(inputJson.value);
    // 格式化解析后的 JSON 对象
    formattedJson.value = JSON.stringify(parsedJson, null, 2);
    // 清空错误信息
  } catch (error) {
    if (error instanceof Error) {
      errorMessage.value = `JSON 格式不正确: \n${error.message}`;
    } else {
      errorMessage.value = "JSON 格式不正确，请检查。";
    }
    // 清空格式化结果
    formattedJson.value = "";
  }
};

const clearInput = () => {
  inputJson.value = "";
};

const copyFormattedJson = () => {``
  navigator.clipboard.writeText(formattedJson.value);
  ElNotification({
    title: '复制成功',
    message: '',
    type: 'success',
  })
};

const compressFormattedJson = () => {
  if (!formattedJson) return;
  const parsedJson = JSON.parse(formattedJson.value);
  if (canCompressed.value) {
    // 格式化解析后的 JSON 对象
    formattedJson.value = JSON.stringify(parsedJson);
  } else {
    // 格式化解析后的 JSON 对象
    formattedJson.value = JSON.stringify(parsedJson, null, 2);
  }
  canCompressed.value = !canCompressed.value;
};
</script>

<style scoped>
.content-container {
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  justify-content: space-between;
  width: 100%;
}

.content-item {
  width: 50%;
  margin-right: 20px;
}
</style>
