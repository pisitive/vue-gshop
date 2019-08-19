<template>
	<section class="msite">
		<!--首页头部title-->
		<!--由msite_header改成header-->
		<HeaderTop :title="address.name">
			<router-link class="header_search" slot="left" to="/search">
				<i class="iconfont icon-sousuo"></i>
			</router-link>
			<router-link class="header_login" slot="right" :to="userInfo._id ? '/userinfo' : '/login'">
				<span class="header_login_text" v-if="!userInfo._id">
					登录|注册
				</span>
				<span class="header_login_text" v-else>
					<i class="iconfont icon-person"></i>
				</span>
			</router-link>
		</HeaderTop>
		<!--首页导航轮播-->
		<div class="miste-content-wrapper">
		  <div class="miste-content">
			<nav class="msite_nav border-1px">
				<div class="swiper-container" v-if="categorys.length">
					<div class="swiper-wrapper">
						<div class="swiper-slide" v-for="(categorys, index) in categorysArr" :key="index">
							<a href="javascript:" class="link_to_food" v-for="(category, index) in categorys" :key="index">
								<div class="food_container">
									<img :src="baseImageUrl + category.image_url">
								</div>
								<span>{{category.title}}</span>
							</a>
						</div>
					</div>
					<!-- 轮播图页码 -->
					<div class="swiper-pagination"></div>
				</div>
				<img alt="back" src="./images/msite_back.svg" v-else>
			</nav>
			<!--首页附近商家列表-->
			<div class="msite_shop_list border-1px">
				<div class="shop_header">
					<i class="iconfont icon-xuanxiang"></i>
					<span class="shop_header_title">附近商家</span>
				</div>
				<ShopList />
			</div>
		  </div>
		</div>
	</section>
</template>

<script>
import {mapState} from 'vuex'
import Swiper from 'swiper'
import 'swiper/dist/css/swiper.min.css'
import BScroll from 'better-scroll'

import HeaderTop from '../../components/HeaderTop/HeaderTop.vue'
import ShopList from '../../components/ShopList/ShopList.vue'

export default {
	data() {
		return {
			baseImageUrl: 'https://fuss10.elemecdn.com'
		}
	},
	mounted() {
		this.$store.dispatch('getCategorys')
		this.$store.dispatch('getShops')
	},
	computed: {
		...mapState(['address', 'categorys', 'userInfo']),
			
		/*
		根据categorys一维数组生成一个二维数组
		小数组中的元素个数最大是8个
		*/	
		categorysArr() {
			const {categorys} = this
		    //准备空的二维数组
			const arr = []
			let minArr = []
			//遍历categorys
			categorys.forEach(c => {
				//如果minArr满了，创建一个新的
				if (minArr.length === 8) {
					minArr = []
				}
				//如果当前minArr是空的，将minArr保存到arr中
				if (minArr.length === 0) {
					arr.push(minArr)
				}
				//将当前分类保存到小数组中
				minArr.push(c)
			})
			return arr
		}
		
	},
	watch: {
		categorys(value) {//categorys数组中有数据了
			// 在修改数据之后立即使用它，然后等待 DOM 更新。
			this.$nextTick(() => {//此条语句要写在数据更新之后
				// 一旦完成界面更新, 立即执行回调
				new Swiper('.swiper-container', {
					autoplay: true,
					pagination: {
						el: '.swiper-pagination',
						clickable: true
					}
				})
				
				new BScroll('.miste-content-wrapper', {
				  click: true
				})
			})
		}
	},
	components: {
		HeaderTop,
		ShopList
	}
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixins.styl"
  .msite
    width 100%
    height: 100%
    .miste-content-wrapper
      position fixed
      top: 45px
      bottom: 46px
      width: 100%
    .msite_nav
      bottom-border-1px(#e4e4e4)
      height 200px
      background #fff
      .swiper-container
        width 100%
        height 100%
        .swiper-wrapper
          width 100%
          height 100%
          .swiper-slide
            display flex
            justify-content center
            align-items flex-start
            flex-wrap wrap
            .link_to_food
              width 25%
              .food_container
                display block
                width 100%
                text-align center
                padding-bottom 10px
                font-size 0
                img
                  display inline-block
                  width 50px
                  height 50px
              span
                display block
                width 100%
                text-align center
                font-size 13px
                color #666
        .swiper-pagination
          >span.swiper-pagination-bullet-active
            background #02a774
    .msite_shop_list
      top-border-1px(#e4e4e4)
      margin-top 10px
      background #fff
      .shop_header
        padding 10px 10px 0 10px
        .shop_icon
          margin-left 5px
          color #999
        .shop_header_title
          color #999
          font-size 14px
          line-height 20px


</style>