### 📌 **一、嵌入式系统中的操作系统角色**

- OS 必须**防止未授权访问**。
    
- 与通用系统不同，嵌入式系统中的安全不是事后考虑，而是**核心优先级**。
    
- 嵌入式 Linux 虽然常用，但复杂性和更新频繁使其存在大量漏洞。
    
- 例子：SELinux 提供多重虚拟桌面支持，常用于敏感数据环境（如美国军用系统）。
    

---

### 📌 **二、实时操作系统（RTOS）概念**

- **RTOS vs 普通 OS**：关键在于是否**时间敏感**。
    
- 分类：
    
    - **硬实时系统（Hard RTOS）**：必须按时完成（如空中交通管控、核电站控制）
        
    - **软实时系统（Soft RTOS）**：可以偶尔延迟（如多媒体播放器）
        
- RTOS 功能包括：
    
    - 任务管理（Task Management）
        
    - 调度（Scheduling）：Round Robin、Priority、Clock-driven
        
    - 资源分配
        
    - 中断处理
        

---

### 📌 **三、RTOS 的内核架构**

- **Microkernel**：更安全、模块化（如 QNX、FreeRTOS 部分版本）
    
- **Monolithic Kernel**：性能好但更复杂（如 Linux with RT patches）
    
- **Dual Kernel**：GPOS 与 RTOS 共存，存在安全威胁（如资源冲突）
    

---

### 📌 **四、MILS 安全架构与 Separation Kernel**

- **MILS（Multiple Independent Levels of Security）**
    
    - 数据隔离（Data Isolation）
        
    - 信息流控制（Information Flow Control）
        
    - 损害限制（Damage Limitation）
        
    - 时段处理（Periods Processing）
        
- **Separation Kernel** 特性：
    
    - Always Invoked
        
    - Tamper-proof
        
    - Evaluable
        

---

### 📌 **五、嵌入式系统安全核心要求**

- **内存保护**与**虚拟内存**：物理地址隐藏、进程隔离
    
- **故障恢复**、**资源保障（quotas）**
    
- **虚拟设备驱动**：防止内核遭攻击
    
- **确定性行为**与**安全调度**
    

---

### 📌 **六、安全问题与缓解措施示例**

|漏洞类型|示例|缓解方式|
|---|---|---|
|Guaranteed Resources|fork bomb 造成 DoS 攻击|安全分区，微内核不共享资源|
|Device Driver|驱动程序栈溢出|使用虚拟设备驱动程序|
|Timing Side Channel|操作系统调用响应时间泄露敏感信息|WCET分析、通道带宽测量|

---

### 📌 **七、访问控制与能力机制**

- **DAC（自主访问控制）** & **MAC（强制访问控制）**
    
- **Capabilities（能力令牌）**：一种简单高效的权限控制机制
    
- **Whitelist vs Blacklist**：白名单更安全，黑名单容易产生权限过大漏洞
    
- **Confused Deputy**：特权提升问题
    

---

### 📌 **八、Hypervisor（虚拟机监控器）**

- 允许多个虚拟系统共享同一硬件
    
- 嵌入式系统中的 Hypervisor 类型：
    
    - Type 2
        
    - Monolithic
        
    - Console Guest
        
    - Microkernel-based
        

---

### 📌 **九、远程管理与可信计算基**

- **远程补丁管理** 示例：
    
    - 黑匣子（飞行记录器）
        
    - 火星探测器 Pathfinder
        
- **安全问题**：
    
    - 远程管理端口可能成为攻击入口
        
- **TCB（Trusted Computing Base）完整性保证**：
    
    - Secure Boot
        
    - Remote Attestation