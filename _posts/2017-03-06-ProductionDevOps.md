---
layout: post
title: DevOps on Production
description: "DevOps on Production"
tags: [DevOps]
image:
  background: triangular.png
---

# 生产环境之运维 #
最近在生产环境上游历了一番，大版本升级，流量控制，问题的追根索源，甚是各种艰辛。

# 线上资源配置问题 #
* 池化连接
  - 必须清楚了解业务进行设置
    + 主要是连接数个数与闲置回收的时间: 防止连接数瓶颈。
    + 超时设置：防止在网络抖动或者数据库不可用的状况下，业务核心线程被阻塞，连接池爆浆。
  - e.g. 数据库连接池
    + 池的最小最大值（不同数据库需）
    + 从DPCP获取连接超时
    + 连接超时
    + 读超时
    + socket超时
    + 重试次数与每次重试相隔时间
* NoSQL
  - 容量 （dimension）
  - TTL
  - 连接管理
* Dev提供友好的配置指引，工程公式及原理，让Ops了解当前系统在不同的部署环境下如何合理配置系统资源。
  - 系统连接数及计算公式
  - 客户端（e.g. http client）的超时设置
  - 事务的超时设置
* 核心业务调用非核心业务的超时
  - 为免非核心业务出现问题时导致主业务线程阻塞，需配备超时设置

# 系统高并发保护 #
系统高并发流量保护是十分重要的。
高并发流量有分正常的业务量与非正常的业务量。非正常的业务量有可能是客户端/服务商的异常行为导致，也有可能是系统被洪水攻击。
当非正常业务流量进入系统的时候，系统保护尤为关键的。
* 系统保护设计相关注意事项
  - 限流 （最常用的系统保护策略之一）
    + 要在业务入口限流。而不是让流量进入到系统内部，消耗各个内部部件（如负载均衡器等）才由核心业务层按照业务的SLA指标限流。
    否则，会导致以下问题。
      - 内部资源被耗尽，如内部负载均衡器资源。
      - 流量进入系统过深，占用过多系统核心业务资源，影响系统正常业务处理能力。
    + 限流应该返回正确的状态码，让客户端正确知道服务器当前状态，而不是简单告知系统错误或系统不可用
      - 不论是http还是电信业务协议，均有限流关联的状态码，用于告知客户端需要等待一段时间再尝试请求，留给系统有一定的喘息时间。
      - 需要在系统接口文档加以强调该返回状态码，规范客户端行为。当然这只是纯粹行为规范，毕竟很难保证客户端的正确行为。
    + 不论系统采取基于令牌算法还是漏桶算法的限流机制，都需要正确认识系统能支撑吞吐量的阈值，并在线下充分测试。
    + 层级限流：在保证不超系统阀值的情况下，让流量进入，各层次逐渐收窄，尽量保证服务的可用性同时又能保护系统。比如
      - 接口层通过漏桶/令牌算法放进流量，业务应用层再根据不同的具体细分业务的SLA进行限流。
  - 降级
    + 基于各种指标的自动化有损降级机制
      - 远程调用降级：比如核心业务远程调用非核心的三方的服务，出现超时时，基于服务性质，看能否先bypass一段时间。
      - 基于系统关键资源，负载情况降级。
  - 缓冲
    + 系统有预留的缓冲队列与资源（线程池），支持系统瓶颈时切换为异步调用。
  - 总体来说，系统保护都需要业务提供当前指标做算法统计，才能充分自动化各种操作。不能否决人为操作，但尽量要避免。
  设计上最好能做到对业务是无侵入的方式，避免各种耦合带来的问题。

# 运维 #
  - 切忌对当前系统的支撑能力与安全保障过份自信，防微杜渐，慎防客户端不可预测的行为与洪水攻击。
    + “墨菲定理”： 如果你担心某种情况发生，那么它就更有可能发生。
  - 加强监控
    + 监控比自动化来得更重要。通过可视化的监控与趋势的预测与预警，能提高运维的能效。
    + 完善日志的输出与收集，outage check list。
      - 虽然通过日志在线发现问题，定位问题，效率堪忧。但是日志能给Dev做事后分析，定位root cause。
  - Ops与Dev加强合作与演练，当线上问题出现才临时呼叫Dev team，秩序混乱，效率，沟通成本太高。
    + 类似Netflix的chaos monkey，随时随地模拟各种恶劣情况，扰乱生产环境（当然是模拟生产环境的staging）的演练。
  - 应急预案
    + 在多机房部署下，应当切换流量开关，尝试先让部分流量切换到其他机房处理，减轻当前机房压力以待观察能否慢慢让系统恢复，提高正常业务处理能力。否则，多机房部署成了纸上模型。
    + 如果快速定位到是客户端问题，需要联系服务商一并修复。

# DevOps #
上面讲的都是原则性的东西，更多的是需要在设计层面，运维层面迭代运转，相互促进。因此DevOps的文化，工具，沟通都是十分的重要。
线下需要同时去系统的性能进行测试，评估，解决瓶颈问题，以及各种演练。系统在上线前需要充分地soak，保证系统能良好的运转与稳定性。
* 开发团队注重运维团队对系统和线上问题的反馈，对收集的日志做及时分析与bug修复。
* 测试团队要保证测试点与测试用例的覆盖率，最好有持续集成测试保证功能的完备性。
* 运维团队需要定义好各种异常场景的模型与预案。当问题出现的时候能及时采取措施而不至于手足无措。
* 监控工具，自动化运维工具需要维护好一个backlog，通过不断的反馈促进工具的开发与运作。
* 沟通方面，需要团队之间形成相互合作的文化，在职责分明的背景下，有序，协调地开展工作，才能高效地解决各种紧急问题。
  - 同步团队之间的知识。
  - 各自build up好competence。