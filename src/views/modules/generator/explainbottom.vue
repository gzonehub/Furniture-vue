<template>
  <div class="mod-config">
<!--    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">-->
<!--      <el-form-item>-->
<!--        <el-input v-model="dataForm.key" placeholder="参数名" clearable></el-input>-->
<!--      </el-form-item>-->
<!--      <el-form-item>-->
<!--        <el-button @click="getDataList()">查询</el-button>-->
<!--        <el-button v-if="isAuth('generator:explainbottom:save')" type="primary" @click="addOrUpdateHandle()">新增</el-button>-->
<!--        <el-button v-if="isAuth('generator:explainbottom:delete')" type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">批量删除</el-button>-->
<!--      </el-form-item>-->
<!--    </el-form>-->
    <el-table
      :data="dataList"
      border
      v-loading="dataListLoading"
      @selection-change="selectionChangeHandle"
      style="width: 100%;">
<!--      <el-table-column-->
<!--        type="selection"-->
<!--        header-align="center"-->
<!--        align="center"-->
<!--        width="50">-->
<!--      </el-table-column>-->
<!--      <el-table-column-->
<!--        prop="id"-->
<!--        header-align="center"-->
<!--        align="center"-->
<!--        label="">-->
<!--      </el-table-column>-->
      <el-table-column
        prop="mianUrl"
        header-align="center"
        align="center"
        label="背景图">
      </el-table-column>
      <el-table-column
        prop="nameOne"
        header-align="center"
        align="center"
        label="模块1">
      </el-table-column>
      <el-table-column
        prop="contentOne"
        header-align="center"
        align="center"
        label="内容1">
      </el-table-column>
      <el-table-column
        prop="nameTwo"
        header-align="center"
        align="center"
        label="模块2">
      </el-table-column>
      <el-table-column
        prop="contentTwo"
        header-align="center"
        align="center"
        label="内容2">
      </el-table-column>
      <el-table-column
        prop="nameThree"
        header-align="center"
        align="center"
        label="模块3">
      </el-table-column>
      <el-table-column
        prop="contentThree"
        header-align="center"
        align="center"
        label="内容3">
      </el-table-column>
      <el-table-column
        prop="nameFour"
        header-align="center"
        align="center"
        label="模块4">
      </el-table-column>
      <el-table-column
        prop="contentFour"
        header-align="center"
        align="center"
        label="内容4">
      </el-table-column>
      <el-table-column
        prop="nameFive"
        header-align="center"
        align="center"
        label="模块5">
      </el-table-column>
      <el-table-column
        prop="contentFive"
        header-align="center"
        align="center"
        label="内容5">
      </el-table-column>
      <el-table-column
        prop="nameSix"
        header-align="center"
        align="center"
        label="模块6">
      </el-table-column>
      <el-table-column
        prop="contentSix"
        header-align="center"
        align="center"
        label="内容6">
      </el-table-column>
      <el-table-column
        prop="site"
        header-align="center"
        align="center"
        label="地址">
      </el-table-column>
      <el-table-column
        prop="phone"
        header-align="center"
        align="center"
        label="电话">
      </el-table-column>
      <el-table-column
        prop="email"
        header-align="center"
        align="center"
        label="邮箱">
      </el-table-column>
      <el-table-column
        prop="edition"
        header-align="center"
        align="center"
        label="备注">
      </el-table-column>
      <el-table-column
        fixed="right"
        header-align="center"
        align="center"
        width="150"
        label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="addOrUpdateHandle(scope.row.id)">修改</el-button>
        </template>
      </el-table-column>
    </el-table>
<!--    <el-pagination-->
<!--      @size-change="sizeChangeHandle"-->
<!--      @current-change="currentChangeHandle"-->
<!--      :current-page="pageIndex"-->
<!--      :page-sizes="[10, 20, 50, 100]"-->
<!--      :page-size="pageSize"-->
<!--      :total="totalPage"-->
<!--      layout="total, sizes, prev, pager, next, jumper">-->
<!--    </el-pagination>-->
    <!-- 弹窗, 新增 / 修改 -->
    <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>
  </div>
</template>

<script>
  import AddOrUpdate from './explainbottom-add-or-update'
  export default {
    data () {
      return {
        dataForm: {
          key: ''
        },
        dataList: [],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        dataListLoading: false,
        dataListSelections: [],
        addOrUpdateVisible: false
      }
    },
    components: {
      AddOrUpdate
    },
    activated () {
      this.getDataList()
    },
    methods: {
      // 获取数据列表
      getDataList () {
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/generator/explainbottom/list'),
          method: 'get',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'limit': this.pageSize,
            'key': this.dataForm.key
          })
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.dataList = data.page.list
            this.totalPage = data.page.totalCount
          } else {
            this.dataList = []
            this.totalPage = 0
          }
          this.dataListLoading = false
        })
      },
      // 每页数
      sizeChangeHandle (val) {
        this.pageSize = val
        this.pageIndex = 1
        this.getDataList()
      },
      // 当前页
      currentChangeHandle (val) {
        this.pageIndex = val
        this.getDataList()
      },
      // 多选
      selectionChangeHandle (val) {
        this.dataListSelections = val
      },
      // 新增 / 修改
      addOrUpdateHandle (id) {
        this.addOrUpdateVisible = true
        this.$nextTick(() => {
          this.$refs.addOrUpdate.init(id)
        })
      },
      // 删除
      deleteHandle (id) {
        var ids = id ? [id] : this.dataListSelections.map(item => {
          return item.id
        })
        this.$confirm(`确定对[id=${ids.join(',')}]进行[${id ? '删除' : '批量删除'}]操作?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
            url: this.$http.adornUrl('/generator/explainbottom/delete'),
            method: 'post',
            data: this.$http.adornData(ids, false)
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.$message({
                message: '操作成功',
                type: 'success',
                duration: 1500,
                onClose: () => {
                  this.getDataList()
                }
              })
            } else {
              this.$message.error(data.msg)
            }
          })
        })
      }
    }
  }
</script>
