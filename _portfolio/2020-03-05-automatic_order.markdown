---
layout: portfolio
title:  "Python을 활용한 마스크 자동 구매 프로그램"
date:   2020-03-05 09:13:34 +0300
image: /assets/images/portfolio/automatic_order/automation_order.png
author: Yunwoong Kim
tags:   ["RPA"]
typora-root-url: ..
---

코로나 19 로 인해 마스크 품귀 현상이 지속되면서 마스크 구매 경쟁이 치열해졌습니다. 공적 마스크를 배분하는 마스크 5부제까지 시행됐지만, 여전히 마스크 구하기는 어려워서 마스크 구매가 가능한 시점에 알림을 주거나 자동으로 구매하는 프로그램을 만들었습니다.

<h4 align="center">
  마스크 자동 구매
</h4>
<div align="center">
  <img src="https://img.shields.io/badge/python-v3.6-blue.svg"/>
  <img src="https://img.shields.io/badge/PyQt5-v5.11.3-blue.svg"/>
  <img src="https://img.shields.io/badge/BeautifulSoup4-v4.8.2-blue.svg"/>
  <img src="https://img.shields.io/badge/selenium-v3.141.0-blue.svg"/>
  <img src="https://img.shields.io/badge/PyAutoGUI==-v0.9.48-blue.svg"/>
</div>



## 추진배경

공급의 부족으로 마스크 가격은 계속 치솟고 기존 가격을 유지하는 착한 마스크라 불리는 마스크는 오전 9시에 쉴 새 없이 새로고침을 누르며 클릭을 하여도 활성화된 [구매하기] 버튼조차 구경하기 어렵습니다. 공대생들은 실시간으로 주문한 마스크를 할머니는 5시간씩 줄을 서도 하루 5장을 구매하지 못했다는 기사가 나오기도 했습니다.



## 추진내용

1. 처음에는 마스크를 게릴라로 판매하는 사이트를 크롤링하여 재고 존재 시 문자로 알람을 보내주는 방식으로 개발하였습니다.

   문제는 받은 문자를 클릭하고 들어가더라도 이미 구매는 불가합니다. 문자 송/수신 시간도 존재하고 사이트는 이미 폭주 상태이기 때문입니다.

   

   <div align="center">
     <img src="/assets/images/portfolio/automatic_order/automation_order_1.png" width="80%">
   </div>

   

   

2. 이후 수정하여 구매가 가능한 시점에 구매하기 버튼까지 클릭하고 결제 대기 상태에 대기하도록 했습니다.

   역시 문제는 이 시점에 결제하기를 눌렀는데도 품절로 구매가 안 되었죠. 사실상 판매 오픈 후 대략 40초 ~ 1분 사이에 품절이 되기 때문입니다.

   <div align="center">
     <img src="/assets/images/portfolio/automatic_order/automation_order_2.png" width="30%">
   </div>

   

3. 네이버스토어 결제 시 비빌번호 입력창(네이버페이)의 경우 입력창 이미지는 랜덤하게 변경되기 때문에 pyautogui를 이용하여 자동으로 인식하여 클릭하도록 반영했습니다.

   <div align="center">
     <img src="/assets/images/portfolio/automatic_order/automation_order_3.png" width="30%">
   </div>

   

4. 네이버페이 결제를 위해 창을 전환하는 시간과 자동으로 인식하는 약간의 시간이 존재하여 조금 더 빠른 결제를 위해 결제수단을 "나중에 결제"로 변경하였습니다.

   <div align="center">
     <img src="/assets/images/portfolio/automatic_order/automation_order_4.png" width="70%">
   </div>



## Automatic order with selenium

시스템을 확인하여 retina 디스플레이인 경우, pyautogui 이미지 클릭에 대한 좌표에 대한 후처리를 위해 별도로 체크해줍니다.

```python
import os
import threading
import platform
import subprocess
import time
import pyautogui
from pynput.mouse import Button, Controller
import cv2
import datetime
from datetime import timedelta
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import ElementClickInterceptedException, imeoutException, NoSuchElementException,  NoSuchWindowException, WebDriverException

if os.name == 'nt':
    from ctypes import windll

    is_retina = False
    if platform.system() == "Darwin":
        is_retina = subprocess.call("system_profiler SPDisplaysDataType | grep 'retina'", shell=True)
else:
    is_retina = True

mouse = Controller()
```
N개로 수행 할 수 있도록 Browser 라는 클래스로 만듭니다. Input으로 User ID, Password, Pay Number를 받습니다.

