# 2020年最新最实用的android-MVVM框架


<img src="https://img-blog.csdnimg.cn/20200421193001875.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1ZjU3Ryb25n,size_16,color_FFFFFF,t_70" width="300" align=center /> <img src="https://img-blog.csdnimg.cn/20200421193130501.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1ZjU3Ryb25n,size_16,color_FFFFFF,t_70" width="300" align=center />
<img src="https://img-blog.csdnimg.cn/20200421193154897.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1ZjU3Ryb25n,size_16,color_FFFFFF,t_70" width="300" align=center /> <img src="https://img-blog.csdnimg.cn/20200421193208122.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1ZjU3Ryb25n,size_16,color_FFFFFF,t_70" width="300" align=center />
<br/>
<br/>
java版mvp参见：https://github.com/VcStrong/RxRetrofitMVPDemo.git<br/>
kotlin版mvp参见：https://github.com/VcStrong/KotlinMVPDemo.git<br/>

> 注册的密码规则是数字加字母超过8位即可
> 测试账号：13126965106 密码：111111aa

## 1.mvvm-v1 2020.04.20
这是一个整合架构，所有功能开发都只能在一个module中

### 1.1 业务功能包含以下：
- 登录注册（跳转主页后关闭，已登录用户可直接进入主页）;
- 仿微信朋友圈，Recyclerview嵌套RecyclerView实现多图布局列表切换（一张横向全屏，二张横向全屏，三张横向全屏，四张分两行且横向全屏）；
- 发表朋友圈回传值刷新（viewmodel中livedata妙用）；
- 多图带参数上传；
- 首页和设置页分别退出登录（intent的flag使用）；

### 1.2 本架构技术功能包含以下：
- ViewModel在xml页面变量同一命名；
- Adapter中item的xml如果经常复用但model数据不一致的情况，切勿盲目binding设置，切勿单向页面绑定，请在adapter中手动控件赋值，不要xml-binding；
- Adapter中复杂控件处理需要大对象，建议灵活使用binding寄存，参见CircleAdapter类
- ObjectBox快速实现对象存取
- LiveData对网络请求数据灵活设置，view层基于观察者模式填充
- viewbinding中onClick，onCheckedChanged等事件使用，其他事件根据Listenter实现方法的名字举一反三

### 1.3 框架包含以下
- <a href="https://developer.android.google.cn/jetpack/androidx">androidx</a>：这个系列的jar包和appcompat.support对立的，参见谷歌官方文档
- <a href="https://developer.android.google.cn/jetpack">lifecycle-viewmodel+livedata+DataBinding</a>：
生命周期管理，完全解耦，方便系统内存管理释放，基于观察者模式实现数据更新等等
- <a href="https://github.com/square/retrofit.git">Retrofit2</a>：感谢Square公司
- <a href="https://github.com/ReactiveX/RxJava.git">rxjava2</a>：感谢ReactiveX，突然发现Rxjava已经发布3.0了
- <a href="https://github.com/objectbox/objectbox-java.git">ObjectBox</a>：android上运行速度最快的数据库，基于c/c++开发，native接口
- <a href="https://github.com/youth5201314/banner.git">banner</a>：banner如果不需要就去掉。
- <a href="https://github.com/facebook/fresco.git">Fresco</a>：感谢FaceBook，另外给大家提个醒，
Fresco有自己的内存回收机制，但是这个回收阈值没有设置，请自行百度解决，提示：Fresco eviction哈哈哈
- <a href="https://github.com/XRecyclerView/XRecyclerView.git">XRecyclerView</a>
- <a href="https://github.com/tbruyelle/RxPermissions.git">RxPermissions</a>：权限申请比较好用
- <a href="https://github.com/alibaba/ARouter.git">Arouter</a>：感谢阿里巴巴技术团队
- <a href="https://github.com/bytedance/BoostMultiDex.git">BoostMultiDex</a>：感谢头条技术团队抖音多dex加载方案


## 2.mvvm-v2 质量的提高来自不断地追求
v2版本在v1基础上进行组件化升级，由于对组件和模块的概念有了更深的了解，参考了网上的组件化教程，实践总结利弊之后，决定自己写一套优秀高效率的组件运行gradle：
- 公司场景：多模块业务联调，统一运行；
- 此demo种组件化打包好处：根据gradle配置动态改变模块的引入，分分钟能解决一个模块或者多个模块打包联调；
- 具体方式如下：
    1. 项目根目录新建了config.gradle存放系统变量；
    2. 项目根目录新建了module.gradle存放业务module中build.gradle公用参数，common和app不建议引入(部分重要配置必须写在这两个module中);
    3. 项目根目录build.gradle使用groovy动态改变app（module）对模块的引入；
    4. 所有选中的模块可根据自己要求，决定是否需要改变AndroidManifest.xml的引入，仿照open_main模块中的sourceSets；
    5. 支持多个Module—Application共存，方便处理推送，IM等组件初始化问题
> 注：请认真查看config.gradle中的变量备注

## 3.mvvm-v3 追风中。
v3版本绝对让你眼前一新，重新提起兴致，追求适配到4.x，由于ObjectBox框架最低支持4.0.3，所以本项目最低只能支持4.0.3版本机型，
新增功能处处都能体现代码的奇妙：
- 重新定义config.gradle中常量：分为SDK_VERSION（不因发布分支改变的常量）和active（跟分支相关的参数）。
这么做出于对项目在不同阶段不同部门的打包的时候，部分参数需要进行调整，例如：推送key，包名，域名，项目名等；
主要目的提升研发-测试-运维运营等部门沟通协作；
- 新增今日头条-<a href="https://github.com/bytedance/BoostMultiDex.git">抖音团队multidex打包</a>，适配4.x平台加载dex问题，详细请参照WDApplication代码；
- 拆分请求接口，放到各个相关module中，开发阶段尽量减少多人操作common包。

### mvvm-v3.1
- 对WDViewModel进行改动，配合WDFragViewModel完成Fragment组件化，你可以仿照open_user模块，完成自己
的Fragment组件开发。
- 组合设计模式（Component），通过对子节点初始化赋值，利用MutableLiveData达到共享数据的目的，建议使用
Message对象，减少代码量，增加功能最大适配性。
 


> @TODO 新想法（我有时间了在继续开发，这阵子先不忙这一块了，v3版本已经很丰满了）：Arouter使用的Path从common包的Constant中拿出来，每个项目单独配置（初步想法是通过插件搞定，难点是每个组件的的常量取出来然后合并）
