# 验证码结构

通过查看网页可以发现滑动验证码的图片由两张图片组成。
需要注意的是在查看图片是可以发现每张图片是由52张小图片组合而成。
而每一张小图片其实都是一样的，通过偏移拼接出了正常的图片。

# 逻辑实现

## 1.获取完整的图片及带缺口的图片

* (1) 获取所有图片所在的div

    `background_images=driver.find_elements_by_xpath(div_path)`

* (2) 匹配所有图片的偏移坐标以及url
    ```
    location['x']=int(re.findall("background-image: url\(\"(.*)\"\); background-position: (.*)px (.*)px;",background_image.get_attribute('style'))[0][1])
        location['y']=int(re.findall("background-image: url\(\"(.*)\"\); background-position: (.*)px (.*)px;",background_image.get_attribute('style'))[0][2])
        image_url=re.findall("background-image: url\(\"(.*)\"\); background-position: (.*)px (.*)px;",background_image.get_attribute('style'))[0][0]
        ```