<template>
	<div class="left-nav">
		<ul>
			<li>
				<i class="iconfont icon-wodezichan"></i>
				<div>收银</div>
			</li>
			<li>
				<i class="iconfont icon-dianpu"></i>
            	<div>店铺</div>
			</li>
			<li>
				<i class="iconfont icon-31quanbushangpin"></i>
            	<div>商品</div>
			</li>
			<li>
				<i class="iconfont icon-huiyuanqia"></i>
				<div>会员</div>
			</li>
			<li>
				<i class="iconfont icon-paixing"></i>
            	<div>统计</div>
			</li>
			<li>
				<i class="iconfont icon-shezhi"></i>
            	<div>设置</div>
			</li>
		</ul>
	</div>
</template>
<script>
	export default{
		name: 'leftNav'
	}
</script>
<style>
	.left-nav{
		color: #fff;
		font-size: 10px;
		width: 5%;
		height: 100%;
		background-color: #1D8ce0;
		float: left;
	}
	.iconfont{
		font-size: 24px;
	}
	.left-nav ul{
		margin: 0;
		padding: 0;
	}
	.left-nav ul li{
		list-style: none;
		text-align: center;
		padding: 10px;
		border-bottom: 1px solid #20a0ff;
	}
</style>