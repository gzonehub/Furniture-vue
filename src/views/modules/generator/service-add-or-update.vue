<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="大图" prop="mainUrl">
      <el-input v-model="dataForm.mainUrl" placeholder="主图 1920 x 700  "></el-input>
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
          :limit="1"
          :on-exceed="handleExceed"
          :file-list="fileList"
          list-type="picture">
          <el-button size="small" type="primary">点击上传</el-button>
          <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
        </el-upload>
      </el-form-item>

    <el-form-item label="售前" prop="preSales">
      <el-input v-model="dataForm.preSales" placeholder="售前 标题"></el-input>
    </el-form-item>
    <el-form-item label="售前说明" prop="preSalesName">
      <el-input type="textarea" v-model="dataForm.preSalesName" placeholder="售前说明"></el-input>
    </el-form-item>
    <el-form-item label="售前图片" prop="preSalesUrl">
      <el-input v-model="dataForm.preSalesUrl" placeholder="售前图片 图片像素 600 x 360"></el-input>
    </el-form-item>
      <el-form-item>
        <el-upload
          class="preSalesUrl"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="preSalesUrlHandle"
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
    <el-form-item label="售中" prop="inSale">
      <el-input v-model="dataForm.inSale" placeholder="售中"></el-input>
    </el-form-item>
    <el-form-item label="销中说明" prop="inSalesName">
      <el-input type="textarea"  v-model="dataForm.inSalesName" placeholder="销中说明"></el-input>
    </el-form-item>
    <el-form-item label="售中图片" prop="inSalesUrl">
      <el-input v-model="dataForm.inSalesUrl" placeholder="售中图片 图片像素 400 x 400 "></el-input>
    </el-form-item>

      <el-form-item>
        <el-upload
          class="inSalesUrl"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="inSalesUrlHandle"
          :before-upload="beforeUploadHandle"
          multiple
          :limit="2"
          :on-exceed="handleExceed"
          :file-list="fileList"
          list-type="picture">
          <el-button size="small" type="primary">点击上传</el-button>
          <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
        </el-upload>
      </el-form-item>

    <el-form-item label="售后" prop="afterSale">
      <el-input v-model="dataForm.afterSale" placeholder="售后"></el-input>
    </el-form-item>
    <el-form-item label="售后说明" prop="afterSaleName">
      <el-input type="textarea" v-model="dataForm.afterSaleName" placeholder="售后说明"></el-input>
    </el-form-item>
    <el-form-item label="售后图片" prop="afterSaleUrl">
      <el-input v-model="dataForm.afterSaleUrl" placeholder="售后图片 图片像素 600 x 360"></el-input>
    </el-form-item>

      <el-form-item>
        <el-upload
          class="afterSaleUrl"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="afterSaleUrlHandle"
          :before-upload="beforeUploadHandle"
          multiple
          :limit="2"
          :on-exceed="handleExceed"
          :file-list="fileList"
          list-type="picture">
          <el-button size="small" type="primary">点击上传</el-button>
          <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
        </el-upload>
      </el-form-item>

    <el-form-item label="品质保证" prop="qualityGuarantee">
      <el-input v-model="dataForm.qualityGuarantee" placeholder="品质保证"></el-input>
    </el-form-item>
    <el-form-item label="保证内容" prop="guaranteeName">
      <el-input type="textarea" v-model="dataForm.guaranteeName" placeholder="保证内容"></el-input>
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
          preSales: '',
          preSalesName: '',
          preSalesUrl: '',
          inSale: '',
          url: '',
          inSalesName: '',
          inSalesUrl: '',
          afterSale: '',
          afterSaleName: '',
          afterSaleUrl: '',
          qualityGuarantee: '',
          guaranteeName: ''
        },
        dataRule: {
          mainUrl: [
            { required: true, message: '大图不能为空', trigger: 'blur' }
          ],
          preSales: [
            { required: true, message: '售前不能为空', trigger: 'blur' }
          ],
          preSalesName: [
            { required: true, message: '售前说明不能为空', trigger: 'blur' }
          ],
          preSalesUrl: [
            { required: true, message: '售前图片不能为空', trigger: 'blur' }
          ],
          inSale: [
            { required: true, message: '售中不能为空', trigger: 'blur' }
          ],
          inSalesName: [
            { required: true, message: '销中说明不能为空', trigger: 'blur' }
          ],
          inSalesUrl: [
            { required: true, message: '售中图片不能为空', trigger: 'blur' }
          ],
          afterSale: [
            { required: true, message: '售后不能为空', trigger: 'blur' }
          ],
          afterSaleName: [
            { required: true, message: '售后说明不能为空', trigger: 'blur' }
          ],
          afterSaleUrl: [
            { required: true, message: '售后图片不能为空', trigger: 'blur' }
          ],
          qualityGuarantee: [
            { required: true, message: '品质保证不能为空', trigger: 'blur' }
          ],
          guaranteeName: [
            { required: true, message: '保证内容不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    created: function () {
      this.url = this.$http.adornUrl(`/sys/oss/upload?classify=service&token=${this.$cookie.get('token')}`)
    },
    methods: {
      handleRemove (file, fileList) {
        console.log(file, fileList)
      },
      handlePreview (file) {
        console.log(file)
      },
      handleExceed (files, fileList) {
        this.$message.warning(`当前限制选择 2 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`)
      },
      mainUrlHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.mainUrl = response.url
        } else {
          this.$message.error(response.msg)
        }
      },
      preSalesUrlHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.preSalesUrl = response.url
        } else {
          this.$message.error(response.msg)
        }
      },
      inSalesUrlHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.inSalesUrl = this.dataForm.inSalesUrl + ' | ' + response.url
        } else {
          this.$message.error(response.msg)
        }
      },
      afterSaleUrlHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.afterSaleUrl = response.url
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
              url: this.$http.adornUrl(`/generator/service/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.mainUrl = data.service.mainUrl
                this.dataForm.preSales = data.service.preSales
                this.dataForm.preSalesName = data.service.preSalesName
                this.dataForm.preSalesUrl = data.service.preSalesUrl
                this.dataForm.inSale = data.service.inSale
                this.dataForm.inSalesName = data.service.inSalesName
                this.dataForm.inSalesUrl = data.service.inSalesUrl
                this.dataForm.afterSale = data.service.afterSale
                this.dataForm.afterSaleName = data.service.afterSaleName
                this.dataForm.afterSaleUrl = data.service.afterSaleUrl
                this.dataForm.qualityGuarantee = data.service.qualityGuarantee
                this.dataForm.guaranteeName = data.service.guaranteeName
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
              url: this.$http.adornUrl(`/generator/service/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'mainUrl': this.dataForm.mainUrl,
                'preSales': this.dataForm.preSales,
                'preSalesName': this.dataForm.preSalesName,
                'preSalesUrl': this.dataForm.preSalesUrl,
                'inSale': this.dataForm.inSale,
                'inSalesName': this.dataForm.inSalesName,
                'inSalesUrl': this.dataForm.inSalesUrl,
                'afterSale': this.dataForm.afterSale,
                'afterSaleName': this.dataForm.afterSaleName,
                'afterSaleUrl': this.dataForm.afterSaleUrl,
                'qualityGuarantee': this.dataForm.qualityGuarantee,
                'guaranteeName': this.dataForm.guaranteeName
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
