<h1 align="center">ONT TAG 认证服务索引 </h1>
<p align="center" class="version">Version 0.9.0 </p>

[TOC]

本文注册登记了所有在ONT TAG上提供认证服务的可验证凭证定义，为认证需求方提供技术接入指导。

* ONT TAG：基于本体区块链的去中心化身份交易平台，ONT TAG主要用于协同用户和需求方进行数据交换，数据全程被加密，ONT TAG并不会触碰用户隐私数据。
* TrustAnchor：信任锚Trust Anchor是指在本体生态上提供认证服务的合作方，其可能是政府机关、大学、银行、第三方认证服务机构（比如CA机构）、生物识别科技公司等等。


## 已注册可验证凭证

| 可验证凭证标识     |     说明 |   签发者   |
| :--------------: | :--------:| :------: |
| credentail:email_authentication |   个人邮箱认证可验证凭证|  ONTO  |
| credentail:mobile_authentication |   个人手机认证可验证凭证|  ONTO  |
| credentail:cfca_authentication |   中国公民实名认证可验证凭证|  CFCA |
| credentail:sensetime_authentication |   中国公民实名认证可验证凭证|  商汤SenseTime |
| credentail:idm_passport_authentication |   全球用户个人护照认证可验证凭证|  IdentityMind |
| credentail:idm_idcard_authentication |    全球用户个人身份证件认证可验证凭证|  IdentityMind |
| credentail:idm_dl_authentication |    全球用户个人驾照认证可验证凭证|  IdentityMind |
| credentail:github_authentication |   Github社媒认证可验证凭证|  Github |
| credentail:twitter_authentication |   Twitter社媒认证可验证凭证|  Twitter |
| credentail:linkedin_authentication |   Linkedin社媒认证可验证凭证| Linkedin |
| credentail:facebook_authentication |   Facebook社媒认证可验证凭证|  Facebook |


## 标准认证模板


| 认证模板类型 | 认证模板标识 | 认证模板描述 | 认证模板对应的可验证凭证模板标识 | 授权逻辑规则 |
| :--------: | :--------:|:---------:|:--------: | :--------:|
| 社交媒体认证    |   authtemplate_social01 |  有关用户各种社交媒体的基本信息认证  | credentail:github_authentication<br><br>credentail:twitter_authentication<br><br>credentail:facebook_auuthentication<br><br>credentail:linkedin_authentication |   任选其一 |
| 联系信息认证    |   authtemplate_contact01 |  有关全球用户的邮箱信息认证  | credentail:email_authentication |   必选 |
| 联系信息认证    |   authtemplate_contact02 |  有关全球用户的手机号信息认证  | credentail:mobile_authentication |   必选 |
| kyc认证    |   authtemplate_kyc01 |  有关全球用户基本个人信息的认证  | credentail:idm_passport_authentication<br><br>credentail:idm_idcard_authentication<br><br>credentail:idm_dl_authentication |   任选其一 |
| kyc认证    |   authtemplate_kyc02 |  有关中国用户的实名信息认证  | credentail:cfca_authentication |   必选 |
