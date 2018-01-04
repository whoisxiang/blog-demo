title: 爬虫demo
author: xiang
date: 2018-01-03 22:03:51
tags: 随笔
---
> 爬一部小说 

```
# -*- coding: UTF-8 -*- 
#name = xiang

import lxml,sys,time
import urllib.request
import urllib.error
from bs4 import BeautifulSoup
import multiprocessing

'''
1. 爬取其中一个url的内容
2. 得到主目录的url
3. 遍历每一个url
4. 个性化显示输出
'''

class notes(object):

    def __init__(self):
        self.server ="http://www.biqudu.com"
        self.target_url="http://www.biqudu.com/25_25584/"
        self.user_agent = 'Mozilla/5.0 (compatible; Baiduspider/2.0; +http://www.baidu.com/search/spider.html)'
        self.headers = {'User-Agent' :self.user_agent}
        self.names=[]
        self.urls=[]
        self.nums=0

    def get_download_urls(self):
        """
        下载所有章节url， 去除前面9个重复章节
        """
        request = urllib.request.Request(url=self.target_url,headers=self.headers)
        response = urllib.request.urlopen(request)
        data = response.read().decode('utf-8')
        html = BeautifulSoup(data,'lxml')
        divs = html.find_all("div",attrs={"class":'box_con'})
        temp_urls = BeautifulSoup(str(divs[1:]),'lxml')
        a = temp_urls.find_all('a')
        self.nums = len(a[9:])
        for each in a[9:] :
            self.names.append(each.string)
            self.urls.append(self.server + each.get('href'))

    def get_contant(self,target_url):
        """
        处理单个html，得到每章节的文本
        """
        request = urllib.request.Request(url=target_url,headers=self.headers)
        response = urllib.request.urlopen(request)
        data = response.read().decode('utf-8')
        html = BeautifulSoup(data,'lxml')
        divs = html.find_all("div",attrs={'id':'content'})
        content = BeautifulSoup(str(divs[0]),'lxml')
        txts=content.get_text()
        txt = txts.replace(r"readx();",' ')
        return  txt

    def write_note(self,name,path,txt):
        """
        写一个方法处理对应的文本
        """
        write_flag = True
        with open(path, 'a', encoding='utf-8') as f:
            f.write(name + '\n')
            f.writelines(txt)
            f.write('\n\n')

if __name__ == '__main__':
    start= time.time()
    spider = notes()
    spider.get_download_urls()
    for i in range(spider.nums):
        spider.write_note(spider.names[i], '江山.txt', spider.get_contant(spider.urls[i]))
        sys.stdout.write("  已下载:%.2f%%" %  float(i/spider.nums) + '\r')
        sys.stdout.flush()
    end = time.time()
    print('《江山》下载完成 ,共花费%sS'%(end-start))

```
