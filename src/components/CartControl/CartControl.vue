<template>
	<div class="cartcontrol">
		<!-- 有一个动画-->
		<transition name="move">
			<div class="iconfont icon-remove_circle_outline" v-if="food.count" @click.stop="updateFoodCount(false)"></div>
		</transition>
		<div class="cart-count" v-if="food.count">{{food.count}}</div>
		<div class="iconfont icon-add_circle" @click.stop="updateFoodCount(true)"></div>
	</div>
</template>

<script>
export default {
	props: {
		food: Object
	},
	methods: {
		updateFoodCount (isAdd) {
			// 这里不能直接处理food.count因为food是在good里的数据对象
			// 应该通过vuex触发action来管理数据 并把当前的food对象参数传递过去
			this.$store.dispatch('updateFoodCount', {isAdd, food: this.food})
		}
	}
}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
  @import "../../common/stylus/mixins.styl"
  .cartcontrol
    font-size: 0
    .icon-remove_circle_outline
      display: inline-block
      padding: 6px
      line-height: 24px
      font-size: 24px
      color: $green
      &.move-enter-active, &.move-leave-active
        transition all .3s
      &.move-enter, &.move-leave-active
        opacity 0
        transform translateX(30px) rotate(90deg)
    .cart-count
      display: inline-block
      vertical-align: top
      width: 12px
      padding-top: 6px
      line-height: 24px
      text-align: center
      font-size: 10px
      color: rgb(147, 153, 159)
    .icon-add_circle
      display: inline-block
      padding: 6px
      line-height: 24px
      font-size: 24px
      color: $green
</style>
