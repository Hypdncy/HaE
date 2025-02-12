<div align="center">
<img src="images/logo.png" style="width: 20%" />
<h4><a href="https://gh0st.cn/HaE/">赋能白帽，高效作战！</a></h4>
<h5>第一作者： <a href="https://github.com/gh0stkey">EvilChen</a>（中孚信息元亨实验室）<br>第二作者： <a href="https://github.com/0chencc">0chencc</a>（米斯特安全团队）<br>第三作者： <a href="https://github.com/vaycore">vaycore</a>（独立安全研究员）</h5>
</div>

## 项目介绍

**HaE**是一款**网络安全（数据安全）领域**下的框架式项目，采用了**乐高积木式**模块化设计理念，实现对HTTP消息（包含WebSocket）精细化的标记和提取。

通过运用**多引擎**的自定义正则表达式，HaE能够准确匹配并处理HTTP请求与响应报文（包含WebSocket），对匹配成功的内容进行有效的标记和信息抽取，从而提升网络安全（数据安全）领域下的**漏洞和数据分析效率**。

> 随着现代化Web应用采用前后端分离的开发模式，日常漏洞挖掘的过程中，捕获的HTTP请求流量也相应增加。若想全面评估一个Web应用，会花费大量时间在无用的报文上。**HaE的出现旨在解决这类情况**，借助HaE，您能够**有效减少**测试时间，将更多精力集中在**有价值且有意义**的报文上，从而**提高漏洞挖掘效率**。

GitHub项目地址：https://github.com/gh0stkey/HaE

GitCode项目地址：https://gitcode.com/gh0stkey/HaE

**所获荣誉**:

