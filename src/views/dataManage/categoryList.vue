<!--
 * @Descripttion: 类别列表
 * @version: 1.0
 * @Author: 笑佛弥勒
 * @Date: 2020-01-22 16:01:53
 * @LastEditors: 笑佛弥勒
 * @LastEditTime: 2020-03-28 22:09:26
 -->
<template>
  <div class="user-list-wrapper">
    <section style="padding:12px;text-align: right">
      <el-button type="success" plain @click="showAddLayer">新增</el-button>
    </section>
    <el-table :data="userList" style="width: 100%">
      <el-table-column align="center" label="名称" prop="name"/>
      <el-table-column align="center" label="新增时间">
        <template slot-scope="scope">
          {{ scope.row.created_at | _formatDate }}
        </template>
      </el-table-column>
      <el-table-column align="center" label="操作">
        <template slot-scope="scope">
          <el-button type="warning" plain>修改</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      background
      layout="prev, pager, next"
      :total="totalPage"
      :page-size="pageSize"
      class="absoulute"
      @current-change="changPage"
    />

    <el-dialog title="新增" :append-to-body="true" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <el-form-item label="名称"
                      label-width="120px"
                      :rules="[
                    { required: true, message: '不能为空'},
                  ]">
          <el-input v-model="form.name" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="submitForm">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import { formatDate } from 'common/js/util'
export default {
  data() {
    return {
      userList: [],
      dialogFormVisible: false,
      form: {
        name: null
      },
      page: 1,
      pageSize: 20,
      totalPage: 0
    }
  },
  mounted() {
    this._getUserList()
  },
  filters: {
    _formatDate(val) {
      return formatDate(val)
    }
  },
  methods: {
    submitForm(){
      this.$refs.form.validate((valid) => {
        if (valid) {
          alert('submit!');
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    showAddLayer(){
      this.dialogFormVisible = true
    },
    _getUserList() {
      const params = {
        page: this.page,
        pageSize: this.pageSize
      }
      this.Service.getUserList(params).then((res) => {
        this.userList = res.data.rows
        this.totalPage = res.data.count
      })
    },
    // 改变分页
    changPage(val) {
      this.page = val
      this._getUserList()
    }
  }
}
</script>

<style lang="scss">
  .user-list-wrapper {
    width: 100%;
    height: 100%;
    background-color: white;
  }
</style>
