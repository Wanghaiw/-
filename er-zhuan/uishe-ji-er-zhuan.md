群发话术: 还在为图片无法下载而烦恼吗，还在重复的下载图片吗？今天忘仙老师带你学习如何批量下载图片。
网站地址：`https://www.pexels.com`,`https://500px.com/`

```
#!/usr/bin/env python
# -*- coding:utf-8 -*-
# @Date    : 2018/6/13 0013 13:48
# @Author  : wangxian (908686161@qq.com)

import re
import requests
from bs4 import BeautifulSoup

# class Download_Image(object):
#
#     def __init__(self):
#         self.base_url = 'https://www.pexels.com/?format=js&page={}'
#
#     def get_response(self,url):
#         return requests.get(url)
#
#     def parse_html(self,response):
#         image_urls = re.findall(r'data-big-src=\\"(.*?)\?auto=compress&amp',response.text,re.S)
#         return image_urls
#
#     def save(self,url):
#         image_path = url.split('/')[-1]
#         print('图片{}开始下载!!!'.format(image_path))
#         with open(image_path,'wb') as f:
#             f.write(self.get_response(url).content)
#
#     def download(self,urls):
#         for image_url in urls:
#             self.save(image_url)
#
#     def main(self):
#         result = self.get_response(self.base_url.format(2))
#         image_urls = self.parse_html(result)
#         self.download(image_urls)
#
# if __name__ == '__main__':
#     d = Download_Image()
#     d.main()


start_url = 'https://500px.com/editors'

session = requests.Session()

r = session.get(start_url).text

token = re.findall(r'<meta name="csrf-token" content="(.*?)" />',r,re.S)[0]
print(token)

url = 'https://api.500px.com/v1/photos?rpp=50&feature=editors&image_size%5B%5D=1&image_size%5B%5D=2&image_size%5B%5D=32&image_size%5B%5D=31&image_size%5B%5D=33&image_size%5B%5D=34&image_size%5B%5D=35&image_size%5B%5D=36&image_size%5B%5D=2048&image_size%5B%5D=4&image_size%5B%5D=14&sort=&include_states=true&include_licensing=true&formats=jpeg%2Clytro&only=&exclude=&personalized_categories=&page=3&rpp=50'

headers = {
    'Accept':'application/json, text/javascript, */*; q=0.01',
    'Accept-Encoding':'gzip, deflate, br',
    'Accept-Language': 'zh-CN,zh;q=0.9',
    'Host':'api.500px.com',
    'Referer':'https://500px.com/editors',
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36',
    'X-CSRF-Token':token,
}

session.options(url)
print('options请求完成')
print(session.get(url,headers=headers).text)

```
