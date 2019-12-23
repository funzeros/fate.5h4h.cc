<template>
	<div class="g-tabbar">
		<router-link class="g-tabbar-item" to="/ceming">
			<i class="iconfont icon-home"></i>
			<span>首页</span>
		</router-link>
		<div class="g-tabbar-item" @click="show">
			<i class="iconfont icon-category"></i>
			<span>更多</span>
		</div>
		<router-link class="g-tabbar-item" to="/personal">
			<i class="iconfont icon-personal"></i>
			<span>我的</span>
		</router-link>
		<transition name="fadeList">
			<div class="header-navlist" v-show="list">
				<div class="header-navlist-title">
					<img src="../../assets/img/dibu.jpg" alt="">
				</div>
				<div class="header-navlist-list pad20">
					<div class="list1" v-for="(item,index) in navlist" :key="index">
						<div class="list1-title">—— {{item.title}} ——</div>
						<ul>
							<li @click="close" class="arrow" v-for="(litem,lindex) in item.list1" :key="lindex">
								<router-link :to="litem.url">{{litem.name}}</router-link>
							</li>
						</ul>
					</div>
				</div>
				<div class="header-navlist-btn">
					<div @click="goHome"><img src="../../assets/img/shouye.png">回到首页</div>
					<div @click="close"><img src="../../assets/img/close.png">关闭导航</div>
				</div>
			</div>
		</transition>
	</div>
</template>

<script>
	export default {
		name: 'Ctabbar',
		data() {
			return {
				list: false,
				navlist: [{
					title: '姓名服务',
					list1: [
						{
							name: '在线测名',
							url: '/ceming'
						}
					]
					},{
					title: '命理服务',
					list1: [
						{
							name: '八字算命',
							url: '/bazisuanming'
						},
						{
							name: '称骨算命',
							url: '/chenggusuanming'
						},
						{
							name: '笔画算命',
							url: '/bihuasuanming'
						},
						{
							name: '属相配对',
							url: '/shuxiangpeidui'
						}
					]
					},{
						title: '星座占卜',
						list1: [
							{
								name: '星座运势',
								url: '/xingzuoyunshi'
							}
						]
					},{
					title: '风水服务',
					list1: [
						{
							name: '阳宅布局',
							url: '/yangzhai'
						},
						{
							name: '阴宅布局',
							url: '/yinzhai'
						},
						{
							name: '风水培训',
							url: '/fengshuipeixun'
						}
					]
					}
					,{
					title: '个人服务',
					list1: [
						{
							name: '个人中心',
							url: '/personal'
						}
					]
					}
				]
			};
		},
		methods:{
			goHome(){
				this.$router.push({path:'/'})
				this.close();
			},
			show(){
				this.list = true;
			},
			close(){
				this.list = false;
			},
		}
	}
</script>

<style lang="scss" scoped>
	.g-tabbar{
		position: fixed;
		bottom: 0;
		left: 0;
		z-index: 9998;
	}
	.router-link-active{
		color: #de181b;
	}


	.fadeList-enter-active {
	  transition: opacity .5s;
	}
	.fadeList-leave-active {
	  transition: all .5s ease;
	}
	.fadeList-enter{
	  opacity: 0;
	}
	.fadeList-leave-to{
		transform: translateX(100%);
		opacity: 0;
	}

	.header-navlist{
			width: 100%;
			height: 100vh;
			overflow: hidden;
			overflow-y: auto;
			background: #FEF9F5;
			position: fixed;
			top: 0;
			left: 0;
			z-index: 9999;
			&-title{
				height: 200px;
				background: #850005;
				>img{
					width: 100%;
				}
			}
			&-list{
				.list1{
					margin-top: 20px;
					&-title{
						text-align: center;
						font-size: 26px;
						font-weight: 600;
						line-height: 1;
						color: #850005;
						padding: 10px 0;
					}
					>ul li{
						height: 80px;
						background: #fff;
						box-sizing: border-box;
						padding: 0 20px;
						margin-bottom: 10px;
						>a{
							display: block;
							width: 100%;
							height: 100%;
							line-height: 80px;
							font-size: 28px;
							color: #333333;
						}
					}
				}
			}
			&-btn{
				display: flex;
				height: 100px;
				margin-top: 30px;
				>div{
					flex: 1;
					height: 100px;
					display: flex;
					justify-content: center;
					align-items: center;
					font-size: 32px;
					background: #850005;
					color: #ffffff;
					border-right: 1px solid #fff; /*no*/
					&:last-child{
						border-right: none;
					}
					>img{
						width: 42px;
						height: 42px;
						margin-right: 7px;
					}
				}
			}
		}
</style>
