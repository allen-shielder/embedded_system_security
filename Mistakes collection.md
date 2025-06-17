
## Role of the Operating System in Securit
操作系统要控制权限、主动防御、优先安全、应对漏洞、防 root 被利用。

The operating system must control permissions, implement proactive defense, prioritize security, respond to vulnerabilities, and prevent root privilege exploitation.

---

## Dual-Kernel Systems（双内核系统）

| 核心点      | 内容说明                                                                                            |
| -------- | ----------------------------------------------------------------------------------------------- |
| **定义**   | 2 kernel(Security and ordinary) in the OS                                                       |
| **设计目的** | 兼顾**安全可信与复杂功能的并存**，将高风险组件与安全关键组件隔离运行                                                            |
| **运行方式** | security kernel run security related business like security policy. Ordinary kernel run feature |
| **安全策略** | **最小化信任边界**：只对安全内核进行高安全验证（如形式化验证），降低攻击面                                                         |
| **典型架构** | 多用于高安全要求嵌入式系统，如**航空航天、军用系统、安全通讯设备**                                                             |

---

## Core Embedded Operating System Security Requirements

| 类别           | 安全点                           | 中文关键词 | 一句话记忆重点                   |
| ------------ | ----------------------------- | ----- | ------------------------- |
| ✅ **隔离与保护类** | **1. Memory Protection**      | 内存隔离  | 每个任务的内存互不干扰，防止越权访问。       |
|              | **2. Virtual Memory**         | 虚拟地址  | 每个任务用自己的“假地址”，互相看不到。      |
|              | **5. Virtual Device Drivers** | 虚拟驱动  | 不直接接触硬件，统一用“虚拟设备”访问，减少风险。 |
| ✅ **资源与恢复类** | **3. Fault Recovery**         | 故障恢复  | 程序崩了不能影响系统，能自动“拉回来”。      |
|              | **4. Guaranteed Resources**   | 资源预留  | 每个任务有自己的资源份额，别人抢不走。       |
| ✅ **行为与调度类** | **6. Impact of Determinism**  | 行为可预期 | 系统响应时间要稳定，不能乱跳，特别是实时系统。   |
|              | **7. Secure Scheduling**      | 安全调度  | 高优先级任务先做，不能被低优任务堵住或拖慢。    |

---


对称加密（Symmetric Encryption）

| 项目/机制               | 说明                   |
| ------------------- | -------------------- |
| **AES**             | 高强度对称加密算法，广泛用于通信、磁盘等 |
| **DES / 3DES**      | 早期对称加密算法，已逐渐淘汰       |
| **IDEA**            | PGP 中用的对称算法，适合块加密    |
| **RC4**             | 早期流加密算法，已废弃          |
| **Salsa20 / Grain** | 新一代流加密算法，用于资源受限设备    |
| **HMAC-SHA256**     | MAC 是对称结构，密钥+哈希实现认证码 |
| **MAC（消息认证码）**      | 必须共享密钥，典型对称机制        |
|                     |                      |

非对称加密（Asymmetric Encryption）

| 项目/机制                 | 说明                                    |
| --------------------- | ------------------------------------- |
| **RSA**               | 最经典非对称加密算法，用于加密与签名                    |
| **ECC（椭圆曲线）**         | 高效的非对称算法，密钥更短                         |
| **ECDSA**             | ECC上的签名算法，用于数字签名                      |
| **Digital Signature** | 数字签名本质 = 私钥签名 + 公钥验证                  |
| **TPM 内的 EK/AIK**     | TPM 模块内的 Endorsement Key（不可更换的非对称密钥对） |
| 数字签名机制                | 基于私钥签名、公钥验证（非对称核心机制）                  |

混合结构（结合对称 + 非对称）

| 项目/机制            | 说明                                |
| ---------------- | --------------------------------- |
| **TLS 协议**       | 握手阶段用非对称（RSA/ECDHE）→ 数据阶段用对称（AES） |
| **GPG / PGP 加密** | 公钥加密对称密钥 + 对称加密正文                 |

---
## 一、定义与核心作用（考试常考定义）

Separation Kernel is very simple  OS based on MILS. It keep each partition completely isolated. Separation Kernel also can control the damage effect, which means if one partition damaged others will not be affected.

## 二、Separation Kernel Policies(MILS Policies also)

| 安全特性                      | 背诵关键词  | 解释                                                                                  |     |
| ------------------------- | ------ | ----------------------------------------------------------------------------------- | --- |
| **1. Information Flow**   | 信息流控制  | Data can not be flow from one partitions to another until security police permitted |     |
| **2. Data Isolation**     | 数据隔离   | Data in different partition can not be shared, read or write                        |     |
| **3. Damage Limitation**  | 限制破坏传播 | If one partition damaged others will not be affected                                |     |
| **4. Periods Processing** | 处理周期控制 | system must prevent data leaking via cache, buffer or registers                     |     |

## 三、Separation Kernel mechanism(Separation Kernel Only)

| 英文术语                  | 中文术语       | 含义解释                                                                                          |
| --------------------- | ---------- | --------------------------------------------------------------------------------------------- |
| **1. Always Invoked** | 始终调用（永远检查） | ==Application== request ==resources and services== from OS can not bypass ==security policy== |
| **2. Tamper-proof**   | 防篡改（不可更改）  | ==Security policy== should be protected and not be changed via ==application==                |
| **3. Evaluable**      | 可评估（可验证）   | ==Components== can be independently evaluated at the highest assurance level                  |
|                       |            |                                                                                               |

---

## TPM Discrete Components 

