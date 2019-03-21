<template>
	<div>
		<a v-on:click="loginOrMod" href="javascript:void(0);">{{username}}</a>
		<a v-on:click="registerOrOut" href="javascript:void(0);">{{status}}</a>
		<p>Username: {{username}}</p>
		<p>Email: {{email}}</p>
		<p>Phone: {{phone}}</p>
		<p>Avatar:</p>
		<img v-bind:src="src" v-if="isLogin" width="200" height="200"/>

	</div>
</template>

<script>
	export default {
		data() {
			return {
				 username:"",
				 isLogin:false,
				 status: "",
				 email:"",
				 phone: "",
				 src: "",
				 display:"none"
				
				 
			};
		},
		created: function () { 
			//在created阶段初始化
			this.getEventData();
		},
		methods: {
			getEventData:function() {
				let routerParamsUsername = this.$route.params.username;
				let routerParamsEmail = this.$route.params.email;
				let routerParamsPhone = this.$route.params.phone;
				let routerParamsImgsrc = this.$route.params.src

				if(routerParamsUsername == null){
					this.username = "登录";
					this.status = "注册";
					this.isLogin = false;
					//可以对用户的token进行检验是否超出登出时间
				}else{
					this.username = routerParamsUsername;
					this.isLogin = true;
					this.status = "退出登录";
					this.email = routerParamsEmail;
					this.phone = routerParamsPhone;
					this.src = routerParamsImgsrc
				}
			},
			loginOrMod: function () {
				if(this.isLogin){							
					this.$router.push({
							path: '/userinfomodify', 
							name: 'userinfomodify',
							params: { 
									username: this.username
							}
					});
				}
				else{
					this.$router.push({
							path: '/userlogin'
					});
				}
			},
			registerOrOut: function () {
				if(this.isLogin){	
					this.isLogin = false;
					this.username = "登录";
					this.status = "注册";
					//退出登录，将用户token失效
				}else{
					//注册
					this.$router.push({
						path: '/userregister'
					});
				}
			}
		}
	}
</script>

<style>
</style>