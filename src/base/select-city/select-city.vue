<template>
  <transition name="slide">
    <div class="select-city">
      <!--头部搜索-->
      <div class="city-navbar">
        <!--左边输入框-->
        <div class="city-input">
          <div class="input-box">
            <input class="input" placeholder="请输入城市名称" type="text">
          </div>
        </div>
        <div class="close" @click="back">
          <i class="iconfont icon-close"></i>
        </div>
        <!--右边关闭按钮-->
        <div></div>
      </div>
      <list-view :hotCitys="hotCityList" :allCitys="cityList" :hotSug="overseasHotCityList" :allSug="overseasCityList"></list-view>
    </div>
  </transition>
</template>

<script>
import {getCitys} from 'api/home'
import ListView from 'base/listview/listview'

export default {
  name: 'SelectCity',
  components: {
    ListView
  },
  data() {
    return {
      hotCityList: [],
      cityList: [],
      overseasHotCityList: [],
      overseasCityList: []
    }
  },
  created() {
    this._getCitys()
  },
  methods: {
    back() {
      this.$router.back()
    },
    _getCitys() {
      getCitys().then((res) => {
        if (res.IsError === false) {
          console.log(res)
          this.hotCityList = res.hotCityList
          this.cityList = res.cityList
          this.overseasHotCityList = res.overseasHotCityList
          this.overseasCityList = res.overseasCityList
        }
      })
    },
    _normalizeChar(obj) {
      let arr = []
      let newObject = {}
      for (let i in obj) {
        let o = {}
        o[i] = obj[i]
        arr.push(o)
      }
      arr.sort((a, b) => {
        return Object.keys(a)[0].charCodeAt(0) - Object.keys(b)[0].charCodeAt(0)
      })
      arr.forEach((item) => {
        let newKey = Object.keys(item)[0]
        let newVal = item[newKey]
        let newArr = []
        for (let i = 0; i < newVal.length; i++) {
          newArr.push({
            name: newVal[i].ItemName
          })
        }
        newObject[newKey] = newArr
      })
      return newObject
    },
    _normalizeHotSug(list) {
      let arr = []
      list.forEach((item) => {
        arr.push({
          CityName: item.name
        })
      })
      return arr
    }
  }
}
</script>

<style lang="stylus" scoped>
@import '~common/stylus/variable'

.select-city
  position: fixed
  z-index: 100
  width: 100%
  top: 0
  bottom: 0
  background: #f4f4f4
  overflow: hidden
  .city-navbar
    display: flex
    border-bottom: 1px solid $color-border-bottom
    background: $color-background
    .city-input
      flex: 1
      position: relative
      height: .86rem
      border: none
      font-size: $font-size-small
      border-radius: .05rem
      padding: .1rem 0 .1rem .1rem
      .input-box
        width: 100%
        height: 100%
        .input
          width: 100%
          height: 100%
          border-radius: .3rem
          background: #f4f4f4
          outline: none
          text-align: center
          color: $color-text
          caret-color: $color-text
          &::placeholder
            color: #ccc
    .close
      width: .86rem
      height: .86rem
      line-height: .86rem
      text-align: center
      .icon-close
        display: inline-block
        padding-top: .2rem
        line-height: .45rem
        font-size: .45rem
        color: #8e8e93
.slide-enter-active, .slide-leave-active
  transition: all 0.3s

.slide-enter, .slide-leave-to
  transform: translate3d(0, 100%, 0)
</style>
