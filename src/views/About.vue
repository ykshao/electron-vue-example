<template>
  <div>
    <section v-if="errored">
      <p>
        We're sorry, we're not able to retrieve this information at the moment,
        please try back later
      </p>
    </section>
    <section v-else>
      <el-container v-loading="loading">
        <el-main>
          <el-table :data="tableData">
            <el-table-column label="头像" width="140">
              <template slot-scope="scope">
                <el-image
                  style="width: 30px; height: 30px"
                  :src="scope.row.pic"
                  fit="fit"
                ></el-image>
              </template>
            </el-table-column>
            <el-table-column
              prop="name"
              label="姓名"
              width="140"
            ></el-table-column>
            <el-table-column prop="email" label="邮箱"></el-table-column>
            <el-table-column label="编辑" align="right">
              <template slot-scope="scope">
                <el-button
                  size="mini"
                  @click="handleEdit(scope.$index, scope.row)"
                  >Edit</el-button
                >
                <el-button
                  size="mini"
                  type="danger"
                  @click="handleDelete(scope.$index, scope.row)"
                  >Delete</el-button
                >
              </template>
            </el-table-column>
          </el-table>
        </el-main>
      </el-container>
    </section>
  </div>
</template>

<script>
import axios from "axios";
import { ipcRenderer } from "electron";

export default {
  data() {
    // const item = {
    //   date: "2016-05-02",
    //   name: "王小虎",
    //   address: "上海市普陀区金沙江路 1518 弄"
    // };
    return {
      tableData: [],
      loading: true,
      errored: false
    };
  },
  mounted() {
    axios
      .get("http://live.yapi.com/mock/19/api/user")
      .then(response => {
        let { data } = response.data;
        this.tableData = data.users;
      })
      .catch(() => {
        this.errored = true;
      })
      .finally(() => {
        this.loading = false;
      });
  },
  methods: {
    handleEdit(index, row) {
      ipcRenderer.send("toggle-user-detail", row);
    },
    handleDelete() {}
  }
};
</script>

<style scope lang="less"></style>
