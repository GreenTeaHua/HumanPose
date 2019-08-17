# HumanPose
人体骨骼14点调参心得

原始工程来源于https://github.com/edvardHua/PoseEstimationForMobile

但是其中有些BUG，我略作修改，如果你没法跑起来原工程，可以把我的training文件夹弄过去覆盖掉原工程的。

# 150M计算量调参心得（双核A53约50ms）

**（1）stage_num=3，分辨率192x192调参阶段**

stage_num=3时，网络设计捉襟见肘，channel和kernel都不大。

这一阶段包括zq1_cpm, zq7_cpm, zq8_cpm, zq9_cpm, zq10_cpm。

PCKh0.2从62.4提高到64.81, 模型结构请查阅代码, 精度请查阅xls。

**（2）stage_num=2，分辨率192x192调参阶段**

stage_num=2时，网络设计可以活动的地方较大，随便设计一个立马超过先前stage_num=3

这一阶段baseline是zq11_cpm， PCKh0.2达到65.98， 越改越差的包括zq12_cpm, zq13_cpm, zq14_cpm, zq15_cpm, zq22_cpm, zq23_cpm。

zq24_cpm与zq11_cpm接近，PCHk0.2达到65.79

模型结构请查阅代码，精度请查阅xls

**（3）stage_num=2,分辨率224x224调参阶段**

考虑到heatmap分辨率太小会导致无论无何都不可能太准，尝试加大分辨率。但不得不缩减channel和kernel。

这一阶段baseline是zq16_cpm （模型设计思路来源于zq11_cpm）， PCKh0.2达到65.64， 越改越差的包括zq17_cpm, zq18_cpm, zq19_cpm, zq20_cpm

第一次改好的结构是zq21_cpm, PCKh0.2达到66.73

# 80M计算量调参心得（双核A53约30ms）

**（1）分辨率160x160调参阶段**

这一阶段包括zq25_cpm、zq29_cpm，PCKh0.2只有不到57

**（2）分辨率192x192调参阶段**

这一阶段包括zq26_cpm, zq27_cpm, zq28_cpm, zq30_cpm, zq31_cpm, zq32_cpm, zq33_cpm,zq34_cpm, zq35_cpm， 由于设计模型时没有严格保证计算量一致，模型计算量从77M逐渐增加到了80M，最后zq35_cpm的PCKh0.2最高，达到59.37