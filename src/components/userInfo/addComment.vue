<template xmlns:v-slot="http://www.w3.org/1999/XSL/Transform">
  <div class="container_pt110 bgeeeeee">
    <!-- 头部 start -->
    <my-header>
      <template v-slot:backs>
        <i class="el-icon-arrow-left"></i>
      </template>
      <template v-slot:header>评价商品</template>
    </my-header>
    <!-- 头部 end -->
    <!-- 内容部分盒子 start -->
    <div class="userinfo_main bgffffff">
      <!-- 商品信息 start -->
      <div class="order_section bgffffff">
        <div class="orderdesc">
          <div>商品名称</div>
          <div class="color666666">{{goods.Productname ? goods.Productname : goods.gdsname}}</div>
        </div>
      </div>
      <!-- 商品信息 end -->
      <!-- 评论区 start -->
      <div class="comment_cont">
        <div class="comment_desc borderc7c7c7">
          <el-input
            type="textarea"
            :rows="6"
            placeholder="请输入内容, 最多140个字符"
            max-length="140"
            v-model="comment">
          </el-input>
        </div>
        <!--<div class="comment_img">-->
          <!--<el-upload-->
            <!--action=""-->
            <!--multiple-->
            <!--list-type="picture-card"-->
            <!--:on-preview="handlePictureCardPreview"-->
            <!--:on-remove="handleRemove">-->
            <!--<i class="el-icon-plus"></i>-->
          <!--</el-upload>-->
          <!--<el-dialog :visible.sync="dialogVisible">-->
            <!--<img width="100%" :src="dialogImageUrl" alt="">-->
          <!--</el-dialog>-->
        <!--</div>-->
        <!-- 按钮部分 start -->
        <div class="btn_box scan_section">
          <!-- 确认按钮 start -->
          <div class="send_btn border_r6 borderff7e42 bgff7e42 colorffffff font32 font_normal" @click="sendComment">确认</div>
          <!-- 确认按钮 end -->
        </div>
        <!-- 按钮部分 end -->
      </div>
      <!-- 评论区 end -->
    </div>
    <!-- 内容部分盒子 end -->
  </div>
</template>

<script>
import MyHeader from '@/components/common/header/myheader'
import tip from '@/utils/tip'

export default {
  name: 'addComment',
  data () {
    return {
      // 商品信息
      goods: this.$route.query.goods,
      // 评价内容
      comment: '',
      // 评价图片
      commentimg: '',
      dialogImageUrl: '',
      dialogVisible: false
    }
  },
  computed: {},
  components: {
    MyHeader
  },
  methods: {
    // 发送评价
    sendComment () {
      let self = this
      if (!self.comment) {
        tip('内容不能为空！')
        return false
      }
      if (self.comment.length > 140) {
        tip('已超过140个字符！')
        return false
      }
      let data = {
        gdsid: self.goods.Gdscode ? self.goods.Gdscode : self.goods.gdscode,
        content: self.comment,
        // 区分微会员和百货，wemember：微会员；generalMerchandise：百货
        flag: 'wemember'
      }
      self.$api.api.addAppraise(data).then(result => {
        let res = result.data
        if (res.code === 200) {
          tip('提交成功!')
          self.$router.back()
        } else {
          tip(res.msg)
        }
      })
    },
    // 删除照片
    handleRemove (file, fileList) {
      console.log(file, fileList)
    },
    // 查看放大图
    handlePictureCardPreview (file) {
      let self = this
      self.dialogImageUrl = file.url
      self.dialogVisible = true
    }
  },
  watch: {},
  beforeCreate () {
  },
  created () {
  },
  beforeMount () {
  },
  mounted () {
  }
}
</script>

<style scoped>
@import "static/css/userInfo.css";
  .order_section {
    margin-top: unset;
  }
</style>
