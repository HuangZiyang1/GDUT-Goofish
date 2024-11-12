<template>
  <!--商品-->
  <div class="goods-box goods-css" :style="{ top: top + 'rpx', left: left + 'rpx', opacity: ifNone }">
    <!--商品详细-->
    <router-link :to="`../../pages/package1/pages/goods_detail/index?goodsType=${goodsType}&getInforId=${getInforId}&goodsImgList=${JsonGoodsImgList}`">
      <div class="goods-content">
        <!--图片-->
        <div class="img-box">
          <img class="goods-img" :src="goodsImgList[0]" mode="widthFix" lazy-load="true" @load="doImg" @error="changeImg" />
        </div>

        <!--书名-->
        <div class="name-box">
          <!--商品类型-->
          <div class="goods-type" :style="{ backgroundColor: goodsTypeColor }">
            {{ reallyGoodsType }}
          </div>
          <span class="goods-name">{{ goodsName }}</span>
        </div>

        <!--价格-->
        <div class="price">
          <div class="price-icon">￥</div>
          <!--最低价-->
          <div class="price-min">{{ goodsInfor.lowPrice }}</div>
          <div>~</div>
          <!--最高价-->
          <div class="price-max">{{ goodsInfor.highPrice }}</div>
        </div>

        <!--浏览量-->
        <div class="view-num">
          <!--图标-->
          <img class="view-icon" src="../../icons/浏览.png" mode="widthFix" />
          <!--数字-->
          <div class="view-text">{{ goodsInfor.pageView }}</div>
        </div>
      </div>
    </router-link>
  </div>
</template>

<script>
export default {
  name: 'Goods',
  props: {
    goodsInfor: {
      type: Object,
      default: () => ({})
    },
    team: {
      type: Array,
      default: () => []
    },
    index: {
      type: Number,
      required: true
    }
  },
  data () {
    return {
      goodsType: '',
      reallyGoodsType: '',
      goodsTypeColor: '',
      getInforId: '',
      goodsImgList: '',
      JsonGoodsImgList: '',
      goodsName: '',
      serviceImg: ['../../icons/server1.png', '../../icons/服务2.png', '../../icons/服务3.png'],
      top: 0,
      left: 0,
      bottomGap: 10,
      ifNone: 0
    }
  },
  mounted () {
    let goodsType = ''
    let goodsTypeColor = ''
    let reallyGoodsType = ''
    let getInforId = ''
    let goodsImgList = ''
    let goodsName = ''

    if (this.goodsInfor.typeId === 1) {
      if (this.goodsInfor.isSelf === 1) {
        goodsType = '书籍'
        reallyGoodsType = '自营'
        goodsTypeColor = '#FF0101'
      } else {
        goodsType = '书籍'
        reallyGoodsType = '书籍'
        goodsTypeColor = '#B9E7AE'
      }
      getInforId = this.goodsInfor.suitId
      goodsName = this.goodsInfor.title

      if (this.goodsInfor.pictures?.length !== 0) {
        goodsImgList = this.goodsInfor.pictures?.map(e => e.url)
      } else {
        goodsImgList = ['../../icons/noImg.png']
      }
    } else if (this.goodsInfor.typeId === 2) {
      if (this.goodsInfor.isSelf === 1) {
        goodsType = '商品'
        reallyGoodsType = '自营'
        goodsTypeColor = '#FF0101'
      } else {
        goodsType = '商品'
        reallyGoodsType = '闲置'
        goodsTypeColor = '#F1F9BB'
      }
      getInforId = this.goodsInfor.businessId
      goodsName = this.goodsInfor.title

      if (this.goodsInfor.pictureList?.length !== 0) {
        goodsImgList = this.goodsInfor.pictureList?.map(e => e.url)
      } else {
        goodsImgList = ['../../icons/noImg.png']
      }
    } else if (this.goodsInfor.typeId === 3) {
      if (this.goodsInfor.isSelf === 1) {
        goodsType = '服务'
        reallyGoodsType = '自营'
        goodsTypeColor = '#FF0101'
      } else {
        goodsType = '服务'
        reallyGoodsType = '服务'
        goodsTypeColor = '#FCDDEC'
      }
      getInforId = this.goodsInfor.serviceId
      goodsName = this.goodsInfor.title

      let index = Math.floor(Math.random() * (2 - 0 + 1) + 0)
      goodsImgList = [this.serviceImg[index]]
    }

    this.goodsType = goodsType
    this.reallyGoodsType = reallyGoodsType
    this.goodsTypeColor = goodsTypeColor
    this.getInforId = getInforId
    this.goodsImgList = goodsImgList
    this.JsonGoodsImgList = JSON.stringify(goodsImgList)
    this.goodsName = goodsName

    this.$emit('getHeight', 200)
  },
  methods: {
    doImg (e) {
      let heightArray = this.team
      let { height, width } = e.target

      let left = 0
      let top = 0
      let bottomGap = this.bottomGap

      let realHeight = Math.ceil(height / width * 143)
      let realBoxHeight = realHeight * 2 + 135

      let min = Math.min(...heightArray)
      let index = heightArray.indexOf(min)
      left = (index === 0 ? 0 : 370)
      top = min + bottomGap
      heightArray[index] = (min + realBoxHeight + bottomGap)

      let listHeight = Math.max(...heightArray)

      this.top = top
      this.left = left
      this.ifNone = 1

      let options = {
        listHeight,
        heightArray
      }

      this.$emit('getHeight', options)
    },
    changeImg () {
      this.$emit('getImg', this.index)
    }
  }
}
</script>

