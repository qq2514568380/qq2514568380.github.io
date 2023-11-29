# docker-compose怎么来的


{{< music auto="https://music.163.com/#/playlist?id=60198" >}}

> 转载自[4'000 Stars and counting, a trip down memory lane](https://brianchristner.io/4000-stars-and-counting-a-trip-down-memory-lane/ "Visit Upstage!")

我很幸运能够在 2014 年成为第一批踏上 Docker 之旅的人之一，尝试将监控堆栈容器化。我在 2014 年初开始了解 Docker，并花了几个月的时间弄清楚它是如何工作的以及在工程师的 DM 中修复错误并提出大量问题。此时只有少数人在聊天，而且社区很小，我们认识每个人。

作为一名监控和数据极客，我的任务是在 2014 年为我们的新云企业构建适应性更强的监控解决方案。最初，我尝试在 Nagios 等 Docker 容器中重建当前的监控解决方案，然后才对容器友好。 、Centreon、Piwik 分析和许多其他工具并没有真正解决我的问题。我偶然发现 InfluxDB 和 Grafana，但很快意识到它不符合我们的用例。

经过对不同工具的多次尝试和错误后，我偶然发现了 Soundcloud 基础设施博客以及他们如何使用微服务架构构建用 Go 编写的时间序列监控工具。在当时，这似乎太过超前和牵强，甚至显得不真实。如果我没记错的话，这是一篇向公众推出 Prometheus 的博客文章 - [https://developers.soundcloud.com/blog/prometheus-monitoring-at-soundcloud](https://developers.soundcloud.com/blog/prometheus-monitoring-at-soundcloud?ref=brianchristner.io)

**首先是将所有东西容器化。**

不管你信不信，回到 2014 年，那是一个没有编曲家的时代。大多数人将一堆单个 Docker 运行命令与 bash 脚本串在一起，并尝试使用 bash 来“编排”容器。

幸运的是，2015 年，一家名为 Orchard 的公司正忙于构建第一个名为“ *fig”的 Docker 容器编排器。*Fig 绝对是一个了不起的工具。你可以编写一些奇怪的代码，称为 YAML，这是我之前从未见过的，基本上，将所有 Docker 运行命令串到一个文件中，允许你将微服务构建到一个文件中......

![img](https://media.tenor.com/OGVAmTtvE70AAAAC/wow-amazed.gif)

Docker 运行命令的单个文件 == 令人震惊

在我开始使用Fig并加快速度后不久，Docker宣布他们已经[收购了母公司Orchard](https://www.informationweek.com/infrastructure-as-a-service/docker-acquires-devops-flavor-with-fig?ref=brianchristner.io#)。[您还可以在 Docker Compose 版本历史记录中看到Fig 于 2014 年 10 月加入 Docker](https://www.informationweek.com/infrastructure-as-a-service/docker-acquires-devops-flavor-with-fig?ref=brianchristner.io#)时的精彩历史快照。

几个月后，[Docker 在 2015 年 2 月将 Fig 更名为 Docker Compose](https://docs.docker.com/compose/release-notes/?ref=brianchristner.io#110)，这是容器历史上我永远不会忘记的里程碑。之后，我开始研究如何将我最喜欢的所有监控工具放入一个撰写文件中。我开始使用 Prometheus、Grafana、Node Exporters 和 Google cAdvisor。我花了几周的时间才弄清楚 compose 内部的网络，因为这还处于早期阶段，我们今天认为理所当然的很多东西还没有实现。让容器通过多个服务相互通信、配置、安装存储等是一项任务，但它确实有效。

下面您可以看到我使用 Prometheus 和 Grafana 创建的第一个仪表板。它并不漂亮，但是嘿，它在 Docker Compose 中协同工作，而且还监视容器和基础设施。

![img](https://brianchristner.io/content/images/2023/07/Dashboard_Prometheus.png)Prometheus 和 Grafana 仪表板的第一个版本

在它启动并运行之后，我在博客上写了几次关于它的文章，它获得了 Docker、Google 和整个社区的大量关注。谷歌实际上主动联系我，要求我在他们位于瑞士苏黎世的总部展示我正在开发的产品。

我不知道我是否是第一个使用 Docker 和 Compose 创建完整监控堆栈的人，但我绝对是第一个创建详细手册、简单的一键部署指南、Docker 监控研讨会并点击会议巡回培训 100 名人员如何开始使用 Docker 和监控。很快就成为第一批 Docker 队长之一，写书，参加采访，并很早就成为 Docker 社区不可或缺的一部分。

[Docker Prometheus 项目](https://github.com/vegasbrianc/prometheus?ref=brianchristner.io)的目标是帮助他人并回馈社区，这对我来说始终很重要。开源不仅对于编写代码非常重要，而且对于支持、教导和指导他人也非常重要。我相信这个项目帮助启动了其他一些项目和组织，以继续构建令人惊叹的事物。

接下来我知道，Docker Prometheus GitHub 项目的启动次数为 1k，然后是 2k，现在是 4k。GitHub 的星星曾经有较早的含义；一些公司甚至使用 GitHub star 的数量作为向 VC 推销的指标。现在，这只是一个虚荣指标，但我仍然喜欢偶尔检查和反思。

这是一段令人惊奇的旅程，但距离完成还很遥远。我鼓励每个人尽其所能帮助回馈开源，从编写代码到修复文档。每一项贡献都有帮助
