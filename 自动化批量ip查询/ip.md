功能

1：打开网址，
2：在中间输入ip地址，
3：点击查询
4：进行屏幕截图，并且用ip的名字进行保存，例如127-0-0-1.jpg
5：保存屏幕源码，并且用ip的名字进行保存，同上
6：将源码中出现（*）的ip地址抽取抽取出来
代码

#-*- coding:utf-8 -*-
import unittest
import time
import re
from selenium import webdriver
from selenium.webdriver.common.keys import Keys



url = "http://www.ipip.net/ip.html"
driver = webdriver.Chrome()

def get_data(ip):
    #打开请求的url
    driver.get(url)

    assert "IPIP" in driver.title
    elem = driver.find_element_by_name("ip") #ip为搜索框的名字

    elem.send_keys(ip) #输入内容
    elem.send_keys(Keys.RETURN)#模拟点击回车

    #driver.save_screenshot("1.png")


    page = driver.page_source.encode('utf-8')
    all_file_name = "/home/xuna/桌面/all_res/" + ip + ".txt"
    res_file_name = "/home/xuna/桌面/res/" + ip + ".txt"
    file = open(all_file_name,"w")
    file.write(page)

    print page.find("**")

    #返回值为 -1:不存在
    if page.find("**") != -1:
        file = open(res_file_name,"w")
        file.write(page)


    file.close()



if __name__ == "__main__":

    """
    get_data("123.1.1.1")
    get_data("1.1.1.1")
    """

    file_path = "/home/xuna/桌面/selenium/res_url.txt"
    fout = open(file_path,"rb")
    n = 0


    for ip in fout:
        try:
            if ip[0] >='1' and ip[0] <='9':
                #正则表达式提取ip,过滤空格，换行符
                res = re.findall(r'\d+\.\d+\.\d+\.\d+',ip)
                print n,res[0]
                get_data(str(res[0]))
                n = n + 1

        except:
            print n,"错误"
            pass



参考资料

http://www.cnblogs.com/hanxiaobei/p/6108677.html
http://cuiqingcai.com/2599.html（这个比较详细）