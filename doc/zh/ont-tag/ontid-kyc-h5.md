# KYC dapp h5对接

此文档为h5版本kyc dapp的对接文档。

[TOC]

##  1. 认证商选择

为了适应dapp会有不同的web环境，选择**Shufti Pro**作为认证商。

## 2. 认证和授权流程

流程中涉及到的多方如下：

- ONT ID账户体系前端和后端，以下简称ONT ID 前端和ONT ID 后端
- ONT TAG
- KYC dapp h5（包含认证和授权功能，可以在h5和钱包中打开）
- 场景方dapp（比如Candybox）

### 2.1 认证

![cert](../res/certification.png)

1. 在应用场景中触发认证请求，重定向到KYC dapp h5（在url后附上用户ONT ID），如果该ONT ID已认证，则显示ONT ID信息，如果未认证，走认证流程
2. 用户上传认证所需证件信息到ONT TAG
3. 提交[认证请求](overview.md)到ONT TAG。这个请求需要认证需求方的签名，即应用方的签名。KYC dapp发送相关数据给应用方后台，应用方后台添加上自己签名后，提交认证请求到ONT TAG。
4. KYC dapp显示等待审核页面。

### 2.2 授权

![auth](../res/auth.png)

1. 应用方在需要用户授权的地方，通过url跳转到KYC dapp h5（在url后附上参数，通过用户ONT ID查询到加密后claim），显示授权页面。
2. 用户确认后，输入密码，发送授权信息到应用方后台（请求数据使用ONT ID 账户体系后台公钥加密），应用方后台转发请求数据到ONT ID后台，得到解密后的Claim。
3. 应用方验证Claim，然后把验证结果，即授权结果，返回给KYC dapp，KYC dapp根据授权结果显示认证成功或失败。

## 3. 应用方对接所需接口和链接

> KYC dapp url后可附带参数控制多语言，默认语言是英文。
>
> lang=en 语言为英文
>
> lang=zh 语言为中文

### 3.1 跳转到KYC dapp认证页面

```
url：host + /#/mgmtHome?ontid={ontid}&requestAuthenticationCallback={requestAuthenticationCallback}
```

`ontid` User's ONT  ID

`requestAuthenticationCallback` 应用方后台用于提交认证请求的回调地址

### 3.2 跳转到KYC dapp授权页面

```
url: host + /#/authHome?userOntid={userOntid}&dappOntid={dappOntid}&dappName={dappName}&dappUrl={dappUrl}&callback={callback}
```

`userOntid` User's ONT ID

`dappOntid` dApp's ONT ID

`dappName` dApp's name

`dappUrl` 授权成功后跳转回dapp的地址

`callback` dapp后台用来接收授权信息的回调地址。

## 4. 应用方需要提供的接口

### 4.1 处理授权请求回调

授权时，用户发送授权信息（使用ONT ID后台公钥加密的）给应用方后台。应用方转发给ONT ID后台解密，得到Claim，对Claim验证，验证成功，则授权成功。返回授权结果给KYC dapp。

### POST

```
url: 由应用方传给KYC dapp(比如http://host/handleAuth)
```

### REQUEST

| Field_Name | Required | Format | Description                                                  |
| ---------- | -------- | ------ | ------------------------------------------------------------ |
| url        | yes      | String | ONT ID 后台用来解密数据的接口                                |
| data       | yes      | string | 使用ONT ID后台公钥加密的请求数据。解密后内容为：                    {message: 'xxxx', password: '', ontid: 'did:ont:Axxxxxxx'} |
| secure     | yes      | string | RSA 加密的key。转发请求时放到请求的header中 secure-key字段   |

### RESPONSE

| Field_Name | Format | Description                             |
| ---------- | ------ | --------------------------------------- |
| version    | String | 版本号，目前是1.0。                     |
| action     | String | 固定值：RequestAuthorizationCallback。  |
| desc       | String | 错误信息。成功即SUCCESS，其他即错误信息 |
| result     | String | 应用方后台验证授权Claim的结果。         |

### 4.2 应用方提交认证请求

> 目前测试环境，由KYC dapp提交认证请求；生产环境由应用方后台提交认证请求。

应用方后台对认证数据签名，并发送认证请求到ONT TAG。

### POST

由应用方传递给KYC dapp.例如：

```
https://host + /sendAuthenticationRequestCallback
```

### REQUEST

| Field_Name | Required | Format | Description |
| ---------- | -------- | ------ | ----------- |
| auth_id    | yes      | String | 认证的id    |

### RESPONSE

| Field_Name | Format | Description                                 |
| ---------- | ------ | ------------------------------------------- |
| version    | String | 版本号，目前是1.0。                         |
| action     | String | 固定值：RequestAuthenticationCallback       |
| desc       | String | 错误信息。成功即SUCCESS，其他即错误信息     |
| result     | String | 提交的结果。true：提交成功；false：提交失败 |

KYC dapp发送给应用方后台的数据如下：

```
{
	auth_id: '' // 代表一次认证的唯一id
}
```

应用方后台构造数据如下：

```
{
	auth_id: '',
	app_ontid: ''
}
```

应用方对构造的数据签名，并将得到的签名添加到上一步构造的数据中，如下：

```
{
	auth_id: '',
	app_ontid: '',
	app_signature: ''
}
```

应用方后台发送上面的数据到ONT TAG，提交认证请求。请求具体内容见[文档](https://documenter.getpostman.com/view/4781757/RznFoHaC#5951d0a6-66a9-4234-b8a5-63e32b3189ec)。返回给KYC dapp结果。

## 5. ONT ID后台需要提供的接口

### 5.1 解密Claim

通过Onid和密码解密claim

#### POST

```
url: /api/v1/ontid/decrypt/claim
```

### REQUEST

| Field_Name | Required | Format                          | Description                                                 |
| ---------- | -------- | ------------------------------- | ----------------------------------------------------------- |
| ontid      | yes      | string 20-255 characters Length | ontid（例如："did:ont:AcrgWfbSPxMR1BNxtenRCCGpspamMWhLuL"） |
| password   | yes      | string 20-255 characters Length | 密码                                                        |
| message    | yes      | JSONArray 3 Length              | 加密后的数据                                                |


### RESPONSE

| Field_Name | Format | Description                             |
| ---------- | ------ | --------------------------------------- |
| version    | String | 版本号，目前是1.0。                     |
| action     | String | 固定值：decrypt。                       |
| error      | int    | 错误码                                  |
| desc       | String | 错误信息。成功即SUCCESS，其他即错误信息 |
| result     | String | 解密后的内容                            |
