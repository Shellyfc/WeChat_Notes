#Banner/Swiper

https://developers.weixin.qq.com/miniprogram/dev/component/swiper.html

	<swiper indicator-dots="{{indicatorDots}}"
        autoplay="{{autoplay}}" interval="{{interval}}" duration="{{duration}}">
        <block wx:for="{{background}}" wx:key="*this">
          <swiper-item>
            <view class="swiper-item {{item}}"></view>
          </swiper-item>
        </block>
      </swiper>