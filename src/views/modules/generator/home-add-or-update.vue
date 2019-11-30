<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="首页轮播" prop="mainUrl">
      <el-input v-model="dataForm.mainUrl" placeholder="首页轮播 图片像素 1920 x 700  "></el-input>
    </el-form-item>
      <el-form-item>
        <el-upload
          class="mainUrl"
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
    <el-form-item label="服务承诺" prop="servicePromises">
      <el-input type="textarea" v-model="dataForm.servicePromises" placeholder="服务承诺"></el-input>
    </el-form-item>
    <el-form-item label="服务承诺图" prop="serviceImg">
      <el-input v-model="dataForm.serviceImg" placeholder="服务承诺图  上传顺序 按照提示尺寸280 x 260  430 x 260  1200 x 300    "></el-input>
    </el-form-item>
      <el-form-item>
        <el-upload
          class="serviceImg"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="serviceImgHandle"
          :before-upload="beforeUploadHandle"
          multiple
          :limit="3"
          :on-exceed="handleExceed"
          :file-list="fileList"
          list-type="picture">
          <el-button size="small" type="primary">点击上传</el-button>
          <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
        </el-upload>
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
          servicePromises: '',
          serviceImg: ''
        },
        dataRule: {
          mainUrl: [
            { required: true, message: '首页轮播不能为空', trigger: 'blur' }
          ],
          servicePromises: [
            { required: true, message: '服务承诺不能为空', trigger: 'blur' }
          ],
          serviceImg: [
            { required: true, message: '服务承诺图不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    created: function () {
      this.url = this.$http.adornUrl(`/sys/oss/upload?classify=home&token=${this.$cookie.get('token')}`)
    },
    methods: {
      handleRemove (file, fileList) {
        console.log(file, fileList)
      },
      handlePreview (file) {
        console.log(file)
      },
      handleExceed (files, fileList) {
        this.$message.warning(`当前限制选择 3 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`)
      },
      mainUrlHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.mainUrl = this.dataForm.mainUrl + ' | ' + response.url
        } else {
          this.$message.error(response.msg)
        }
      },
      serviceImgHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.serviceImg = this.dataForm.serviceImg + ' | ' + response.url
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
              url: this.$http.adornUrl(`/generator/home/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.mainUrl = data.home.mainUrl
                this.dataForm.servicePromises = data.home.servicePromises
                this.dataForm.serviceImg = data.home.serviceImg
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
              url: this.$http.adornUrl(`/generator/home/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'mainUrl': this.dataForm.mainUrl,
                'servicePromises': this.dataForm.servicePromises,
                'serviceImg': this.dataForm.serviceImg
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
