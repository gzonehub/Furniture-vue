<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-form-item label="主图" prop="mainUrl">
        <el-input v-model="dataForm.mainUrl" placeholder="上传主图图片 限制1张 图片建议像素 1920 x 700"></el-input>
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

      <el-form-item label="卡梵蒂妮" prop="mainImg">
        <el-input v-model="dataForm.mainImg" placeholder="上传关于卡梵蒂妮的例图  限1张  图片建议像素 600 x 480"></el-input>
      </el-form-item>

      <el-form-item>
        <el-upload
          class="mainImg"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="mainImgHandle"
          :before-upload="beforeUploadHandle"
          multiple
          :on-exceed="handleExceed"
          :file-list="fileList"
          list-type="picture">
          <el-button size="small" type="primary">点击上传</el-button>
          <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
        </el-upload>
      </el-form-item>

      <el-form-item label="关于我们" prop="mainIntroduce">
        <el-input  type="textarea" v-model="dataForm.mainIntroduce" placeholder="上传关于卡梵蒂妮的说明文字"></el-input>
      </el-form-item>
      <el-form-item label="企业文化" prop="enterpriseCultureImg">
        <el-input v-model="dataForm.enterpriseCultureImg" placeholder="上传企业文化的例图 限1张 图片建议像素 400 x 300 "></el-input>
      </el-form-item>

      <el-form-item>
        <el-upload
          class="enterpriseCultureImg"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="enterpriseCultureImgHandle"
          :before-upload="beforeUploadHandle"
          multiple
          :on-exceed="handleExceed"
          :file-list="fileList"
          list-type="picture">
          <el-button size="small" type="primary">点击上传</el-button>
          <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
        </el-upload>
      </el-form-item>

      <el-form-item label="企业说明" prop="enterpriseCulture">
        <el-input  type="textarea" v-model="dataForm.enterpriseCulture" placeholder="上传关于企业文化的说明文字"></el-input>
      </el-form-item>
      <el-form-item label="荣誉证书" prop="certificateUrl">
        <el-input v-model="dataForm.certificateUrl" placeholder="上传荣誉证书的例图  图片建议像素 386 x 280"></el-input>
      </el-form-item>

      <el-form-item>
        <el-upload
          class="certificateUrl"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="certificateUrlHandle"
          :before-upload="beforeUploadHandle"
          multiple
          :on-exceed="handleExceed"
          :file-list="fileList"
          list-type="picture">
          <el-button size="small" type="primary">点击上传</el-button>
          <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
        </el-upload>
      </el-form-item>

      <el-form-item label="车间图" prop="productionUrl">
        <el-input v-model="dataForm.productionUrl" placeholder="上传车间图的例图  图片建议像素 386 x 280"></el-input>
      </el-form-item>

      <el-form-item>
        <el-upload
          class="certificateUrl"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="productionUrlHandle"
          :before-upload="beforeUploadHandle"
          multiple
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
          mainImg: '',
          mainIntroduce: '',
          enterpriseCultureImg: '',
          enterpriseCulture: '',
          certificateUrl: '',
          productionUrl: '',
          status: ''
        },
        dataRule: {
          mainUrl: [
            { required: true, message: '主图不能为空', trigger: 'blur' }
          ],
          mainImg: [
            { required: true, message: '卡梵蒂妮图不能为空', trigger: 'blur' }
          ],
          mainIntroduce: [
            { required: true, message: '关于卡梵蒂妮不能为空', trigger: 'blur' }
          ],
          enterpriseCultureImg: [
            { required: true, message: '企业文化图不能为空', trigger: 'blur' }
          ],
          enterpriseCulture: [
            { required: true, message: '企业文化不能为空', trigger: 'blur' }
          ],
          certificateUrl: [
            { required: true, message: '荣誉证书不能为空', trigger: 'blur' }
          ],
          productionUrl: [
            { required: true, message: '车间图不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    created: function () {
      this.url = this.$http.adornUrl(`/sys/oss/upload?classify=aboutUs&token=${this.$cookie.get('token')}`)
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
      mainImgHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.mainImg = response.url
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
      enterpriseCultureImgHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.enterpriseCultureImg = response.url
        } else {
          this.$message.error(response.msg)
        }
      },
      certificateUrlHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.certificateUrl = this.dataForm.certificateUrl + ' | ' + response.url
        } else {
          this.$message.error(response.msg)
        }
      },
      productionUrlHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.productionUrl = this.dataForm.productionUrl + ' | ' + response.url
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
              url: this.$http.adornUrl(`/generator/aboutus/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.mainUrl = data.aboutUs.mainUrl
                this.dataForm.mainImg = data.aboutUs.mainImg
                this.dataForm.mainIntroduce = data.aboutUs.mainIntroduce
                this.dataForm.enterpriseCultureImg = data.aboutUs.enterpriseCultureImg
                this.dataForm.enterpriseCulture = data.aboutUs.enterpriseCulture
                this.dataForm.certificateUrl = data.aboutUs.certificateUrl
                this.dataForm.productionUrl = data.aboutUs.productionUrl
                this.dataForm.status = data.aboutUs.status
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
              url: this.$http.adornUrl(`/generator/aboutus/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'mainUrl': this.dataForm.mainUrl,
                'mainImg': this.dataForm.mainImg,
                'mainIntroduce': this.dataForm.mainIntroduce,
                'enterpriseCultureImg': this.dataForm.enterpriseCultureImg,
                'enterpriseCulture': this.dataForm.enterpriseCulture,
                'certificateUrl': this.dataForm.certificateUrl,
                'productionUrl': this.dataForm.productionUrl,
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
