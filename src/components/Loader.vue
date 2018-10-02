<template>
<div>
    <div>
        <div class="loader" v-if="visible">
            <div class="lds-dual-ring"></div>
        </div>
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <defs>
    <filter id="goo">
      <feGaussianBlur in="SourceGraphic" stdDeviation="10" result="blur" />
      <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 18 -7" result="goo" />
      <feBlend in="SourceGraphic" in2="goo" />
  	</filter>
  </defs>
</svg>
        </div>
    </div>
</div>
</template>

<script>
export default {
	name: 'Loader',
	props: {
		visible: false,
	},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.loader {
	width: 100%;
	height: 100vh;
	background: rgba(200, 200, 200, 0.7);
	position: absolute;
	left: 0;
	top: 0;
	display: flex;
	align-items: center;
	justify-content: center;
}

.lds-dual-ring {
	display: inline-block;
	width: 64px;
	height: 64px;
}
.lds-dual-ring:after {
	content: ' ';
	display: block;
	width: 46px;
	height: 46px;
	margin: 1px;
	border-radius: 50%;
	border: 5px solid #fff;
	border-color: #fff transparent #fff transparent;
	animation: lds-dual-ring 1.2s linear infinite;
}
@keyframes lds-dual-ring {
	0% {
		transform: rotate(0deg);
	}
	100% {
		transform: rotate(360deg);
	}
}
</style>
