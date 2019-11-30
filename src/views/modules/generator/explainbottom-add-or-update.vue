<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="背景图" prop="mianUrl">
      <el-input v-model="dataForm.mianUrl" placeholder="背景图 1920 x 500-600"></el-input>
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

    <el-form-item label="模块1" prop="nameOne">
      <el-input v-model="dataForm.nameOne" placeholder="模块1"></el-input>
    </el-form-item>
    <el-form-item label="内容1" prop="contentOne">
      <el-input v-model="dataForm.contentOne" placeholder="内容1"></el-input>
    </el-form-item>
    <el-form-item label="模块2" prop="nameTwo">
      <el-input v-model="dataForm.nameTwo" placeholder="模块2"></el-input>
    </el-form-item>
    <el-form-item label="内容2" prop="contentTwo">
      <el-input v-model="dataForm.contentTwo" placeholder="内容2"></el-input>
    </el-form-item>
    <el-form-item label="模块3" prop="nameThree">
      <el-input v-model="dataForm.nameThree" placeholder="模块3"></el-input>
    </el-form-item>
    <el-form-item label="内容3" prop="contentThree">
      <el-input v-model="dataForm.contentThree" placeholder="内容3"></el-input>
    </el-form-item>
    <el-form-item label="模块4" prop="nameFour">
      <el-input v-model="dataForm.nameFour" placeholder="模块4"></el-input>
    </el-form-item>
    <el-form-item label="内容4" prop="contentFour">
      <el-input v-model="dataForm.contentFour" placeholder="内容4"></el-input>
    </el-form-item>
    <el-form-item label="模块5" prop="nameFive">
      <el-input v-model="dataForm.nameFive" placeholder="模块5"></el-input>
    </el-form-item>
    <el-form-item label="内容5" prop="contentFive">
      <el-input v-model="dataForm.contentFive" placeholder="内容5"></el-input>
    </el-form-item>
    <el-form-item label="模块6" prop="nameSix">
      <el-input v-model="dataForm.nameSix" placeholder="模块6"></el-input>
    </el-form-item>
    <el-form-item label="内容6" prop="contentSix">
      <el-input v-model="dataForm.contentSix" placeholder="内容6"></el-input>
    </el-form-item>
    <el-form-item label="地址" prop="site">
      <el-input v-model="dataForm.site" placeholder="地址"></el-input>
    </el-form-item>
    <el-form-item label="电话" prop="phone">
      <el-input v-model="dataForm.phone" placeholder="电话"></el-input>
    </el-form-item>
    <el-form-item label="邮箱" prop="email">
      <el-input v-model="dataForm.email" placeholder="邮箱"></el-input>
    </el-form-item>
    <el-form-item label="备注" prop="edition">
      <el-input v-model="dataForm.edition" placeholder="备注"></el-input>
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
          mianUrl: '',
          nameOne: '',
          contentOne: '',
          nameTwo: '',
          contentTwo: '',
          nameThree: '',
          contentThree: '',
          nameFour: '',
          contentFour: '',
          nameFive: '',
          contentFive: '',
          nameSix: '',
          contentSix: '',
          site: '',
          phone: '',
          email: '',
          edition: ''
        },
        dataRule: {
          mianUrl: [
            { required: true, message: '背景图不能为空', trigger: 'blur' }
          ],
          nameOne: [
            { required: true, message: '模块1不能为空', trigger: 'blur' }
          ],
          contentOne: [
            { required: true, message: '内容1不能为空', trigger: 'blur' }
          ],
          nameTwo: [
            { required: true, message: '模块2不能为空', trigger: 'blur' }
          ],
          contentTwo: [
            { required: true, message: '内容2不能为空', trigger: 'blur' }
          ],
          nameThree: [
            { required: true, message: '模块3不能为空', trigger: 'blur' }
          ],
          contentThree: [
            { required: true, message: '内容3不能为空', trigger: 'blur' }
          ],
          nameFour: [
            { required: true, message: '模块4不能为空', trigger: 'blur' }
          ],
          contentFour: [
            { required: true, message: '内容4不能为空', trigger: 'blur' }
          ],
          nameFive: [
            { required: true, message: '模块5不能为空', trigger: 'blur' }
          ],
          contentFive: [
            { required: true, message: '内容5不能为空', trigger: 'blur' }
          ],
          nameSix: [
            { required: true, message: '模块6不能为空', trigger: 'blur' }
          ],
          contentSix: [
            { required: true, message: '内容6不能为空', trigger: 'blur' }
          ],
          site: [
            { required: true, message: '地址不能为空', trigger: 'blur' }
          ],
          phone: [
            { required: true, message: '电话不能为空', trigger: 'blur' }
          ],
          email: [
            { required: true, message: '邮箱不能为空', trigger: 'blur' }
          ],
          edition: [
            { required: true, message: '备注不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    created: function () {
      this.url = this.$http.adornUrl(`/sys/oss/upload?classify=explainbottom&token=${this.$cookie.get('token')}`)
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
          this.dataForm.mianUrl = response.url
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
              url: this.$http.adornUrl(`/generator/explainbottom/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.mianUrl = data.explainBottom.mianUrl
                this.dataForm.nameOne = data.explainBottom.nameOne
                this.dataForm.contentOne = data.explainBottom.contentOne
                this.dataForm.nameTwo = data.explainBottom.nameTwo
                this.dataForm.contentTwo = data.explainBottom.contentTwo
                this.dataForm.nameThree = data.explainBottom.nameThree
                this.dataForm.contentThree = data.explainBottom.contentThree
                this.dataForm.nameFour = data.explainBottom.nameFour
                this.dataForm.contentFour = data.explainBottom.contentFour
                this.dataForm.nameFive = data.explainBottom.nameFive
                this.dataForm.contentFive = data.explainBottom.contentFive
                this.dataForm.nameSix = data.explainBottom.nameSix
                this.dataForm.contentSix = data.explainBottom.contentSix
                this.dataForm.site = data.explainBottom.site
                this.dataForm.phone = data.explainBottom.phone
                this.dataForm.email = data.explainBottom.email
                this.dataForm.edition = data.explainBottom.edition
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
              url: this.$http.adornUrl(`/generator/explainbottom/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'mianUrl': this.dataForm.mianUrl,
                'nameOne': this.dataForm.nameOne,
                'contentOne': this.dataForm.contentOne,
                'nameTwo': this.dataForm.nameTwo,
                'contentTwo': this.dataForm.contentTwo,
                'nameThree': this.dataForm.nameThree,
                'contentThree': this.dataForm.contentThree,
                'nameFour': this.dataForm.nameFour,
                'contentFour': this.dataForm.contentFour,
                'nameFive': this.dataForm.nameFive,
                'contentFive': this.dataForm.contentFive,
                'nameSix': this.dataForm.nameSix,
                'contentSix': this.dataForm.contentSix,
                'site': this.dataForm.site,
                'phone': this.dataForm.phone,
                'email': this.dataForm.email,
                'edition': this.dataForm.edition
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
