## 介绍
scrapy的图片管道,在ImagePipeline类中实现 ,提供了一个方便并具有额外特性的方法,来下载并本地存储图片:

* 将所有下载的图片转换成通用的格式(JPG)和模式(RGB)

* 避免重新下载最近已经下载过的图片

* 缩略图生成

* 检测图像的宽/高,确保它们满足最小的限制

这个管道也会为那些当前安排好要下载的图片保留一个内部队列,并将那些到达的包含相同图片的项目连接到那个队列中. 这可以避免多次下载几个项目共享的同一个图片.
Pillow是用来生成缩略图,并将图片归一化为JPEG/RGB格式,因此为了使用图片管道,需要安装Pillow库,不推荐PIL.



## 工作流程
```
　* 在一个爬虫里,你抓取一个项目,把其中图片的URL放入image_urls组内.

　* 项目从爬虫内返回,进入项目管道.

　* 当项目进入ImagePipeline, image_urls组内的URLs将被Scrapy的调度器和下载器安排下载(这意味着调度器和中间件可以复用),当优先级更高,会在其他页面被抓取前处理. 项目会在这个特定的管道阶段保持"locker"的状态,直到完成图片的下载(或者由于某些原因未完成下载).

　* 当图片下载完, 另一个组(images)将被更新到结构中,这个组将包含一个字典列表,其中包括下载图片的信息,比如下载路径,源抓取地址(从image_urls组获得)和图片的校验码. images列表中的图片顺序将和源image_urls组保持一致.如果某个图片下载失败,将会记录下错误信息,图片也不会出现在images组中.
```
## 项目步骤

1.定义items文件
```
import scrapy

class DemoItem(scrapy.Item):
    image_urls = scrapy.Field()
    images = scrapy.Field()
    
```
2.修改settings文件
```
# 开启图片管道
ITEM_PIPELINES = {
   'scrapy.pipelines.images.ImagesPipeline': 300,
}

# 设置用来存储图片的有效路径,否则图片管道保持禁用状态 既然在ITEM_PIPELINES开启了.
IMAGES_STORE = 'E:\\pics'
```
3.修改pipeline文件
```
from scrapy.pipelines.images import ImagesPipeline
from scrapy.exceptions import DropItem
import scrapy


class MyImagesPipeline(ImagesPipeline):

    def get_media_requests(self, item, info):
        for image_url in item['images_urls']:
            yield scrapy.Request(image_url)


    def item_completed(self, results, item, info):
        image_path = [x['path'] for ok,x in results if ok]

        if not image_path:
            raise DropItem('Item contains no images..')
        item['image_path'] = image_path
        return item

```
4.编写spider文件
```
import scrapy
from ..items import DemoItem

class ExampleSpider(scrapy.Spider):
    name = 'example'
    start_urls = ['http://699pic.com/people.html']

    def parse(self, response):
        items = DemoItem()
        src_list = response.xpath('//div[@class="list"]/a/img/@data-original').extract()
        items['image_urls'] = src_list
        yield items
```


