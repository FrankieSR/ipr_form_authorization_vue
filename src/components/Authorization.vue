<template>
    <div class = "authorization">

        <Loader :visible = "loaderVisible"/>

        <form class = "authorization__form"
              method="post"
              @submit.prevent="getAuth">

            <div class="authorization__input-group">

              <span class="authorization__label">login</span>
              <input class = "authorization__username"
                     :class = "{error: errorWatch}"
                     v-model="username"
                     v-on:change = "ifChangedInput" />
            </div>
            <div class="authorization__input-group">

              <span class="authorization__label">password</span>
              <input class = "authorization__password"
                     type="password"
                     :class = "{error: errorWatch}"
                     v-model="pass" />
            </div>
            <div class="authorization__error-text"
                 v-show = "errorWatch">
                    incorrect login or password
            </div>
            <span v-if="isAuth">
               <svg id="successAnimation" class="animated" xmlns="http://www.w3.org/2000/svg" width="70" height="70" viewBox="0 0 70 70">
                    <path id="successAnimationResult" fill="#D8D8D8" d="M35,60 C21.1928813,60 10,48.8071187 10,35 C10,21.1928813 21.1928813,10 35,10 C48.8071187,10 60,21.1928813 60,35 C60,48.8071187 48.8071187,60 35,60 Z M23.6332378,33.2260427 L22.3667622,34.7739573 L34.1433655,44.40936 L47.776114,27.6305926 L46.223886,26.3694074 L33.8566345,41.59064 L23.6332378,33.2260427 Z"/>
                    <circle id="successAnimationCircle" cx="35" cy="35" r="24" stroke="#979797" stroke-width="2" stroke-linecap="round" fill="transparent"/>
                    <polyline id="successAnimationCheck" stroke="#979797" stroke-width="2" points="23 34 34 43 47 27" fill="transparent"/>
                </svg>
            </span>
            <span v-else>
                <button class = "authorization__submit material-button-raised" type="submit">Sign in</button>
            </span>
        </form>
    </div>

</template>

<script>
let empty = '';

import axios from 'axios';
import Vue from 'vue';
import router from 'vue-router';
import Loader from '@/components/Loader.vue';

Vue.use(router);
export default {
	name: 'Authorization',
	props: {},
	data: function() {
		return {
			username: empty, // user name
			pass: empty, // user pass
			isAuth: 0, // если авторизация не верная
			isError: 0, // если данные авторизации не верные
			loaderVisible: 0, // отображение лоадера
		};
	},
	methods: {
		getAuth: async function() {
			let _response, _promise;
			this.isError = 0;
			this.loaderVisible = 1;
			_response = await axios.post('http://frankiesr.getsandbox.com/login', {
				username: this.username,
				pass: this.pass,
			});

			_promise = new Promise((resolve, reject) => {
				_response.data.Auth === 'Allow' ? resolve('success') : reject('failure');
			});

			_promise
				.then(
					success => {
						this.isAuth = 1;
						this.loaderVisible = 0;
						localStorage.username = this.username;
						this.routeToPage();
					},
					failure => {
						this.isAuth = 0;
						this.isError = 1;
						this.loaderVisible = 0;
						this.clearUsersData();
					}
				)
				.catch(erorr => {
					console.log(error);
				});

			// !! можно так же описать через if else, но запись не так читаема.

			// if (response.data.Auth === 'Allow') {
			// 	this.isAuth = 1;
			// 	this.loaderVisible = 0;
			// 	localStorage.username = this.username;
			// 	this.routeToPage();
			// } else {
			// 	this.isAuth = 0;
			// 	this.isError = 1;
			// 	this.loaderVisible = 0;
			// 	this.clearUsersData();
			// }
		},
		ifChangedInput: function() {
			this.isError === 1 ? (this.isError = false) : 1;
		},
		clearUsersData: function() {
			this.username = empty;
			this.pass = empty;
			localStorage.username = empty;
		},
		routeToPage: function(pageName = '/success') {
			setTimeout(() => {
				this.$router.push(pageName);
			}, 800);
		},
	},
	computed: {
		authWatch: function() {
			return this.isAuth;
		},
		errorWatch: function() {
			return this.isError;
		},
	},
	components: {
		Loader,
	},
};
</script>

<style lang="scss">
$circle-length: 151px;
$check-length: 36px;

@keyframes scaleAnimation {
	0% {
		opacity: 0;
		transform: scale(1.5);
	}
	100% {
		opacity: 1;
		transform: scale(1);
	}
}

@keyframes drawCircle {
	0% {
		stroke-dashoffset: $circle-length;
	}
	100% {
		stroke-dashoffset: 0;
	}
}

@keyframes drawCheck {
	0% {
		stroke-dashoffset: $check-length;
	}
	100% {
		stroke-dashoffset: 0;
	}
}

@keyframes fadeOut {
	0% {
		opacity: 1;
	}
	100% {
		opacity: 0;
	}
}

