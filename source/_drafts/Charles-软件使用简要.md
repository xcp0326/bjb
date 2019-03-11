---
title: Charles 软件使用简要
tags: [Charles]
---
- Focused Hosts
    - 在View/Focused Hosts弹窗里，添加要关注的host（比如guanjia.xxkuaipao.com)，在Sequence的界面右下checkbox勾选Focused，打开系统代理后，session里的request列表就会只显示Focused了的Hosts的相关请求了。
    - 另一个添加Focused Hosts的方法是，在session request列表里面选中要关注的Host，右键选中Focus
    - 取消Focus，右键取消勾选的Focus
    - Focused Hosts要与勾选Sequence的Focused checkbox同时使用，才能有效果
    - Focused Hosts的另一个体现是Structure界面有区分Focused Hosts与Other Hosts
    - 还可以直接Ignore某个Host，同上，右键Host，选择Ignore
- Viewer Mappings
    - 根据请求的content-type和返回的response的content-type来线上