```python
class Browser:
    def __init__(self, user_id, password, pay_number):
        self.timeout = 2
        self.user_id = user_id
        self.password = password
        self.pay_number = pay_number

        self.base_width = 1920
        self.base_height = 1080

        # 모니터 해상도 가져오기
        self.scr_width, self.scr_height = pyautogui.size()
        print('width={0}, height={1}'.format(self.scr_width, self.scr_height))

        self.w_rat = self.base_width / self.scr_width
        self.h_rat = self.base_height / self.scr_height

        if os.name == 'nt':
            user32 = windll.user32
            user32.SetProcessDPIAware()
```
크롬을 사용합니다. 창을 활성화하지 않을 경우 option으로 headless를 추가합니다.

```python
    def popup(self):
        chrome_options = Options()
        #chrome_options.add_argument('--headless')
        chrome_options.add_argument('--no-sandbox')
        chrome_options.add_argument('--disable-gpu')
        chrome_options.add_argument('window-size=1920x1080')  # 가져올 크기를 결정

        self.driver = webdriver.Chrome('chromedriver', chrome_options=chrome_options)
```

구매상품에 따라 옵션이 존재 할 수 있습니다. (Ex - 사이즈, 색상 등) 구매하고자 하는 옵션을 입력받고 존재하지 않은 경우 (품절 포함) 다른 상품을 선택하여 구매할 지 입력합니다.

```python
    def setOption(self, url, combo=True, option='', another=True):
        self.url = url
        self.combo = combo
        self.option = option
        self.another = another
```

로그인을 수행합니다. 자동 로그인 방지를 우회하기 위해 스크립트로 id와 password를 변경합니다.

```python
    def login(self):
        # Naver Login
        self.driver.get('https://nid.naver.com/nidlogin.login')
        self.driver.execute_script("document.getElementsByName('id')[0].value=\'" + self.user_id + "\'")
        self.driver.execute_script("document.getElementsByName('pw')[0].value=\'" + self.password + "\'")
        WebDriverWait(self.driver, self.timeout).until(EC.element_to_be_clickable((By.XPATH, '//*[@id="frmNIDLogin"]/fieldset/input'))).click()
        # self.driver.find_element_by_xpath('//*[@id="frmNIDLogin"]/fieldset/input').click()

        print('{} 로그인'.format(self.user_id))
        time.sleep(1)
```

상품의 재고 여부를 조회합니다. 재고가 존재할 때까지 반복적으로 수행합니다.

```python
    def checkStock(self):
        print('{} 상품 재고 조회'.format(self.user_id))
        fiedset = WebDriverWait(self.driver, self.timeout).until(EC.element_to_be_clickable((By.CLASS_NAME, 'prd_num')))

        while True:
            self.driver.refresh()
            try:
                not_good = self.driver.find_element_by_class_name('not_goods')
            except NoSuchElementException:
                not_good = ''
                break

        if self.combo:
            try:
                self.option_combo = WebDriverWait(self.driver, 2).until(EC.element_to_be_clickable((By.CLASS_NAME, '_combination_option')))
                all_options = self.option_combo.find_elements_by_tag_name("option")

                try:
                    product_list = WebDriverWait(self.driver, self.timeout).until(EC.element_to_be_clickable((By.CLASS_NAME, 'opt_price')))
                    index = next(idx for idx, option in enumerate(all_options) if
                                 option.get_attribute("text") not in ["사이즈", "필수옵션"] and option.get_attribute(
                                     "title") == self.option)

                    # 입력한 옵션값이 있을 경우 품절여부 판단
                    if index > 0:
                        sold_out = all_options[1].get_attribute("text").find("품절") > -1

                        # 입력한 옵션은 품절이지만 다른 상품구매도 원한다면 변경하여 진행
                        if sold_out and self.another:
                            try:
                                index = next(idx for idx, option in enumerate(all_options) if
                                             option.get_attribute("text") not in ["사이즈", "필수옵션"] and option.get_attribute(
                                                 "text").find("품절") == -1)
                            except StopIteration:
                                print('전상품 품절')
                                time.sleep(600)
                        elif sold_out:
                            print('{} 품절 - 재조회'.format(self.option))
                            self.driver.get(self.url)
                            thread = threading.Thread(target=self.checkStock)
                            thread.start()

                except StopIteration:
                    print('입력값에 해당하는 옵션 없음'.format(self.option))
                    if self.another:
                        try:
                            index = next(idx for idx, option in enumerate(all_options) if
                                         option.get_attribute("text") not in ["사이즈", "필수옵션"] and option.get_attribute(
                                             "text").find("품절") == -1)
                        except StopIteration:
                            print('전상품 품절')
                            time.sleep(600)
                    else:
                        print('더이상 진행하지 않음')
                        time.sleep(600)
            except TimeoutException:
                print('서버폭주로 비활성화')
                self.driver.get(self.url)
                thread = threading.Thread(target=self.checkStock)
                thread.start()

            if self.another:
                input_option = all_options[index].get_attribute("title")
                self.option_combo.send_keys(input_option)
            else:
                try:
                    self.option_combo.send_keys(self.option)
                except AttributeError:
                    print('Combobox Error')
                    self.driver.get(self.url)
                    thread = threading.Thread(target=self.checkStock)
                    thread.start()

        self.checkout()
```
구매를 진행합니다. 위에 설명해 드린 것처럼 자동결제까지 구현했다가 좀 더 빠른 구매를 위해 "나중에 결제"로 변경하였습니다.

