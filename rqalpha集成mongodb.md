# 目的

rqalpha作为一个开源平台，提供了量化分析很多便利：精准的复权交易数据（日级别）、事件驱动的回测框架等。
使用rqalpha的分析功能，将分析结果、配置条件等存储在mongodb，以便进一步进行分析。

一个典型的应用场景：

使用rqalpha进行选股，将符合条件的股票放入到mongodb中。然后进行追踪，例如何时产生交易信号、模拟买入、模拟卖出等。这样可以统计选股策略的准确程度。基于这些数据，可以建立一个股票投资辅助系统。

# 环境设置

## 安装Mongodb

这里不展开。可以参考官方文档。

## 安装pymongo

pymongo是mongodb官方推荐的python驱动。在conda虚拟环境中运行:

```pip install pymongo```

# 程序示例

```
# run_func_demo
from rqalpha.api import *
from rqalpha import run_func
#引入pymongo依赖
from pymongo import MongoClient
#初始化连接，并指向signal文档
client = MongoClient()
signals = client.rqtrading.signal

def init(context):
    logger.info("init")
    context.s1 = "000001.XSHE"
    update_universe(context.s1)
    context.fired = False

def before_trading(context):
    pass

def handle_bar(context, bar_dict):
    if not context.fired:
        # order_percent并且传入1代表买入该股票并且使其占有投资组合的100%
        order_percent(context.s1, 1)
        context.fired = True
        # 当发出买入信号时，往数据库中插入一条记录
        signals.insert_one({"security": context.s1, "action": 'buy'})

config = {
  "base": {
    "start_date": "2016-06-01",
    "end_date": "2016-12-01",
    "benchmark": "000300.XSHG",
    "accounts": {
        "stock": 100000
    }
  },
  "extra": {
    "log_level": "verbose",
  },
  "mod": {
    "sys_analyser": {
      "enabled": True,
      "plot": True
    }
  }
}

# 您可以指定您要传递的参数
run_func(init=init, before_trading=before_trading, handle_bar=handle_bar, config=config)

# 如果你的函数命名是按照 API 规范来，则可以直接按照以下方式来运行
# run_func(**globals())
```

# 参考资料

1. pymongo文档：https://api.mongodb.com/python/current/index.html

