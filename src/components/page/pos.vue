<template>
	<div class="pos">
		<el-row>
			<el-col :span="8" id="order-list" class="post-order">
				<div class="menu">
					<el-tabs>
						<el-tab-pane label="点餐">
							<el-table :data="tableData" border style="width:100%" height="300">
								<el-table-column prop="goodsName" label="商品名称"></el-table-column>
								<el-table-column prop="count" label="数量"></el-table-column>
								<el-table-column prop="price" label="金额"></el-table-column>
								<el-table-column label="操作" width="100" fixed="right">
									<template slot-scope="scope">
										<el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
										<el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
									</template>
								</el-table-column>
							</el-table>
							<div class="totalDiv">
								<small>数量：</small>
								<strong>{{totalCount}}</strong>&nbsp;&nbsp;&nbsp;&nbsp;
								<small>总计：</small>
								￥<strong>{{totalPrice}}</strong>元
							</div>
							<div class="order-btn">
								<el-button type="warning">挂单</el-button>
								<el-button type="danger" @click="delOrderList">删除</el-button>
								<el-button type="success" @click="checkOut">结账</el-button>
							</div>
						</el-tab-pane>
						<el-tab-pane label="挂单">挂单</el-tab-pane>
						<el-tab-pane label="外卖">外卖</el-tab-pane>
					</el-tabs>
				</div>
				
			</el-col>
			<el-col :span="16">
				<div class="title">常用商品</div>
				<div class="often-goods-list">
					<ul>
						<li v-for="goods in oftenGoods" @click="addOrderList(goods)">
							<span>{{goods.goodsName}}</span>
							<span class="goodsPrice">￥{{goods.price}}元</span>
						</li>
						
					</ul>
				</div>
				<div class="goods-type">
					<el-tabs>
						<el-tab-pane label="汉堡">
							<ul class='cookList'>
							    <li v-for="goods in type0Goods" @click="addOrderList(goods)">
							        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
							        <span class="foodName">{{goods.goodsName}}</span>
							        <span class="foodPrice">￥{{goods.price}}元</span>
							    </li>

							</ul>
						</el-tab-pane>
						<el-tab-pane label="小食">
							<ul class='cookList'>
							    <li v-for="goods in type1Goods" @click="addOrderList(goods)">
							        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
							        <span class="foodName">{{goods.goodsName}}</span>
							        <span class="foodPrice">￥{{goods.price}}元</span>
							    </li>

							</ul>
						</el-tab-pane>
						<el-tab-pane label="饮料">
							<ul class='cookList'>
							    <li v-for="goods in type2Goods" @click="addOrderList(goods)">
							        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
							        <span class="foodName">{{goods.goodsName}}</span>
							        <span class="foodPrice">￥{{goods.price}}元</span>
							    </li>

							</ul>
						</el-tab-pane>
						<el-tab-pane label="套餐">
							<ul class='cookList'>
							    <li v-for="goods in type3Goods" @click="addOrderList(goods)">
							        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
							        <span class="foodName">{{goods.goodsName}}</span>
							        <span class="foodPrice">￥{{goods.price}}元</span>
							    </li>

							</ul>
						</el-tab-pane>
					</el-tabs>
				</div>
			</el-col>
		</el-row>
	</div>
</template>

<script>
	import axios from 'axios';
	export default{
		name: 'pos',
		data() {
			return {
				tableData: [],
		        oftenGoods: [],
		        type0Goods: [],
		        type1Goods: [],
		        type2Goods: [],
		        type3Goods: [],
		        totalCount: 0,
		        totalPrice: 0


			}
		},
		created: function() {
			axios.get("http://jspang.com/DemoApi/oftenGoods.php").then(response=>{
				this.oftenGoods = response.data;
			}).catch(error=>{
				alert("网络错误！");
			});
			axios.get("http://jspang.com/DemoApi/typeGoods.php").then(response=>{
				this.type0Goods = response.data[0];
				this.type1Goods = response.data[1];
				this.type2Goods = response.data[2];
				this.type3Goods = response.data[3];
			}).catch(error=>{
				alert("网络错误！");
			});
		},
		mounted: function() {
			let clientHeight = document.body.clientHeight;
			document.getElementById("order-list").style.height = clientHeight + "px";

		},
		methods: {
			addOrderList: function(goods) {
				this.totalCount = 0;
				this.totalPrice = 0;
				let isHave = false;
				for (let i = 0; i < this.tableData.length; i++) {
					if (goods.goodsId == this.tableData[i].goodsId) {
						isHave = true;
					}
				}
				if (isHave) {
					let arr = this.tableData.filter(o=>o.goodsId == goods.goodsId);
					arr[0].count++;
				} else{
					let newGoods = {goodsId: goods.goodsId, goodsName: goods.goodsName, price: goods.price, count: 1};
					this.tableData.push(newGoods);
				}
				this.getTotal();
			},
			delSingleGoods: function(goods) {
				this.tableData = this.tableData.filter(o=>o.goodsId != goods.goodsId);
				this.getTotal();
			},
			delOrderList: function() {
				this.tableData = [];
				this.totalPrice = 0;
				this.totalCount = 0;
			},
			getTotal: function() {
				this.totalCount = 0;
				this.totalPrice = 0;
				if (this.tableData) {
					this.tableData.forEach((element) => {
						this.totalCount += element.count;
						this.totalPrice = this.totalPrice + element.count*element.price;
					});
				}
				
			},
			checkOut: function() {
				if (this.totalCount) {
					this.tableData = [];
					this.totalCount = 0;
					this.totalPrice = 0;
					this.$message({
						message: '结账成功，感谢！',
						type: 'success'
					});
				} else{
					this.$message.error('订单为空，不能急着结账！');
				}
			}

		}

	}
</script>
<style>
	.post{
		font-size: 16px;
	}
	.post-order{
		background-color: #F9FAFC;
    	border-right: 1px solid #C0CCDA;
	}
	.menu{
		padding: 0 5px 0 10px;
	}
	.totalDiv{
		text-align: right;
		font-size: 16px;
		background-color: #fff;
		border-bottom: 1px solid #E5E9F2;
		padding: 10px;
	}
	.order-btn{
		margin-top: 10px;
		text-align: center;
	}
	.title{
		height: 20px;
		border-bottom: 1px solid #D3DCE6;
		background-color: #F9FAFC;
		padding: 10px 10px 9px 10px;
	}
	.often-goods-list {
	    border-bottom: 1px solid #C0CCDA;
	    height: auto;
	    overflow: hidden;
	    padding-bottom: 10px;
	    background-color: #F9FAFC;
	}
	.often-goods-list ul li{
		list-style: none;
		float: left;
		border: 1px solid #E5E9F2;
		background-color: #fff;
		padding: 10px;
		margin: 5px;
		cursor: pointer;
	}
	.goodsPrice{
		color: #58B7FF;
	}
	.goods-type{
		padding-left: 10px;
		clear: both;
	}
	.cookList{
		padding-left: 20px
	}
	.cookList li{
		list-style: none;
		width: 25%;
		border: 1px solid #E5E9F2;
		height: auto;
		float: left;
		overflow: hidden;
		padding: 2px;
		margin: 2px;
		background-color: #fff;
		cursor: pointer;
	}
	.cookList li span{
		display: block;
		float: left;
	}
	.foodImg{
		width: 40%;
	}
	.foodName {
	    font-size: 18px;
	    padding-left: 10px;
	    color: brown;
	}
	.foodPrice {
	    font-size: 16px;
	    padding-left: 10px;
	    padding-top: 10px;
	}
</style>