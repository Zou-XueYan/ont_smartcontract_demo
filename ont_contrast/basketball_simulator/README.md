# 篮球比赛模拟

### **1 模型建立**

#### **元素抽象**

篮球比赛逻辑比较复杂，参与的变量比较多，在这里简单抽象为以下几种元素：

- 比赛：包含多个状态，包括时间、比分等

- 球员：包含防守、进攻两种状态，进攻有传球、投篮两种选择

#### **流程描述**

1. ```
   1. 初始化比赛，包括球员能力、名称，比赛的初始时间、比分
   2. 进行一轮模拟：在每轮模拟中，假设有24s时间，在其间进攻球员可能投篮或者传球，命中率依据进攻能力与防守能力以及一定的随机性来得到，随机几次动作后，结束该轮模拟并且改写比赛状态。
   3. 当比赛时间耗尽时，结束模拟，展示数据统计
   ```


### **2 技术选型**

选择编写本体智能合约，结合SDK实现比赛模拟，语言为python，开发工具pycharm，编译工具smartx，部署环境Polaris。



### **3 工程设计和实现**

#### **设计架构及功能**

![设计图](https://github.com/Zou-XueYan/ont_smartcontract_demo/blob/master/imgs/basketball.png)

右边为智能合约部分，下面介绍接口作用：

| **接口**             | **参数**                                     | **功能**                              |
| -------------------- | -------------------------------------------- | ------------------------------------- |
| init                 | **随机数种子**                               | **初始化合约状态**                    |
| next_round           | **随机数种子**                               | **模拟下一轮比赛**                    |
| random_int_from_zero | **随机数边界；随机数种子**                   | **得到****[0,num-1]****内的随机整数** |
| shoot                | **进攻球员；防守球员；当前轮剩余时间；球队** | **模拟进攻动作**                      |
| pass_ball            | **持球人；防守球员；球队**                   | **模拟传球操作或个人进攻动作**        |
| pick_ball_holder     | **球队；是否是开场跳球**                     | **对某球队选择当前持球人**            |
| jump_ball            | **无**                                       | **模拟开场跳球**                      |
| rand_player          | **球队**                                     | **在球队内随机挑选球员**              |
| get_team_scores      | **无**                                       | **返回当前比分**                      |
| get_curr_time        | **无**                                       | **返回当前比赛时间**                  |

左边为SDK实现的客户端，实现了部署合约、调用合约的功能：

| 函数                         | 功能               |
| ---------------------------- | ------------------ |
| **deploy_game**              | **部署合约**       |
| **init_game**                | **初始化合约**     |
| **next_round**               | **模拟下一轮比赛** |
| **get_game_time**            | **获得比赛时间**   |
| **get_team_scores**          | **获得比分**       |
| **get_technical_statistics** | **获得技术统计**   |

#### **使用方法**

 ```
1. smartx编译代码，并将字节码、abi复制到utils下的tools模块的code和abi，生成sdk对象等必需工具；
2. Simulator部署合约至测试网；
3. Simulator初始化合约状态，即比赛状态
4. Simulator调用接口模拟每轮比赛：利用随机数，随机产生球员动作，命中率等参数，使整个过程更加逼近现实
5. 等待结束事件，结束后返回数据统计和最终比分
 ```

#### **代码实现**


代码包含：合约、客户端、单元测试

