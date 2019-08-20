---
categories:
  - Python   #注意一定要大写
published: true
title: spider_icourse
---

# spider

'''

Try to Acquire the *icourse.com* information  between course_name and comment_number!

 My first spider！^ ^

'''

#### Show code

```python
#- - coding: utf - 8 - -
from urllib import request
import re



class Spider():
	url = 'http://www.icourses.cn/mooc/'

    #一级范围：将所需要的全部包含在一个正则表达式子中然后经过精炼找到所需要的数据
	root_pattern = '<div class="icourse-list-content pull-right">([\s\S]*?)</li>'

    # ([\s\S]*?) ([\w\W]*?) 匹配所有字符、非贪婪模式、成组
	course_name_pattern = 'target="_blank">([\w\W]*?)<'   

    #course_teacher_pattern = '<span>([\w\W]*?)</span><span class="icourse-list-		teacher">([\w\W]*?)</span>' #匹配不准确，因为前面也有可以一样匹配的，匹配多了。
	school_name_pattern = '<span>([\w\W]*?) </span><span class="icourse-list-school">([\w\W]*?)</span>'

	course_teacher_pattern = '<span>([\u4e00-\u9fa5 : ]*?)</span><span class="icourse-list-teacher">([\w\W]*?)</span>'
	course_intuoduction_pattern = '<p class="icourse-list-content-text">([\w\W]*?)</p>'
	student_number_pattern = '<span class="icourse-list-number">([\w\W]*?)</span><span>([\w\W]*?)</span>'

     #[\u4e00-\u9fa5]*?</a>' #只匹配中文字符
	comment_number_pattern = '<span class="icourse-list-number-comment">([\w\W]*?)</span><span>([\w\W]*?)</span>'



	def __fetch_content(self):
		r = request.urlopen(Spider.url)
		htmls = str(r.read(),encoding='utf-8') #将编码以utf-8格式输出，r.read()是byte格式显示所以要转换成字符串后编码输出
		return htmls
		#print(htmls)

	#数据分析
	def __analysis(self,pattern_htmls):

        #将一级范围的信息找出，并存为列表，每组想要的信息，都是一个元素
		root_html = re.findall(Spider.root_pattern , pattern_htmls)
		list_course = []
		for i in root_html:

            '''
            [j for i in a for j in i]
            把包含元组的列表变成一个列表:[(a,),(b,)(c,)...]分割变成[a,b,c....]
            course_teacher输出结果为字符串类型
            '''
			course_name = ''.join([j for i in re.findall(self.course_name_pattern , i) 				for j in i])
			school_name = ''.join([j for i in re.findall(self.school_name_pattern , i) 				for j in i])
			course_teacher = ''.join([j for i in re.findall(self.course_teacher_pattern , 				i) for j in i])			
			course_intuoduction = ''.join([j for i in 				re.findall(self.course_intuoduction_pattern , i) for j in i])
			student_number = ''.join([j for i in re.findall(self.student_number_pattern , 				i) for j in i])
			comment_number = ''.join([j for i in re.findall(self.comment_number_pattern , 				i) for j in i])

            #将数据组合成字典，字典由组合成列表，优点易于按照字典标签排序 [{},{},{},{}]
			content = {'course_name':course_name,'school_name':school_name,'course_teacher':course_teacher,'course_intuoduction':course_intuoduction,'student_number':student_number,'comment_number':comment_number}
			list_course.append(content)

        #list_course为[{},{},{},{}]
		return(list_course)

    #排序
	def __sort(self,list_course):
		sort_list_course = sorted(list_course , key = self.__sort_seed , reverse = True)
		return sort_list_course

    #排序方式
	def __sort_seed(self,list_course_1):
		number = eval(list_course_1['student_number'][0:-3])
		return number

    #将排列打印展示
	def __show(self , sort_list_course):
		n = 1
		for i in sort_list_course:
			print(str(n) + '.' , end = '' )
			print('{: <20}{: <20}{:		<20}'.format(i['school_name'],i['course_name'],i['student_number']))
			n = n + 1

	#入口
	def go(self):
		pattern_htmls = self.__fetch_content()
		list_course = self.__analysis(pattern_htmls)
		sort_list_course = self.__sort(list_course)
		self.__show(sort_list_course)

spider = Spider()
spider.go()
```

#### result：

```python
1.学校 :哈尔滨工业大学         C语言程序设计             577487人学习
2.学校 :中央财经大学          金融学                 517001人学习
3.学校 :南昌大学            高等数学                446413人学习
4.学校 :清华大学            经济学原理               338636人学习
5.学校 :武汉大学            西方哲学史               318935人学习
6.学校 :哈尔滨工业大学         理论力学                262840人学习
7.学校 :西安交通大学          电路                  224587人学习
8.学校 :北京大学            量子力学                151825人学习
9.学校 :上海财经大学          货币银行学               142559人学习
10.学校 :南开大学            大学语文                127032人学习
11.学校 :东南大学            大学英语                120361人学习
12.学校 :清华大学            电路原理                110184人学习
13.学校 :清华大学            面向对象程序设计            98945人学习
14.学校 :华南理工大学          建筑设计基础              97888人学习
15.学校 :北京大学            医学微生物学              79179人学习
16.学校 :清华大学            中国工艺美术史             43191人学习
17.学校 :武汉大学            毛泽东思想和中国特色社会主义理论体系概论43061人学习
18.学校 :武汉大学            测绘学概论               42996人学习
19.学校 :厦门大学            无机化学                38441人学习
20.学校 :西安交通大学          计算机程序设计             38221人学习
```

#### reference:

[]: https://segmentfault.com/q/1010000016895194
[]: https://www.bilibili.com/video/av51299056/?p=60
[]: https://class.imooc.com/sc/62
[]: https://www.runoob.com/

#### Changelog
2019-08-21-add-by-C.W.  spider_icourses
