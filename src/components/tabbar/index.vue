<template>
	<div class="g-tabbar">
		<router-link class="g-tabbar-item" to="/ceming">
			<i class="iconfont icon-home"></i>
			<span>首页</span>
		</router-link>
		<a class="g-tabbar-item" target="_self" href="http://www.china8188.com/index.php/ad/ad-goods-index.html?tdsourcetag=s_pctim_aiomsg">
			<i class="iconfont icon-category"></i>
			<span>商城</span>
		</a>
		<router-link class="g-tabbar-item" to="/personal">
			<i class="iconfont icon-personal"></i>
			<span>个人中心</span>
		</router-link>
	</div>
</template>

<script>
	export default {
		name: 'Ctabbar',
		data() {
			return {
				
			};
		},
		methods:{
			// goHome(){
			// 	this.$router.push({name:'ceming'})
			// }
		}
	}
</script>

<style lang="scss" scoped>
	.g-tabbar{
		position: fixed;
		bottom: 0;
		left: 0;
	}
	.router-link-active{
		color: #de181b;
	}
</style>
