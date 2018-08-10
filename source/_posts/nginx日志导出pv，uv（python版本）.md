---
title: nginx日志导出pv，uv（python版本）
categories: python
tags: python
keywords: python
abbrlink: cda56fe8
date: 2018-06-22 16:15:00
---

```
# encoding: utf-8
# get yesterday's date

import datetime
import xlwt
import os

workbook = xlwt.Workbook(encoding = 'utf-8')

worksheet = workbook.add_sheet('My Worksheet')
worksheet.col(0).width = 256*20
worksheet.write(0, 0, label = '日期')
worksheet.write(0, 1, label = 'PV')
worksheet.write(0, 2, label = 'UV')



f = open('n.jointwisdom.cn.log','r')

# Gets the statistics of the day's response file.

str = f.read()

f.close()

# Divide by blank lines.

arr = str.split('\n');

base_data =  []
for base in arr:
	if base.split(' ')[0] != '210.13.49.224':
		base_data.append(base.split(' '))
	
time = []
rep_data_pv = []
arr_uv = []
rep_data_uv = []
all_uv = []

all_pv_count = 0
for index in range(len(base_data)):
	for indexs in range(len(base_data[index])):
		api = base_data[index][5].split('/')
		length = len(api)
		targ = api[length-1].split('?')[0]
		
		if(indexs == 2 and targ == 'province'):
			all_pv_count = all_pv_count + 1
		if(indexs == 2 and base_data[index][indexs].split('[')[1].split(':')[0] not in time and targ == 'province'):
			if(base_data[index][0] not in all_uv):
				all_uv.append(base_data[index][0])
			time.append(base_data[index][indexs].split('[')[1].split(':')[0])
			rep_data_pv.append(1)
			rep_data_uv.append(len(arr_uv))
			arr_uv = []
			arr_uv.append(base_data[index][0])
			
		else:
			if(indexs == 2 and targ == 'province'):
				if(base_data[index][0] not in all_uv):
					all_uv.append(base_data[index][0])
				pv_len = len(rep_data_pv)
				rep_data_pv[pv_len-1] = rep_data_pv[pv_len-1] + 1
				if(base_data[index][0] not in arr_uv):
					arr_uv.append(base_data[index][0])
rep_data_uv.append(len(arr_uv))
rep_data_uv.pop(0)

print(all_pv_count)
all_uv_con = len(all_uv)

for index in range(len(time)):
	lawn = index + 1
	worksheet.write(lawn, 0, label = time[index])
	worksheet.write(lawn, 1, label = rep_data_pv[index])
	worksheet.write(lawn, 2, label = rep_data_uv[index])
	
lawns = len(time) + 1

all_pv = 0

for index in range(len(rep_data_pv)):
	all_pv = all_pv + int(rep_data_pv[index])

worksheet.write(lawns, 0, label = '合计')
worksheet.write(lawns, 1, label = all_pv)
worksheet.write(lawns, 2, label = all_uv_con)
workbook.save('市场热度报告.xls')


```