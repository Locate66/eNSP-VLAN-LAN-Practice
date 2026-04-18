# 企业级园区网 VLAN 规划与隔离部署实战 (VLAN-Enterprise-Implementation)

## 📌 项目简介
[cite_start]本项目基于 **Huawei eNSP** 模拟器开发，模拟了一个典型的中小型企业办公网络架构。项目核心目标是通过 VLAN 技术实现部门间的二层逻辑隔离，优化广播域，并为后续的三层互通与安全策略打下基础 [cite: 1]。

## 🏗️ 网络拓扑架构
> <img width="479" height="270" alt="image" src="https://github.com/user-attachments/assets/09d68a6c-f49a-4aea-9a36-b6216800abc8" />


### 1. 业务 VLAN 规划
| 业务部门 | VLAN ID | IP 网段 (建议) | 描述 |
| :--- | :--- | :--- | :--- |
| 财务部/PC1 | VLAN 10 | 192.168.10.0/24 | 核心敏感数据区 |
| 销售部/PC2 | VLAN 20 | 192.168.20.0/24 | 普通业务办公区 |
| 备用/PC3 | VLAN 30 | 192.168.30.0/24 | 预留业务扩展区 |

## 🛠️ 核心配置逻辑
[cite_start]本项目包含四台华为交换机（SW1-SW4），实现了接入层到汇聚层的完整链路配置 [cite: 1]。

### 1. 接入层 (Access Layer)
[cite_start]在 SW1/SW2 上通过 `port link-type access` 将终端接口划分至对应 VLAN，确保流量进入交换机时被打上正确的 VLAN 标签 [cite: 1]。

### 2. 汇聚层 (Distribution Layer)
[cite_start]在 SW3/SW4 之间及各级联口配置 `port link-type trunk`，并使用 `port trunk allow-pass vlan all` 放行所有业务 VLAN，实现跨设备的数据透传 [cite: 1]。

## 📂 文件目录说明
* `/configs/` : 包含 SW1、SW2、SW3、SW4 的完整 `display current-configuration` 文本脚本。
* `Enterprise_VLAN_Lab.topo` : eNSP 工程源文件，下载后可直接在模拟器中运行测试。
* `README.md` : 项目说明文档。

## ✅ 实验验证与分析
* [cite_start]**同 VLAN 连通性**：PC1 与 PC2 在同一 VLAN 下可正常通信，延迟稳定 [cite: 1]。
* [cite_start]**跨 VLAN 隔离性**：PC1 与 PC3 分属不同 VLAN，在未配置三层路由前，二层报文被物理隔离，符合安全预期 [cite: 1]。
* **排障心得**：实验过程中重点解决了 Trunk 链路未放行特定 VLAN 导致的通信故障，强化了对 802.1Q 封装过程的理解。

---
**作者**：Locate66
