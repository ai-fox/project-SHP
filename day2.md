1:编程式路由跳转到当前路由(参数不变)，多次执行会抛出NavigationDuplicated的警告错误？
--路由跳转有两种形式：声明式导航、编程式导航
--声明式导购没有这类问题的，因为vue-router底层已经处理好了
1.1.为什么编程式导航进行路由跳转的适合，就有这种警告错误呢？
“vue-router”：“3.5.3”：最新的vue-router引入promise
1.2.通过给push方法传递相对应的成功、失败的回调函数，可以捕获到当前错误，可以解决。

1.3.通过底部的代码，可以实现解决错误
this.$router.push({name:"search",params:{keyword:this.keyword},query:{k:this.keyword.toUpperCase()}},()=>{},()=>{});
这种写法：治标不治本，将来在别的组件当中push|replace：编程式导航还是有类似错误。

1.4.
this：当前组件实例（search）
this.$router属性：当前的这个属性，属性值VueRouter类的一个实例，当再入口文件注册路由的时候，给组件实例添加$router|$route属性
           //这个地方给大家说一下，这个push不叫vuerouter的实例，实例是指实例化对象的过程，push只是对象的一个方法
push：VueRouter类的一个对象

function VueRouter(){

}
//原型对象的方法
VueRouter.prototype.push = function(){
    //函数的上下文为VueRouter类的一个实例
}

let $router = new VueRouter();

$router.push(xxx);

this.$router.push();

因为router是vuerouter所创建的实例 所以它身上可以通过原型链找到 vuerouter身上的属实方法


2：Home模块组件拆分
--先把静态页面完成
--拆分出静态组件
--获取服务器的数据进行展示
--动态业务


3：三级联动组件完成
---由于三级联动，在Home、Search、Detail，把三级联动注册为全局组件
好处：只需要注册一次，就可以在项目任意地方使用

4:完成其余静态组件
HTML + CSS + 图片资源


5:POSTMAN接口测试
--刚刚经过postman工具测试，接口是没问题的



6：axios二次封装
XMLHttpRequest、fetch、JQ、axios
6.1 为什么需要进行二次封装aios？
请求拦截器、响应拦截器：请求拦截器，可以在发请求之前可以处理一些业务、响应拦截器，当服务器数据返回之后，可以处理一些事情
        安装axios命令：cnpm --save -axios

6.2.在项目当中经常API文件夹【axios】
接口当中：路径都带有“/api

6.3.有的同学axios基础不好，可以参考git|NPM关于axios文档



7：接口统一管理

项目很小：完全可以在组件的生命周期函数中发请求

项目大：若是每个请求都直接在自己的组件内写和发送，那么我们后期在需要修改他们的时候，就要
        一个一个去找他在那里，比较不好维护

7.1.跨域问题
什么是跨域：协议、域名、端口号不同请求，称之为跨域
http://localhost:8080/#/home  ---前端项目本地服务器
http://39.98.123.211          ---后台服务器

JSONP、CROS、代理


8:nprogress进度条的使用
        //安装命令：cnpm install --save nprogress

start:进度条开始
done：进度条结束
可以在nprogress/nprogress.css文件中修改样式

9：Vuex状态管理库
         //安装命令：cnpm install --save vuex
9.1：Vuex是什么？
Vuex是官方提供一个插件，状态管理库，集中式管理项目中组件共用的数据。
切记，并不是全部项目都需要Vuex，如果项目很小，完全不需要Vuex，如果项目很大，组件很多，数据很多，数据维护很费劲，Vuex
state（状态，状况）：1.存放需要共享的状态信息，使用时通过 $store.state.counter 即可拿到状态信息。
                    2.单一状态树即单一数据源，在一个项目中只使用一个store对象，来存储所有共享的状态信息。
mutations（突变，变化，转变）：定义一些方法    处理异步操作时，能够引起页面的响应式变化
actions（行动，动作）：如果确实需要进行一些异步操作，比如网络请求，建议在 Actions 中进行处理，这样 devtools 就能够进行跟踪，由 Actions 处理异步操作，具体的函数部分仍交由 Mutations 进行处理。
getters（获得者）：类似于计算属性，在数据展示前进行一些变化处理，具有缓存功能，能够提高运行效率

简单理解：①Mutation：用于变更Store中的数据   注意：只能通过mutation变更Store数据，不可以直接操作Store中的数据
        ②Action：用于处理异步任务   通过异步操作变更数据，必须通过Action，而不能通过Mutation，但是在Action中还是要通过触发Mutation的方式简介变更数据

需要手动传参数时，可以在 getters 中返回一个 function：eg
    moreAgeStu(state){
      return function(age){
        return state.students.filter(s => s.age > age)
      }
    }

9.2.vuex基本使用
    store/index.js文件
        //state：仓库存储数据的地方
        const state = {
        count:1
        };
        //mutations：修改state的唯一手段
        const mutations = {
        ADD(state){
        state.count++;
        }
        };
        //action：处理action，可以书写自己的业务逻辑，也可以处理异步
        const actions = {
        ADD(state){
        state.count++;
        }
        };
        //getters：理解为计算属性，用于简化仓库数据，让组件获取仓库的数据更加方便
        const getters = {};

9.3:vuex实现模块式开发
*****所以说,以后 一个组件对应一个数据仓库,一个api文档
*****如果项目过大，组件过多，接口也很多，数据也很多,可以让Vuex实现模块式开发

每个模块都有自己的state...，然后每个模块写成一个文件，最后index中引入其他的模块进行暴露

模拟state存储数据
{
    count:1,
    search:{a:1},
    detail:{sss},
    pay:{}
}

9.4:用处：
一般情况下，我们会在 Vuex 中存放一些需要在多个界面中进行共享的信息。比如用户的登录状态、用户名称、头像、地理位置信息、商品的收藏、购物车中的物品等，这些状态信息，我们可以放在统一的地方，对它进行保存和管理。

10:完成TypeNav三级联动展示数据业务
[
   {
       id:1,
       name:'电子书',
       child:[
               {id:2,name:'喜羊羊},
               {id:2,name:'喜羊羊}
       ]
   },
   {},
   {}     
]