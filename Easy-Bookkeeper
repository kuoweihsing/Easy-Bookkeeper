import tkinter as tk
from tkinter import ttk
from tkinter import messagebox
from tkinter import font
from tkcalendar import Calendar
import csv
import datetime
import sys
from os import path
import matplotlib.pyplot as plt
import pandas
import html5lib
import ssl
import calendar

"""檔案路徑目前位置：23行和693行"""
class Tab:
	def __init__(self):
		# 介面框架
		self.root = tk.Tk()
		self.root.title("記帳小幫手")
		#self.root.iconbitmap("記帳本.ico")
		# 帳本檔案路徑
		self.fileLoc = "book.csv"

		# 分頁框架
		self.tabControl = ttk.Notebook(self.root)
		# 分頁一：記帳功能
		self.tab1 = ttk.Frame(self.tabControl)
		self.tabControl.add(self.tab1, text = "記帳")
		self.tabControl.pack(expand = 1, fill = "both")
		# 分頁二：查詢功能
		self.tab2 = ttk.Frame(self.tabControl)
		self.tabControl.add(self.tab2, text = "查詢")
		self.tabControl.pack(expand = 1, fill = "both")
		# 分頁三：分析功能
		self.tab3 = ttk.Frame(self.tabControl)
		self.tabControl.add(self.tab3, text = "分析")
		self.tabControl.pack(expand = 1, fill = "both")
		# 分頁四：預算功能
		self.tab4 = ttk.Frame(self.tabControl) 
		self.tabControl.add(self.tab4, text = "預算")
		self.tabControl.pack(expand = 1, fill = "both")

		""" 記帳分頁widgets """
		self.lbl_date = tk.Label(self.tab1, text = "交易日期", width = 20, height = 2)	# 顯示交易日期
		self.lbl_type = tk.Label(self.tab1, text = "交易種類", width = 20, height = 2)	# 顯示交易種類
		self.lbl_subtype = tk.Label(self.tab1, text = "科目類別", width = 20, height = 2)  # 顯示交易科目
		self.lbl_dollar = tk.Label(self.tab1, text = "交易金額", width = 20, height = 2)  # 顯示交易金額
		self.lbl_item = tk.Label(self.tab1, text = "交易摘要", width = 20, height = 2, pady = 5)	# 顯示交易摘要
		self.lbl_rate = tk.Label(self.tab1, text = "匯率", width = 20, height = 2, pady = 5)	# 顯示匯率

		self.btn_date = tk.Button(self.tab1, width = 20, height = 2, pady = 5, text = "日曆", command = self.calendar_view)  # 日曆按鈕
		self.lbl_date_selected = tk.Label(self.tab1, width = 20, pady = 5, height = 2)  # 顯示交易日期
		self.btn_rev = tk.Button(self.tab1, width = 20, height = 2, pady = 5, text = "收入", bg = "gold", command = self.click_rev)  # 收入按鈕
		self.btn_exp = tk.Button(self.tab1, width = 20, height = 2, pady = 5,text = "支出", bg = "white", command = self.click_exp)  # 支出按鈕
		# 子類別下拉選單
		self.droplist1 = ttk.Combobox(self.tab1, width = 40, values = ["請選擇類別", "薪資收入", "獎金收入", "投資收入", "副業收入", "臨時收入", "其他收入"], state = "readonly")
		self.droplist1.current(0)
		# 匯率幣別下拉選單
		SelectCurrency = ["NTD", "(USD)", "(EUR)", "(CNY)", "(JPY)", "(HKD)", "(GBP)", "(AUD)", "(CAD)", "(SGD)", "(CHF)", "(ZAR)", "(SEK)", "(NZD)", "(THB)"]
		self.droplistRate = ttk.Combobox(self.tab1, width = 40, values = SelectCurrency, state = "readonly")
		self.droplistRate.current(0)
		self.txt_dollar = tk.Entry(self.tab1)  # 輸入交易金額
		self.txt_item = tk.Entry(self.tab1)	 # 輸入交易摘要

		# 功能按鈕
		self.btn_clear = tk.Button(self.tab1, text = "清除", width = 20, height = 2, padx = 5, comman = self.click_clear)  # 清除資料
		self.btn_save = tk.Button(self.tab1, text = "存檔", width = 20, height = 2, command = self.click_save)  # 存檔資料
		self.btn_exit = tk.Button(self.tab1, text = "離開", width = 20, height = 2, bg = "red", command = self.click_exit)  # 離開程式

		""" 查詢分頁widgets """
		self.transactionList = []
		self.lbl2_guidesearchmethod1 = tk.Label(self.tab2, text = " ===== 搜尋方法1：按日期 ===== ", width = 60, height = 2, pady = 5)  # 引導搜尋方法一  
		self.lbl2_date = tk.Label(self.tab2, text = "交易日期", width = 20, height = 2, pady = 5)	 # 顯示交易日期
		self.lbl2_guidesearchmethod2 = tk.Label(self.tab2, text = " ===== 搜尋方法2：按交易金額或關鍵字 ===== ", width = 60, height = 2, pady = 5)  # 引導搜尋方法二
		self.lbl2_dollar = tk.Label(self.tab2, text = "交易金額", width = 20, height = 2, pady = 5)  # 顯示交易金額
		self.lbl2_item = tk.Label(self.tab2, text = "交易關鍵字", width = 20, height = 2, pady = 5)  # 顯示交易關鍵字

		self.btn2_date = tk.Button(self.tab2, width = 20, height = 2, text = "日曆", command = self.calendar_view2)  # 日曆按鈕
		self.lbl2_date_selected = tk.Label(self.tab2, width = 20, height = 2)  # 顯示交易日期
		self.txt2_dollar = tk.Entry(self.tab2)	# 輸入交易金額
		self.txt2_item = tk.Entry(self.tab2)  # 輸入交易摘要

		# 功能按鈕
		self.btn2_clear = tk.Button(self.tab2, text = "清除", width = 30, height = 2, command = self.click_clear2)
		self.btn2_delete = tk.Button(self.tab2, text = "刪除", width = 30, height = 2, command = self.click_delete)
		self.btn2_view = tk.Button(self.tab2, text = "檢視", width = 20, height = 2, padx = 3, command = self.click_view2)
		self.btn2_search = tk.Button(self.tab2, text = "搜尋", width = 20, height = 2, padx = 3, command = self.click_search)
		self.btn2_exit = tk.Button(self.tab2, text = "離開", width = 20, height = 2, padx = 3, command = self.click_exit)

		""" 分析分頁widgets """
		self.lbl3_title = tk.Label(self.tab3, text = "輸入分析時間並點選以產生圖表", width = 60, height = 2)  # 頁面說明
		self.blank1 = tk.Label(self.tab3, text = "===========================================================", width = 60, height = 2, pady = 5)
		self.lbl3_year = tk.Label(self.tab3, text = "分析年份", width = 20, height = 2, pady = 5)  # 顯示
		self.txt3_year = tk.Entry(self.tab3)  # 輸入製作圖表的年份
		self.btn3_yearrevbar = tk.Button(self.tab3, text = "顯示月收入長條圖", width = 15, height = 2, pady = 5, command = self.click_yrevb)  # 按出年收入(1~12月)長條圖
		self.btn3_yearexpbar = tk.Button(self.tab3, text = "顯示月支出長條圖", width = 15, height = 2, command = self.click_yexpb)  # 按出年支出(1~12月)長條圖
		self.btn3_yearrevpie = tk.Button(self.tab3, text = "顯示月收入圓餅圖", width = 15, height = 2, command = self.click_yrevp)  # 按出年收入(各項收入)圓餅圖
		self.btn3_yearexppie = tk.Button(self.tab3, text = "顯示月支出圓餅圖", width = 15, height = 2, command = self.click_yexpp)  # 按出年支出(各項支出)長條圖
		self.blank2 = tk.Label(self.tab3, text = "===========================================================", width = 60, height = 2)
		self.lbl3_month = tk.Label(self.tab3, text = "分析月份", width = 20, height = 2, pady = 5)  # 顯示
		self.txt3_month = tk.Entry(self.tab3)  # 輸入製作圖表的月份
		self.btn3_monthrevbar = tk.Button(self.tab3, text = "顯示日收入長條圖", width = 15, height = 2, pady = 5, command = self.click_mrevb)  # 按出月收入(30天)長條圖
		self.btn3_monthexpbar = tk.Button(self.tab3, text = "顯示日支出長條圖", width = 15, height = 2, command = self.click_mexpb)  # 按出月支出(30天)長條圖
		self.btn3_monthrevpie = tk.Button(self.tab3, text = "顯示日收入圓餅圖", width = 15, height = 2, command = self.click_mrevp)  # 按出月收入(各項收入)圓餅圖
		self.btn3_monthexppie = tk.Button(self.tab3, text = "顯示日支出圓餅圖", width = 15, height = 2, command = self.click_mexpp)  # 按出月支出(各項支出)圓餅圖

		# 記帳分頁設定位置
		self.lbl_date.grid(row = 0, column = 0, columnspan = 2, sticky = tk.NE + tk.SW)
		self.lbl_type.grid(row = 1, column = 0, columnspan = 2, sticky = tk.NE + tk.SW)
		self.lbl_subtype.grid(row = 2, column = 0, columnspan = 2, sticky = tk.NE + tk.SW)
		self.lbl_dollar.grid(row = 3, column = 0, columnspan = 2, sticky = tk.NE + tk.SW)
		self.lbl_rate.grid(row = 4, column = 0, columnspan = 2, sticky = tk.NE + tk.SW)
		self.lbl_item.grid(row = 5, column = 0, sticky = tk.NE + tk.SW)
		self.lbl_date_selected.grid(row = 0, column = 2, columnspan = 2, sticky = tk.NE + tk.SW)
		self.btn_date.grid(row = 0, column = 4, columnspan = 2, sticky = tk.NE +tk.SW)
		self.btn_rev.grid(row = 1, column = 2, columnspan = 2, sticky = tk.NE + tk.SW)
		self.btn_exp.grid(row = 1, column = 4, columnspan = 2, sticky = tk.NE + tk.SW)
		self.droplist1.grid(row = 2, column = 2, columnspan = 4, sticky = tk.NE + tk.SW)
		self.droplistRate.grid(row = 4, column = 2, columnspan = 4, sticky = tk.NE + tk.SW)
		self.txt_dollar.grid(row = 3, column = 2, columnspan = 4, sticky = tk.NE + tk.SW)
		self.txt_item.grid(row = 5, column = 2, columnspan = 4, sticky = tk.NE + tk.SW)
		self.btn_clear.grid(row = 7, column = 0, columnspan = 2, sticky = tk.NE + tk.SW)
		self.btn_save.grid(row = 7, column = 2, columnspan = 2, sticky = tk.NE + tk.SW)
		self.btn_exit.grid(row = 7, column = 4, columnspan = 2, sticky = tk.NE + tk.SW)

		# 查詢分頁設定位置
		self.lbl2_guidesearchmethod1.grid(row = 0, column = 0, columnspan = 6, sticky = tk.NE + tk.SW)
		self.lbl2_date.grid(row = 1, column = 0, columnspan = 2, sticky = tk.NE + tk.SW)
		self.lbl2_date_selected.grid(row = 1, column = 2, columnspan = 2, sticky = tk.NE + tk.SW)
		self.btn2_date.grid(row = 1, column = 4, columnspan = 2, sticky = tk.NE +tk.SW)
		self.lbl2_guidesearchmethod2.grid(row = 2, column = 0, columnspan = 6, sticky = tk.NE + tk.SW)
		self.lbl2_dollar.grid(row = 3, column = 0, columnspan = 2, sticky = tk.NE + tk.SW)
		self.lbl2_item.grid(row = 4, column = 0, columnspan = 2, sticky = tk.NE + tk.SW)
		self.txt2_dollar.grid(row = 3, column = 2, columnspan = 4, sticky = tk.NE + tk.SW)
		self.txt2_item.grid(row = 4, column = 2, columnspan = 4, sticky = tk.NE + tk.SW)
		self.btn2_clear.grid(row = 5, column = 0, columnspan = 3, sticky = tk.NE + tk.SW)
		self.btn2_delete.grid(row = 5, column = 3, columnspan = 3, sticky = tk.NE + tk.SW)
		self.btn2_view.grid(row = 6, column = 0, columnspan = 2, sticky = tk.NE + tk.SW)
		self.btn2_search.grid(row = 6, column = 2, columnspan = 2, sticky = tk.NE + tk.SW)
		self.btn2_exit.grid(row = 6, column = 4, columnspan = 2, sticky = tk.NE + tk.SW)

		# 分析分頁設定位置
		self.lbl3_title.grid(row = 0, column = 0, columnspan = 12, sticky = tk.NE + tk.SW)
		self.blank1.grid(row = 1, column = 0, columnspan = 12, sticky = tk.NE + tk.SW)
		self.lbl3_year.grid(row = 2, column = 0, columnspan = 4, sticky = tk.NE + tk.SW)
		self.txt3_year.grid(row = 2, column = 4, columnspan = 8, sticky = tk.NE + tk.SW)
		self.btn3_yearrevbar.grid(row = 3, column = 0, columnspan = 3, sticky = tk.NE + tk.SW)
		self.btn3_yearexpbar.grid(row = 3, column = 3, columnspan = 3, sticky = tk.NE + tk.SW)
		self.btn3_yearrevpie.grid(row = 3, column = 6, columnspan = 3, sticky = tk.NE + tk.SW)
		self.btn3_yearexppie.grid(row = 3, column = 9, columnspan = 3, sticky = tk.NE + tk.SW)
		self.blank2.grid(row = 4, column = 0, columnspan = 12, sticky = tk.NE + tk.SW)
		self.lbl3_month.grid(row = 5, column = 0, columnspan = 4, sticky = tk.NE + tk.SW)
		self.txt3_month.grid(row = 5, column = 4, columnspan = 8, sticky = tk.NE + tk.SW)
		self.btn3_monthrevbar.grid(row = 6, column = 0, columnspan = 3, sticky = tk.NE + tk.SW)
		self.btn3_monthexpbar.grid(row = 6, column = 3, columnspan = 3, sticky = tk.NE + tk.SW)
		self.btn3_monthrevpie.grid(row = 6, column = 6, columnspan = 3, sticky = tk.NE + tk.SW)
		self.btn3_monthexppie.grid(row = 6, column = 9, columnspan = 3, sticky = tk.NE + tk.SW)

		""" 預算分頁widgets及位置"""
		self.frm4_0 = tk.Frame(self.tab4,bd=1,relief='raised')
		self.frm4_0.pack(side='top',padx=3,pady=3,ipadx=1,ipady=1,fill='both')
		self.frm4_01 = tk.Frame(self.frm4_0)
		self.frm4_01.pack(side='top',padx=3,pady=3,ipadx=1,ipady=1,fill='both')
		self.year_now = datetime.datetime.now().year #獲取當前年份
		self.month_now = datetime.datetime.now().month #獲取當前月份
		self.txt4_year = ttk.Combobox(self.frm4_01,values=[str(i) for i in range(self.year_now,1900,-1)],state = "readonly",height=12,width=5)
		self.txt4_year.pack(side='left')
		self.txt4_year.current(0)
		tk.Label(self.frm4_01,text='年').pack(side='left')
		self.txt4_month = ttk.Combobox(self.frm4_01,values=[str(i) for i in range(1,13)],state = "readonly",height=12,width=3)
		self.txt4_month.pack(side='left')
		self.txt4_month.current(self.month_now-1) #默認當前月份
		tk.Label(self.frm4_01,text='月').pack(side='left')
		self.btn4_01 = tk.Button(self.frm4_01,command=self.click_btn4_0,bg='blue',text='BLUE',fg='white',width=5)
		self.btn4_01.pack(side='right')
		tk.Label(self.frm4_01,text='提示:設置總預算後請點擊藍色按鍵  ',fg='dimgrey').pack(side='right')
		
		self.frm4_1 = tk.Frame(self.frm4_0)
		self.frm4_1.pack(side='top',fill='both',padx=3)
		self.cvs4 = tk.Canvas(self.frm4_1,height=100,width=100)
		self.cvs4.grid(row=0,column=0,rowspan=3,sticky='n'+'s'+'w'+'e',pady=1)
		self.cvs4.create_oval(10,10,90,90,outline='green',width=10)
		self.cvs_text = self.cvs4.create_text(50,50,text='--%')
		tk.Label(self.frm4_1,text='設定預算').grid(row=0,column=1,padx=50)
		self.ety4_0_var = tk.StringVar()
		self.ety4_0 = tk.Entry(self.frm4_1,textvariable=self.ety4_0_var)
		self.ety4_0.grid(row=0,column=2)
		tk.Label(self.frm4_1,text='支出合計').grid(row=1,column=1)
		self.lbl4_0 = tk.Label(self.frm4_1,bg='white',height=1,width=20)
		self.lbl4_0.grid(row=1,column=2,sticky='w')
		tk.Label(self.frm4_1,text='目前餘額').grid(row=3,column=1)
		self.lbl4_1 = tk.Label(self.frm4_1,bg='white',height=1,width=20)
		self.lbl4_1.grid(row=3,column=2,sticky='w')

		self.frm4_2 = tk.Frame(self.tab4,bd=1,relief='raised')
		self.frm4_2.pack(side='top',padx=1,pady=1,ipadx=3,ipady=3,fill='both')
		self.txt4_0 = ttk.Combobox(self.frm4_2,values=['請選擇類別————',"餐飲支出", "日用品支出", "衣服支出", "交通支出","水電支出", "娛樂支出", "電話支出", "其他支出"],state = "readonly",width=15)
		self.txt4_0.grid(row=0,column=0)
		self.txt4_0.bind('<<ComboboxSelected>>',self.choose_txt4_0)
		self.txt4_0.current(0)
		self.lbl4_8 = tk.Label(self.frm4_2,height=1,width=20)
		self.lbl4_8.grid(row=0,column=1,sticky=tk.E)
		self.ety4_1 = tk.Entry(self.frm4_2)
		self.ety4_1.grid(row=0,column=2,columnspan=2)
		tk.Label(self.frm4_2,text='提示:設置子類別預算後請點擊紅色按鍵',fg='dimgrey').grid(row=1,column=0,columnspan=3,sticky='e'+'w')
		self.btn4_1 = tk.Button(self.frm4_2,text='RED',command=self.click_btn4_1,bg='red',width=5)
		self.btn4_1.grid(row=1,column=2,sticky='e',columnspan=2)
		self.cvs4_1 = tk.Canvas(self.frm4_2,width = 437,height = 10,bg='orange')
		self.cvs4_1.grid(row = 2,column = 0,columnspan=4)
		self.rec4_0 = self.cvs4_1.create_rectangle(0,0,0,10,fill="black")
		tk.Label(self.frm4_2,text='餘額:').grid(row=3,column=0)
		self.lbl4_6 = tk.Label(self.frm4_2)
		self.lbl4_6.grid(row=3,column=1,sticky='w')
		tk.Label(self.frm4_2,text='支出:').grid(row=3,column=2)
		self.lbl4_7 = tk.Label(self.frm4_2)
		self.lbl4_7.grid(row=3,column=3)
		
		self.frm4_3 = tk.Frame(self.tab4,bd=1,relief='raised')
		self.frm4_3.pack(side='top',padx=1,pady=1,ipadx=3,ipady=3,fill='both')
		self.lbl4_2 = tk.Label(self.frm4_3,bg = "thistle",width=10)
		self.lbl4_2.grid(row=0,column=0)
		self.lbl4_3 = tk.Label(self.frm4_3,bg = "plum",width=10)
		self.lbl4_3.grid(row=0,column=1)
		self.lbl4_4 = tk.Label(self.frm4_3,bg = "violet",width=10)
		self.lbl4_4.grid(row=0,column=2)
		tk.Label(self.frm4_3,text='剩餘天數').grid(row=1,column=0,ipadx=40)
		tk.Label(self.frm4_3,text='剩餘預算/天').grid(row=1,column=1,ipadx=40)
		tk.Label(self.frm4_3,text='實際支出/天').grid(row=1,column=2,ipadx=40)

		# Listener
		self.root.mainloop()

	def click_clear(self):
		self.lbl_date_selected.config(text = "")
		self.txt_dollar.delete(0, 100)
		self.txt_item.delete(0, 100)
		self.droplist1.current(0)

	def click_clear2(self):
		self.lbl2_date_selected.config(text = "重開後再使用搜尋方法2")
		self.txt2_dollar.delete(0, 100)
		self.txt2_item.delete(0, 100)

	def click_save(self):
		if path.exists(self.fileLoc) == False:	 # 電腦檔名位置，新增book.csv
			with open(self.fileLoc, mode = "a") as book:
				book.write("交易日期, 交易種類, 科目類別, 交易金額, 交易摘要")
		temp = []
		try: # 取得各欄位值，並存檔至csv中，設置防呆機制
			date = self.selected_date
			types = self.droplist1.get()
			country = self.droplistRate.get()
			rate = self.exchange_rate(country)
			subtype = self.droplist1.get()
			dollar = self.txt_dollar.get()
			item = self.txt_item.get()

			if types == "請選擇類別":
				tk.messagebox.showinfo(title = "錯誤", message = "未選擇類別")
			else:
				temp.append(date)
				temp.append(types[-2:])
				temp.append(subtype)
				try:
					Afterrate = int(float(dollar) * float(rate))
					temp.append(Afterrate)
					temp.append(item + "." + country)  # 標註幣別
					# 複寫全部檔案
					self.showall()
					self.transactionList.append(temp)
					with open(self.fileLoc, mode = "w", newline = '') as f2:
						filewriter2 = csv.writer(f2)
						for i in range(len(self.transactionList)):
							filewriter2.writerow(self.transactionList[i])
					tk.messagebox.showinfo(title = "訊息", message = "已存檔")
					self.lbl_date_selected.config(text = "")
					self.droplist1.current(0)
					self.txt_dollar.delete(0, 100)
					self.txt_item.delete(0, 100)
				except:
					tk.messagebox.showinfo(title = "錯誤", message = "金額格式錯誤")
		except:
			tk.messagebox.showinfo(title = "錯誤", message = "未選擇日期")

	def click_exit(self):
		sys.exit()

	def click_rev(self):  # 收入子類別
		self.droplist1.configure(values = ["請選擇類別", "薪資收入", "獎金收入", "投資收入", "副業收入", "臨時收入", "其他收入"])
		self.droplist1.current(0)
		self.btn_rev.configure(bg = "gold")
		self.btn_exp.configure(bg = "white")

	def click_exp(self):  # 支出子類別
		self.droplist1.configure(values = ["請選擇類別", "餐飲支出", "日用品支出", "衣服支出", "交通支出", "水電支出", "娛樂支出", "電話支出", "其他支出"])
		self.droplist1.current(0)
		self.btn_rev.configure(bg = "white")
		self.btn_exp.configure(bg = "gold")

	def calendar_view(self):
		def print_sel():
			self.selected_date = cal.selection_get()  # 資料為datetime格式
			self.lbl_date_selected.config(text = self.selected_date)
			top.destroy()

		top = tk.Toplevel(self.root)

		cal = Calendar(top,
					font = "Arial 14", selectmode = 'day',
					cursor = "hand1", date_pattern = 'yyyy/mm/dd', locale = 'zh_TW')
		cal.pack(fill = "both", expand = True)
		ttk.Button(top, text = "確認", command = print_sel).pack()

	def calendar_view2(self):
		def print_sel2():
			self.selected2_date = cal.selection_get()
			self.lbl2_date_selected.config(text = self.selected2_date)
			top.destroy()

		top = tk.Toplevel(self.root)

		cal = Calendar(top,
					font = "Arial 14", selectmode = 'day',
					cursor = "hand1", date_pattern = 'yyyy/mm/dd', locale = 'zh_TW')
		cal.pack(fill = "both", expand = True)
		ttk.Button(top, text = "確認", command = print_sel2).pack()

	# 整理所有帳簿交易資料到transactionList函式
	def showall(self):
		self.transactionList = []
		with open(self.fileLoc, mode = "r", newline = "\n") as book:
			filereader = csv.reader(book)  # csv讀取器
			for i in filereader:
				self.transactionList.append(i)
		return self.transactionList

	# 檢視所有交易資料
	def click_view2(self):
		self.showall()
		message = ""
		for i in range(len(self.transactionList)):
			if i != 0:
				message += "\n"
			for j in range(5):
				if j != 0:
					message += "\t"
				message += str(self.transactionList[i][j])
				if self.transactionList[i][j] == "":
					message += "無"
		tk.messagebox.showinfo(title = "全部帳簿資料：", message = message)

	def click_search(self):
		""" 讀目前csv檔案，輸出符合關鍵字結果至新視窗 """
		self.showall()
		searchresultList = []
		# 搜尋條件
		searchruleList = [0, 0, ""]
		try:
			searchruleList[0] = datetime.date.strftime(self.selected2_date, "%Y-%m-%d")
		except:
			searchruleList[1] = str(self.txt2_dollar.get())
			searchruleList[2] = str(self.txt2_item.get())

       # 1.用關鍵字查科目類別欄&交易摘要欄位
		# 2.不輸入關鍵字，可直接查日期
		for i in range(len(self.transactionList)):
			criteria = 0  # 匹配分數高則列為搜尋結果
			for j in range(5):
				if j == 0 and searchruleList[0] != 0:  # 核對日期
					if self.transactionList[i][j] == searchruleList[0]:
						criteria += 2
				elif (j == 2 or j == 4) and (searchruleList[2] != 0): # 關鍵字查科目類別、交易摘要
					for k in range(len(str(searchruleList[2]))):
						if searchruleList[2][k] in str(self.transactionList[i][j]):
							criteria += 1
				elif j == 3 and searchruleList[1] != 0:	 # 核對金額
					if self.transactionList[i][j] == searchruleList[1]:
						criteria += 2
			if (i not in searchresultList) and (criteria > 1):  # 配對分數大於等於2者，加入結果列表
				searchresultList.append(i)
		message = ""  # 印出結果
		for i in range(len(searchresultList)):
			if i != 0:
				message += "\n"
			for j in range(5):
				if j != 0:
					message += "\t"
				message += str(self.transactionList[searchresultList[i]][j])

		if len(searchresultList) == 0:  # 如果沒有符合項目，跳出無結果
			tk.messagebox.showinfo(title = "搜尋結果", message = "無結果")
		else:  # 如果有符合項目，跳出結果
			self.root2 = tk.Tk()
			self.root2.title("搜尋結果")
			self.lbl_result = tk.Label(self.root2, text = message)
			self.lbl_result.grid(row = 0, column = 0)

		# 清除所有搜尋資料
		message = ""
		self.lbl2_date_selected.config(text = "")
		self.selected2_date = ""
		self.txt2_dollar.delete(0, 100)
		self.txt2_item.delete(0, 100)

	def click_delete(self):
		self.showall()
		AfterdeleteList = []
		# 搜尋條件
		deleteList = [0, 0, ""]
		try:
			deleteList[0] = datetime.date.strftime(self.selected2_date, "%Y-%m-%d")
			deleteList[1] = str(self.txt2_dollar.get())
			deleteList[2] = str(self.txt2_item.get())
		except:
			tk.messagebox.showinfo(title = "異常", message = "刪除之(輸入)失敗請重試")
		try:
			for i in range(len(self.transactionList)):
				criteria = 0  # 匹配分數高則刪除(至少日期和金額一致，關鍵字至少兩字匹配)
				for k in range(len(str(deleteList[2]))):
					if deleteList[2][k] in str(self.transactionList[i][4]) or deleteList[2][k] in str(self.transactionList[i][2]):
						criteria += 1
				if (deleteList[0] != self.transactionList[i][0]) or (deleteList[1] != self.transactionList[i][3]) or (criteria < 2):
					AfterdeleteList.append(self.transactionList[i])
			with open(self.fileLoc, mode = "w", newline = '') as f2:
				filewriter2 = csv.writer(f2)
				for i in range(len(AfterdeleteList)):
					filewriter2.writerow(AfterdeleteList[i])
			if len(AfterdeleteList) < len(self.transactionList):
				tk.messagebox.showinfo(title = "結果", message = "刪除成功")
			else:
				tk.messagebox.showinfo(title = "結果", message = "未刪除任何資料筆數")
		except:
			tk.messagebox.showinfo(title = "異常", message = "刪除之(條件)失敗請重試")

	def click_yrevb(self):
		xyrList = []
		yyrrevList = [0] * 12
		for i in range(1, 13):
			xyrList.append(i)
		year = self.txt3_year.get()
		self.showall()
		for i in range(len(self.transactionList)):
			if (i == 0) or (self.transactionList[i][1] == "支出"):
				continue
			yr = self.transactionList[i][0].split("-")[0]
			m = int(self.transactionList[i][0].split("-")[1])
			if year != yr:
				continue
			for j in range(len(xyrList)):
				if m == xyrList[j]:
					yyrrevList[j] += int(self.transactionList[i][3])
		x = xyrList
		y = yyrrevList
		plt.bar(x, y, width = 0.5)
		plt.title('Revenue (Monthly)')
		plt.show()

	def click_yexpb(self):
		xyrList = []
		yyrexpList = [0] * 12
		for i in range(1, 13):
			xyrList.append(i)
		year = self.txt3_year.get()
		self.showall()
		for i in range(len(self.transactionList)):
			if (i == 0) or (self.transactionList[i][1] == "收入"):
				continue
			yr = self.transactionList[i][0].split("-")[0]
			m = int(self.transactionList[i][0].split("-")[1])
			if year != yr:
				continue
			for j in range(len(xyrList)):
				if m == xyrList[j]:
					yyrexpList[j] += int(self.transactionList[i][3])
		x = xyrList
		y = yyrexpList
		plt.bar(x, y, width = 0.5)
		plt.title('支出（月）')
		plt.show()

	def click_yrevp(self):
		xyrList = ["薪資收入", "獎金收入", "投資收入", "副業收入", "臨時收入", "其他收入"]
		xyrListnew = ["薪資收入", "獎金收入", "投資收入", "副業收入", "臨時收入", "其他收入"]
		yyrevList = [0] * len(xyrList)
		year = self.txt3_year.get()
		self.showall()
		for i in range(len(self.transactionList)):
			if (i == 0) or (self.transactionList[i][1] == "支出"):
				continue
			yr = self.transactionList[i][0].split("-")[0]
			m = self.transactionList[i][2]
			if year != yr:
				continue
			for j in range(len(xyrList)):
				if m == xyrList[j]:
					yyrevList[j] += int(self.transactionList[i][3])
		xaxis = []
		yaxis = []
		for i in range(len(xyrList)):
			if yyrevList[i] == 0:
				continue
			xaxis.append(xyrListnew[i])
			yaxis.append(yyrevList[i])
		plt.figure(figsize = (6,9))    # 顯示圖框架大小
		labels = xaxis  # 製作圓餅圖的類別標籤
		size = yaxis # 製作圓餅圖的數值來源
		plt.pie(size, 							   # 數值
				labels = labels,                # 標籤
				autopct = "%1.1f%%",            # 將數值百分比並留到小數點一位
				pctdistance = 0.6,              # 數字距圓心的距離
				textprops = {"fontsize" : 12},  # 文字大小
				shadow=True)                    # 設定陰影
		plt.axis('equal')                                          # 使圓餅圖比例相等
		plt.title("Annual Revenue (Category)", {"fontsize" : 18})  # 設定標題及其文字大小
		plt.legend(loc = "best")                                   # 設定圖例及其位置為最佳
		plt.show()

	def click_yexpp(self):
		xyrList = ["餐飲支出", "日用品支出", "衣服支出", "交通支出", "水電支出", "娛樂支出", "電話支出", "其他支出"]
		xyrListnew = ["餐飲支出", "日用品支出", "衣服支出", "交通支出", "水電支出", "娛樂支出", "電話支出", "其他支出"]
		yyexpList = [0] * len(xyrList)
		year = self.txt3_year.get()
		self.showall()
		for i in range(len(self.transactionList)):
			if (i == 0) or (self.transactionList[i][1] == "收入"):
				continue
			yr = self.transactionList[i][0].split("-")[0]
			m = self.transactionList[i][2]
			if year != yr:
				continue
			for j in range(len(xyrList)):
				if m == xyrList[j]:
					yyexpList[j] += int(self.transactionList[i][3])
		xaxis = []
		yaxis = []
		for i in range(len(xyrList)):
			if yyexpList[i] == 0:
				continue
			xaxis.append(xyrListnew[i])
			yaxis.append(yyexpList[i])
		plt.figure(figsize = (6,9))    # 顯示圖框架大小
		labels = xaxis  # 製作圓餅圖的類別標籤
		size = yaxis # 製作圓餅圖的數值來源
		plt.pie(size, 							   # 數值
				labels = labels,                # 標籤
				autopct = "%1.1f%%",            # 將數值百分比並留到小數點一位
				pctdistance = 0.6,              # 數字距圓心的距離
				textprops = {"fontsize" : 12},  # 文字大小
				shadow=True)                    # 設定陰影
		plt.axis('equal')                                          # 使圓餅圖比例相等
		plt.title("月支出（種類）", {"fontsize" : 18})  # 設定標題及其文字大小
		plt.legend(loc = "best")                                   # 設定圖例及其位置為最佳
		plt.show()

	def click_mrevb(self):
		daydict = {'1':31, '2':29, '3':31, '4':30, '5':31, '6':30, '7':31, '8':31, '9':30, '10':31, '11':30, '12':31}
		year = self.txt3_year.get()
		month = int(self.txt3_month.get())
		xyrList = []
		day_num = daydict[str(month)]
		for i in range(1, day_num+1):
			xyrList.append(i)
		yyrevList = [0] * day_num
		self.showall()
		for i in range(len(self.transactionList)):
			if (i == 0) or (self.transactionList[i][1] == "支出"):
				continue
			yr = self.transactionList[i][0].split("-")[0]
			m = int(self.transactionList[i][0].split("-")[1])
			d = int(self.transactionList[i][0].split("-")[2])
			if (year != yr) or (month != m):
				continue
			for j in range(len(xyrList)):
				if d == xyrList[j]:
					yyrevList[j] += int(self.transactionList[i][3])
		x = xyrList
		y = yyrevList
		plt.bar(x, y, width = 0.5)
		plt.title('收入（日）')
		plt.show()

	def click_mexpb(self):
		daydict = {'1':31, '2':29, '3':31, '4':30, '5':31, '6':30, '7':31, '8':31, '9':30, '10':31, '11':30, '12':31}
		year = self.txt3_year.get()
		month = int(self.txt3_month.get())
		xyrList = []
		day_num = daydict[str(month)]
		for i in range(1, day_num+1):
			xyrList.append(i)
		yyrevList = [0] * day_num
		self.showall()
		for i in range(len(self.transactionList)):
			if (i == 0) or (self.transactionList[i][1] == "收入"):
				continue
			yr = self.transactionList[i][0].split("-")[0]
			m = int(self.transactionList[i][0].split("-")[1])
			d = int(self.transactionList[i][0].split("-")[2])
			if (year != yr) or (month != m):
				continue
			for j in range(len(xyrList)):
				if d == xyrList[j]:
					yyrevList[j] += int(self.transactionList[i][3])
		x = xyrList
		y = yyrevList
		plt.bar(x, y, width = 0.5)
		plt.title('支出（日）')
		plt.show()

	def click_mrevp(self):
		xyrList = ["薪資收入", "獎金收入", "投資收入", "副業收入", "臨時收入", "其他收入"]
		xyrListnew = ["薪資收入", "獎金收入", "投資收入", "副業收入", "臨時收入", "其他收入"]
		yyrevList = [0] * len(xyrList)
		year = self.txt3_year.get()
		month = int(self.txt3_month.get())
		self.showall()
		for i in range(len(self.transactionList)):
			if (i == 0) or (self.transactionList[i][1] == "支出"):
				continue
			yr = self.transactionList[i][0].split("-")[0]
			m = int(self.transactionList[i][0].split("-")[1])
			if (year != yr) or (month != m):
				continue
			for j in range(len(xyrList)):
				if self.transactionList[i][2] == xyrList[j]:
					yyrevList[j] += int(self.transactionList[i][3])
		xaxis = []
		yaxis = []
		for i in range(len(xyrList)):
			if yyrevList[i] == 0:
				continue
			xaxis.append(xyrListnew[i])
			yaxis.append(yyrevList[i])
		plt.figure(figsize = (6,9))    # 顯示圖框架大小
		labels = xaxis  # 製作圓餅圖的類別標籤
		size = yaxis # 製作圓餅圖的數值來源
		plt.pie(size, 							   # 數值
				labels = labels,                # 標籤
				autopct = "%1.1f%%",            # 將數值百分比並留到小數點一位
				pctdistance = 0.6,              # 數字距圓心的距離
				textprops = {"fontsize" : 12},  # 文字大小
				shadow = True)                    # 設定陰影
		plt.axis('equal')                                          # 使圓餅圖比例相等
		plt.title("月收入（種類）", {"fontsize" : 18})  # 設定標題及其文字大小
		plt.legend(loc = "best")                                   # 設定圖例及其位置為最佳
		plt.show()

	def click_mexpp(self):
		xyrList = ["餐飲支出", "日用品支出", "衣服支出", "交通支出", "水電支出", "娛樂支出", "電話支出", "其他支出"]
		xyrListnew = ["餐飲支出", "日用品支出", "衣服支出", "交通支出", "水電支出", "娛樂支出", "電話支出", "其他支出"]
		yyrevList = [0] * len(xyrList)
		year = self.txt3_year.get()
		month = int(self.txt3_month.get())
		self.showall()
		for i in range(len(self.transactionList)):
			if (i == 0) or (self.transactionList[i][1] == "收入"):
				continue
			yr = self.transactionList[i][0].split("-")[0]
			m = int(self.transactionList[i][0].split("-")[1])
			if (year != yr) or (month != m):
				continue
			for j in range(len(xyrList)):
				if self.transactionList[i][2] == xyrList[j]:
					yyrevList[j] += int(self.transactionList[i][3])
		xaxis = []
		yaxis = []
		for i in range(len(xyrList)):
			if yyrevList[i] == 0:
				continue
			xaxis.append(xyrListnew[i])
			yaxis.append(yyrevList[i])
		plt.figure(figsize = (6,9))    # 顯示圖框架大小
		labels = xaxis  # 製作圓餅圖的類別標籤
		size = yaxis # 製作圓餅圖的數值來源
		plt.pie(size, 							   # 數值
				labels = labels,                # 標籤
				autopct = "%1.1f%%",            # 將數值百分比並留到小數點一位
				pctdistance = 0.6,              # 數字距圓心的距離
				textprops = {"fontsize" : 12},  # 文字大小
				shadow = True)                    # 設定陰影
		plt.axis('equal')                                          # 使圓餅圖比例相等
		plt.title("Monthly Expense (Category)", {"fontsize" : 18})  # 設定標題及其文字大小
		plt.legend(loc = "best")                                   # 設定圖例及其位置為最佳
		plt.show()

	def exchange_rate(self, country):
		if country == "NTD":
			return 1
		else:
			fileLocCurrency = "currency.csv"  # 檔案儲存位置

			ssl._create_default_https_context = ssl._create_unverified_context
			dfs = pandas.read_html('http://rate.bot.com.tw/xrt?Lang=zh-TW')

			currency = dfs[0]
			currency = currency.iloc[0:20, 0:5]
			currency.columns = ['curren','bankbye','banksell','shortbankbuy', 'rate']
			currency['curren'] = currency['curren'].str.extract('\\((\\w+)\\)')
			currency.to_csv(fileLocCurrency)
			# 開檔並取得幣別對匯率的Dict
			RateList = []
			RateDict = dict()  # 這次使用
			with open(fileLocCurrency, mode = "r", newline = "\n") as ratebook:
				filereaderCurrency = csv.reader(ratebook)  # csv讀取器
				j = 0
				for i in filereaderCurrency:
					RateList.append(i)
					foreignCurrency = RateList[j][1]
					rate = RateList[j][5]  # 取得銀行即期賣出匯率
					RateDict[foreignCurrency] = rate
					j += 1
			return RateDict[country]

	def click_btn4_0(self):
		'''獲取數據'''
		budget_num = self.ety4_0.get().strip()  # 設定預算金額
		if budget_num == '':  # 檢查輸入值
			tk.messagebox.showwarning(title = '提示', message = '預算金額未設定')
		try:
			budget_num = float(budget_num)
		except ValueError:
			tk.messagebox.showwarning(title = '提示', message = '請輸入數字格式')
			self.ety4_0_var.set('')
			return
		if budget_num < 0:
			tk.messagebox.showwarning(title = '提示', message = '預算金額不能為負')
			self.ety4_0_var.set('')
			return
		elif budget_num == 0:
			tk.messagebox.showwarning(title = '提示', message = '預算金額不能為零')
			self.ety4_0_var.set('')
			return
		budget_year = int(self.txt4_year.get())  # 當前或指定年份
		budget_mon = int(self.txt4_month.get())  # 當前或指定月份
		exp_tol = 0   # 總實際支出
		self.showall()  # 調取檔案取得記賬資料
		del self.transactionList[0]
		for i in range(len(self.transactionList)):
			yr = int(self.transactionList[i][0].split("-")[0])
			m = int(self.transactionList[i][0].split("-")[1])
			if (budget_year == yr) and (budget_mon == m):
				if self.transactionList[i][1] == "支出":
					exp_tol += float(self.transactionList[i][3])
		budget_rem = budget_num - exp_tol  # 預算餘額
		'''frm4_1內容'''
		self.lbl4_0.config(text = str(exp_tol))  # 支出合計
		self.lbl4_1.config(text = str(budget_rem), fg = 'black')  # 目前餘額
		bdg_rad = (exp_tol / budget_num)*360  # 環形顯示花費及文字百分比
		self.cvs_fill = self.cvs4.create_arc(10, 10, 90, 90, start = 90, extent = bdg_rad, outline = 'red', width = 10, style = 'arc')
		self.cvs4.itemconfig(self.cvs_text, text = str(int((budget_rem / budget_num) * 100)) + '%')
		try:  # 清除環形顏色
			self.cvs4.delete(self.cvs_fill)
		except AttributeError:
			pass
		'''frm4_3內容'''
		day_now = datetime.datetime.now().day  # 獲取當前日期
		budget_monthday = calendar.monthrange(budget_year,budget_mon)[1]  # 當前或指定年月的天數
		if (budget_year != self.year_now) or (budget_mon != self.month_now):  # 輸入日期非當前年月時的處理
			self.lbl4_2.config(text = '0')
			self.lbl4_3.config(text = '0')
			self.lbl4_4.config(text = '%.2f' % (exp_tol / budget_monthday))
		else:  # 輸入日期為當前年月
			budget_remmon = budget_monthday - day_now  # 當前年月剩餘天數
			self.lbl4_2.config(text = str(budget_remmon))
			self.lbl4_3.config(text = '%.2f'%(budget_rem / budget_remmon))
			self.lbl4_4.config(text = '%.2f'%(exp_tol / day_now))
		'''當預算不足時的處理'''
		if budget_rem <= 0:
			self.lbl4_1.config(text = '0', fg = 'red')
			self.cvs_fill = self.cvs4.create_oval(10, 10, 90, 90, outline = 'red', width = 10)
			self.cvs4.itemconfig(self.cvs_text, text = '0%')
			self.lbl4_3.config(text = '0')

	def click_btn4_1(self):
		budget_year = int(self.txt4_year.get())
		budget_mon = int(self.txt4_month.get())
		bdg_exp_dict = {"餐飲支出":0, "日用品支出":0, "衣服支出":0, "交通支出":0, "水電支出":0, "娛樂支出":0, "電話支出":0, "其他支出":0}
		self.showall()
		del self.transactionList[0]
		for i in range(len(self.transactionList)):
			yr = int(self.transactionList[i][0].split("-")[0])
			m = int(self.transactionList[i][0].split("-")[1])
			if (budget_year == yr) and (budget_mon == m):
				if self.transactionList[i][1] == "支出":
					for key in bdg_exp_dict:
						if self.transactionList[i][2] == key:
							bdg_exp_dict[key] += float(self.transactionList[i][3])  # 獲取帳本數據
		budget_category = self.txt4_0.get()  # 選定子類別
		if budget_category == '請選擇類別————':  # 避免子類別為默認提示
			tk.messagebox.showwarning(title = '提示', message = '未選定支出類別')
			return
		budget_ctgbdg = self.ety4_1.get().strip()  # 子類別設定預算
		if budget_ctgbdg == '':  # 檢查輸入預算值
			tk.messagebox.showwarning(title = '提示', message = '子類別預算金額未設定')
			return
		try:
			budget_ctgbdg = float(budget_ctgbdg)
		except ValueError:
			tk.messagebox.showwarning(title = '提示', message = '請輸入數字格式')
			self.ety4_0_var.set('')
			return
		if budget_ctgbdg < 0:
			tk.messagebox.showwarning(title = '提示', message = '子類別預算金額不能為負')
			self.ety4_0_var.set('')
			return
		budget_ctgexp = bdg_exp_dict[budget_category]  # 子類別實際支出
		budget_ctgrem = budget_ctgbdg - budget_ctgexp  # 子類別餘額
		self.lbl4_7.config(text = float(budget_ctgexp))
		self.lbl4_6.config(text = float(budget_ctgrem))
		if budget_ctgbdg == 0:
			self.btn4_1.config(text = '0%')
			self.lbl4_6.config(text = '預算外支出', fg = 'hotpink')
			rec4_0_wid = 437
			self.cvs4_1.itemconfig(self.rec4_0, fill = 'hotpink')
		else:
			rec4_0_wid = (budget_ctgexp / budget_ctgbdg) * 437
			if budget_ctgrem <= 0:
				self.btn4_1.config(text = '0%')
			else:
				self.btn4_1.config(text = str(int((budget_ctgrem / budget_ctgbdg) * 100)) + '%')
		try:  # 清除進度條顏色
			self.cvs4_1.delete(self.rec4_1)
		except AttributeError:
			pass
		self.cvs4_1.coords(self.rec4_0,(0,0,rec4_0_wid,10))

	def choose_txt4_0(self,event):
		budget_category = self.txt4_0.get()  # 選定子類別
		if budget_category == '請選擇類別————':  # 控制選定子類別
			tk.messagebox.showwarning(title = '提示', message = '未選定支出類別')
			return
		self.lbl4_8.config(text = '請設定' + budget_category + '預算:', fg = 'dimgrey')

tab = Tab()  # 使用上述class，所有功能都包在這個class裡面
