<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-form-item label="主图" prop="mainUrl">
        <el-input v-model="dataForm.mainUrl" placeholder="上传主图图片 限制1张 图片像素 1920 x 700  "></el-input>
      </el-form-item>

      <el-form-item>
        <el-upload
          class="certificateUrl"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="mainUrlHandle"
          :before-upload="beforeUploadHandle"
          multiple
          :on-exceed="handleExceed"
          :file-list="fileList"
          list-type="picture">
          <el-button size="small" type="primary">点击上传</el-button>
          <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
        </el-upload>
      </el-form-item>

    <el-form-item label="新闻标题" prop="newsName">
      <el-input v-model="dataForm.newsName" placeholder="新闻标题"></el-input>
    </el-form-item>
    <el-form-item label="新闻内容" prop="news">
      <el-input type="textarea" v-model="dataForm.news" placeholder="新闻内容"></el-input>
    </el-form-item>
      <el-form-item label="缩略图" prop="newsMainUrl">
        <el-input v-model="dataForm.newsMainUrl" placeholder="缩略图 图片像素 340 x 240 "></el-input>
      </el-form-item>
      <el-form-item>
        <el-upload
          class="mainUrl"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="newsHandle"
          :before-upload="beforeUploadHandle"
          multiple
          :on-exceed="handleExceed"
          :file-list="fileList"
          list-type="picture">
          <el-button size="small" type="primary">点击上传</el-button>
          <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
        </el-upload>
      </el-form-item>
    <el-form-item label="新闻图片" prop="newsUrl">
      <el-input v-model="dataForm.newsUrl" placeholder="新闻图片 图片像素 1200 x 600-700 "></el-input>
    </el-form-item>
      <el-form-item>
        <el-upload
          class="mainUrl"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="successHandle"
          :before-upload="beforeUploadHandle"
          multiple
          :on-exceed="handleExceed"
          :file-list="fileList"
          list-type="picture">
          <el-button size="small" type="primary">点击上传</el-button>
          <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
        </el-upload>
      </el-form-item>
    <el-form-item label="状态" prop="status">
      <el-radio-group v-model="dataForm.status">
        <el-radio :label="1">禁用</el-radio>
        <el-radio :label="0">正常</el-radio>
      </el-radio-group>
    </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          id: 0,
          mainUrl: '',
          newsName: '',
          news: '',
          newsMainUrl: '',
          newsUrl: '',
          newsDate: '',
          status: 0
        },
        dataRule: {
          mainUrl: [
            { required: true, message: '主图不能为空', trigger: 'blur' }
          ],
          newsName: [
            { required: true, message: '新闻标题不能为空', trigger: 'blur' }
          ],
          news: [
            { required: true, message: '新闻内容不能为空', trigger: 'blur' }
          ],
          newsMainUrl: [
            { required: true, message: '缩略图不能为空', trigger: 'blur' }
          ],
          newsUrl: [
            { required: true, message: '新闻图片不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '状态不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    created: function () {
      this.url = this.$http.adornUrl(`/sys/oss/upload?classify=news&token=${this.$cookie.get('token')}`)
    },
    methods: {
      handleRemove (file, fileList) {
        console.log(file, fileList)
      },
      handlePreview (file) {
        console.log(file)
      },
      handleExceed (files, fileList) {
        this.$message.warning(`当前限制选择 1 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`)
      },
      successHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.newsUrl = this.dataForm.newsUrl + ' | ' + response.url
        } else {
          this.$message.error(response.msg)
        }
      },
      mainUrlHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.mainUrl = response.url
        } else {
          this.$message.error(response.msg)
        }
      },
      newsHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.newsMainUrl = response.url
        } else {
          this.$message.error(response.msg)
        }
      },
      beforeUploadHandle (file) {
        if (file.type !== 'image/jpg' && file.type !== 'image/jpeg' && file.type !== 'image/png' && file.type !== 'image/gif') {
          this.$message.error('只支持jpg、png、gif格式的图片！')
          return false
        }
        this.num++
      },
      init (id) {
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/generator/news/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.mainUrl = data.news.mainUrl
                this.dataForm.newsName = data.news.newsName
                this.dataForm.news = data.news.news
                this.dataForm.newsMainUrl = data.news.newsMainUrl
                this.dataForm.newsUrl = data.news.newsUrl
                this.dataForm.newsDate = data.news.newsDate
                this.dataForm.status = data.news.status
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/generator/news/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'mainUrl': this.dataForm.mainUrl,
                'newsName': this.dataForm.newsName,
                'news': this.dataForm.news,
                'newsMainUrl': this.dataForm.newsMainUrl,
                'newsUrl': this.dataForm.newsUrl,
                'status': this.dataForm.status
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
