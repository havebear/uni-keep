<template>
  <div>
    <div>
      <swiper class="swiper" indicator-dots autoplay interval="5000" duration>
        <swiper-item v-for="item in banners" :key="item.img">
          <view class="swiper-item uni-bg-red">
            <img :src="item.img" alt="">
          </view>
        </swiper-item>
      </swiper>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      banners: [
        {
          img: 'https://images.pexels.com/photos/260409/pexels-photo-260409.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500'
        },
        {
          img: 'https://images.pexels.com/photos/221210/pexels-photo-221210.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500'
        },
        {
          img: 'https://images.pexels.com/photos/163351/girl-boxer-ring-boxing-pear-163351.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500'
        }
      ]
    }
  }
}
</script>

<style>
</style>
