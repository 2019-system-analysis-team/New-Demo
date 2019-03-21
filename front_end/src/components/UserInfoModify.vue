<template>
<body>
	<div class = "head" >
		<h2>修改</h2>
	</div>
	<div id="login">
		<p id="title">修改信息</p>
        <form action="">
            <div class="input">
                <p style="padding-left: 10px;">用户名 : <input v-model="username" disabled="disabled" type="text" name="user" id="_user"></p>
                <p style="padding-left: 15px;">密码 : <input v-model="password" type="password" name="password" id="_id"></p>
                <p style="padding-left: 20px;">电话 : <input v-model="phone" type="text" name="phone" id="_phone"></p>
                <p style="padding-left: 25px;">邮箱 : <input v-model="email" type="text" name="email" id="_email"></p><br>
                <input v-on:click="cancel" type="button" value="取消" id="_reset">
                <input v-on:click="submit" type="button" value="提交">
            </div>
        </form>
	</div>
</body>
</template>

<script>
	export default {
		data() {
			return {
				username:"",
				password:"",
				phone:"",
				email:""
			};
		},
		created: function () { 
			this.getEventData();
		},
		methods:{
			submit: function () {
				//提交修改的用户信息到后端
				//如果修改信息成功则跳转到主页
				this.$router.push({
						path: '/', 
						name: 'mainpage',
						params: { 
							username: this.username
						},
				});
			},
			cancel: function () {
				//返回主页
				this.$router.push({
					path: '/', 
					name: 'mainpage',
					params: { 
						username: this.username
					},
				})
			},
			getEventData:function() {
				let routerParams = this.$route.params.username;
				this.username = routerParams;
				//是否在登录状态，如果没有在则不允许进入该页面
				//判断token是否有效
				//从后台获取用户的信息，前端不存储所有的用户信息
				
			},
		}
		
	}
</script>

<style scoped>
*{
    margin: 0;
    padding: 0;
}
h2{
    margin-top: 10%;
    margin-bottom: 20px;
}
body{
    text-align: center;
}

.head{
    font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
    font-size: 38px;
    line-height: 1.42857143;
    color:#757D82;
}
#title{
    margin-top: 10px;
    margin-bottom: 20px;
}
#_user{
    margin-right: 15px;
}
form span{
    margin-bottom: 20px;
    opacity: 0;
    font-size: 10px;
}
#_reset{
    margin-top: 10px;
    margin-left: 160px;
}
div>input {
    width: 60px;
    height:35px;
    background-color: #757D82;
    outline:none;
    border-radius: 15%;
    color:  #FFFFFF;
}
#login{
    margin: 0 auto;
    width: 300px;
    height: 300px;
    border: 5px solid #3A3F44;
    border-radius: 10px;
    background-color:#383D42;
}
span{
    color: red;
}
p input {
    opacity: 0.8;
}
.input{
    color: #B5B5B6;
}
.input>p{
	padding-bottom: 10px;
}
</style>
