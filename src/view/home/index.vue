<template>
  <div class="home">
    <div class="container" id="container">
	    <div class="page__hd">
            <img style="width:100%;height:100%" src="@/asserts/images/home_pg.png" />
            <div class="btn">
                <van-button type="warning" block to="goods" color="#FF5E00" height="50px;">立即体验</van-button>
            </div>
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
        position:absolute;
        z-index:-1
        /*background-image: url("../../asserts/images/home_pg.png");*/
	}
	.btn{
		position: fixed;
		width: 100%;
		bottom: 0px;
        font-size: 18px;
	}
</style>