<style scoped>
.goods-box {
  position: absolute;
  box-sizing: border-box;
  border-radius: 16rpx;
  border: 2rpx solid rgba(0, 0, 0, 0.5);
  background-color: #fff;
  transition: opacity 2s;
}
.goods-box .goods-content {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  position: relative;
  padding-top: 20rpx;
  padding-right: 20rpx;
  padding-bottom: 15rpx;
  width: 336rpx;
}
.goods-box .goods-content .img-box {
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  box-sizing: border-box;
  border-radius: 10rpx;
  border: 4rpx solid #EDEFF2;
  width: 286rpx;
}
.goods-box .goods-content .img-box .goods-img {
  box-sizing: border-box;
  border-radius: 10rpx;
}
.goods-box .goods-content .name-box {
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 10rpx;
  width: 284rpx;
  height: 35rpx;
}
.goods-box .goods-content .name-box .goods-type {
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
  margin-right: 4rpx;
  border: 2rpx solid rgba(0, 0, 0, 0.5);
  border-radius: 20rpx;
  height: 35rpx;
  width: 90rpx;
  background-color: #B9E7AE;
  font-size: 20rpx;
  font-weight: 400;
  line-height: 39.2rpx;
}
.goods-box .goods-content .name-box .goods-name {
  opacity: 78%;
  padding-top: 5rpx;
  word-break: break-all;
  word-wrap: break-word;
  width: 300rpx;
  height: 30rpx;
  font-size: 26rpx;
  font-weight: 400;
  line-height: 28rpx;
}
.goods-box .goods-content .price {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  margin-top: 8rpx;
  height: 28rpx;
  width: 284rpx;
  font-size: 28rpx;
  font-weight: 600;
  line-height: 28rpx;
  color: #FB1E00;
}
.goods-box .goods-content .price .price-icon {
  margin-right: 4rpx;
}
.goods-box .goods-content .view-num {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  bottom: 90rpx;
  left: 255rpx;
  z-index: 999;
  opacity: 90%;
  height: 20rpx;
  min-width: 60rpx;
  background-color: #D9D9D9;
  font-size: 16rpx;
  font-weight: 800;
  line-height: 28rpx;
  color: #FFFFFF;
}
.goods-box .goods-content .view-num .view-icon {
  height: 12rpx;
  width: 24rpx;
}
.goods-box .goods-content .view-num .view-text {
  overflow: hidden;
  white-space: nowrap;
  max-width: 40rpx;
}
</style>
