# 关于 SaaS 的一切

核心的 SaaS API，多版本产品客户端，以及平台端的管理后台。

Enterprise-ready SaaS Boilerplate 基础框架调研: https://phab.xyz/T19
招聘系统（示例客户项目）:https://www.craft.me/s/Xcplu3hEPHsxAh

### SaaS 产品的技术挑战

SaaS presents developers with a unique blend of challenges: multi-tenancy 多租户, onboarding 客户生命周期管理, security 安全问题, data partitioning 数据分区, tenant isolation 租户隔离, and identity 身份认证.

基础服务：

- send an SMS message
- ### Tenant Onboarding
   - **User Identity**：associating each tenant with a separate Cognito User Pool
      - sign-in policies
      - standardized attributes or **Add custom attribute**
      - Tenant Connection Attribute
         - **tenant_id** (string, default max length, ***not*** mutable)
         - **tier** (string, default max length, mutable)
         - **company_name** (string, default max length, mutable)
         - **role** (string, default max length, mutable)
         - **account_name** (string, default max length, mutable)
      - configure password and administration policies,enable MFA
      - **customize your user invitation messages**
      - create app client id and secert
   - **Identity Pool**
      - **Authentication Providers**
   - Managing Users：Cognito API
   - Managing Tenants
      - Tenants must be represented and managed separate from users.
   - Onboarding
      - register：account & tenant & plan
      - check your email for the validation message that was sent by Cognito
      - login & change password
      - As a new tenant to the system, you are created as a **Tenant Administrator**.
- ### Multi-Tenant Microservices
   - build a multi-tenant aware microservice:

![Image.tiff](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/609C7D6B-093D-441C-AFDE-A325627C7FEE/D05ECDA9-86DB-4662-B46E-5DDA91D29B1C_2/8EkwFxUs8oPPKNabivIZBAOVIkrxzICjCK6Kof6YZl0z/Image.tiff)

   - **Identity and Tenant Context：**our services need some standard way to acquire the current user's role and authorization along with the tenant context.
   - **Multi-Tenant Data Partitioning：**figure out how to partition, persist, and acquire data in a multi-tenant model.
   - **Tenant Aware Logging, Metering, and Analytics**
- ### Isolating Tenant Data
   - create a set of IAM roles for each tenant.
   - With IAM policies, we can create rules that control the level of access a user has to tenant resources.
   - associate roles for tenants

## 面向的客户群体

- 开发者
- 中小企业客户

## 商业诉求的考虑

- 利用开源力量，帮助客户在构建自己的 SaaS 产品时节省时间和金钱
- 多个不同的客户端版本，轻松集成到客户自身的技术栈
- 服务端核心的 API 集成（Rust），稳定、高效、安全、易部署，文档清晰
- 长期的开源维护和技术支持，保证技术的稳定性
- 国际市场和国内市场的大量第三方 API 集成

## 商业模式

- 赞助 & 教程售卖 & 商业授权
- 集成定制服务（报价制）
- 提供云服务

## 基础功能

- Server-side rendering for fast initial load and SEO
- User authentication，第三方登录集成
- 邮件通知（账户和交易）、Adding email addresses to newsletter lists
- 文件上传和加载
- Team creation, Team Member invitation, and settings for Team and User.
- 日志服务定制
- 网站数据分析
- Production-ready, scalable architecture
- 基础的 Web Component 组建
- 订阅服务（支付平台集成，账单管理）
   - subscribe/unsubscribe Team to plan
   - update card information
   - verified Stripe webhook for failed payment for subscription.
- Deploy 服务

### 产品框架

服务端

管理平台

数据层

客户端（Web）

### 代码架构

订阅服务的架构

