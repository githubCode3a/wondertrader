### 0.3.4
* 正式开源的第一个版本


### 0.3.5
* 把手数相关的都从整数改成浮点数，主要目的是为了以后兼容虚拟币交易(虚拟币交易数量都是小数单位)
* 优化手数改成浮点数以后带来的日志输出不简洁的问题(浮点数打印会显示很多“0000”)
* 逐步完善文档
* XTP实盘适配，主要是修复bug

### 0.3.6
* 执行器使用线程池，减少对网络线程的时间占用
* 增加了一个实盘仿真模块TraderMocker，可以满足目前已经支持的股票和期货的仿真交易
* 更多接口支持（飞马、tap、CTPMini）
* 内置执行算法增加TWAP
* 继续完善文档

### 0.4.0
* 新增一个**选股调度引擎**，用于调度应用层的选股策略，得到目标组合以后，提供自动执行服务，暂时只支持日级别以上的调度周期，执行会放到第二天
* 因为新增了选股调度引擎，所以全面重构`WtPorter`和`WtBtPorter`导出的接口函数，以便调用的时候区分
* 新增一个**独立的执行器模块**`WtExecMon`，并导出C接口提供服务。主要是剥离了策略引擎逻辑，提供单纯的执行服务，方便作为单纯的执行通道，嫁接到其他框架之下
* `Windows`下的开发环境从`vs2013`升级到`vs2017`，`boost1.72`和`curl`需要同步升级


### 0.5.0
* 股票数据新增前复权因子处理逻辑，直接从未复权数据转换成前复权数据
* `demos`目录下，将`python`的demo迁移到`wtpy`之下，然后新增一个`C++`版本demo的解决方案，提供了cta策略、高频策略、执行单元、选股策略四个demo，并提供了一个回测入口程序，用于本地调试
* 日志模块调整,主要是利用`fmtlib`优化了一些日志输出的细节
* 完成了高频策略引擎C接口导出

### 0.5.1
* 实盘引擎（CTA、HFT、SEL）在启动的时候，将策略列表和交易通道列表输出到一个配置文件中，方便监控服务读取查看
* 新增一个事件通知组件EventNotifier，主要作用是通过UDP通道，向指定的接受端发送成交回报、订单回报，以后还会扩展到其他盘中需要监控的数据
* 回测引擎，回测过程中产生的数据记录（成交、信号、平仓、资金）不在回测过程中写入文件，而是到了回测结束以后统一写入文件
* 完善了系统中合约代码标准化对股指期权IO的处理

### 0.5.2
* 修改CTPLoader为显式加载CTP模块，方便设置CTP模块的路径
* 将所有的通道对接模块（行情、交易）改成显示加载三方依赖库，并统一检查了版本的一致性
* 修正了股票Level2数据落地的一些细节问题
* 修正了风控触发时，限制手数比例为小数时，没有进行四舍五入导致的问题
* 历史数据新增了对Mysql数据库的支持，涉及到的模块包括数据读取模块WtDataReader、数据落地模块WtDataWriter、回测框架WtBtCore，Mysql库版本为6.11
* 修正了UDP广播模块中，中转的广播消息对象对WTSObject的处理的bug