```python
     def checkout(self):
        try:
            self.selectItem(self.option_combo)
            
            try:
                WebDriverWait(self.driver, self.timeout).until(EC.element_to_be_clickable((By.CLASS_NAME, 'cart'))).click()
            except TimeoutException:
                print('페이지오류 - 재조회')
                self.driver.get(self.url)
                thread = threading.Thread(target=self.checkStock)
                thread.start()
            
            try:
                cart_popup = WebDriverWait(self.driver, 1).until(EC.element_to_be_clickable((By.CLASS_NAME, '_layer_cart_add')))
                WebDriverWait(cart_popup, 1).until(EC.element_to_be_clickable((By.CLASS_NAME, 'button_close'))).click()
                print('{} 장바구니'.format(self.user_id))
            except TimeoutException:
                pass

            try:
                WebDriverWait(self.driver, self.timeout).until(EC.element_to_be_clickable((By.PARTIAL_LINK_TEXT, '구매하기'))).click()
            except TimeoutException:
                print('페이지오류 - 재조회')
                self.driver.get(self.url)
                thread = threading.Thread(target=self.checkStock)
                thread.start()

            print('{} 구매하기'.format(self.user_id))

            try:
                msg_popup = WebDriverWait(self.driver, 1).until(EC.element_to_be_clickable((By.CLASS_NAME, '_layer_order_check')))
                WebDriverWait(msg_popup, 1).until(EC.element_to_be_clickable((By.CLASS_NAME, 'button_close'))).click()
                self.selectItem(self.option_combo)
                WebDriverWait(self.driver, self.timeout).until(EC.element_to_be_clickable((By.PARTIAL_LINK_TEXT, '구매하기'))).click()
            except TimeoutException:
                pass

            payment = WebDriverWait(self.driver, self.timeout).until(EC.element_to_be_clickable((By.CLASS_NAME, 'payment')))
            paymethod_list = WebDriverWait(payment, self.timeout).until(EC.element_to_be_clickable((By.CLASS_NAME, 'paymethod_list')))
            paymethod = WebDriverWait(paymethod_list, self.timeout).until(EC.element_to_be_clickable((By.XPATH, '//label[@for="generalPayments"]')))
            paymethod.click()

            #payment = WebDriverWait(self.driver, 1).until(EC.element_to_be_clickable((By.CLASS_NAME, 'payment_list')))
            #btn_payNext = WebDriverWait(self.driver, self.timeout).until(EC.element_to_be_clickable((By.XPATH,'//li[4]/span/span')))
            #btn_payNext.click()

            print('URL : {}'.format(self.driver.current_url))

            try:
                msg_popup = WebDriverWait(self.driver, 1).until(EC.element_to_be_clickable((By.XPATH, "//form[@id='orderForm']/div/div[5]/div/div[2]/ul/li[3]/div/label")))
                WebDriverWait(msg_popup, 0).until(EC.element_to_be_clickable((By.XPATH, "(//button[@type='button'])[14]"))).click()
            except TimeoutException:
                pass

            payment_item = paymethod.find_element_by_xpath('//label[@for="pay18"]')
            payment_item.click()

            btn_allAgree = WebDriverWait(self.driver, self.timeout).until(EC.element_to_be_clickable((By.XPATH, '//*[@id="allAgree"]')))
            btn_allAgree.click()

            self.order()

        except ElementClickInterceptedException:
            print('버튼 비활성화')
            thread = threading.Thread(target=self.checkStock)
            thread.start()
            # 10분간 대기 이후 Thread는 종료
            #time.sleep(600)
        except TimeoutException:
            print('서버폭주로 비활성화')
            self.driver.get(self.url)
            thread = threading.Thread(target=self.checkStock)
            thread.start()
        except NoSuchWindowException as e:
            print('NoSuchWindowException')
            print('Browser종료')
            # 10분간 대기 이후 Thread는 종료
            time.sleep(600)
        except WebDriverException as e:
            print('WebDriverException')
            print('Browser종료')
            # 10분간 대기 이후 Thread는 종료
            time.sleep(600)
```
모든 수행은 Thread로 수행됩니다.

