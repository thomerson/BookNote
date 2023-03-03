## CI/CD

通过在应用开发阶段引入**自动化**来频繁向客户交付应用的方法


### CI

```Continuous Integration```/持续集成

帮助开发者更加方便地将代码更改合并到主分支

一旦开发人员将改动的代码合并到主分支，系统就会通过自动构建应用，并运行不同级别的自动化测试（通常是单元测试和集成测试）来验证这些更改，确保这些更改没有对应用造成破坏。

### CD 

* ```Continuous Delivery```/持续交付

CI 在完成了构建、单元测试和集成测试这些自动化流程后，持续交付可以自动把已验证的代码发布到企业自己的存储库。

持续交付旨在建立一个可随时将开发环境的功能部署到生产环境的代码库

* ```Continuous Deployment```/持续部署

作为持续交付的延伸，持续部署可以自动将应用发布到生产环境。

实际上，持续部署意味着开发人员对应用的改动，在编写完成后的几分钟内就能及时生效（前提是它通过了自动化测试）。这更加便于运营团队持续接收和整合用户反馈。


![CI/CD](https://pic1.zhimg.com/80/v2-5a0deaf7277dde3ba6ff57b65b906f30_720w.webp)

### 流程

![CI/CD流程](https://upload-images.jianshu.io/upload_images/7378149-6d123dcd754e4822.png?imageMogr2/auto-orient/strip|imageView2/2/w/1008/format/webp)

### 工具

1. 代码管理 

    * gitlab

    * Gogs

2. 自动构建项目

    * Jenkins

3. 部署

    * docker
