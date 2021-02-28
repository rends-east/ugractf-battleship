# ugractf-battleship

Задание:

![image](https://user-images.githubusercontent.com/73061822/109430238-5724e300-7a11-11eb-8c8e-cfb9e8838f51.png)

Сайт:

![image](https://user-images.githubusercontent.com/73061822/109430245-686def80-7a11-11eb-8080-c2a732ad16fb.png)


Если мы начнем игру и выстрелим, то увидим:

![image](https://user-images.githubusercontent.com/73061822/109430273-94897080-7a11-11eb-9c4f-e6c12601db59.png)

Пришло в голову несколько раз быстро выстрелить. Заметил, что при выстрелах в течение одной секунды дистанция до корабля не меняется

Тогда сделаем так:
1)выстрелим 2 раза в 2 разных места
2)проведем две окружности, найдем их пересечения
3)выстрелим в получившиеся две точки, одна из них будет кораблем
4)сделать все это в течение секунды
5)????
6)PROFIT

'''python

import requests as rq
import math

def get_intercetions(x0, y0, r0, x1, y1, r1):
    d=math.sqrt((x1-x0)**2 + (y1-y0)**2)
    if d > r0 + r1 :
        return None
    if d < abs(r0-r1):
        return None
    if d == 0 and r0 == r1:
        return None
    else:
        a=(r0**2-r1**2+d**2)/(2*d)
        h=math.sqrt(r0**2-a**2)
        x2=x0+a*(x1-x0)/d
        y2=y0+a*(y1-y0)/d
        x3=x2+h*(y1-y0)/d
        y3=y2-h*(x1-x0)/d
        x4=x2-h*(y1-y0)/d
        y4=y2+h*(x1-x0)/d

        d = rq.post("https://battleship.q.2021.ugractf.ru/b6afef3a9019c9a3/fire", data={'x': str(x3), 'y': str(y3)},
                    cookies=r.cookies)
        print(d.text)
        d = rq.post("https://battleship.q.2021.ugractf.ru/b6afef3a9019c9a3/fire", data={'x': str(x4), 'y': str(y4)},
                    cookies=r.cookies)
        print(d.text)

        return (x3, y3, x4, y4)

r = rq.post("https://battleship.q.2021.ugractf.ru/b6afef3a9019c9a3/reset")
print(r.text)
r = rq.post("https://battleship.q.2021.ugractf.ru/b6afef3a9019c9a3/fire", data={'x': 0, 'y': 0}, cookies=r.cookies)
print(r.text)
jsonResponse = r.json()
r1 = float(jsonResponse["distance"])
r = rq.post("https://battleship.q.2021.ugractf.ru/b6afef3a9019c9a3/fire", data={'x': 400, 'y': 0}, cookies=r.cookies)
jsonResponse = r.json()
r2 = float(jsonResponse["distance"])
print(r.text)
print(get_intercetions(0, 0, r1, 400, 0, r2))
'''