```python
    def selectItem(self, option_combo):
        if self.combo:
            try:
                purchase_list = self.driver.find_elements_by_class_name('_purchase_unit')

                if purchase_list:
                    for item in purchase_list:
                        item_text = item.find_elements_by_tag_name('em')[0].text
                        if self.option != item_text:
                            btn_del = item.find_element_by_partial_link_text("삭제")
                            btn_del.click()

                    purchase_list = self.driver.find_elements_by_class_name('_purchase_unit')

                    if purchase_list == []:
                        option_combo.send_keys(self.option)
                else:
                    option_combo.send_keys(self.option)
            except NoSuchElementException:
                option_combo.send_keys(self.option)

    def order(self):
        btn_pay = WebDriverWait(self.driver, self.timeout).until(
            EC.element_to_be_clickable((By.CLASS_NAME, 'btn_payment')))

        # storing the current window handle to get back to dashbord
        main_page = self.driver.current_window_handle

        btn_pay.click()

        print('{} 구매완료'.format(self.user_id))
        # 10분간 대기 이후 Thread는 종료
        time.sleep(600)

    def startBrowser(self):
        self.popup()

        self.login()

        # Url로 이동
        self.driver.get(self.url)
        print('{} Url로 이동'.format(self.user_id))

        self.checkStock()
```
```python
browser1 = Browser(아이디, 패스워드, 페이번호)
browser1.setOption('https://smartstore.naver.com/mfbshop/products/4819214174', True, 'Small (소)', False)
browser_thread1 = threading.Thread(target=browser1.startBrowser)
browser_thread1.start()
```



## Summary

위에 소스에는 생략되었지만 pyautogui 를 이용하여 네이버페이를 클릭하여 자동결제도 가능하게 구현했습니다. 단점은 듀얼모니터의 경우는 주모니터만 인식되고 약간의 Delay가 발생합니다. 하지만 간단한 방법으로 화면 캡처한 결과를 좌표로 활용하여 마우스와 키보드를 제어하도록 구현하는데 좋은 것 같습니다.

```python
distinct_num = [char for char in self.pay_number]
num_position = {}

for num in list(set(distinct_num)):
    img_num = cv2.imread('img//{num}.png'.format(num=num))
    img_height, img_width, img_channel = img_num.shape
    resize_img = cv2.resize(img_num,(int(img_width * self.w_rat), int(img_height * self.h_rat)))
    num_position[num] = pyautogui.locateCenterOnScreen(resize_img, confidence=0.9)

for num in distinct_num:
    print(num_position[num])
    if num_position[num] is None:
    	break
    else:
        if is_retina:
            pyautogui.moveTo(num_position[num][0] / 2, num_position[num][1] / 2)
            mouse.click(Button.left)
        else:
            pyautogui.click(num_position[num])
```
다만 retina 디스플레이를 대비하여 아래와 같이 이미지를 두벌로 준비해야 합니다.



<div align="center">
  <img src="/assets/images/portfolio/automatic_order/automation_order_5.png" width="100%">
</div>



마지막으로 UI를 포함하여 개발하였습니다. UI를 개발할 때에는 다른 사용자가 사용한다는 생각으로 개발하다보니 고민해야 할 게 항상 많은 것 같습니다.

<div align="center">
  <img src="/assets/images/portfolio/automatic_order/automation_order_6.png" width="100%">
</div>

프로그램을 이용하여 대량으로 구매하진 않았습니다. 마스크 품귀 현상이 지속되고 이미 많은 사람이 기술(UiPath, Selenium, Macro 등)을 이용하여 구매하고 있었죠. **기술과 정보의 격차로 인한 마스크 수급의 차이**가 있었는데 마스크 페이지가 열리면 "새로고침"을 하고 있는 저를 보면서 **나는 과연 지금까지 나의 편의를 위해 개발을 한 적이 있었는지 의문**에서 개발하였습니다.

다양한 방법을 고민하는 시간이 재밌었습니다.