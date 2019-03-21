<template>
<body>
    <div class = "head" >
        <h2>登录</h2>
    </div>
    <div id="login">
        <p id="title">用户登录</p>
        <form action="" method="post">
            <div class="input">
                <p style="padding-left: 10px;">用户名 : <input v-model="username" type="text" name="user" id="_user"></p><span id="_usererr">用户名不存在</span>
                <p style="padding-left: 10px;">密码 : <input v-model="password" type="password" name="password" id="_id"></p><span id="_iderr">密码错误</span><br>
                <input v-on:click="reset" type="button" value="重置" id="_reset">
                <input v-on:click="submit" type="button" value="登录">
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
                password:""
            };
        },
        created: function () { 
            this.getEventData();
        },
        methods: {
            getEventData:function() {
                let routerParams = this.$route.params.username;
                this.username = routerParams;
            },
            submit: function () {
                //调用后端验证用户是否有效之后进行登录操作
                //然后跳转主页
                this.$router.push({
                    path: '/', 
                    name: 'mainpage',
                    params: { 
                        username: this.username
                    },
                });
            },
            reset: function () {
                this.$data.username = "";
                this.$data.password = "";
            }
        },
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
    height: 200px;
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
</style>