1. [入选2022年KCon兵器谱](https://mp.weixin.qq.com/s/JohMsl1WD29LHCHuLf8mVQ)
2. [入选GitCode G-Star项目](https://gitcode.com/gh0stkey/HaE)

**注意事项**:

1. HaE 3.0版本开始采用`Montoya API`进行开发，使用新版HaE需要升级你的BurpSuite版本（>=2023.12.1）。
2. HaE 2.6版本后对规则字段进行了更新，因此无法适配<=2.6版本的规则，请用户自行前往[规则转换页面](https://gh0st.cn/HaE/ConversionRule.html)进行转换。
3. 自定义HaE规则必须用左右括号`()`将所需提取的表达式内容包含，例如你要匹配一个**Shiro应用**的响应报文，正常匹配规则为`rememberMe=delete`，在HaE的规则中就需要变成`(rememberMe=delete)`。

## 使用方法

插件装载: `Extender - Extensions - Add - Select File - Next`

初次装载`HaE`会从Jar包中加载离线的规则库，如果更新可以点击`Reinit`进行重新初始化。内置规则库地址可以在Github上找到：`https://github.com/gh0stkey/HaE/blob/master/src/main/resources/rules/Rules.yml`。

配置文件（`Config.yml`）和规则文件（`Rules.yml`）会放在固定目录下：

1. Linux/Mac用户的配置文件目录：`~/.config/HaE/`
2. Windows用户的配置文件目录：`%USERPROFILE%/.config/HaE/`

除此之外，您也可以选择将配置文件存放在`HaE Jar包`的同级目录下的`/.config/HaE/`中，**以便于离线携带**。

### 规则释义

HaE目前的规则一共有8个字段，详细的含义如下所示：

| 字段      | 含义                                                                                                                                                                                                   |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name      | 规则名称，主要用于简短概括当前规则的作用。                                                                                                                                                               |
| F-Regex     | 规则正则，主要用于填写正则表达式。在HaE中所需提取匹配的内容需要用`(`、`)`将正则表达式进行包裹。|
| S-Regex     | 规则正则，作用及使用同F-Regex。S-Regex为二次正则，可以用于对F-Regex匹配的数据结果进行二次的匹配提取，如不需要的情况下可以留空。|
| Format     | 格式化输出，在NFA引擎的正则表达式中，我们可以通过`{0}`、`{1}`、`{2}`…的方式进行取分组格式化输出。默认情况下使用`{0}`即可。          |
| Scope     | 规则作用域，主要用于表示当前规则作用于HTTP报文的哪个部分。支持请求、响应的行、头、体，以及完整的报文。                                                                                                                                               |
| Engine    | 正则引擎，主要用于表示当前规则的正则表达式所使用的引擎。**DFA引擎**：对于文本串里的每一个字符只需扫描一次，速度快、特性少；**NFA引擎**：要翻来覆去标注字符、取消标注字符，速度慢，但是特性（如:分组、替换、分割）丰富。 |
| Color     | 规则匹配颜色，主要用于表示当前规则匹配到对应HTTP报文时所需标记的高亮颜色。在HaE中具备颜色升级算法，当出现相同颜色时会自动向上升级一个颜色进行标记。                                                                                                                               |
| Sensitive | 规则敏感性，主要用于表示当前规则对于大小写字母是否敏感，敏感（`True`）则严格按照大小写要求匹配，不敏感（`False`）则反之。                                                                                      |

## 优势特点

1. **功能**：通过对HTTP报文的颜色高亮、注释和提取，帮助使用者获取有意义的信息，**聚焦高价值报文**。
2. **界面**：清晰可视的界面设计，以及**简洁的界面交互**，帮助使用者更轻松的了解和配置项目，**避免`多按钮`式的复杂体验**。
3. **查询**：将HTTP报文的高亮、注释和提取到的相关信息**集中在一个数据面板**，可以一键查询、提取信息，从而提高测试和梳理效率。
4. **算法**：内置高亮颜色的升级算法，当出现相同颜色时**会自动向上升级一个颜色**进行标记，**避免`屠龙者终成恶龙`场景**。
5. **管理**：**融入BurpSuite的项目数据管理**，当使用BurpSuite进行项目存储时HaE数据也会一并存储。
6. **实战**：官方规则库和规则字段作用功能，都是**基于实战化场景总结输出**的，**以此提高数据的有效性、精准性发现**。

| 界面名称                  | 界面展示                                              |
| ------------------------ | ---------------------------------------------------- |
| Rules（规则管理）     | <img src="images/rules.png" style="width: 80%" />     |
| Config（配置管理）    | <img src="images/config.png" style="width: 80%" />    |
| Databoard（数据集合） | <img src="images/databoard.png" style="width: 80%" /> |
| MarkInfo（数据展示） | <img src="images/markinfo.png" style="width: 80%" /> |

## 赞赏榜单

以下名单以赞赏时间进行排序，不分先后，如有遗留可联系项目作者进行补充。

| ID       | 金额     |
| -------- | -------- |
| 毁三观大人      | 200元     |
| ttt      | 50元     |
| C_soon5  | 66.66元  |
| 1wtbb    | 25元     |
| Deep     | 66.66元  |
| NaTsUk0  | 50元     |
| Kite     | 39元    |
| 红色键盘 | 99.99元  |
| 曾哥     | 188.88元 |
| vaycore     | 188.88元 |
| xccc     | 168.00元 |

## 支持项目

如果你觉得HaE好用，可以打赏一下作者，给作者持续更新下去的动力！

<div align=center>
<img src="images/reward.jpeg" style="width: 30%" />
</div>

## 404StarLink 2.0 - Galaxy

![404StarLink Logo](https://github.com/knownsec/404StarLink-Project/raw/master/logo.png)

`HaE` 是 404Team [星链计划2.0](https://github.com/knownsec/404StarLink2.0-Galaxy) 中的一环，如果对 `HaE` 有任何疑问又或是想要找小伙伴交流，可以参考星链计划的加群方式。

- [https://github.com/knownsec/404StarLink2.0-Galaxy#community](https://github.com/knownsec/404StarLink2.0-Galaxy#community)
