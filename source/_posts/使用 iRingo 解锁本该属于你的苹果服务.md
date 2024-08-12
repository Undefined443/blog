为什么别人的 Spotlight 可以通过航班号查询航班信息，而我的不行？为什么别人的 Spotlight 可以直接看英超联赛的比分信息？为什么我的 Apple News 打不开？这其实是因为这些功能都被锁区了。Apple 通过你的网络信息判断你所处的区域，如果判断你处于中国大陆，那么这些功能就不会为你启用。

## Setup

不过，通过网络开发工具，我们可以修改网络信息来欺骗 Apple 误以为我们处于其他地区，从而解锁这些功能。可以使用的网络开发工具有 [Shadowrocket](https://apps.apple.com/us/app/shadowrocket/id932747118?l=zh-Hans-CN)、[Quantumult X](https://apps.apple.com/us/app/quantumult-x/id1443988620?l=zh-Hans-CN)、[Surge](https://apps.apple.com/us/app/surge-5/id1442620678?l=zh-Hans-CN)、[Loon](https://apps.apple.com/us/app/loon/id1373567447?l=zh-Hans-CN)、[Stash](https://apps.apple.com/us/app/stash/id1596063349?l=zh-Hans-CN) 。

安装了网络开发工具后，我们需要安装插件来修改网络请求。[iRingo](https://github.com/VirgilClyne/iRingo) 项目为我们提供了众多解锁各种服务的插件。安装需要的插件后，我们还需要一个控制面板 [BoxJs](https://docs.boxjs.app/) 来控制插件的行为。

> 关于解锁服务的具体操作，请移步 iRingo 项目。

## Usage

安装好相关插件后，首先打开全局代理，关闭蜂窝移动网络，打开飞行模式，打开 Wi-Fi，然后打开地图再杀掉后台，再次打开地图，如果发现地图左下角没有显示“高德地图”，则说明当前网络位置信息已不在中国大陆。此时可以尝试使用我们需要的服务，比如搜索“英超联赛”，如果出现了赛事信息，那么说明服务解锁成功。

|搜索赛事信息|搜索航班信息|搜索 Siri 知识|
|:--:|:--:|:--:|
|![image](https://s2.loli.net/2024/07/17/ngQd74xUsi2Y1GF.png =200x)|![image](https://s2.loli.net/2024/07/17/e2Tif5Xy3BGDbpC.png =200x)|![image](https://img2024.cnblogs.com/blog/2778973/202407/2778973-20240717174622042-1014555581.png =200x)|
