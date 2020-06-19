selenium 设置无界面浏览，设置代理

```python

from selenium import webdriver
from selenium.webdriver.chrome.options import Options

chromeOptions = Options()

# 设置无浏览器界面
chromeOptions.add_argument('--headless')
chromeOptions.add_argument('--disable-gpu')

# 设置ua
chromeOptions.add_argument('User-Agent=%s'%user_agent)

# 设置代理
chromeOptions.add_argument("--proxy-server=http://58.218.92.94:2874")
# 一定要注意，=两边不能有空格，不能是这样--proxy-server = http://202.20.16.82:10152
browser = webdriver.Chrome(chrome_options = chromeOptions)
 
# 查看本机ip，查看代理是否起作用
browser.get("http://httpbin.org/get")
print(browser.page_source)
 
# 退出，清除浏览器缓存
browser.quit()
```







常用参数配置

```python
from selenium import webdriver
option = webdriver.ChromeOptions()

# 添加UA
options.add_argument('user-agent="MQQBrowser/26 Mozilla/5.0 (Linux; U; Android 2.3.7; zh-cn; MB200 Build/GRJ22; CyanogenMod-7) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1"')

# 指定浏览器分辨率
options.add_argument('window-size=1920x3000') 

# 谷歌文档提到需要加上这个属性来规避bug
chrome_options.add_argument('--disable-gpu') 

 # 隐藏滚动条, 应对一些特殊页面
options.add_argument('--hide-scrollbars')

# 不加载图片, 提升速度
options.add_argument('blink-settings=imagesEnabled=false') 

# 浏览器不提供可视化页面. linux下如果系统不支持可视化不加这条会启动失败
options.add_argument('--headless') 

# 以最高权限运行
options.add_argument('--no-sandbox')

# 手动指定使用的浏览器位置
options.binary_location = r"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" 

#添加crx插件
option.add_extension('d:\crx\AdBlock_v2.17.crx') 

# 禁用JavaScript
option.add_argument("--disable-javascript") 

# 设置开发者模式启动，该模式下webdriver属性为正常值
options.add_experimental_option('excludeSwitches', ['enable-automation']) 

# 禁用浏览器弹窗
prefs = {  
    'profile.default_content_setting_values' :  {  
        'notifications' : 2  
     }  
}  
options.add_experimental_option('prefs',prefs)

driver=webdriver.Chrome(chrome_options=chrome_options)
```