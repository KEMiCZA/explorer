<style>
	.vue-home .container {
		background-color: #f7f7f7;
	}

	.vue-home .mt16 {
		margin-top: 16px;
	}

	.vue-home .list {
		height: 725px;
		overflow: auto;
	}

	.vue-home .banner_left .list_tab {
		width: 100%;
		height: 60px;
		border-bottom: 3px solid #f7f7f7;
		background-color: #fff;
		position: relative;
	}

	.vue-home .banner_left .list_tab .img {
		position: absolute;
		top: 33%;
		left: 2%;
		font-size: 16px;
	}

	.vue-home .banner_left .list_tab .btn {
		position: absolute;
		right: 2%;
		top: 20%;
	}

	.vue-home .li {
		align-items: center;
		background-color: #fff;
		border-bottom: 4px solid #f7f7f7;
		display: flex;
		height: 120px;
	}

	.vue-home li .monospace {
		display: block;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	.vue-home .blocks li .monospace {
		width: 350px;
	}

	.vue-home .txs li .monospace {
		width: 400px;
	}

	.vue-home .blocks li>.img {
		background: url(/static/img/block_bg.jpg) center no-repeat;
		flex-shrink: 0;
		margin-right: 20px;
		height: 90px;
		width: 160px;
	}

	.vue-home .blocks li>.img>* {
		color: white;
		display: block;
		text-align: center;
	}

	.vue-home .tab-right {
		height: 60px;
		border-bottom: 3px solid #f7f7f7;
		background-color: #fff;
		position: relative;
	}

	.vue-home .tab-right .img {
		position: absolute;
		top: 33%;
		left: 2%;
		font-size: 16px;
	}

	.vue-home .tab-right .btn {
		position: absolute;
		right: 2%;
		top: 20%;
	}

	.vue-home .txs li {
		/* 右侧列表 状态不同 左边框有不同的状态颜色, 目前不知道啥状态所以是按顺序给颜色 */
		border-left: 2px solid;
	}

	.vue-home .txs li>img {
		flex-shrink: 0;
		margin: 0 20px;
	}

	.vue-home .txs li tr>td:first-child {
		height: 28px;
		width: 40px;
	}

	.vue-home .txs li:nth-of-type(4n + 1) {
		border-left-color: #3cba54;
	}

	.vue-home .txs li:nth-of-type(4n + 2) {
		border-left-color: #f4c20d;
	}

	.vue-home .txs li:nth-of-type(4n + 3) {
		border-left-color: #db3236;
	}

	.vue-home .txs li:nth-of-type(4n + 4) {
		border-left-color: #4885ed;
	}

	/*
		chart
		*/

	.vue-home #chart {
		height: 300px;
		max-width: 800px;
	}

	.vue-home .chart_banner .name,
	.value {
		color: black;
		border-bottom: 1px inset;
		margin-left: 12px;
		margin-right: 12px;
	}

	.vue-home .chart_banner .name {
		font-size: 30px;
	}

	.vue-home .chart_banner .value {
		font-size: 60px;
	}

	.vue-home .chart_banner .msg {
		font-size: 16px;
		margin-top: 24px;
	}

	.vue-home .chart_banner .msg div {
		margin-top: 10px;
	}

	.vue-home .chart_banner .msg .msg_change_right {
		float: right;
		margin-right: 10px;
		color: black;
	}

	.vue-home .chart_banner .msg .red {
		color: red;
	}

	.vue-home .chart_banner .msg .green {
		color: green;
	}

	.vue-home .chart_banner .msg .msg_change_left {
		margin-left: 10px;
		color: black;

	}

	.vue-home text.highcharts-credits {
		display: none;
	}

	.vue-home .updataTime {
		float: right;
	}
