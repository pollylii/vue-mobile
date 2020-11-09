<template>
	<div class="info">
		<div class="container" id="container">
			<div class="page__hd">
				<img style="width:100%;height:100%" src="../../../src/asserts/images/ads.png" />
			</div>
		</div>
	</div>
</template>

<script>
import { Step, Steps, Button, Row, Col } from 'vant';
import InputData from '@/components/InputData';
import Products from '@/components/Products';
import ProductApply from '@/components/ProductApply';
import Feedback from '@/components/Feedback';

export default {
	components: {
		[Step.name]: Step,
		[Steps.name]: Steps,
		[Button.name]: Button,
		[Row.name]: Row,
		[Col.name]: Col,
		InputData,
		Products,
		ProductApply,
		Feedback
	},
	data() {
		return {
			list: [
				'InputData',
				'Products',
				'ProductApply',
				'Feedback'
			],
			currentView: 'InputData',
			buttonTip: "下一步",
			active: 0
		};
	},
	methods: {
		nextStep() {
			this.active = ++this.active;
			if (this.active <= 3)
				this.currentView = this.list[this.active];
			else
				this.$router.push("/search");

			if (this.active >= 3) {
				this.buttonTip = "查询进度";
			}
		}
	}
};

</script>

<style scoped>
.page__hd {
	width: 100%;
	height: 100%;
	position:absolute;
	z-index:-1;
}
</style>
