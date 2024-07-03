# 匠魂三基础开发手册 (1.19.2)

## 导航页

<ul>

<li><details open>
<summary><a href="/手册集/新材料开发.md"><b>新材料开发</b></a></summary>
<ul>
<li><a href="/手册集/新材料开发.md#完善材料的数据包部分">数据包部分</a></li>
<li><a href="/手册集/新材料开发.md#完善材料的资源包部分">资源包部分</a></li>
</ul>
</details></li>

<li><details open>
<summary><a href="/手册集/配方.md"><b>配方开发</b></a></summary>
<ul>
<li><a href="/手册集/配方开发.md#融化">融化配方</a></li>
<li><a href="/手册集/配方开发.md#铸件台浇筑">浇筑配方</a></li>
<li><a href="/手册集/配方开发.md#铸件台压模">压模配方</a></li>
<li><a href="/手册集/配方开发.md#融化燃料">自定义燃料</a></li>
</ul>
</details></li>

</ul>

## 关于

* 关于手册
  
> * _为什么要写这本手册？_
> 
> 答：截止项目开始（2024.6.12），我并未找到一个适合新手入门的匠魂三1.19.2的开发教程，匠魂本身的wiki也很潦草

* 关于项目

> * _发起者_
>
> 小狼娘fufu，英文名LitterWolf_fufu。也可以叫fufu或小狼
> 
> * _合作的朋友_
>
> Qi-Month 柒月 https://github.com/Qi-Month
>
> 如果项目有疏漏或者问题，请向我提交Issue或者PR，感谢！

* 推荐Mods
  * [JEI](https://www.curseforge.com/minecraft/mc-mods/jei)(主要用于查看配方以及配方ID)
  * [TConJEI](https://www.curseforge.com/minecraft/mc-mods/tconjei)(主要用于查看材料信息,可以不用点开匠魂的手册)(1.19版本似乎会崩...1.18应该没问题,1.19的朋友就最好别加了...除非自己测试过不会崩溃...)
  * [Jade](https://www.curseforge.com/minecraft/mc-mods/jade)(主要用于在屏幕上方显示实体以及方块或物品信息)
  * [Jade Addons](https://www.curseforge.com/minecraft/mc-mods/jade-addons)(Jade的附属Mod,用于显示更多详细信息)
  * [Json Things](https://www.curseforge.com/minecraft/mc-mods/json-things)(用于制作全局数据包和资源包,不用担心换存档时忘记迁移数据包的问题,同时也是[**匠魂杂物**](https://www.curseforge.com/minecraft/mc-mods/tinkers-things-json)的前置Mod,这个Mod同时也和匠魂有一定联动,可以用这个Mod来制作简单的工具)
  * [KubeJS](https://www.curseforge.com/minecraft/mc-mods/kubejs)(主要用于获取手上的物品信息,手持物品输入`kjs_hand`(1.18)或`kjs hand`(1.19+)即可获取信息,点击就可以复制)(当然如果你会KubeJS魔改的话那肯定是如虎添翼了)