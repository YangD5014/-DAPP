## API接口说明文档 2024.06.02更新

URL: `http://39.98.54.173:3000/api/` +具体接口名称


| 序号 | 接口名称     |                         接口描述                         | 请求方法 | 所需参数 |  请求示例  |
| :----: | :------------- | :---------------------------------------------------------: | :--------: | :---------: | :-----------: |
|  11  | GetNFTbyID   |                  通过特定TokenID查找NFT                  |   POST   | NFTID:int | {"NFTID":0} |
|  12  | GetAllNFT    |                     获取所有的NFT信息                     |   Get   |   None   |    None    |
|  13  | GetDetailNFT | 获得特定ID的NFT详细信息,本质上是继续访问了接口11的URI链接 |   POST   | NFTID:int | {"NFTID":0} |

下面介绍11-13接口的返回值如何解读：

```json
{
    "success": true,
    "result": "[0,\"0xa4ff4ea6F8dCfB67a1bFB5a447eBaFE0267F7628\",\"0xa4ff4ea6F8dCfB67a1bFB5a447eBaFE0267F7628\",\"0x2Ae5B1C57057067A48Ca0c9028C8bE1525CC77E0\",\"http://xianxiangchain.xyz/DAPP/NFT_JSON/nfts_0.json\",1717293216,false]",
    "message": "TokenID为0的NFT查询成功!"
}
```

重点是result里的数据如何解读呢？ 这是根据下面的结构体定义返回的。

所以你在前端展示的时候,应当在二级页面展示一些信息:发行者地址、TokenID...等和这个结构体的注释信息相同

此外，如果在开发的过程中遇到了新的需求,比如要求新增NFT的兑换截止日期，该如何实现呢？再次修改合约是成本很高的方法,况且也就新加个键值对的事。

因此可以用到descriptionURI这个属性指向的JSON数据,在JSON数据里新加就行。同时区块链不可以存大型文件，因此图片等数据也会放到json数据里，比如图片链接放里头。

所以你卡面上要放的图片链接,应当在13接口获得

```cpp
    struct FreshJoyCard {
        //重要信息存在区块链上 补充信息存在中心化服务器里
        uint256 tokenID; //TokenID 信息
        //mapping(uint128 => address) transction; //所有历史交易记录
        address owner;
        address publisher; //发行人
        address auditor; //审核人
        string descriptionURI; //描述
        uint256 publishDate; //发布日期的时间戳
        bool burn; //是否被销毁
    }
```
