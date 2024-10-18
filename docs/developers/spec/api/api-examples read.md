以下是该文档的中文翻译，以及每个接口的作用说明：

---
sidebar_position: 2
id: api-examples
---

# 实现参考

本指南旨在使用真实示例展示AVS节点API的实际应用。使用的示例包括：

- EigenDA

## EigenDA示例

### GET /eigen/node

作用：获取节点的基本信息，包括节点名称、规范版本和节点版本。

### 响应

```
startLine: 21
endLine: 25
```

### GET /eigen/node/health

作用：检查节点的健康状态。

### 响应

```
startLine: 31
endLine: 33
```

### GET /eigen/node/services

作用：获取节点上运行的所有服务的状态信息。

### 响应

```
startLine: 39
endLine: 59
```

### GET /eigen/node/services/graph-node-da/health

作用：检查特定服务（这里是graph-node-da）的健康状态。

### 响应

```
startLine: 65
endLine: 67
```

### GET /eigen/node/services/ipfs-da/health

作用：检查IPFS服务的健康状态。

### 响应

```
startLine: 73
endLine: 75
```

### GET /eigen/node/services/postgres-da/health

作用：检查PostgreSQL数据库服务的健康状态。

### 响应

```
startLine: 81
endLine: 83
```

这些接口共同构成了EigenDA节点的健康检查和状态监控系统。它们允许操作员和开发者快速了解节点及其各个组件的运行状况，有助于及时发现和解决潜在问题，确保EigenDA网络的稳定运行。