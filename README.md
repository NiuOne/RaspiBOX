# RaspiBOX

#ZH：  这是一个DIY带风扇的树莓派外壳，尤其适用于树莓派3B+这样的发热大户
#EN：  This is a box to put your RaspberryPi (2B\3B\3B+)in. It usrally use for the Heatmaker called Raspi 3B+,

#最近弄了一块树莓派3B+，到手后发现说明上要求必须要用DC5V，2.5A的电源适配器。较前几代的树莓派来说功耗增加了好多。
无奈iPhone的1.5A充电头无法满足它，只好用ipad的大头供电才勉强带起来，在开机到加载桌面的几秒钟会出现电量告警，开机启动后就不再报警了。

#俗话说香蕉大香蕉皮也大，电流这么大，发热必定大。
#正常轻载负荷下学习结束后拔掉电源主板竟然烫手，连带着USB LAN GPIO排针等非常热。
这么热万一热死怎么办，不如给它做个38°C机箱吧。
查阅一番资料后，说干就干。

一、资料收集
    搜索了网上各种树莓派的盒子，概括下：
    
    1、亚克力切割 ：简单便宜
    2、3D打印外壳 ：
    3、注塑外壳：
    4、游戏机外壳：对于怀旧、游戏爱好者来说
二、实现过程

    1、首先测量树莓派PCB的全身尺寸，画出草图作为机械设计的参照
    2、根据板型和初步机械结构设计思路，找出发热区，（此处差一张热成像照片），合理规划风道，找好风扇安装位置，设计风扇进气孔排气孔。
    3、根据散热器尺寸设计风扇进气孔（皮一下，放个萌萌的树莓君进去，后来因为大小和3d打印拆支撑问题，去掉了叶子）。
    4、到处文件进行打印设置，这里选择了支撑少的仰卧姿势
    5、分层查看切片效果、支撑效果
    6、初步完成外壳的设计，进行组合验证，修饰边边角角。    
    7、到处文件进行打印设置，这里选择了支撑少的仰卧姿势
三、测试过程

    1、安装主板，安装风扇，装好开机。哔~哔哔~~
    2、去掉键盘鼠标，直接SSH登陆
    3、用Python 编写硬件信息采集Demo，这里再Windows下写好，XShell传进去的。
    4、装散热之前，裸板测试 CPU温度54.2°C
    5、装散热器后，开风扇测试 CPU温度38.1°C，完全满足之前的38度机箱预期。
    
四、The End

    图纸设计还有不足的地方，希望逐步完善。欢迎各路大神点评。
    
附代码

    //测试CPU温度代码 
    #Begin Code
    import os
    # Return CPU temperature as a character string 
    def getCPUtemperature():
    res = os.popen('vcgencmd measure_temp').readline()
    return(res.replace("temp=","").replace("'C\n",""))
    # Return RAM information (unit=kb) in a list 
    # Index 0: total RAM 
    # Index 1: used RAM 
    # Index 2: free RAM 
    def getRAMinfo():
    p = os.popen('free')
    i = 0
    while 1:
    i = i + 1
    line = p.readline()
    if i==2:
    return(line.split()[1:4])
    # Return % of CPU used by user as a character string 
    def getCPUuse():
    return(str(os.popen("top -n1 | awk '/Cpu\(s\):/ {print $2}'").readline().strip()))
    # Return information about disk space as a list (unit included) 
    # Index 0: total disk space 
    # Index 1: used disk space 
    # Index 2: remaining disk space 
    # Index 3: percentage of disk used 
    def getDiskSpace():
    p = os.popen("df -h /")
    i = 0
    while 1:
    i = i +1
    line = p.readline()
    if i==2:
    return(line.split()[1:5])
    # CPU informatiom
    CPU_temp = getCPUtemperature()
    CPU_usage = getCPUuse()
    # RAM information
    # Output is in kb, here I convert it in Mb for readability
    RAM_stats = getRAMinfo()
    RAM_total = round(int(RAM_stats[0]) / 1000,1)
    RAM_used = round(int(RAM_stats[1]) / 1000,1)
    RAM_free = round(int(RAM_stats[2]) / 1000,1)
    # Disk information
    DISK_stats = getDiskSpace()
    DISK_total = DISK_stats[0]
    DISK_used = DISK_stats[1]
    DISK_perc = DISK_stats[3]
    if __name__ == '__main__':
    print('')
    print('CPU Temperature = '+CPU_temp) 
    print('CPU Use = '+CPU_usage)
    print('')
    print('RAM Total = '+str(RAM_total)+' MB')
    print('RAM Used = '+str(RAM_used)+' MB')
    print('RAM Free = '+str(RAM_free)+' MB')
    print('') 
    print('DISK Total Space = '+str(DISK_total)+'B')
    print('DISK Used Space = '+str(DISK_used)+'B')
    print('DISK Used Percentage = '+str(DISK_perc))

    #END CODE


#End ReadMe
