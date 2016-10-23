#作业L1 2.10题强化版（引入迎面风阻）
#作业L2 2.10题进一步升级，发展“超级辅助精确打击系统”
（考虑炮弹初始发射的时候发射角度误差度，速度有5%的误差，迎面风阻误差10%，以炮弹落点与打击目标距离差平方均值最小为优胜）<br/>
以上所有计算需要考虑海拔高度的影响，使用绝热模型进行计算，误差描述使用最简单的均匀分布描述（也就是在误差范围内每个取值概率是一样的）。<br/>
完成L2作业的同学，下次上课可以比赛，对100km的目标看谁打得最精确！
##以下为2.10的代码
<pre><code>
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
import pylab as pl
import math
class cannon_shell:
    def __init__(self,time_step=0.01,):
        self.dt = time_step
        self.l_x=[0]
        self.l_y=[0]
    def run(self):
        print('distanceX->')
        distance_x=float(input())
        print('distanceY(Y<0)->')
        distance_y=float(input())

        x=0
        y=0
        angle=9
        vmin = []
        Angle=[]
        while(angle<=50):                 #扫描角度，求各角度下，发射距离一定，炮弹的发射速度，求出速度最小值
            angle=angle+1
            x0=0
            v0=150
            while(x0<=distance_x):         #扫描速度，求角度一定，发射距离一定时的炮弹发射速度

                cos1 = math.cos(angle*math.pi/180)
                sin1 = math.sin(angle*math.pi/180)
                vx = v0*cos1
                vy = v0*sin1
                while(y>=distance_y):  #仅支持distance_y<0
                    g = 9.8             #求角度一定，速度一定时，发射距离
                    a = (1 - 6.5e-3 * y / 288) ** 2.5
                    x = x + vx * self.dt
                    y = y + vy * self.dt
                    v = math.sqrt(vx ** 2 + vy ** 2)
                    vx = vx - 4e-5 * v * vx * self.dt * a
                    vy = vy - (4e-5 * v * vy * self.dt * a + g) * self.dt

                x0=x
                v0=v0+10
            vmin.append(v0)
            Angle.append(angle)
        vm=min(vmin)
        print(vm-10)
a=cannon_shell()
a.run()
</code></pre>