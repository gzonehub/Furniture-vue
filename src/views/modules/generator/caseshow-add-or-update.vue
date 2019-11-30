<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-form-item label="主图" prop="mainUrl">
        <el-input v-model="dataForm.mainUrl" placeholder="上传主图图片 限制1张"></el-input>
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

      <el-form-item label="标题" prop="caption">
        <el-input v-model="dataForm.caption" placeholder="输入标题"></el-input>
      </el-form-item>

      <el-form-item label="缩略图" prop="exampleMainUrl">
        <el-input v-model="dataForm.exampleMainUrl" placeholder="缩略图  图片像素1200 x 600 - 700"></el-input>
      </el-form-item>
      <el-form-item>
        <el-upload
          class="mainUrl"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="mainHandle"
          :before-upload="beforeUploadHandle"
          multiple
          :on-exceed="handleExceed"
          :file-list="fileList"
          list-type="picture">
          <el-button size="small" type="primary">点击上传</el-button>
          <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
        </el-upload>
      </el-form-item>
    <el-form-item label="展示图" prop="exampleUrl">
      <el-input v-model="dataForm.exampleUrl" placeholder="展示图 320 x 420"></el-input>
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
    <el-form-item label="案例地址" prop="exampleSite">
      <el-input v-model="dataForm.exampleSite" placeholder="案例地址"></el-input>
    </el-form-item>
    <el-form-item label="交付日期" prop="deliveryTime">
      <el-input v-model="dataForm.deliveryTime" placeholder="交付日期"></el-input>
    </el-form-item>
    <el-form-item label="说明" prop="exampleExplain">
      <el-input type="textarea"  v-model="dataForm.exampleExplain" placeholder="说明"></el-input>
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
          caption: '',
          mainUrl: '',
          exampleUrl: '',
          exampleSite: '',
          exampleMainUrl: '',
          deliveryTime: '',
          exampleExplain: '',
          status: 0
        },
        dataRule: {
          mainUrl: [
            { required: true, message: '主图不能为空', trigger: 'blur' }
          ],
          caption: [
            { required: true, message: '标题不能为空', trigger: 'blur' }
          ],
          exampleMainUrl: [
            { required: true, message: '缩略图不能为空', trigger: 'blur' }
          ],
          exampleUrl: [
            { required: true, message: '展示图不能为空', trigger: 'blur' }
          ],
          exampleSite: [
            { required: true, message: '案例地址不能为空', trigger: 'blur' }
          ],
          deliveryTime: [
            { required: true, message: '交付日期不能为空', trigger: 'blur' }
          ],
          exampleExplain: [
            { required: true, message: '说明不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '状态不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    created: function () {
      this.url = this.$http.adornUrl(`/sys/oss/upload?classify=caseShow&token=${this.$cookie.get('token')}`)
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
      mainUrlHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.mainUrl = response.url
        } else {
          this.$message.error(response.msg)
        }
      },
      successHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.exampleUrl = this.dataForm.exampleUrl + ' | ' + response.url
        } else {
          this.$message.error(response.msg)
        }
      },
      mainHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.exampleMainUrl = response.url
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
              url: this.$http.adornUrl(`/generator/caseshow/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.mainUrl = data.caseShow.mainUrl
                this.dataForm.caption = data.caseShow.caption
                this.dataForm.exampleMainUrl = data.caseShow.exampleMainUrl
                this.dataForm.exampleUrl = data.caseShow.exampleUrl
                this.dataForm.exampleSite = data.caseShow.exampleSite
                this.dataForm.deliveryTime = data.caseShow.deliveryTime
                this.dataForm.exampleExplain = data.caseShow.exampleExplain
                this.dataForm.status = data.caseShow.status
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
              url: this.$http.adornUrl(`/generator/caseshow/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'mainUrl': this.dataForm.mainUrl,
                'caption': this.dataForm.caption,
                'exampleMainUrl': this.dataForm.exampleMainUrl,
                'exampleUrl': this.dataForm.exampleUrl,
                'exampleSite': this.dataForm.exampleSite,
                'deliveryTime': this.dataForm.deliveryTime,
                'exampleExplain': this.dataForm.exampleExplain,
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