</style>
<template>
	<div class="container vue-home">
		<div class=top>
			<div class=row>
				<div class=col-md-8>
					<div id=chart></div>
				</div>
				<div class=col-md-4>
					<div class=chart_banner v-if=market>
						<div class=name>NAS</div>
						<div class=value>$ {{ market.price }}</div>
						<div class=msg>
							<div class=msg_change>
								<span class="msg_change_left homelocalizable" id="home24HourChange"></span>
								<span class="msg_change_right" v-bind:class="[ market.trends == 1 ? 'green' : 'red']">{{ market.trends == 1 ? '+' : '-' }} {{ market.change24h }}%</span>
							</div>
							<div class=msg_volume>
								<span class="msg_change_left homelocalizable" id="home24HourVolume"></span>
								<span class="msg_change_right">$ {{ numberAddComma(market.volume24h) }}</span>
							</div>
							<div class=msg_market>
								<span class="msg_change_left homelocalizable" id="homeMarketCapTitle"></span>
								<span class=msg_change_right>$ {{ numberAddComma(market.marketCap) }}</span>
							</div>
						</div>
						<div class="mt16 updataTime"><span class="homelocalizable" id="homeUpdateTimeTitle"></span><span class="homelocalizable" id="homeUpdateTimePrefix"></span> {{ timeConversion(Date.now() - market.createdAt) }}<span class="homelocalizable" id="homeUpdateTimeSuffix"></span></div>
					</div>
				</div>
			</div>
		</div>
		<div class=mt20>
			<div class=row>
				<div class="banner_left col-md-6">
					<div class=list_tab>
						<div class=img>
							<span class="fa fa-th-large" aria-hidden=true></span>
							<span class="homelocalizable" id="homeBlocksTitle"></span>
						</div>
						<router-link class="btn btn-default pull-right" v-bind:to='fragApi + "/blocks"' role=button><span class="homelocalizable" id="homeBlocksViewAll"></span></router-link>
					</div>
					<ul class="blocks list">
						<li class=li v-for="(o, i) in blocks" :key="i">
							<div class=img>
								<router-link class=mt20 v-bind:to='fragApi + "/block/" + o.height'><span class="homelocalizable" id="homeBlockText"></span> {{ o.height }}</router-link>
								<div class=mt20><span class="homelocalizable" id="homeUpdateTimePrefix2"></span>{{ timeConversion(msVmReady - o.timestamp) }}<span class="homelocalizable" id="homeUpdateTimeSuffix2"></span></div>
							</div>
							<div class=right>
								<span class="homelocalizable" id="homeMintedBy"></span>
								<router-link class=monospace v-bind:to='fragApi + "/address/" + o.miner.hash'>{{ o.miner.hash }}</router-link>
								<div class=mt16>
									<router-link v-bind:to='fragApi + "/txs?block=" + o.height'>
										<b>{{ o.txnCnt }}</b> <span class="homelocalizable" id="homeTransactions"></span></router-link>
								</div>
							</div>
						</li>
					</ul>
				</div>
				<div class=col-md-6>
					<div class=tab-right>
						<div class=img>
							<span class="fa fa-list" aria-hidden=true></span>
							<span class="homelocalizable" id="homeTransaction"></span>
						</div>
						<router-link class="btn btn-default pull-right" v-bind:to='fragApi + "/txs"' role=button><span class="homelocalizable" id="homeViewAll"></span></router-link>
					</div>
					<ul class="list txs">
						<li class=li v-for="(o, i) in txs" :key="i">
							<img src=/static/img/icon.png height=43 width=43 alt="">
							<div>
								<table>
									<tr>
										<td class="homelocalizable" id="homeTxNumber"></td>
										<td>
											<router-link class=monospace v-bind:to='fragApi + "/tx/"+ o.hash'>{{ o.hash }}</router-link>
										</td>
									</tr>
									<tr>
										<td class="homelocalizable" id="homeTxFrom"></td>
										<td>
											<router-link class=monospace v-bind:to='fragApi + "/address/" + o.from.hash'>{{ o.from.hash }}</router-link>
										</td>
									</tr>
									<tr>
										<td class="homelocalizable" id="homeTxTo"></td>
										<td>
											<router-link class=monospace v-bind:to='fragApi + "/address/" + o.to.hash'>{{ o.to.hash }}</router-link>
										</td>
									</tr>
									<tr>
										<td colspan=2> ><span class="homelocalizable" id="homeTimeStampPrefix"></span>{{timeConversion(msVmReady - o.timestamp)}}<span class="homelocalizable" id="homeTimeStampSuffix"></span></td>
									</tr>
								</table>
							</div>
						</li>
					</ul>
				</div>
			</div>
		</div>
	</div>
