# nonebot_plugin_gamedraw

## 介绍

基于爬取wiki实现自动更新的抽卡插件（阴阳师爬取自官网）  
 
某一些卡池抓取的资料包含角色的属性等，如果你希望做一个查看角色武器资料的话或许可以帮上忙）

**注意：此requirements不包含nonebot2及依赖以及 [nonebot_plugin_apscheduler](https://github.com/nonebot/plugin-apscheduler) (定时任务)插件**

## 目前支持的抽卡
* 原神
* 明日方舟
* 赛马娘
* 坎公骑冠剑
* 公主连结（国/台）
* 碧蓝航线
* 命运-冠位指定（FGO）
* 阴阳师

## 支持自定义抽卡 
* 在对应json文件中添加格式相同的数据，并在对应的图片文件夹下添加角色名称的 拼音.png 图片（不添加图片则以全黑图片代替）  
* 可在draw_card_config/draw_card_config.json修改抽卡概率

## 限定区分情况
| 名称                   |      自动更新数据     | 自动更新UP池 |  限定区分  | 
| ----------------------| :--------------------:| :--------:  | :------:
| 原神                   |         √            |     √        |  √
| 明日方舟               |         √             |    √       |    √
| 赛马娘                 |         √            |     √      |    √
| 坎公骑冠剑              |         √            |   √        |    ×
| 公主连结                |         √           |    ×        |  ~~√~~区分部分
| 碧蓝航线                |         √            |  √       |      √
| 命运-冠位指定（FGO）     |         √            |   ×        |   ~~√~~区分部分
| 阴阳师                 |          √           |   ×        |  ~~√~~区分部分

## 命令
### 抽卡命令
'   .* ?方舟[1-9|一][0-9]{0,2}[抽|井]'                       
'   .* ?原神(武器|角色)?池?[1-9|一][0-9]{0,2}[抽|井]'    
'   .* ?马娘卡?[1-9|一][0-9]{0,2}[抽|井]'    
'   .* ?坎公骑冠剑武?器?[1-9|一][0-9]{0,2}[抽|井]'  
'   .* ?(pcr|公主连结|公主连接|公主链接|公主焊接)[1-9|一][0-9]{0,2}[抽|井]'  
'   .* ?碧蓝航?线?(轻型|重型|特型|活动)池?[1-9|一][0-9]{0,2}[抽]'  
'   .* ?fgo[1-9|一][0-9]{0,2}[抽]'  
'   .* ?阴阳师[1-9|一][0-9]{0,2}[抽]'  
<br>

#### 注（特别抽卡命令详解）

1.**原神**（当武器和角色无UP池时，所有抽卡命令都指向 '常驻池'）
  * 原神N抽  （常驻池）
  * 原神角色N抽  （角色UP池）
  * 原神武器N抽  （武器UP池）

2.**赛马娘**
  * 赛马娘N抽 （抽马）
  * 赛马娘卡N抽  （抽卡）

3.**坎公骑冠剑**
  * 坎公骑冠剑N抽 （抽角色）
  * 坎公骑冠剑武器N抽   （抽武器）

4.**碧蓝航线**
  * 碧蓝轻型N抽  （轻型池）
  * 碧蓝重型N抽  （重型池）
  * 碧蓝特型N抽  （特型池）  
  * 碧蓝活动N抽  （活动池）  

### 其他命令
'重置原神抽卡'（重置保底）  
'重载原神卡池'  
'重载方舟卡池'  
'重载赛马娘卡池'  
'重载坎公骑冠剑卡池'

### 更新命令
'更新明日方舟信息'  
'更新原神信息'  
'更新赛马娘信息'  
'更新赛坎公骑冠剑信息'  
'更新pcr信息'  
'更新碧蓝航线信息'  
'更新fgo信息'  
'更新阴阳师信息'  

## 使用
  ```
  1.是否需要变更资源路径嘛？（默认路径 data/draw_card/）
    如果需要变更路径，在.env文件中添加DRAW_PATH绝对路径【注：如果你的项目开启了debug冷重载，建议更换路径！】
    示例：
      DRAW_PATH = "D:/xxx/data/draw_card/"
   
  2.是否需要关闭某些抽卡呢？（即不下载资源不使用对应抽卡命令）
    如果需要关闭某些卡池，在.env文件中添加对应的卡池FLAG并设置为False
    # 不添加或不设置默认为True
    
    PRTS_FLAG = False       # 明日方舟
    GENSHIN_FLAG = False    # 原神
    PRETTY_FLAG = False     # 赛马娘
    GUARDIAN_FLAG = False   # 坎公骑冠剑
    PCR_FLAG = False        # 公主连结
    AZUR_FLAG = False       # 碧蓝航线
    FGO_FLAG = False        # 命运-冠位指定（FGO）
    ONMYOJI_FLAG = False    # 阴阳师
  
  3.是否需要更改一些其他配置呢？（不添加或不设置默认为 False）
    在.env文件中添加对应 属性，以下为默认值
    
    PCR_TAI = True          # 公主连结使用台服卡池（即添加国服未时装角色）删除原pcr.json文件再重启bot自动更新即可
    SEMAPHORE = 5           # 限制并发数量(主要是 碧蓝航线 和 FGO 数据)
    
  4.在bot入口文件添加
    nonebot.load_plugin("nonebot_plugin_gamedraw")
  ```
    
## 更新：

### 2022/3/20

 * 修复了方舟抽卡图文不符的bug [pull/44](https://github.com/HibiKier/nonebot_plugin_gamedraw/pull/44)
 * 修复了方舟许多干员抽不出来的bug [pull/44](https://github.com/HibiKier/nonebot_plugin_gamedraw/pull/44)

### 2022/2/26

 * 优化代码 [pull/40](https://github.com/HibiKier/nonebot_plugin_gamedraw/pull/40)

### 2022/2/25
 * 优化抽卡结果
 * 修复原神抽卡统计
 * 碧蓝航线新增活动池
 * 碧蓝航线自动更新UP [@pull/38](https://github.com/HibiKier/nonebot_plugin_gamedraw/pull/38)

### 2022/2/22

 * 优化抽卡图片结果 [@pull/35](https://github.com/HibiKier/nonebot_plugin_gamedraw/pull/35)

### 2022/2/18

 * 修复bwiki页面变动导致pcr数据无法获取

### 2022/2/11

 * 优化代码，使用pydantic解析配置等 [@pull/31](https://github.com/HibiKier/nonebot_plugin_gamedraw/pull/31)

### 2022/2/7

 * 修复原神bwiki页面修改导致爬取错误引起导入错误

### 2022/1/27

 * 适配nb beat1 [@pull/27](https://github.com/HibiKier/nonebot_plugin_gamedraw/pull/27)

### 2022/1/6

 * 修复UP卡池无法正常初始化
 * 原神提供大保底机制

### 2021/12/29

 * 修复碧蓝航线无法正确获取数据

### 2021/12/16

 * 修复当UP角色不存在本地数据内时报错

### 2021/11/23

 * 修复原神云堇无法正确导入角色数据
 * 修复碧蓝wiki格式变更导致无法获取数据

### 2021/10/1

 * 修复碧蓝航线新外框导致无法正确初始化碧蓝航线抽卡数据

### 2021/9/1

 * 修复原神UP池含有‘版本更新后’无法正确更新开启时间（现当时间含有‘版本更新后’时会以结束时间 - 20天作为开始时间）

### 2021/7/29

 * 赛马娘UP池公告更新

### 2021/7/5

 * 将所有print替换为nonebot.log

### 2021/7/2

 * 修复当bwiki未上线最新UP角色时（无法更新到），会导致抽卡报错

### 2021/6/29

 * 可恶，为什么bwiki马娘UP公告一直改排版！（此次更新已向前兼容）

### 2021/6/28

 * 再修复赛马娘UP池更新

### 2021/6/24

 * 修复赛马娘UP卡池更新
 * 明日方舟UP卡池改为从官网获取数据（修复）
 * 赛马娘，坎公骑冠剑增加十连保底

### 2021/6/22

 * 修复碧蓝航线wiki无头像图片时报错

### 2021/6/20

 * 坎公骑冠剑实现自动更新UP池（测试）

### 2021/6/17
 * 添加配置项SEMAPHORE，用于限制并发数(并发数过高会导致数据更新不完全，需要删除xxx.json重新更新)
 * 区分赛马娘支援卡限定
 * 添加赛马娘UP卡池（测试）
 * 添加了一个requirements [issues #8](https://github.com/HibiKier/nonebot_plugin_gamedraw/issues/8)
 * 修复 当资源图片无法打开时，会删除并在下次更新时下载
 * 修复 当原神 限定/常驻UP 属于 未知 时不会在数据中更新该角色的 限定/常驻UP 数据导致报错
 * 修复 当更新UP卡池超时时会使用上一次更新的 UP卡池数据(会判断是否在开启时间内)
 * 修改SEMAPHORE默认值为 5

### 2021/6/13
 * 重新区分了 明日方舟 限定
 * 明日方舟抽卡添加规则，当抽卡次数大于50抽时，第50抽开始每抽六星概率增加2%(抽到六星后重新计算)
 * 明日方舟bwiki更改，不需要再从额外数据中获取 '获取途径' 数据
### 2021/6/9
 * 修复 up卡池 比官方开启时间要慢一天
### 2021/6/7
 * 将 Linux 系统下角色名称内字符 '/' 替换为 '\\'
 * 碧蓝航线通过获取 建造时间 实现限定区分（需要删除原azur.json）
### 2021/6/3
 * 重载卡池时会发送当前的卡池名称与图片
 * 添加原神每日重载卡池的定时任务与手动触发命令
 * 修复Linux系统下字符 '/'（现会将字符串中的禁止字符删除）

****

<details>
<summary>更久前的更新</summary>
 
### 2021/6/22
 
  * 修复碧蓝航线wiki缺失头像图片错误

### 2021/6/2
  * 原神实现自动更新当前UP池子（角色和武器），并将抽卡命令分为 原神N抽（常驻），原神武器N抽（武器UP），原神角色N抽（角色UP），当没有UP池时，所有命令都会指向常驻池【注：需要删除原 genshin.json，更新下载额外数据】
### 2021/5/31
  * 将 明日方舟 获取额外数据（招募方式）改为异步多任务访问
  * 为 碧蓝航线 抽卡图片添加头像外框（更清晰的显示品质）【注：若之前已存在azur.json，删除该文件重启即刻下载头像框图片（不下载抽卡也不会报错）】   
  * 每日自动更新改为异步多任务
  * 命运-冠位指定（FGO）！！！
  * 阴阳师！！！
### 2021/5/29
  * 碧蓝航线 重新区分了 限定（大部分应该都区分了）
### 2021/5/28
  * 感谢 [AdamXuD](https://github.com/AdamXuD) 的pr，修复 2021/5/27 更新后的 bug：因重复字符串 "base64://" 导致图片无法发送
  * 修复当处于Windows系统时，下载图片的名称若包含 \/:*?"<>| 导致报错（现会将字符串中的禁止字符删除）
### 2021/5/27
  * 公主连结区分国服/台服
  * 启动时下载资源改为异步多任务下载（提速！）
  * 碧蓝航线！！！
### 2021/5/26
  * 添加更改路径的配置
  * 添加卡池的开关的配置
  * 公主连结！！！
### 2021/5/24
  * 修复了明日方舟文本资料 某些角色的获取途径 含有html文本的信息
  * 坎公骑冠剑！！！
### 2021/5/23
  * 修复明日方舟卡池无法区分 商店限定 的问题 [issues #3](https://github.com/HibiKier/nonebot_plugin_gamedraw/issues/3)
### 2021/5/21
  * 将抽卡方法向py3.9以下兼容 [issues #2](https://github.com/HibiKier/nonebot_plugin_gamedraw/issues/2)
### 2021/5/19
  * 实现 [issues #1](https://github.com/HibiKier/nonebot_plugin_gamedraw/issues/1) 提供的比较完善的抽卡逻辑）感谢# [@Jerry-FaGe](https://github.com/Jerry-FaGe)
### 2021/5/18
  * 明日方舟实现爬取游戏公告自动更新当前的up卡池
 
 </details>
  
****

## 注意
1.第一次启动会下载信息和图片资源  
2.默认资源路径是data/draw_card/  
3.含有定时任务，大部分情况不需要手动触发更新命令  
4.若图片缺失，抽卡时则会以全黑的头像替代（一般是正式服还未上线的角色或物品）  

## Todo

  * 各池子的UP（大概）
  * 历史卡池切换
  * 抽卡统计


