# lb68

import os
import re

import requests as rq
from bs4 import BeautifulSoup

from sqlite_lb import Sqlite
from myThread import MyThread

class Meizitu(object):
    #从网页爬取图片，获取每一套图片的数量，编号，发表时间，并按照不同的年来存放。
    def __init__(self,NOs):
        self.img_url="http://img.mmjpg.com/"
        self.page_url='http://www.mmjpg.com/mm/'
        self.NOs=NOs

    def parse(self):
        #解析网页
        try:
            maxr=maxr=rq.get(self.page_url+str(self.NOs))
            maxr.status_code
            maxr.encoding=maxr.apparent_encoding
            self.soup=BeautifulSoup(maxr.text,'html.parser')
            return self.soup
        except:
            print("解析网页失败！")

    def get_numbers(self):
        #获取图片数量
        self.parse()
        maxpages=re.findall('>\d\d<',str(self.soup))
        maxpage1=maxpages[0]
        self.maxpage2=int(maxpage1[1:-1])
        return self.maxpage2

    def get_year(self):
        #获取图片的发表时间
        self.parse()
        year=re.findall('\d\d\d\d年\d+月\d+日',str(self.soup))
        self.date=year[0][0:4]+'-'+year[0][5:7]+'-'+year[0][8:10]
        self.year=self.date[0:4]
        return self.date,self.year
    
    def download(self):
        #下载图片
        self.get_numbers()
        self.get_year()
        root="G://mqiqi//"+self.year+'//'
        title=self.soup.find('title')
        t=str(title)[7:-12]
        root=root+t+'//'
        for i in range(1,self.maxpage2+1):
            url=self.img_url+self.year+'/'
            url=url+str(self.NOs)+'/'
            url=url+str(i)+'.jpg'
            path=root+url.split('/')[-1]
            try :
                if not os.path.exists(root):
                    os.mkdir(root)
                if not os.path.exists(path):
                    r=rq.get(url)
                    with open(path,'wb') as f:
                        f.write(r.content)
                        f.close()
                        print('成功',url)
                else:
                    print("文件已存在",url)
            except:
                print('下载失败：',url)
        return (self.img_url+self.year+'/',t)
