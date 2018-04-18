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
        
* (3) 根据图片的偏移值对图片进行拼接
    ```
    im = image.open(filename)
    new_im = image.new('RGB', (260,116))
    im_list_upper=[]
    im_list_down=[]

    for location in location_list:

        if location['y']==-58:
            im_list_upper.append(im.crop((abs(location['x']),58,abs(location['x'])+10,166)))
        if location['y']==0:
            im_list_down.append(im.crop((abs(location['x']),0,abs(location['x'])+10,58)))

    new_im = image.new('RGB', (260,116))

    x_offset = 0
    for im in im_list_upper:
        new_im.paste(im, (x_offset,0))
        x_offset += im.size[0]

    x_offset = 0
    for im in im_list_down:
        new_im.paste(im, (x_offset,58))
        x_offset += im.size[0]
    ```
## 2. 计算缺口的值
* (1) 对比RGB的值
```
    pixel1=image1.getpixel((x,y))
    pixel2=image2.getpixel((x,y))

    for i in range(0,3):
        if abs(pixel1[i]-pixel2[i])>=50:
            return False
    return True
```
* (2)获取缺口的位置
```
    image1.save('1.jpg')
    image2.save('2.jpg')
    i=0
    for i in range(0,260):
        for j in range(0,116):
            if is_similar(image1,image2,i,j)==False:
                return  i
```

## 3.  根据缺口的位置模拟x轴移动的轨迹

