### front-end:

(0) cd front-end

(1) npm install

(2) npm run dev 

### back-end:

(0) cd back-end

(1) pip install -r requirements.txt

(2) if no site.db in /moneyapp, run python db_create.py

(3) python run.py

### 前后端数据传输：
front_end/config/index.js
```javascript
module.exports = {
  dev: {

    // Paths
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {                 // proxyTable配置接口地址代理,会直接post到localhost:5000 -> 后端
        '/users': {    
            target: 'http://localhost:5000',
            changeOrigin: true
        }
    },
    //...
}
```

front_end/src/components

```javascript
submit: function () {
	var data = new FormData();   // 使用表单提交
    data.append('username', this.username)
    data.append('password', this.password)
    data.append('telephone', this.phone)
    console.log(this.phone)
    data.append('email', this.email)
    if(document.getElementById('file').files[0]){     // 选择作为头像的图片
    	data.append('file', document.getElementById('file').files[0]);
    }
    else {
    	alert('emm')

    }
                    

    axios.post('/users/register', data).then((res) => {
        console.log(res.data)
        console.log(res.data.result.email);
        var path = "http://localhost:5000/static/profile_pics/" + res.data.result.image_file
        console.log(path)
        this.$router.push({
        	path: '/', 
            name: 'mainpage',
            params: { 
            	username: res.data.result.username,
                email: res.data.result.email,
                phone: res.data.result.telephone,
                src: path     // 图片路径
            }
        });

	}).catch(e => {
		console.error(e)
	}) 
},
```

back_end/moneyapp/routes.py

```python
@app.route('/users/register', methods=['POST', 'GET'])
def uploadFile():
    if request.method == 'POST':
        username = request.form['username']
        email = request.form['email']
        password = request.form['password']
        telephone = request.form['telephone']
        hashed_password = bcrypt.generate_password_hash(password).decode('utf-8')
       

        if request.files and request.files['file'] :   # 头像图片
            file = request.files['file']
            filename = secure_filename(file.filename)

            # Gen GUUID File Name
            fileExt = filename.split('.')[1]
            autoGenFileName = uuid.uuid4()

            newFileName = str(autoGenFileName) + '.' + fileExt

            target = UPLOAD_FOLDER
            print(target)

            if not os.path.isdir(target):
                os.mkdir(target)
            file.save(os.path.join(app.config['UPLOAD_FOLDER'], newFileName))    # 储存到后端 back_end/moneyapp/stiatic/profile_pics

        else:  # 如果没有选择文件则使用默认的图片作为头像
            filename = 'default.jpg'
            newFileName = 'default.jpg'

        # 加入到数据库
        addUser(username, email, hashed_password, telephone, newFileName)

        result = {
            'username': username,
            'email': email,
            'password': password,
            'telephone': telephone,
            'image_file': newFileName
        }

        return jsonify({'result': result})
```

现在完成了一个简单的只可以注册的demo

（1）在前端后端都运行的情况下访问localhost:8080

（2）点击注册，跳转到注册页面，然后随便填信息（信息要填），然后图片可选可不选

（3）之后点提交，就会跳回首页，上面有个人信息和头像

（4）部署了一个demo在服务器上 -》 访问http://95.179.171.246:8080/ （十分卡= =）
