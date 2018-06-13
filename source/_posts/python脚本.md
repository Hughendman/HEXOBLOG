---
title: python导数据脚本
categories: python
tags: python
keywords: python
abbrlink: 5dd43de5
date: 2018-06-13 11:15:00
---

刚刚上手，代码很不优雅，有待优化

```
# encoding: utf-8
# get yesterday's date

import datetime
import xlwt
import os
#import smtplib
#from email.mime.text import MIMEText
#from email.mime.multipart import MIMEMultipart
#from email.header import Header

today = datetime.date.today()

yesterday = today - datetime.timedelta(days=1)
day = yesterday.strftime("%Y-%m-%d")

# create excel

workbook = xlwt.Workbook(encoding = 'utf-8')

worksheet = workbook.add_sheet('My Worksheet')
worksheet.col(0).width = 256*20
worksheet.col(3).width = 256*20
worksheet.col(4).width = 256*20
worksheet.write(0, 0, label = '日期')
worksheet.write(0, 1, label = 'PV')
worksheet.write(0, 2, label = 'UV')
worksheet.write(0, 3, label = '生成报告数')
worksheet.write(0, 4, label = '生成报告人数')

def date(time):
	rep_data_pv = 0
	rep_data_uv = 0
	rep_data_report = 0
	rep_data_vistor = 0
	for index in range(time):
	
		flag = index + 1
		date_time = today - datetime.timedelta(days=flag)
		day_time = date_time.strftime("%Y-%m-%d")
		response_name = 'response-' + day_time + '.log'
		if os.path.exists(response_name):

			# Open the file for the day.

			f = open(response_name,'r')

			# Gets the statistics of the day's response file.

			str = f.read()

			f.close()

			# Divide by blank lines.

			arr = str.split('\n');

			# whitespace

			base_data =  []
			for base in arr:
				base_data.append(base.split(' '))

			# Statistical PV UV

			page = []
			ip = []
				
			for index in range(len(base_data)):
				for indexs in range(len(base_data[index])):
					if indexs == 5:
						ip.append(base_data[index][indexs])
					elif indexs == 4:
						page.append(base_data[index][indexs])

			# get page view and report

			pv = 0
			report = 0
			for init in page:
				target = init.split('?')[0].split('/')
				num = len(target)-1
				if target[num] == 'getData':
					pv = pv + 1
				elif target[num] == 'table':
					report = report + 1

			# get unique visitor

			uv_arr = []
			uv = 0

			for init in ip:
				if init not in uv_arr:
					uv = uv + 1
					uv_arr.append(init)

			# get report visitor

			view_ip = []
			for index in range(len(page)):
				target = page[index].split('?')[0].split('/')
				num = len(target)-1
				if target[num] == 'table':
					view_ip.append(ip[index])
					
			vistor_arr = []
			vistor = 0

			for init in view_ip:
				if init not in vistor_arr:
					vistor = vistor + 1
					vistor_arr.append(init)
			rep_data_pv = rep_data_pv + pv
			rep_data_uv = rep_data_uv + uv
			rep_data_report = rep_data_report  + report
			rep_data_vistor = rep_data_vistor + vistor
			
			worksheet.write(flag, 0, label = day_time)
			worksheet.write(flag, 1, label = pv)
			worksheet.write(flag, 2, label = uv)
			worksheet.write(flag, 3, label = report)
			worksheet.write(flag, 4, label = vistor)
		else:
			data = {'pv':0,'uv':0,'report':0,'vistor':0}
			worksheet.write(flag, 0, label = day_time)
			worksheet.write(flag, 1, label = 0)
			worksheet.write(flag, 2, label = 0)
			worksheet.write(flag, 3, label = 0)
			worksheet.write(flag, 4, label = 0)
	sum = time+1
	worksheet.write(sum, 0, label = '合计')
	worksheet.write(sum, 1, label = rep_data_pv)
	worksheet.write(sum, 2, label = rep_data_uv)
	worksheet.write(sum, 3, label = rep_data_report)
	worksheet.write(sum, 4, label = rep_data_vistor)
			
		
		
date(27)		
workbook.save('报告--'+yesterday.strftime("%Y-%m-%d")+'.xls')

```