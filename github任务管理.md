##任务管理

[任务主页](https://github.com/Pamakids/PamakidsThings/issues?state=open)

某些任务完成则完成某个[里程碑](https://github.com/Pamakids/PamakidsThings/issues/milestones?page=1&sort=due_date)，一个里程碑可理解为一个版本

任务标签分为`asset`(缺少资源)、`bug`(程序错误)、`enhancement`(功能增强新需求增加)以及`ready`(bug或enhancement完成后待验证)，简单的流程如下：

1. `产品或测试`发现程序问题或有新的需求，新建问题
2. `开发`发现有问题或需求完成后，提交代码并将问题标签修改为`ready`
3. `产品或测试`在稍后发布的测试版里对ready问题一一进行验证，确认没问题就`close`
4. issue解决完需要发布之前，需要根据`需求文档`通测确保没有任何问题才能发布版本