| 功能类别           | 模块组件                                    | 功能关键词               |
| -------------- | --------------------------------------- | ------------------- |
| 🔁 **IO**      | Input/Output (I/O)                      | TPM 与主系统通信接口        |
| 💾 **存储相关**    | Non-Volatile Storage                    | 存储密钥                |
|                | Platform Configuration Registers (PCRs) | 存储系统状态（度量值）         |
| 🔐 **密钥/加密相关** | Attestation Identity Keys (AIKs)        | 远程证明使用的密钥           |
|                | RSA Key Generation                      | 生成签名/存储密钥（2048位）    |
|                | RSA Engine                              | 提供 RSA 签名、加解密功能     |
|                | SHA-1 Engine                            | 哈希运算，用于签名/密钥 Blob 等 |
|                | Random Number Generator (RNG)           | 生成密钥/nonce 等        |
| ⚙️ **控制类**     | Execution Engine                        | 执行 TPM 内部程序（初始化、测量） |
|                | Program Code                            | 内部固件，执行度量逻辑         |
|                | Opt-In                                  | 允许用户禁用 TPM          |

---

| OSI 层级  | 代表协议                |
| ------- | ------------------- |
| 7 应用层   | HTTP、FTP、SMTP、TLS ✅ |
| 6 表示层   | 加密、编码、压缩等（TLS 涉及）   |
| 5 会话层   | SSL/TLS 建立安全会话 ✅    |
| 4 传输层   | TCP、UDP             |
| 3 网络层   | IP、ICMP             |
| 2 数据链路层 | Ethernet            |
| 1 物理层   | 电缆、射频、光纤信号          |

---

UART 协议的数据帧中，起始位（Start Bit）是==0(low)==

I2C 协议最多可以支持==128==个唯一设备地址（7位地址）

==CSMA/CD==以太网协议机制用于避免同时发送引发冲突

==Shell 接管==可以利用 UART 调试接口进行

在 GPG 加密流程中，接收者必须拥有发送者的==公钥==

==确认消息（ACK）==机制可用于检测 ==CoAP== 消息是否成功收到

==需要共享密钥==属于 MAC（Message Authentication Code）的特征

==I2C== 协议通过“仲裁机制”保证总线上只有一个主设备发送

Zigbee 最适合用于==智能家居中传感器控制==

==Clock Driven==调度策略最适合处理周期性任务(Round Robin不适合因为Round Robin 对周期性任务没有固定调度)


==优先级继承==机制可以防止任务因资源冲突而出现优先级反转

在数字签名中，签名方使用==私钥==进行签名

消息认证码（MAC）的输出结果依赖于==密钥和数据==

CVE-2019-25162 描述的是==Use-after-free==

---

1. UART 数据帧通常包括：1位起始位（为0），5–9位数据位，0或1位奇偶校验位（可选），1或2位停止位（为1）。用于异步串行传输。
    
2. Confused Deputy 是一种权限混淆攻击。攻击者利用系统中==权限较高==的组件来执行本不应执行的操作。例如让打印服务替自己写入本不应访问的日志路径。
    
3. 信息流控制是MILS中的一项策略，指的是除非得到授权，数据不能在不同安全域间流动，以防数据泄露或越权访问。
    
4. 分离内核（Separation Kernel）只提供最基本的资源隔离、时间隔离和调度功能，并可被验证（Tamper-proof），防止系统一部分故障影响全局。
    
5. 抢占式调度允许高优先级任务中断当前任务执行，静态资源分配确保高优先级任务资源不会被低优先级任务占用，两者配合保障实时性和可预测性。

- I2C 使用两根线（SCL/SDA），支持多主多从结构，优点是接线少、支持多个设备；缺点是速度慢、距离短、易受噪声干扰。
    
- Secure Boot 是一种启动时验证机制，只有签名通过验证的固件/操作系统才能启动，防止恶意代码植入。

    
- ==Hash== 只==对数据摘要==，任何人都可计算；==MAC 结合密钥进行摘要==，==提供完整性 + 身份验证==。
- ==HMAC = Key + Hash 函数==，==输出固定长度认证码==；用于==验证数据完整性与来源==，如 **API 请求签名、消息传输认证**。

- ==数字签名==使用==非对称密钥对消息摘要签名==，==验证身份 + 完整性 + 不可否认性==, ==数字签名可公开验证==；==MAC是对称密钥 只验证完整性和身份==。

- Periods Processing 是一种时间隔离机制，用于清理共享资源（如缓存、寄存器）防止数据在不同任务间泄漏。



    
- MQTT 使用发布/订阅机制，轻量、低功耗，适合 IoT；风险包括==认证缺失、明文传输、DoS 攻击==。
    
- Stream Cipher 加密效率高、硬件开销小，适合嵌入式设备；但对密钥管理敏感，密钥重复使用可能导致泄密。

- Round Robin 每个任务轮流使用 CPU 一段时间（公平但可能缺乏实时性）；Clock Driven 是按预设时间周期运行任务，适合实时任务。
    
- UART 接口暴露系统命令行或调试功能，攻击者可连接串口直接进入 root shell 或上传恶意固件，属于高风险通道。
    

    
- TLS 提供三大核心安全性：加密（防窃听）、完整性（防篡改）、认证（验证身份），共同保障通信安全。

- TLS 
	- TLS 握手(5次)
		客户端发起请求  → 服务端返回证书和公钥 → 客户端生成密钥 → 双方确认密钥 → 建立加密通道。
	- 密钥协商
		==客户端==使用服务器的<span style="background:#fff88f">公钥加密会话</span>密钥，<span style="background:#fff88f">服务器用私钥解密</span>，之后==双方用对称密钥进行通信==

- 数字签名的流程:
	- sender: hash the message -> sign the hash with private key -> send message with signature
	- receiver: verify the signature with sender's public key -> if it match, the authentication is successfully

