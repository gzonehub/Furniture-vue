<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="订单id" prop="orderformId">
      <el-input v-model="dataForm.orderformId" placeholder="订单id"></el-input>
    </el-form-item>
    <el-form-item label="产品名称" prop="orderformName">
      <el-input v-model="dataForm.orderformName" placeholder="产品名称"></el-input>
    </el-form-item>
    <el-form-item label="进度图" prop="schedule">
      <el-input v-model="dataForm.schedule" placeholder="进度图   图片像素 1200 x 无限 "></el-input>
    </el-form-item>
      <el-form-item>
        <el-upload
          class="schedule"
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

      <el-form-item label="主图" prop="standby">
        <el-input v-model="dataForm.standby" placeholder="上传主图"></el-input>
      </el-form-item>

      <el-form-item>
        <el-upload
          class="standby"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="standbyHandle"
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
          orderformId: '',
          url: '',
          orderformName: '',
          schedule: '',
          status: 0,
          standby: ''
        },
        dataRule: {
          orderformId: [
            { required: true, message: '订单id不能为空', trigger: 'blur' }
          ],
          orderformName: [
            { required: true, message: '产品名称不能为空', trigger: 'blur' }
          ],
          schedule: [
            { required: true, message: '进度图不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '状态不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    created: function () {
      this.url = this.$http.adornUrl(`/sys/oss/upload?classify=orderList&token=${this.$cookie.get('token')}`)
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
          this.dataForm.schedule = this.dataForm.schedule + ' | ' + response.url
        } else {
          this.$message.error(response.msg)
        }
      },
      standbyHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.standby = response.url
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
              url: this.$http.adornUrl(`/generator/orderlist/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.orderformId = data.orderList.orderformId
                this.dataForm.orderformName = data.orderList.orderformName
                this.dataForm.schedule = data.orderList.schedule
                this.dataForm.standby = data.orderList.standby
                this.dataForm.status = data.orderList.status
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
              url: this.$http.adornUrl(`/generator/orderlist/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'orderformId': this.dataForm.orderformId,
                'orderformName': this.dataForm.orderformName,
                'schedule': this.dataForm.schedule,
                'standby': this.dataForm.standby,
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