@keyframes fadeIn {
	0% {
		opacity: 0;
	}
	100% {
		opacity: 1;
	}
}

#successAnimationCircle {
	stroke-dasharray: $circle-length $circle-length;
	stroke: #41b883;
}

#successAnimationCheck {
	stroke-dasharray: $check-length $check-length;
	stroke: #41b883;
}

#successAnimationResult {
	fill: #41b883;
	opacity: 0;
}

#successAnimation.animated {
	animation: 0.6s ease-out 0s 1 both scaleAnimation;

	#successAnimationCircle {
		animation: 0.6s cubic-bezier(0.77, 0, 0.175, 1) 0s 1 both drawCircle, 0.3s linear 0.9s 1 both fadeOut;
	}

	#successAnimationCheck {
		animation: 0.6s cubic-bezier(0.77, 0, 0.175, 1) 0s 1 both drawCheck, 0.3s linear 0.9s 1 both fadeOut;
	}

	#successAnimationResult {
		animation: 0.3s linear 0.9s both fadeIn;
	}
}

.authorization {
	&__form {
		max-width: 600px;
		margin: 0 auto;
	}

	&__input-group {
		position: relative;
		margin: 0 auto;
	}

	&__username,
	&__password {
		min-width: 80%;
		margin: 15px 0;
		height: 40px;
		border: none;
		border-radius: 18px;
		padding: 0 10px 0 20px;
		transition: 0.3s;

		&:focus {
			transform: translateY(-4px);
			box-shadow: 0px 2px 4px -1px rgba(0, 0, 0, 0.2), 0px 4px 5px 0px rgba(0, 0, 0, 0.14),
				0px 1px 10px 0px rgba(0, 0, 0, 0.12);
			border: none;
			outline: none;
		}

		&:hover {
			box-shadow: 0px 2px 4px -1px rgba(0, 0, 0, 0.2), 0px 4px 5px 0px rgba(0, 0, 0, 0.14),
				0px 1px 10px 0px rgba(0, 0, 0, 0.12);
		}
	}

	&__label {
		position: absolute;
		top: -5px;
	}

	&__error-text {
		transition: 0.3s;
		color: #f44336;
		position: relative;
		bottom: 15px;
		font-size: 14px;
	}

	&__submit {
		position: relative;
		display: inline-block;
		box-sizing: border-box;
		margin: 0 8px;
		border: none;
		border-radius: 18px;
		padding: 0 16px;
		min-width: 64px;
		height: 36px;
		vertical-align: middle;
		text-align: center;
		text-overflow: ellipsis;
		text-transform: uppercase;
		color: #fff;
		background-color: #41b883;
		font-size: 14px;
		line-height: 36px;
		overflow: hidden;
		transition: 0.5px;
		outline: none;
		cursor: pointer;
		transition: box-shadow 0.2s;

		&:hover,
		&:focus {
			box-shadow: 0px 2px 4px -1px rgba(0, 0, 0, 0.2), 0px 4px 5px 0px rgba(0, 0, 0, 0.14),
				0px 1px 10px 0px rgba(0, 0, 0, 0.12);
		}

		&:active {
			box-shadow: 0px 5px 5px -3px rgba(0, 0, 0, 0.2), 0px 8px 10px 1px rgba(0, 0, 0, 0.14),
				0px 3px 14px 2px rgba(0, 0, 0, 0.12);
		}

		&:disabled {
			color: rgba(0, 0, 0, 0.38);
			background-color: rgba(0, 0, 0, 0.12);
			box-shadow: none;
			cursor: initial;
		}

		&:before {
			content: '';
			position: absolute;
			left: 0;
			right: 0;
			top: 0;
			bottom: 0;
			background-color: currentColor;
			opacity: 0;
			transition: opacity 0.2s;
		}

		&:after {
			content: '';
			position: absolute;
			left: 50%;
			top: 18px;
			border-radius: 50%;
			padding: 50%;
			width: 32px;
			height: 32px;
			background-color: currentColor;
			opacity: 0;
			transform: translate(-50%, -50%) scale(1);
			transition: opacity 1s, transform 0.5s;
		}

		&:active::after {
			opacity: 0.4;
			transform: translate(-50%, -50%) scale(0);
			transition: transform 0s;
		}

		&:disabled::after {
			opacity: 0;
		}

		&:hover::before {
			opacity: 0.12;
		}

		&:active::before {
			opacity: 0.32;
		}

		&:focus::before {
			opacity: 0.2;
		}

		&:disabled::before {
			opacity: 0;
		}
	}

	&__icon-success {
		height: 50px;
		width: 50px;
	}
}

.error {
	transition: 0.2s;
	box-sizing: content-box;
	border: 1px solid #f44336 !important;
	animation: errorMove 0.25s ease-in-out;
}

.empty {
	border: 1px solid #f44336 !important;
}

@keyframes errorMove {
	25% {
		transform: translateX(-12px);
	}
	50% {
		transform: translateX(12px);
	}
}
</style>
