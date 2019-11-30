<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="家具名称" prop="furnitureName">
      <el-input v-model="dataForm.furnitureName" placeholder="家具名称"></el-input>
    </el-form-item>
    <el-form-item label="价格" prop="furniturePrice">
      <el-input v-model="dataForm.furniturePrice" placeholder="价格"></el-input>
    </el-form-item>
    <el-form-item label="材质" prop="texture">
      <el-input v-model="dataForm.texture" placeholder="材质"></el-input>
    </el-form-item>
    <el-form-item label="尺寸" prop="size">
      <el-input v-model="dataForm.size" placeholder="尺寸"></el-input>
    </el-form-item>
    <el-form-item label="风格" prop="style">
      <el-input v-model="dataForm.style" placeholder="风格"></el-input>
    </el-form-item>
    <el-form-item label="颜色" prop="colour">
      <el-input v-model="dataForm.colour" placeholder="颜色"></el-input>
    </el-form-item>
    <el-form-item label="品牌" prop="brand">
      <el-input v-model="dataForm.brand" placeholder="品牌"></el-input>
    </el-form-item>
    <el-form-item label="系列" prop="series">
      <el-input v-model="dataForm.series" placeholder="系列"></el-input>
    </el-form-item>
    <el-form-item label="主分类" prop="mainClassify">
      <el-input v-model="dataForm.mainClassify" placeholder="请输入主分类 如 桌 床 椅 "></el-input>
    </el-form-item>
    <el-form-item label="二级分类" prop="classify">
      <el-input v-model="dataForm.classify" placeholder="请输入二级分类  如 课桌 双人床 "></el-input>
    </el-form-item>
    <el-form-item label="主缩略图" prop="mainUrl">
      <el-input v-model="dataForm.mainUrl" placeholder="主缩略图只有一张 请先上传文件 这里会显示url 图片像素 260 x 260 "></el-input>
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
    <el-form-item label="轮播图" prop="furnitureUrl">
      <el-input v-model="dataForm.furnitureUrl" placeholder="轮播图 图片像素 1000 x 660   "></el-input>
    </el-form-item>
      <el-form-item>
        <el-upload
          class="furnitureUrl"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="furnitureUrlHandle"
          :before-upload="beforeUploadHandle"
          multiple
          :on-exceed="handleExceed"
          :file-list="fileList"
          list-type="picture">
          <el-button size="small" type="primary">点击上传</el-button>
          <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
        </el-upload>
      </el-form-item>
    <el-form-item label="详情图" prop="explainUrl">
      <el-input v-model="dataForm.explainUrl" placeholder="详情图 请先上传文件 这里会显示url 图片像素 1200 x 无限 "></el-input>
    </el-form-item>
      <el-form-item>
        <el-upload
          class="explainUrl"
          :action="url"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :on-success="explainUrlHandle"
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
          furnitureName: '',
          furniturePrice: '',
          texture: '',
          size: '',
          url: '',
          style: '',
          colour: '',
          brand: '',
          series: '',
          mainClassify: '',
          classify: '',
          mainUrl: '',
          furnitureUrl: '',
          explainUrl: '',
          furnitureDate: '',
          status: 0
        },
        dataRule: {
          furnitureName: [
            { required: true, message: '家具名称不能为空', trigger: 'blur' }
          ],
          furniturePrice: [
            { required: true, message: '价格不能为空', trigger: 'blur' }
          ],
          texture: [
            { required: true, message: '材质不能为空', trigger: 'blur' }
          ],
          size: [
            { required: true, message: '尺寸不能为空', trigger: 'blur' }
          ],
          style: [
            { required: true, message: '风格不能为空', trigger: 'blur' }
          ],
          colour: [
            { required: true, message: '颜色不能为空', trigger: 'blur' }
          ],
          brand: [
            { required: true, message: '品牌不能为空', trigger: 'blur' }
          ],
          series: [
            { required: true, message: '系列不能为空', trigger: 'blur' }
          ],
          mainClassify: [
            { required: true, message: '主分类不能为空', trigger: 'blur' }
          ],
          classify: [
            { required: true, message: '二级分类不能为空', trigger: 'blur' }
          ],
          mainUrl: [
            { required: true, message: '主缩略图不能为空', trigger: 'blur' }
          ],
          furnitureUrl: [
            { required: true, message: '轮播图不能为空', trigger: 'blur' }
          ],
          explainUrl: [
            { required: true, message: '说明图不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '状态不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    created: function () {
      this.url = this.$http.adornUrl(`/sys/oss/upload?classify=furniture&token=${this.$cookie.get('token')}`)
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
      furnitureUrlHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.furnitureUrl = this.dataForm.furnitureUrl + ' | ' + response.url
        } else {
          this.$message.error(response.msg)
        }
      },
      explainUrlHandle (response) {
        if (response && response.code === 0) {
          this.dataForm.explainUrl = this.dataForm.explainUrl + ' | ' + response.url
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
              url: this.$http.adornUrl(`/generator/furniture/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.furnitureName = data.furniture.furnitureName
                this.dataForm.furniturePrice = data.furniture.furniturePrice
                this.dataForm.texture = data.furniture.texture
                this.dataForm.size = data.furniture.size
                this.dataForm.style = data.furniture.style
                this.dataForm.colour = data.furniture.colour
                this.dataForm.brand = data.furniture.brand
                this.dataForm.series = data.furniture.series
                this.dataForm.mainClassify = data.furniture.mainClassify
                this.dataForm.classify = data.furniture.classify
                this.dataForm.mainUrl = data.furniture.mainUrl
                this.dataForm.furnitureUrl = data.furniture.furnitureUrl
                this.dataForm.explainUrl = data.furniture.explainUrl
                this.dataForm.furnitureDate = data.furniture.furnitureDate
                this.dataForm.status = data.furniture.status
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
              url: this.$http.adornUrl(`/generator/furniture/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'furnitureName': this.dataForm.furnitureName,
                'furniturePrice': this.dataForm.furniturePrice,
                'texture': this.dataForm.texture,
                'size': this.dataForm.size,
                'style': this.dataForm.style,
                'colour': this.dataForm.colour,
                'brand': this.dataForm.brand,
                'series': this.dataForm.series,
                'mainClassify': this.dataForm.mainClassify,
                'classify': this.dataForm.classify,
                'mainUrl': this.dataForm.mainUrl,
                'furnitureUrl': this.dataForm.furnitureUrl,
                'explainUrl': this.dataForm.explainUrl,
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
