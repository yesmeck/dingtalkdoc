#  ISV通用技术方案
- category: ISV接入指南
- order: 8
## ISV套件对企业独立部署方案
### 背景：
在面向企业级软件领域，ISV提供企业的软件系统分2种形态：1，SAAS应用软件；2，独立部署的软件系统；钉钉的C＋＋战略立足于与ISV合作伙伴一起为企业提供专业的、安全的移动办公解决方案，因此需要解决SAAS应用软件和ISV独立部署软件系统与钉钉结合的版本可以快速部署，降低整体方案的推广与实施成本。

### 概述：
本方案对于ISV软件与钉钉的结合版本通过统一的授权模型实现可对任意企业客户统一实施钉钉＋X的版本，由钉钉根据ISV提供的套件相关信息统一生成二维码，企业用户通过二维码下载钉钉、注册钉钉企业、对套件进行授权后使用。
技术实现说明：（此处只针对独立部署的场景进行描述，SAAS类系统见[<font color=red >ISV接入开发指南</font>](#isv接入开发指南)）
### 系统实现：
#### 名词解释：
ISV套件授权系统：ISV用于与钉钉开放平台进行授权的独立系统，部署在阿里云的通用授权服务
ISV独立部署应用系统：ISV为企业独立部署的软件系统
#### 实现说明：
本方案要求ISV在企业进行套件使用授权时提供统一的授权服务

a)ISV套件授权系统通过钉钉开放平台完成授权，参考[<font color=red >ISV接口调用整体流程</font>](#1-isv接口调用整体流程)

b)ISV套件授权系统通过钉钉开放平台接口获取企业输入的关联码与ISV独立部署系统进行关联

c)ISV套件授权系统调用钉钉开放平台接口完成企业独立部署软件IP白名单设置，参考[<font color=red >为授权方的企业单独设置ip白名单</font>](#10-isv为授权方的企业单独设置ip白名单)

d)ISV套件授权系统获取企业永久授权码并进行存储

- 方案一：ISV将获取到的永久授权码回传至独立部署的应用系统，独立部署应用系统通过授权码直接调用钉钉开放平台API

- 方案二：ISV独立部署应用数据请求先发送至ISV套件服务器，ISV套件服务器使用授权码调用钉钉开放平台API

#### 系统交互图：
![system_design](https://img.alicdn.com/tps/TB1TgCgLXXXXXXqXVXXXXXXXXXX-802-514.png)

## ISV用户账号绑定方案

### 背景：
在ISV开发钉钉套件时，存在2个独立账号体系，为提升用户的体验，需要对2个独立账号体系进行账号绑定，目前的账号分为3种情况：

1，只存在于ISV系统中的用户

2，只存在于钉钉系统中的用户

3，在钉钉与ISV系统中同时存在的用户
### 概述：
本方案用于ISV软件与钉钉的结合版本用户账号绑定的场景
### 系统实现：
1，只存在于ISV系统中的用户：

 通过邀请码注册的企业可以调用钉钉开放平台的通讯录接口，具有通讯录维护的权限，通过接口直接同步企业组织结构及人员信息到钉钉

1.1 有手机号码：

通过钉钉开放平台接口同步企业及用户数据，ISV系统采用钉钉userid进行账号绑定

1.2 有邮箱：

采用快速部署方式，企业创建成功后通过钉钉接口获取加入团队链接，邮件通知企业员工进行加入，ISV系统采用钉钉userid进行账号绑定

2，只存在于钉钉系统中的用户：

ISV在企业授权时通过钉钉接口获取组织结构与人员信息，通过钉钉userid与ISV用户信息进行关联实现绑定

3，在钉钉与ISV系统中同时存在的用户：

ISV需以钉钉通讯录为准进行账号同步与绑定，与只存在于钉钉系统中的用户采用相同的处理方式，产品上可以实现让用户显示绑定已有ISV系统账号，通过免登获取钉钉userid进行系统账号绑定