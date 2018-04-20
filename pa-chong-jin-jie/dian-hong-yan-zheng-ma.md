# 点触验证码

点触验证码是由杭州微触科技有限公司研发的新一代的互联网验证码，使用点击或者拖动的形式完成验证。采用专利的印刷算法以及加密算法，保证每次请求到的验证图具有极高的安全性；点击与拖动的形式，为移动互联网量身定制，在PC端也具有非常好的用户体验，是一种安全，有趣，互动形式的新型验证方法。

# 超级鹰介绍
就是一个打码平台，使用起来非常简单。
## 1.下载python的demo文件 
## 2. 使用方法
```
from chaojiying import Chaojiying
CHAOJIYING_USERNAME = '908686161'  # 账号
CHAOJIYING_PASSWORD = 'admin123'   # 密码
CHAOJIYING_SOFT_ID = 894600        # 生成的唯一key
CHAOJIYING_KIND = 9004             # 题型
cjy = Chaojiying(CHAOJIYING_USERNAME, CHAOJIYING_PASSWORD, CHAOJIYING_SOFT_ID)  # 创建实例
```

# 


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
把需要识别的图片和提示一起上传 返回坐标
```
 result = self.chaojiying.post_pic(bytes_array.getvalue(), CHAOJIYING_KIND) # 提交图片进行验证
 groups = result.get('pic_str').split('|') # 对返回的数据进行解析  获取x坐标和y坐标
 locations = [[int(number) for number in group.split(',')] for group in groups]
```

## 3.根据坐标顺序依次点击
 根据x和y坐标依次点击图片当中的文字
 self.get_touclick_element() 获取图片的位置
 move_to_element_with_offset  将鼠标移动到距某个元素多少距离的位置
```
for location in locations:
    ActionChains(self.browser).move_to_element_with_offset(self.get_touclick_element(), location[0],location[1]).click().perform()
    time.sleep(1)
```
