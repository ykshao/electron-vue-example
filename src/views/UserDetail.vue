<template>
  <div>
    <el-image
      style="width: 400px; height: 400px"
      :src="info.pic"
      fit="fit"
    ></el-image>
  </div>
</template>

<script>
import { ipcRenderer } from "electron";

export default {
  data() {
    return {
      info: null
    };
  },
  created() {
    ipcRenderer.on("show-user-detail", (event, arg) => {
      this.info = arg;
    });
  },
  mounted() {}
};
</script>

<style scope lang="less"></style>
