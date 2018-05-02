# 为什么需要代理池
大数据时代来临，爬虫工作者的春天也随之来了。然而在我们进行爬虫业务时，却经常受到目标网站反爬虫机制的阻碍，尤其是分布式爬虫，因为采集信息量和采集速度过快，常常给对方服务器带来巨大负荷，不用猜也知道你是爬虫，怎么可能不被封。要想解决这种窘境，使用代理IP堪称一个捷径，当遇到IP被封，换个IP就可以继续访问。

# 代理池的设计
## 爬虫部分
用于在网络上获取免费的ip,功能分为二个部分，一个请求，一个解析.

## 数据库设计
分为两个部分  一个为数据库的基本操作 一个为添加数据
```
class MongoHelper(object):
    def __init__(self):
        self.client = pymongo.MongoClient(DB_CONFIG['DB_CONNECT_STRING'])
        self.db = self.client.proxy
        self.proxys = self.db.proxys

    def drop_db(self):
        self.client.drop_database(self.db)

    def insert(self, value=None):
        if value:
            proxy = dict(ip=value['ip'], port=value['port'], types=value['types'],
                         country=value['country'],
                         area=value['area'], speed=value['speed'], score=DEFAULT_SCORE)
            self.proxys.insert(proxy)

    def delete(self, conditions=None):
        if conditions:
            self.proxys.remove(conditions)
            return ('deleteNum', 'ok')
        else:
            return ('deleteNum', 'None')

    def update(self, conditions=None, value=None):
        # update({"UserName":"libing"},{"$set":{"Email":"libing@126.com","Password":"123"}})
        if conditions and value:
            self.proxys.update(conditions, {"$set": value})
            return {'updateNum': 'ok'}
        else:
            return {'updateNum': 'fail'}

    def select(self,count=None, conditions=None):
        if count:
            count = int(count)
        else:
            count = 0
        if conditions:
            conditions = dict(conditions)
            if 'count' in conditions:
            
                del conditions['count']
            conditions_name = ['types', 'protocol']
            for condition_name in conditions_name:
                value = conditions.get(condition_name, None)
                if value:
                    conditions[condition_name] = int(value)
        else:
            conditions = {}
        items = self.proxys.find(conditions, limit=count).sort(
            [("speed", pymongo.ASCENDING), ("score", pymongo.DESCENDING)])
        results = []
        for item in items:
            result = (item['ip'], item['port'], item['score'])
            results.append(result)
        return results
        
```

