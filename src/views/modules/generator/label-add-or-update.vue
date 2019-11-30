<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="标签名字" prop="labelName">
      <el-input v-model="dataForm.labelName" placeholder="标签名字"></el-input>
    </el-form-item>
    <el-form-item label="标签等级" prop="labelLv">
      <el-input v-model="dataForm.labelLv" placeholder="标签等级  有0 1 2 具体说明见文档"></el-input>
    </el-form-item>
    <el-form-item label="上级标签" prop="labelSuperior">
      <el-input v-model="dataForm.labelSuperior" placeholder="上级标签 输入父级 0级标签不用"></el-input>
    </el-form-item>
    <el-form-item label="例图" prop="labelUrl">
      <el-input v-model="dataForm.labelUrl" placeholder="只有分类标签需要上传例图 建议图片像素 100 x 100 "></el-input>
    </el-form-item>

      <el-form-item>
        <el-upload
          class="upload-demo"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="successHandle"
          :before-upload="beforeUploadHandle"
          multiple
          :limit="1"
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
          labelId: 0,
          labelName: '',
          url: '',
          labelLv: '',
          labelSuperior: '',
          labelUrl: '',
          status: 0
        },
        fileList: [],
        dataRule: {
          labelName: [
            { required: true, message: '标签名字不能为空', trigger: 'blur' }
          ],
          labelLv: [
            { required: true, message: '标签等级不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '状态不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    created: function () {
      this.url = this.$http.adornUrl(`/sys/oss/upload?classify=label&token=${this.$cookie.get('token')}`)
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
          this.dataForm.labelUrl = response.url
        } else {
          this.$message.error(response.msg)
        }
      },
      init (id) {
        this.dataForm.labelId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.labelId) {
            this.$http({
              url: this.$http.adornUrl(`/generator/label/info/${this.dataForm.labelId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.labelName = data.label.labelName
                this.dataForm.labelLv = data.label.labelLv
                this.dataForm.labelSuperior = data.label.labelSuperior
                this.dataForm.labelUrl = data.label.labelUrl
                this.dataForm.status = data.label.status
              }
            })
          }
        })
      },
      beforeUploadHandle (file) {
        if (file.type !== 'image/jpg' && file.type !== 'image/jpeg' && file.type !== 'image/png' && file.type !== 'image/gif') {
          this.$message.error('只支持jpg、png、gif格式的图片！')
          return false
        }
        this.num++
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/generator/label/${!this.dataForm.labelId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'labelId': this.dataForm.labelId || undefined,
                'labelName': this.dataForm.labelName,
                'labelLv': this.dataForm.labelLv,
                'labelSuperior': this.dataForm.labelSuperior,
                'labelUrl': this.dataForm.labelUrl,
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
