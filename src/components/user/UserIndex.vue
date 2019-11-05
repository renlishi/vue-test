<template>
  <el-container>
    <el-aside style="width: 200px;margin-top: 20px">
      <switch></switch>
      <!--<SideMenu></SideMenu>-->
    </el-aside>
    <el-main>
      <el-table :data="users" style="width: 100%" height="250">
        <el-table-column fixed prop="date" label="日期" width="150"></el-table-column>
        <el-table-column prop="name" label="姓名" width="120"></el-table-column>
        <el-table-column prop="province" label="省份" width="120"></el-table-column>
        <el-table-column prop="city" label="市区" width="120"></el-table-column>
        <el-table-column prop="address" label="地址" width="300"></el-table-column>
        <el-table-column prop="zip" label="邮编" width="120"></el-table-column>
      </el-table>
      <div class="block">
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="currentPage"
          :page-sizes="[2,100, 200, 300, 400]"
          :page-size="pageSize"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total">
        </el-pagination>
      </div>
    </el-main>
  </el-container>
</template>

<script>
  export default {
    name: 'UserIndex',
    data() {
      return {
        users: [],
        currentPage: 1,
        pageSize: 2,
        total: 20
      }
    },
    mounted: function () {
      this.findUser()
    },
    methods: {
      handleSizeChange(val){
        var _this = this
        _this.pageSize = val;
        _this.findUser();
      },
      handleCurrentChange(val){
        var _this = this
        _this.currentPage = val;
        _this.findUser();
      },
      findUser () {
        var _this = this
        this.$axios.post('/user/findUser', {
          currentPage: this.currentPage,
          pageSize: this.pageSize
        }).then(resp => {
          console.log(resp.data)
          if (resp && resp.data.code === 200) {
            _this.users = resp.data.data
            _this.total = resp.data.total
          }
        })
      }
    }
  }
</script>
