<template>
    <div class = "authorization">
        <Loader :visible = "loaderVisible"/>
        <h1 class = "authorization__title"></h1>
        <form class = "authorization__form" method="post" @submit.prevent="getAuth">
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
                <img class="authorization__icon-success" alt="success" src="../assets/success.png">
            </span>
            <span v-else>
                <button class = "authorization__submit material-button-raised" type="submit">Sign in</button>
            </span>
        </form>
    </div>

</template>

<script>
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
			username: 'admin',
			pass: 'adMin235',
			isAuth: 0,
			isError: 0,
			loaderVisible: 0,
		};
	},
	methods: {
		getAuth: function() {
			this.loaderVisible = 1;
			axios
				.post('http://frankiesr.getsandbox.com/login', {
					username: this.username,
					pass: this.pass,
				})
				.then(response => {
					let promise = new Promise((resolve, reject) => {});
					if (response.data.Auth === 'Allow') {
						this.isAuth = 1;
						this.loaderVisible = 0;
						localStorage.username = this.username;
						setTimeout(() => {
							this.$router.push('/success');
						}, 700);
					} else {
						this.isAuth = 0;
						this.isError = 1;
						this.username = '';
						this.pass = '';
						this.loaderVisible = 0;
					}
				});
		},
		ifChangedInput: function() {
			this.isError === 1 ? (this.isError = false) : 1;
			console.log(2);
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
.authorization {
	&__title {
	}

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

	&__username {
	}

	&__password {
	}

	&__error-text {
		transition: 0.3s;
		color: red;
		position: relative;
		bottom: 15px;
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
		background-color: #2196f3;
		font-size: 14px;
		font-weight: 500;
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
	border: 1px solid red !important;
}

.empty {
	border: 1px solid red !important;
}
</style>
