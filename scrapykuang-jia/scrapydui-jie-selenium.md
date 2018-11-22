```
import time
from selenium import webdriver
from scrapy.http import HtmlResponse


class Downloadmiddleware(object):

    def __init__(self):
        self.driver = webdriver.Chrome()


    def process_request(self,request,spider):
        print('spider:{}'.format(spider))
        if spider.name =='nhsq':
            self.driver.get(request.url)
            time.sleep(3)
            return HtmlResponse(url=request.url,body=self.driver.page_source,request=request,encoding='utf-8')


    def spider_close(self,spider):
        print('spider:结束了...........,关闭浏览器.....')
        self.driver.quit()


    def process_response(self,request,response,spider):
        dispatcher.connect(self.spider_close,signals.spider_closed)

        return response
