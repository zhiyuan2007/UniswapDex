# UniswapDex
> 以太坊 Ropsten测试链 去中心化交易所

# 部署教程
## 前言
继部署完自己的erc20代币合约后, 当然还要了解一下现在称之为DeFi明珠的Uniswap, 还有什么是比自己部署一遍了了解的更快呢

## [在线预览](https://shixiaofeia.github.io/UniswapDex/#/swap)

### Uniswap简单介绍
简而言之, Uniswap是部署在以太坊上的去中心化交易所, 主要是吸引用户提供数字货币流动性, 然后利用恒定乘积算法(x * y = k)自动做市获取手续费再分发给流动性提供者的模式, 用户可以在上面自由的交换ERC20代币对令牌, 流动性提供者也因此获利;

### 部署准备
> npm, yarn

> MetaMask钱包及测试ETH,  [remix编辑器](http://remix.ethereum.org)及部署步骤     

钱包和remix编辑器不熟建议先看我[上篇文章的教程](http://remix.ethereum.org), 部署代币熟悉一下流程, 正好这块添加流动性也需要测试代币

之前看别的博主教程搭的, 但是最终提供流动性那块还是有问题, 这里是记录下从头到尾搭建测试无误的教程, 参考搭建博客还有一个uniswap源码解析的大佬博客都贴下面了, 有兴趣的朋友可以看看

## 合约部署

### Uniswap合约地址
1. [WETH合约](https://cn.etherscan.com/address/0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2#code)
2. [工厂合约](https://cn.etherscan.com/address/0x5C69bEe701ef814a2B6a3EDD4B1652CB9cc5aA6f#code)
3. [路由合约](https://cn.etherscan.com/address/0x7a250d5630b4cf539739df2c5dacb4c659f2488d#code)

### WETH部署
![部署一](https://img-blog.csdnimg.cn/20201112184251138.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3ZGh6eGY=,size_16,color_FFFFFF,t_70#pic_center)

![部署二](https://img-blog.csdnimg.cn/2020111218440960.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3ZGh6eGY=,size_16,color_FFFFFF,t_70#pic_center)

完事别忘了看交易哈希获取合约地址

### 工厂合约部署
![部署一](https://img-blog.csdnimg.cn/20201112184604666.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3ZGh6eGY=,size_16,color_FFFFFF,t_70#pic_center)

#### 获取生成合约地址的codeHash
这里**编译完**后需要先获取一下 UniswapV2Pair 合约的字节码, 用于路由合约自动获取币对合约地址的时候使用

![codeHash1](https://img-blog.csdnimg.cn/20201112184946782.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3ZGh6eGY=,size_16,color_FFFFFF,t_70#pic_center)

复制到编辑器取 object 字段内容
```json
{
	"linkReferences": {},
	"object": "取这里的内容",
	"opcodes": "",
	"sourceMap": ""
}
```
进入[在线哈希](http://emn178.github.io/online-tools/keccak_256.html), Input type 选择 hash, 获取输出的哈希值, 保留会在router合约部署中使用

![codeHash2](https://img-blog.csdnimg.cn/20201113092107640.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3ZGh6eGY=,size_16,color_FFFFFF,t_70#pic_center)

继续部署工厂合约, 这里部署需要设置一个钱包地址来作为 feeto 的设置者

![部署二](https://img-blog.csdnimg.cn/2020111309233213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3ZGh6eGY=,size_16,color_FFFFFF,t_70#pic_center)

### 路由合约部署
用工厂合约中获取到的codeHash替换pariFor函数中的codeHash, 然后进行编译

![部署一](https://img-blog.csdnimg.cn/20201113092941367.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3ZGh6eGY=,size_16,color_FFFFFF,t_70#pic_center)

这里部署合约需要填写上面部署的 工厂合约和weth合约地址

![部署二](https://img-blog.csdnimg.cn/2020111309304062.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3ZGh6eGY=,size_16,color_FFFFFF,t_70#pic_center)

### 合约部署完成

> 记住三个合约的合约地址和codeHash值

## 前端部署

### [项目地址](https://github.com/Uniswap/uniswap-interface)

### 修改项目中的配置文件
> src/constants/index.ts 第6行, 更换路由合约地址
```
export const ROUTER_ADDRESS = '0x9bAc11F0e7d80e936B5B294A76B51a0Fe6Fb3b9e'
```

> src/state/swap/hooks.ts 第91行, 更换工厂和路由地址, 这块是预防输入错误的收币地址
```
const BAD_RECIPIENT_ADDRESSES: string[] = [
  '0x2c176A5C9b461e66Fff4D8E7De007D6Ba7Dc1894', // v2 factory
  // '0xf164fC0Ec4E93095b804a4795bBe1e041497b92a', // v2 router 01
  '0x9bAc11F0e7d80e936B5B294A76B51a0Fe6Fb3b9e' // v2 router 02
]
```


### 修改调用sdk依赖中写死的工厂合约和codeHash(之前就卡这块了)
> sdk主要是给uniswap提供币对, 币种价格, 流动性数量及任意精度的算术运算等, 这里package.json中指定了 "@uniswap/sdk"="3.0.3"依赖, 但是里面的配置文件写死了
>工厂合约和codeHash, 所以需要进行更改, 不然你就连到uniswap的合约上去了

#### 方法一 修改node_modules(就只能在本地将就用)
> 使用 yarn 命令安装依赖, 修改目录 node_modules/@uniswap/sdk/dist, sdk.cjs.development.js和sdk.esm.js 的41和42行
```
var FACTORY_ADDRESS = '0x2c176A5C9b461e66Fff4D8E7De007D6Ba7Dc1894';
var INIT_CODE_HASH = '0x989024026f7dc4bb5d3efa1e563684305a278fe19f590dbf2055d4295d6e4bce';
```
#### 方法二 修改sdk项目源码, 修改指定依赖
[sdk项目地址](https://github.com/Uniswap/uniswap-sdk)

1. fork项目代码, 克隆到本地
2. src/constants.ts 第25行, 更换工厂地址和codeHash
```
export const FACTORY_ADDRESS = '0x2c176A5C9b461e66Fff4D8E7De007D6Ba7Dc1894'
export const INIT_CODE_HASH = '0x989024026f7dc4bb5d3efa1e563684305a278fe19f590dbf2055d4295d6e4bce'
```
3. yarn && yarn build
4. 连同dist目录一块打包提交git (这块interface项目安装sdk依赖必须要是打包好的, 前端不熟只能这样做, 有知道怎么操作的可以留言教我一下)
5. 修改interface项目的package.json中sdk的依赖配置
```
"@uniswap/sdk": "git://github.com/git用户名/uniswap-sdk.git#v2",
``` 
6. 使用yarn 安装interface项目依赖

### 启动项目
> yarn start 

## 把项目部署到git上
> sdk项目需要使用方法二

### 安装gh-pages
```
yarn add gh-pages
yarn build
```
修改package.json
```
"homepage": "https://git用户名.github.io/UniswapDex",

// 在scripts中添加部署脚本
"scripts": {
    ...
    "deploy": "gh-pages -d build" // 这个
},
```
提交代码至git

### 部署到git
```
yarn deploy
```
完成后打开 `https://git用户名.github.io/项目名称/index.html` 就好了



## 参考博客
> [Uniswap源码解析](https://blog.csdn.net/weixin_39430411/article/details/108665694)

> [搭建教程博客](https://blog.csdn.net/zgf1991/article/details/109127260)

> [git部署教程](https://zhuanlan.zhihu.com/p/212397361)