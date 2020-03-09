<template>
  <div class="dialog-preview">
    <el-dialog
      v-if="previewVisible"
      :visible="previewVisible"
      :append-to-body="true"
      :close-on-press-escape="false"
      title="图片预览"
      top="6vh"
      width="889px"
      class="preview-wrapper"
      @close="previewClose">
      <div class="big-img-wrapper">
        <img
          v-if="activeUrl"
          :src="activeUrl"
          width="100%"
          alt="图片预览">
        <div
          v-else
          class="big-text">{{ activeText }}</div>
      </div>
      <flex class="img-list-wrapper">
        <div
          class="btn prev-img"
          @click="prevOrNext('prev')">
          <i
            class="el-icon-arrow-left" />
        </div>
        <flex class="img-list">
          <flex
            v-if="typeof fileList[0] === 'object'"
            ref="scrollImg"
            class="scroll-list">
            <div
              v-for="item in fileList"
              :key="item.fileId"
              :class="activeId === item.fileId ? 'active' : ''"
              class="img-wrapper"
              @click="changeActive(item)">
              <img
                :src="item.fileUrl">
            </div>
          </flex>
          <flex
            v-else
            ref="scrollText"
            class="scroll-list">
            <div
              v-for="item in fileList"
              :key="item"
              :class="activeText === item ? 'active' : ''"
              class="img-wrapper"
              @click="changeActive(item)">
              <flex
                justify="center"
                class="text-wrapper-sm">
                {{ item }}
              </flex>
            </div>
          </flex>
        </flex>
        <div
          class="btn next-img"
          @click="prevOrNext('next')">
          <i
            class="el-icon-arrow-right" />
        </div>
      </flex>
    </el-dialog>
  </div>
</template>
<script>
export default {
  props: {
    fileList: {
      type: Array,
      default: () => []
    },
    previewVisible: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      activeUrl: '',
      activeText: '',
      activeId: '',
      activeIndex: 0, // 当前预览的索引
      viewIndex: 0 // 当前预览图在底部图片的相对位置
    }
  },
  mounted () {
    if (this.fileList.length) {
      if (typeof this.fileList[0] === 'object') {
        this.activeUrl = this.fileList[0].fileUrl
        this.activeId = this.fileList[0].fileId
      } else {
        this.activeText = this.fileList[0]
      }
    }
  },
  methods: {
    changeActive (item) {
      if (this.activeId !== item.fileId) {
        this.activeUrl = item.fileUrl
        this.activeId = item.fileId
        for (let i = 0; i < this.fileList.length; i++) {
          if (item.fileId === this.fileList[i].fileId) {
            this.activeIndex = i
            // 比较预览索引和相对索引的位置关系
            this.viewIndex !== ((i - 1) % 3) && (this.viewIndex = i > 2 ? Math.abs(((i - 1) % 3)) : i)
            break
          }
        }
      } else if (item !== this.activeText) {
        this.activeText = item
        for (let i = 0; i < this.fileList.length; i++) {
          if (item === this.fileList[i]) {
            this.activeIndex = i
            this.viewIndex !== ((i - 1) % 3) && (this.viewIndex = i > 2 ? Math.abs(((i - 1) % 3)) : i)
            break
          }
        }
      }
    },
    prevOrNext (type) {
      if (this.fileList.length === 1) return
      if (type === 'prev') {
        if (this.viewIndex > 0) {
          this.viewIndex -= 1
        }
        this.activeIndex -= 1
        // 如果预览索引小于零，跳到最后一个
        if (this.activeIndex < 0) {
          this.activeIndex = this.fileList.length - 1
          this.viewIndex = 2
          this.$refs.scrollImg && (this.$refs.scrollImg.$el.style.transform = `translateX(-${(this.activeIndex - 2) * 255}px)`)
          this.$refs.scrollText && (this.$refs.scrollText.$el.style.transform = `translateX(-${(this.activeIndex - 2) * 255}px)`)
        }
        // 只有相对索引为0时，才移动
        if (this.activeIndex < this.fileList.length - 3 && this.viewIndex === 0) {
          this.$refs.scrollImg && (this.$refs.scrollImg.$el.style.transform = `translateX(-${this.activeIndex * 255}px)`)
          this.$refs.scrollText && (this.$refs.scrollText.$el.style.transform = `translateX(-${this.activeIndex * 255}px)`)
        }
        if (typeof this.fileList[0] === 'object') {
          this.activeUrl = this.fileList[this.activeIndex].fileUrl
          this.activeId = this.fileList[this.activeIndex].fileId
        } else {
          this.activeText = this.fileList[this.activeIndex]
        }
      } else {
        if (this.viewIndex < 2) {
          this.viewIndex += 1
        }
        this.activeIndex += 1
        // 如果预览索引大于数组长度 - 1，跳到第一个
        if (this.activeIndex > this.fileList.length - 1) {
          this.activeIndex = 0
          this.viewIndex = 0
          this.$refs.scrollImg && (this.$refs.scrollImg.$el.style.transform = 'translateX(0)')
          this.$refs.scrollText && (this.$refs.scrollText.$el.style.transform = 'translateX(0)')
        }
        // 只有相对索引为2时，才移动
        if (this.activeIndex > 2 && this.viewIndex === 2) {
          this.$refs.scrollImg && (this.$refs.scrollImg.$el.style.transform = `translateX(-${(this.activeIndex - this.viewIndex) * 255}px)`)
          this.$refs.scrollText && (this.$refs.scrollText.$el.style.transform = `translateX(-${(this.activeIndex - this.viewIndex) * 255}px)`)
        }
        if (typeof this.fileList[0] === 'object') {
          this.activeUrl = this.fileList[this.activeIndex].fileUrl
          this.activeId = this.fileList[this.activeIndex].fileId
        } else {
          this.activeText = this.fileList[this.activeIndex]
        }
      }
    },
    previewClose () {
      this.$emit('update:previewVisible', false)
    }
  }
}
</script>
<style lang="stylus" scoped>
.big-img-wrapper
  padding 20px
  box-sizing border-box
  border-radius 6px
  background-color #FFFFFF
.img-list-wrapper
  width 100%
  margin-top 20px
  overflow hidden
  box-sizing border-box
  .btn
    width 42px
    height 188px
    flex none
    text-align center
    line-height 188px
    background-color #FFFFFF
    border-radius 6px
    cursor pointer
    box-sizing border-box
  .el-icon-arrow-left,
  .el-icon-arrow-right
    font-size 26px
    color #909399
  .img-list
    width calc(100% - 84px)
    height 188px
    box-sizing border-box
    overflow hidden
  .img-wrapper
    width 245px
    height 188px
    margin 0 5px
    padding 18px 0
    background-color #FFFFFF
    box-sizing border-box
    cursor pointer
    flex none
    &.active
      padding 16px 0
      border 2px solid #638FF6
    img
      width 100%
      height 100%
  .text-wrapper-sm
    width 100%
    height 152px
    background-color #eee
    font-size 12px
    box-sizing border-box
.big-text
  width 100%
  height 30vh
  text-align center
  line-height 30vh
  font-size 16px
  background-color #eee
</style>

<style lang="stylus">
.preview-wrapper
  .el-dialog__body
    padding 20px
    background-color #E9EAEC
</style>
