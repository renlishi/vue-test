<template>
  <div>
    <el-row style="height: 840px;">
      <!--<search-bar></search-bar>-->
      <el-tooltip effect="dark" placement="right"
                  v-for="item in books"
                  :key="item.id">
        <p slot="content" style="font-size: 14px;margin-bottom: 6px;">{{item.title}}</p>
        <p slot="content" style="font-size: 13px;margin-bottom: 6px">
          <span>{{item.author}}</span> /
          <span>{{item.date}}</span> /
          <span>{{item.press}}</span>
        </p>
        <p slot="content" style="width: 300px" class="abstract">{{item.abs}}</p>
        <el-card style="width: 135px;margin-bottom: 20px;height: 233px;float: left;margin-right: 15px" class="book"
                 bodyStyle="padding:10px" shadow="hover">
          <div class="cover">
            <img :src="item.cover" alt="封面">
          </div>
          <div class="info">
            <div class="title">
              <a href="">{{item.title}}</a>
            </div>
          </div>
          <div class="author">{{item.author}}</div>
        </el-card>
      </el-tooltip>
    </el-row>
    <el-row>
      <!--@current-change="handleCurrentChange"-->
      <el-pagination
        @current-change= "handleCurrentChange"
        @size-change= "handleSizeChange"
        :current-page= "currentPage"
        :page-sizes="[2, 4, 6, 8]"
        :page-size= "pageSize"
        layout="total,sizes, prev, pager, next"
        :total= "books.length">
      </el-pagination>
    </el-row>
  </div>
</template>

<script>
  export default {
    name: 'Books',
    // components: {EditForm, SearchBar, ViewSwitch},
    data () {
      return {
        books: [],
        currentPage: 1,
        pageSize: 2,
        total: 20
      }
    },
    mounted: function () {
      this.loadBooks()
    },
    methods: {
      handleSizeChange(val){
        var _this = this
        _this.pageSize = val;
        _this.loadBooks();
      },
      handleCurrentChange(val){
        var _this = this
        _this.currentPage = val;
        _this.loadBooks();
      },
      loadBooks () {
        var _this = this
        this.$axios.post('/books', {
          currentPage: this.currentPage,
          pageSize: this.pageSize
        }).then(resp => {
          if (resp && resp.status === 200) {
            _this.books = resp.data.data
            _this.total = resp.data.total
          }
        })
      }
    }
  }
</script>

<style scoped>
  .cover {
    width: 115px;
    height: 172px;
    margin-bottom: 7px;
    overflow: hidden;
    cursor: pointer;
  }

  img {
    width: 115px;
    height: 172px;
    /*margin: 0 auto;*/
  }

  .title {
    font-size: 14px;
    text-align: left;
  }

  .author {
    color: #333;
    width: 102px;
    font-size: 13px;
    margin-bottom: 6px;
    text-align: left;
  }

  .abstract {
    display: block;
    line-height: 17px;
  }

  a {
    text-decoration: none;
  }

  a:link, a:visited, a:focus {
    color: #3377aa;
  }
</style>

