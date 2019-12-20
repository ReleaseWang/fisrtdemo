# fisrtdemo
English_and_coding_are_so_difficult_for_me



一个实现前后端分离的简单demo，主要介绍实现步骤。

前端主要使用vue，后端使用springboot。制作了一个简单的登陆界面。

核心思想是前端页面通过 ajax 调用后端的 restuful api 进行数据交互。

前端为8080端口，后端为8443端口。

主要参考文献：https://blog.csdn.net/Neuf_Soleil/article/details/88925013 系列文章  

  

安装Vue CLI
* 因为需要使用npm安装Vue CLI，而npm是集成在 Node.js中的，所以第一步需要安装Node.js，访问官网 https://nodejs.org/en/ ，首页即可下载。  
* 完成之后检测安装是否成功。minglinghang输入npm -g install npm，将npm更新至最新版本.  
* 之后，使用npm install -g vue-cli 安装脚手架。  

构建前端项目  
  可在命令行构建项目，也可在安装webstorm之后在其中创建新项目。

后端项目创建  
  安装IDEA，在其中新建项目

登录页面开发  
主要逻辑如下：
登录界面中，输入用户名和密码，点击登录按钮，即向后端 /login 接口发送数据，  
后端根据接收到的 用户名和密码 做出响应， 将相应发送回前端，前端对此作出反应。  
在后端中，若用户名和密码均正确，返回状态码200；若不正确，返回状态码400。前端根据返回的状态码不同做出对应的不同响应。  
步骤：  
* 前端页面开发：Login.vue    AppIndex.vue   
* 前端相关配置：  

设置反向代理：修改 src\main.js; 因为使用了新的模块 axios，所以需要进入到项目文件夹中，执行 npm install --save axios，以安装这个模块  

配置页面路由：修改 src\router\index.js  

跨域支持：为了让后端能够访问到前端的资源，需要配置跨域支持。在 config\index.js 中，找到 proxyTable 位置，修改  

* 可运行前端项目查看登陆页面效果
 
前端主要代码：
  
      methods: {
          login () {
            axios.post('http://localhost:8443/api/login', { //向后端该接口发送数据
              username: this.loginForm.username,    //用户名
              password: this.loginForm.password     //密码
            })
              .then(successResponse => {      //获取后端返回的状态码
                if (successResponse.data.code === 200) {    //若后端返回的状态码为200，即用户名和密码均正确
                  this.$router.replace({path: '/index'})    
                } else {                      //返回400，用户名或密码错误
                  this.$router.replace({path: '/shibai'})
                }
              })
              .catch(failResponse => {
              })
          }
        }


后端开发   
* 新建User类，接收前端发送的JS 对象  
* 新建Result 类，构造 response，主要是响应码。  
* LoginController, 处理响应  
* 找到 application.properties 文件配置端口，即加上 server.port=8443  
 后端主要代码：  
   
      public Result login(@RequestBody User requestUser) {
            // 对 html 标签进行转义，防止 XSS 攻击
            String username = requestUser.getUsername();
            username = HtmlUtils.htmlEscape(username);
            //若用户名或密码错误
            if (!Objects.equals("admin", username) || !Objects.equals("123456", requestUser.getPassword())) {
                String message = "账号密码错误";
                System.out.println("test");
                return new Result(400);
            } else {    //用户名和密码均正确
                return new Result(200);
            }
        }
  
测试项目   
* 同时运行前端和后端项目，访问 localhost:8080/#/login，输入用户名 admin，密码 123456.  
* 若正确会跳转到index页面，输出hello world  
* 若密码错误会跳转到shibai页面(由于个人水平不足，这个页面也没做好)     
  
  至此，该简易demo构建完成。这里只给出了大致思路，其详细步骤请查阅参考文献。   
  此demo仅作为课程作业使用，很多知识都是初学导致质量欠佳，请见谅。  

感想  
* 现在比较想说的还是用户名密码错误后的反馈问题，因为是第一次使用vue，而且springboot也没怎么用过，所以导致登陆失败后的界面及其简陋，希望日后能够提高。
* 难点主要在前后端的数据交互，卡了很久最后还是莫名其妙解决了。
* 本人目前对其中的很多原理不甚了解，这也是日后需要学习的重点。


* 最后感谢参考文献的作者Evan-Nightly，这一系列文档给了我很大的帮助。
* 附上Evan-Nightly的该项目的github地址：https://github.com/Antabot/White-Jotter