</template>
<script>
	import { EventBus } from '../events.js';
	import { jsonStrings } from '../l10nstrings.js';
	var api = require("@/assets/api"),
		utility = require("@/assets/utility");

	module.exports = {
		data() {
			return {
				blocks: [],
				chartConfig: {
					legend: {
						align: "right",
						layout: "vertical",
						verticalAlign: "middle"
					},
					// plotOptions: {
					//	 area: {
					//		 fillColor: {
					//			 linearGradient: {
					//				 x1: 0,
					//				 y1: 0,
					//				 x2: 0,
					//				 y2: 1
					//			 },
					//			 stops: [
					//				 [0, "#2233ee"],
					//				 [1, "#2266ee00"]
					//			 ]
					//		 },
					//		 marker: {
					//			 radius: 2
					//		 },
					//		 lineWidth: 1,
					//		 states: {
					//			 hover: {
					//				 lineWidth: 1
					//			 }
					//		 },
					//		 threshold: null
					//	 }
					// },
					series: [{
						data: null,
						name: "transactions"
						// , type: "area"
					}],
					subtitle: {
						text: "Data Source：Nebulas"
					},
					title: {
						text: "transactions"
					},
					xAxis: {
						labels: {
							format: "{value:%m-%d}",
							rotation: -30
						},
						type: "datetime"
					},
					yAxis: {
						title: {
							text: "Amount"
						}
					}
				},
				fragApi: this.$route.params.api ? "/" + this.$route.params.api : "",
				market: null,
				msVmReady: Date.now(),
				txs: []
			};
		},
		methods: {
			removeTempInterval() {
				clearInterval(this.tempInterval);
			},
			checkStaticTranslations() {
				// Unique elements, identified by id attr
				var myLocalizableElements = document.getElementsByClassName("homelocalizable");
				var totalElements = myLocalizableElements.length;
				var i;
				for (i = 0; i < totalElements; i++) {
					var elementId = myLocalizableElements[i].getAttribute("id");
					if (myLocalizableElements[i].getAttribute("localize")) {
						var elementAttribute = myLocalizableElements[i].getAttribute("localize");
						myLocalizableElements[i].setAttribute(elementAttribute, jsonStrings[this.$selectedLanguage][elementId]);
					}
					else {
						myLocalizableElements[i].innerText = jsonStrings[this.$selectedLanguage][elementId];
					}
				}
			},
			checkDynamicTranslations() {
				// Multiple elements, identified with name attr
				var myMultiLocalizableElements = document.getElementsByClassName("homemultilocalizable");
				var totalElements = myMultiLocalizableElements.length;
				var i;
				for (i = 0; i < totalElements; i++) {
					var elementName = myMultiLocalizableElements[i].getAttribute("name");
					if (myMultiLocalizableElements[i].getAttribute("localize")) {
						var elementAttribute = myMultiLocalizableElements[i].getAttribute("localize");
						myMultiLocalizableElements[i].setAttribute(elementAttribute, jsonStrings[this.$selectedLanguage][elementName]);
					}
					else {
						myMultiLocalizableElements[i].innerText = jsonStrings[this.$selectedLanguage][elementName];
					}
				}
			},
			numberAddComma(n) {
				return utility.numberAddComma(n);
			},
			timeConversion(ms) {
				// if a vue directive uses a vue method, this method will get called on any vue data's any change
				//
				// https://github.com/vuejs/vue/issues/5682
				// "In 2.x a component's entire render function is called when it is updated."
				return utility.timeConversion(ms);
			}
		},
		mounted() {
			EventBus.$on('changeLanguage', foo => {this.checkStaticTranslations()});
			if (typeof this.$selectedLanguage != 'undefined') {
				this.checkStaticTranslations();
			}
			this.translationsInterval = setInterval(() => {
				this.checkDynamicTranslations();
			}, 1000);
			this.tempInterval = setInterval(() => {
				this.checkStaticTranslations();
				this.removeTempInterval();
			}, 1500);
			api.getBlock({ type: "latest" }, o => this.blocks = o);
			api.getTx({ type: "latest" }, o => this.txs = o);
			api.getMarketCap(o => this.market = o);

			api.getTx("cnt_static", o => {
				var i, arr = [], div = document.querySelector("#chart");

				if (div) {
					for (i in o) arr.push([Date.parse(i), o[i]]);

					arr.sort(function (a, b) { return a[0] - b[0]; });

					// series 全部是 0 时没法计算纵坐标, highcharts 会把一条线居中显示. 如果想让线靠下就需要给 max 一个非零的值比如 1
					// this.chartConfig.yAxis.max = 1;

					this.chartConfig.series[0].data = arr;
					require("highcharts").chart(div, this.chartConfig);
				}
			});
		}
	};
</script>