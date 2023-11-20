# 密码学、信息安全与区块链

# Blockchian for **Identity Management**

#### 自主身份 Self-Sovereign Identity solutions 的必要技术：

- **Blockchain**，cryptography and zero-knowledge proofs
- Verifiable Credentials
- Decentralized Identifiers

> **::What is blockchain？::**

> **Distributed Ledger Technology** (DLT, 分布式账本技术), commonly simply called “**Blockchain** Technology”, refers to the technology behind **decentralised databases【分布式数据库】** providing control over the evolution of data between entities through a peer-to-peer network, using **consensus algorithms【共识算法】** that ensure replication across the nodes of the network.

- 不可篡改的性质
- solve the double-spend problem of digital currency
- considered a system with high Byzantine Fault tolerance

[了解区块链的基本（第一部分）：拜占庭容错(Byzantine Fault Tolerance) - SegmentFault 思否](https://segmentfault.com/a/1190000014009235)

[了解区块链的基本（第二部分）：工作量证明(PoW)和股权证明(PoS) - SegmentFault 思否](https://segmentfault.com/a/1190000014058722)

**Permissioned Blockchain**

[Home - Sovrin](https://sovrin.org/)

## **::What is Identity Management?::**

Also known as “**identity and access management**”, or IAM, identity management comprises all the processes and technologies within an organisation that are used to **identify, authenticate and authorize** someone to access services or systems in that said organisation or other associated ones.

- Identities need to be **portable and verifiable** everywhere, any time, and digitization can enable that.
- Identities also need to be **private** and **secure**.

# ::Decentralized Identifiers (DIDs)::

[Decentralized Identifiers](https://tykn.tech/decentralized-identifiers-dids/) are globally, unique and persistent（永久） identifiers.

They are entirely controlled by the identity owner 个人拥有数据

DID 独立于中央注册机构、权威机构或身份提供者。

[Decentralized Identifiers (DIDs): A Beginners Guide!](https://tykn.tech/decentralized-identifiers-dids/)

*A new spec is coming up in* ***W3C:***

[Decentralized Identifiers (DIDs) v1.0](https://w3c.github.io/did-core/)

![Image](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/7D1DC97E-E57B-48F6-9234-D4435331ADD1/1336F28D-BE80-4FEA-AC3B-C2A7157E0C25_2/Image)

## **::Verifiable Credential 可验证凭证::**

[Verifiable Credentials: The Ultimate Beginners Guide!](https://tykn.tech/verifiable-credentials/)

The Blockchain acts as a **verifiable data registry 可验证的注册数据表**

A “phonebook” that anyone can consult to verify what organisation a specific Public DID belongs to.

***::In identity management::***, a distributed ledger (a “blockchain”) enables everyone in the network to have the **same source of truth** （大家都拿到的相同的真实数据来源）about which credentials are valid and who **attested** to the validity of the data inside the credential（自证明身份的有效性）, **without revealing the actual data**.（无需透漏实际的数据）。

主要角色 🎭：

- identity owners ：🆔 身份所有者
   - The identity owner can **store** those credentials in their personal identity wallet
   - use them later to **prove** statements about his or her identity to a third party (the verifier).
- identity **issuer： 身份颁发者**
   - can **issue** personal credentials for an identity owner (the user).
   - the identity issuer **attests** to the validity of the personal data in that credential
- identity verifiers：**身份验证者**

> A **Credential** is a set of multiple identity attributes and an identity attribute is a piece of information about an identity (a name, an age, a date of birth).

凭证的有用性和可靠性完全取决于发行人的声誉/可信度。

**Revocation** means deleting or updating a credential.

[How Identity Revocation on the blockchain works - Tykn](https://tykn.tech/identity-revocation-blockchain/)

## **::Privacy and Security::**

**do not need** to check the validity of the actual data, but can rather use the blockchain to check the validity of the **attestation** and **attesting party**

> For example, when an **identity owner** presents a **proof** of their date-of-birth, rather than actually checking the truth of the date of birth itself, the **verifying party** will validate the government’s signature who issued and attested to this credential to then decide whether he **trusts** the government’s assessment about the accuracy of the data.

**No personal data should ever be put on a blockchain.**

## What exactly goes on the Blockchain?

- Only references and the associated attestation of a user’s verified credential are put on the ledger.
- **Public Decentralised Identifiers (Public DIDs)** and associated **DID Descriptor Objects** (DDOs) with verification keys and endpoints.
- **Schemas:** The formal description for the structure of a credential.
- **Credential definitions:** The different (often tangible) **proofs of identity or qualification issued by authorities**
- **Revocation registries**. 登记撤销
- **Proofs of consent for data sharing**. 数据分享同意文件

## **Cryptography & Zero-Knowledge Proof**

**authentication: prove** something about our identity – either our name, address or passport number.

A **Zero-Knowledge Proof** is a method of authentication that, through the use of **cryptography**

This is especially useful when and where the prover entity does not trust the **verifying entity** but still has to prove to them that he knows a specific information.

## 🔧 工具： 身份钱包应用和开放 API

Tykn’s APIs and Mobile SDK let you quickly integrate **decentralized identity** tech into your app or web-based user journeys. The **Issuer & Verifier** **Web Portal** and **Mobile Wallet App** you see in this demo are “off-the-shelf” components you can whitelabel if you prefer a no-integration approach.

[The Mobile Wallet Demo - Tykn](https://tykn.tech/resources/demos/mobile-wallet-demo/)

[Decentralized Identity Product Suite](https://tykn.tech/product-suite/)

### Rreference

[Blockchain Identity Management: The Definitive Guide (2021 Update)](https://tykn.tech/identity-management-blockchain/)

[Digital Identity and the Blockchain: Universal Identity Management and the Concept of the “Self-Sovereign” Individual](https://www.frontiersin.org/articles/10.3389/fbloc.2020.00026/full)

[](https://www.ibm.com/blockchain/solutions/identity)

[Blockchain for Digital Identity: Real World Use Cases | ConsenSys](https://consensys.net/blockchain-use-cases/digital-identity/)

[Building A Trusted Identity: Blockchain ID Demo](https://www.youtube.com/watch?v=QYy8a7HDJ0g)

[Blockchain for Digital Identity | Accenture](https://www.accenture.com/us-en/services/blockchain/digital-identity)

[Blockchain for identity management: Implications to consider](https://searchsecurity.techtarget.com/tip/Blockchain-for-identity-management-Implications-to-consider)

[Elsevier Enhanced Reader](https://reader.elsevier.com/reader/sd/pii/S2096720921000099?token=578B43BEE5F80EEDD6561BE1C517184190F7DCA2E3702E0F07EFF7D0E044007218ABF38E121F6363857F6EE4D146ECCF&originRegion=us-east-1&originCreation=20210924092944)

[Decentralized Identity, Blockchain, and Privacy | Microsoft Security](https://www.microsoft.com/en-ww/security/business/identity-access-management/decentralized-identity-blockchain)

[Forrester](https://www.forrester.com/report/Prepare-For-Decentralized-Digital-Identity-Security-SWOT/RES159143)

[Jolocom launches new GDPR-compliant blockchain identity management app SmartWallet 2.0 - SiliconANGLE](https://siliconangle.com/2021/07/19/jolocom-launches-new-gdpr-compliant-blockchain-identity-management-app-smartwallet-2-0/)

国内：

[Home | Nervos Network](https://www.nervos.org/)

[DID分布式身份标识技术调研_九筒的博客-CSDN博客](https://blog.csdn.net/weixin_44343282/article/details/112240590)

[分布式身份TDID_可信数字身份_可信数字凭证 - 腾讯云](https://cloud.tencent.com/product/tdid)

[聚链集团](http://www.juchaintimes.com/#/product)

[太一公有云](http://cloud.taiyiyun.com/)

[数字身份链](https://www.eidledger.com/)

[区块链分布式身份服务解决方案_数字身份_身份授权_阿里云](https://cn.aliyun.com/solution/blockchain/DIS)

[分布式身份服务_TDIS_区块链服务_区块链开发-华为云](https://www.huaweicloud.com/product/bcs/tdis.html)

[区块链分布式身份技术解密——重新定义你的“身份”管理-云社区-华为云](https://bbs.huaweicloud.com/blogs/234838)

[](https://dida.org/)

[https://dl.brop.cn/wechat/DIDA/DIDA白皮书.pdf](https://dl.brop.cn/wechat/DIDA/DIDA%E7%99%BD%E7%9A%AE%E4%B9%A6.pdf)

![IMG_0594.png](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/4997B162-463D-4A1D-B400-6CD3255B6010_2/IMG_0594.png)

# XMTP: The communication protocol and network for web3 and crypto

aligns incentives: 调整激励措施

pervasive：无孔不入

[XMTP home](https://xmtp.com/)