![Image.tiff](https://res.craft.do/user/full/cfe4d8ac-b1b3-3abe-9e76-468303587884/doc/609C7D6B-093D-441C-AFDE-A325627C7FEE/14BFECA2-0497-48E8-BE6A-6DFC185BBCB9_2/l5yUNIyasQQCN0Y8ImO9yFa2xEQiLe1IEXp4to0Qa58z/Image.tiff)

### 相似产品

[GitHub - vercel/nextjs-subscription-payments: Clone, deploy, and fully customize a SaaS subscription application with Next.js.](https://github.com/vercel/nextjs-subscription-payments)

[GitHub - gmpetrov/ultimate-saas-ts: Template to quickstart a SAAS business](https://github.com/gmpetrov/ultimate-saas-ts)

[GitHub - bullet-train-co/bullet_train: The Open Source Ruby on Rails SaaS Template](https://github.com/bullet-train-co/bullet_train)

[GitHub - saasforge/open-source-saas-boilerpate: Free SaaS boilerplate (Python/PostgreSQL/ReactJS/Webpack)](https://github.com/saasforge/open-source-saas-boilerpate)

[WaVer - Free React template for building an SaaS or admin application](https://reactsaastemplate.com/)

[Wave - The Software as a Service Starter Kit built on Laravel & Voyager](https://wave.devdojo.com/)

[GitHub - denoland/saaskit: A modern SaaS template built on Fresh.](https://github.com/denoland/saaskit)

[GitHub - SimonHoiberg/saas-template: SaaS template for AWS, Amplify, React, NextJS and Chakra](https://github.com/SimonHoiberg/saas-template)

[GitHub - boxyhq/saas-starter-kit: Enterprise SaaS Starter Kit - Kickstart your enterprise app development with Next.js SaaS Starter Kit](https://github.com/boxyhq/saas-starter-kit)

[GitHub - miracuthbert/saas-boilerplate: SaaS boilerplate built in Laravel, Bootstrap 4 and VueJs.](https://github.com/miracuthbert/saas-boilerplate)

[GitHub - saasitive/django-react-boilerplate: DIY Django + React Boilerplate for starting your SaaS](https://github.com/saasitive/django-react-boilerplate)

[GitHub - jbarbier/SaaS_Memcached: Build your own Memcached SaaS](https://github.com/jbarbier/SaaS_Memcached)

[GitHub - go-saas/saas: go data framework for saas(multi-tenancy)](https://github.com/go-saas/saas)

[Pi Engine](https://github.com/pi-engine)

[GitHub - alectrocute/flaskSaaS: A great starting point to build your SaaS in Flask & Python, with Stripe subscription billing 🚀](https://github.com/alectrocute/flaskSaaS)

[GitHub - Saas-Starter-Kit/SAAS-Starter-Kit-Pro: 🚀A boilerplate for building Software-as-Service (SAAS) apps with Reactjs, and Nodejs](https://github.com/Saas-Starter-Kit/SAAS-Starter-Kit-Pro)

[GitHub - dromara/lamp-cloud: lamp-cloud 基于Jdk11 + SpringCloud + SpringBoot 开发的微服务中后台快速开发平台，专注于多租户(SaaS架构)解决方案，亦可作为普通项目（非SaaS架构）的基础开发框架使用，目前已实现插拔式数据库隔离、SCHEMA隔离、字段隔离 等租户隔离方案。](https://github.com/dromara/lamp-cloud)

[GitHub - saasform/saasform: Add signup & payments to your SaaS in minutes.](https://github.com/saasform/saasform)

### 关联产品

[GitHub - Blazity/next-saas-starter: ⚡️ Free Next.js responsive landing page template for SaaS products made using JAMStack architecture.](https://github.com/Blazity/next-saas-starter)

[GitHub - cruip/open-react-template: A free React / Next.js landing page template designed to showcase open source projects, SaaS products, online services, and more. Made by](https://github.com/cruip/open-react-template)

### 参考资料

[Books at builderbook.org](https://builderbook.org/book)

[GitHub - aws-samples/aws-saas-factory-bootcamp: SaaS on AWS Bootcamp - Building SaaS Solutions on AWS](https://github.com/aws-samples/aws-saas-factory-bootcamp)

[GitHub - RunaCapital/awesome-oss-alternatives: Awesome list of open-source startup alternatives to well-known SaaS products 🚀](https://github.com/RunaCapital/awesome-oss-alternatives)

[Getting Started](https://bullettrain.co/docs/getting-started)

[Enterprise-ready SaaS Starter Kit | Security Building Blocks for Developers](https://boxyhq.com/blog/enterprise-ready-saas-starter-kit)

### 潜在可以合作的工程师

[goxiaoy - Overview](https://github.com/goxiaoy)


