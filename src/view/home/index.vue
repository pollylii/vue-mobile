<template>
  <div class="home">
    <div class="container" id="container">
	    <div class="page__hd">
            <img style="width:100%;height:100%" src="@/asserts/images/home_pg.png" />
	    </div>
		<div class="btn">
            <van-button type="warning" block to="goods" color="#FF5E00" height="50px;">立即体验</van-button>
        </div>
	</div>
  </div>
</template>

<script>
import { Button } from 'vant';

export default {
	components: {
		[Button.name]: Button,
    }
};
</script>

<style scoped>
	.page__hd {
		width: 100%;
        height: 100%;
	}
	.btn{
		position: fixed;
		width: 100%;
		bottom: 0px;
	}
</style>
