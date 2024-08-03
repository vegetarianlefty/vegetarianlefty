### git flow 分支策略的好处

该策略的优点是分支清晰，能够应付开发流程中的许多情况，缺点是分支较多，开发过程中会经常需要进行合并操作，于是在基于该策略的基础上做适当的简化，并考虑并行迭代的情况，综合制定了该工作流程。

### git flow 分支策略说明

![img](http://image.wangxiaohuan.com/blog/image/202303071541594.png)

##### 长期分支

- master：用于存放对外发布的版本，任何时候在这个分支拿到的都是稳定的版本。

- develop：用于日常的开发，存放最新的开发版本。

##### 临时分支

- feature：用于开发特定功能从 develop 中分出来的，一个 feature 分支对应一次迭代开发，开发完成后合并到 develop 分支中；

- hotfix：用于修复 bug，从 master 中分出来的，开发完成后合并到 master、deveop 分支。

- release：指发布正式版本之前（即合并到 Master 分支之前），我们可能需要有一个预发布的版本进行测试。

### git flow 分支工作流程

##### 项目初始

- gitlab 上创建项目

- 创建 develop 分支，并将其设置为保护性分支，具有 Maintainer 权限的用户才可以直接 push 和  merge，其他用户需要将代码提交到自己的分支后发起 merge request（以下简称 MR）请求，由 Maintainer  和团队成员进行代码评审决定是否接受合并。

- 修改 master 分支为不允许任何人推送，该分支只能通过 MR 进行变更。

![img](http://image.wangxiaohuan.com/blog/image/202303071546244.png)

##### 迭代开始阶段

- 情况 1：迭代时间较长、分工明确

- 开发人员各自新建属于自己的 feature 分支，代码持续集成到 develop。

- **情况 2：迭代时间较短、分工不明确或需要多个人开发同一功能**

- **开发人员共享一个 feature 分支，代码持续集成到 develop。**

- **情况 3：迭代 A、迭代 B 同时启动，A 为主要迭代**

- **主要迭代根据情况 1、2 选择开发方式，代码持续集成到 develop**

- **次要迭代从 develop 分出 sprint-{版本}，feature 基于该版本开发，代码持续集成到 sprint-{版本}**

- **情况 4：主要迭代 A 已启动，突然启动迭代 B**

- **迭代 B 从 master 分出 sprint-{版本}，feature 基于该版本开发，代码持续集成到 sprint-{版本}**

**注意：情况 2、3、4 均为特殊情况，应尽量避免**，尤其是 3、4 两种情况需要将代码持续集成到另一个版本，那么需要配置另一套持续集成环境，同时也加大了代码评审、分支合并的麻烦。下面描述的开发阶段以情况 1 为准，若为 3、4 情况，则需要将 develop 分支替换为 sprint 分支。

##### 迭代开发阶段

- 开发人员完成 feature 的开发或阶段性开发时，发起到 develop 的 MR。

- Maintainer 和团队成员对 MR 进行 review，拒绝的需要提交者重新修改代码后再次提交 MR，接受的将合并到 develop，并自动构建到测试环境。

- 迭代整体测试通过后进入发布准备阶段。

##### 发布准备阶段

- 对于无预发布环境的项目，跳过准备阶段，进入发布阶段。

- **对于有预发布环境的项目，从 develop 中新建 release 为保护性分支（若已有则合并，该分支可在部署稳定后删除）。**

- **release 分支构建到预发布环境进行测试，若存在 bug，在该分支上进行修改。**

- **完成 release 分支的测试后，进入发布阶段。**

##### 发布阶段

- 提交发布申请，运维审核通过后进行代码合并

- 存在 release 时，release 分支合并到 develop 和 master，不存在时，develop 合并到 master。

- 通过 master 构建并发布到生产环境。

- 在 master 上打上版本 tag 标签，标记发布。

##### 线上发布回滚情况

- 出现发布回滚情况时，合并到 master 上的代码也需进行回滚操作。

##### 线上 bug 修复

- 从 master 新建 hotfix 分支用于修复 bug。

- 完成后将 hotfix 合并到 develop 进行测试。

- 测试通过后合并到 master 进行发布，并打上小版本号的 tag 标签。

- **若当前存在 release 分支，还需将 hotfix 合并到 release。**

- 删除 hotfix 分支。

### git flow 分支使用规范

##### 临时分支命名规范

| 临时分支 | 前缀     | 备注                                                         |
| -------- | -------- | ------------------------------------------------------------ |
| feature  | feature- | 若 gitlab 中包含任务 issue，可以采用 feature-{issueid}-{简单描述}命名 |
| release  | release- | 以版本名称或版本号结尾                                       |
| hotfix   | hotfix-  | 若 gitlab 中包含 bug issue，可以采用 feature-{issueid}-{简单描述}命名 |
| sprint   | sprint-  | 以迭代名称或版本号结尾                                       |

CI 管道可通过前缀匹配进行相关的自动化操作。

分支前缀，一般用-/连接，很少用*，*表示两个词语有关联，feature 分支这里实际上是一种分组，所以一般用-/，可以在前缀之后使用_。

##### feature 分支使用规范

- 分支粒度：以一个功能为单位，尽量对应 gitlab 上的一个任务 issue，开发下一个功能时新建 feature

- 存在时间：尽量短，合并到 develop 后选择合适的时间点删除 feature

- 集成频率：每天最少一次，feature 要经常合并到 develop 中，便于 code review，开发要经常从 develop 中获取最新代码；

- 合并请求：当 feature 完成的时候即可发起 MR，但当一个 feature 开发时间较长时，也应当尽量提早发起 MR，此时可在 MR 中添加 WIP 前缀，表示当前工作正在处理中，相关人员可以提前审核，但不合并

[![img](http://image.wangxiaohuan.com/blog/image/202303071549354.png)]

##### hotfix 分支使用规范

- 与 feature 鉴别：线上除紧急 bug 修复，当需要做功能性调整并快速上线时，也需要新建 hotfix 分支，而非 feature。

##### sprint 分支使用规范

- sprint 的使用与 develop 相同，也为保护性分支。

- 需要从 sprint 中分出 feature 分支进行开发，从 feature 提交 MR 到 sprint 中做代码审查。

##### release 分支使用规范

- release 为保护性分支。

- 使用 hotfix 分支进行 bug 修复。