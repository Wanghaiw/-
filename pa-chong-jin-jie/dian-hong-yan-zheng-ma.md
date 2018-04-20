# 点触验证码

点触验证码是由杭州微触科技有限公司研发的新一代的互联网验证码，使用点击或者拖动的形式完成验证。采用专利的印刷算法以及加密算法，保证每次请求到的验证图具有极高的安全性；点击与拖动的形式，为移动互联网量身定制，在PC端也具有非常好的用户体验，是一种安全，有趣，互动形式的新型验证方法。

# 超级鹰介绍


# 逻辑实现

## 1.获取需要识别的图片
在获取需要的识别的图片时，一般需要讲图片以及文字提示。
通过selenium的截图方法,获取到所需的信息。
```
        self.browser.save_screenshot('aa.png') # 先把整个屏幕截图
        element = self.browser.find_element_by_xpath('/html/body/div[2]/div/div[2]/div[2]/div[3]/div/div[2]/div[3]/div/div') # 获取图片所在的div

        left = element.location['x']
        top = element.location['y'] - 100
        right = element.location['x'] + element.size['width']
        bottom = element.location['y'] + element.size['height']

        im = Image.open('aa.png')
        captcha = im.crop((left, top, right, bottom))  # 根据div的长宽在整个屏幕上面截图
        captcha.save('captcha.png')
 ```
 
## 2.识别需要点击的坐标
```
 result = self.chaojiying.post_pic(bytes_array.getvalue(), CHAOJIYING_KIND) # 提交图片进行验证
   groups = captcha_result.get('pic_str').split('|')

        locations = [[int(number) for number in group.split(',')] for group in groups]
        return locations
 
```


## 3.根据坐标顺序依次点击


## 