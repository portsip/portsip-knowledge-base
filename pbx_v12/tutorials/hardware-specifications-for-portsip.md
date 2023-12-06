# Hardware Specifications

## The Hardware Specifications for PortSIP PBX

The PortSIP PBX usually is deployed for SMB which users less than 50K,  the hardware specifications are listed below.

### **Up to 100 simultaneous calls**

<table data-header-hidden><thead><tr><th width="400">HARDWARE</th><th>REQUIREMENTS</th></tr></thead><tbody><tr><td>HARDWARE</td><td>REQUIREMENTS</td></tr><tr><td>CPU Family</td><td>Intel i5 (Gen.8) or higher</td></tr><tr><td>vCPUs</td><td>4</td></tr><tr><td>vCPUs with Recording</td><td>4</td></tr><tr><td>Memory</td><td>4 GB</td></tr><tr><td>HDD</td><td>100 GB</td></tr><tr><td>HDD with Recording</td><td>200 GB or higher</td></tr><tr><td>Virtualized</td><td>YES</td></tr><tr><td>Multi-tenant</td><td>YES</td></tr></tbody></table>

### **Up to 200 simultaneous calls**

| HARDWARE             | REQUIREMENTS               |
| -------------------- | -------------------------- |
| CPU Family           | Intel i5 (Gen.8) or higher |
| vCPUs                | 4                          |
| vCPUs with Recording | 6 - 8                      |
| Memory               | 8 GB                       |
| HDD                  | 100 GB                     |
| HDD with Recording   | 500 GB or higher           |
| Virtualized          | YES                        |
| Multi-tenant         | YES                        |

### **Up to 500 simultaneous calls**

| HARDWARE             | REQUIREMENTS               |
| -------------------- | -------------------------- |
| CPU Family           | Intel i7 (Gen.8) or higher |
| vCPUs                | 4                          |
| vCPUs with Recording | 8                          |
| Memory               | 8 GB                       |
| HDD                  | 100 GB                     |
| HDD with Recording   | 500 GB or higher           |
| Virtualized          | YES                        |
| Multi-tenant         | YES                        |

### **Up to 1,000 simultaneous calls**

| HARDWARE             | REQUIREMENTS               |
| -------------------- | -------------------------- |
| CPU Family           | Intel Xeon E5 v4 or higher |
| vCPUs                | 6 - 8                      |
| vCPUs with Recording | 8 - 10                     |
| Memory               | 16 GB                      |
| HDD                  | 100 GB                     |
| HDD with Recording   | 500 GB or higher           |
| Virtualized          | YES                        |
| Multi-tenant         | YES                        |

### **Up to 2,000 simultaneous calls**

| HARDWARE             | REQUIREMENTS               |
| -------------------- | -------------------------- |
| CPU Family           | Intel Xeon E5 v4 or higher |
| vCPUs                | 10                         |
| vCPUs with Recording | 16                         |
| Memory               | 16 GB                      |
| HDD                  | 100 GB                     |
| HDD with Recording   | 1T or higher               |
| Virtualized          | YES                        |
| Multi-tenant         | YES                        |

### **Up to 5,000 simultaneous calls**

| HARDWARE            | REQUIREMENTS               |
| ------------------- | -------------------------- |
| CPU Family          | Intel Xeon E7 v4 or higher |
| vCPUs               | 16                         |
| vCPUs wit Recording | 24 - 32                    |
| Memory              | 32 or 64 GB                |
| HDD                 | 100 GB                     |
| HDD with Recording  | 2 TB                       |
| Virtualized         | YES                        |
| Multi-tenant        | YES                        |

### **Up to 8,000 simultaneous calls**

| HARDWARE            | REQUIREMENTS               |
| ------------------- | -------------------------- |
| CPU Family          | Intel Xeon E7 v4 or higher |
| vCPUs               | 24                         |
| vCPUs wit Recording | 32 or 32+                  |
| Memory              | 64 GB or higher            |
| HDD                 | 100 GB                     |
| HDD with Recording  | 2T GB or higher            |
| Virtualized         | YES                        |
| Multi-tenant        | YES                        |



## The Hardware Specifications for PortSIP UCaaS

The PortSIP UCaaS is deployed as a Kubernetes Cluster with the following hardware specifications: it can support 200K users and can support more users by adding more servers.

### Typically Deployment

| Service    | Linux OS                                           | CPU Cores | <p>Memory</p><p>(GB)</p> | Hard Dis Size (GB)                                                                                      | Static Private IP | Public Static IP | Quantities |
| ---------- | -------------------------------------------------- | --------- | ------------------------ | ------------------------------------------------------------------------------------------------------- | ----------------- | ---------------- | ---------- |
| K8s Master | CentOS 7.9/Ubuntu 18.04/20.04, 64bit               | >=4       | >=8                      | <p>System partition >=50; </p><p>Data partition >=300                                              </p> | 1                 | 0                | >=3        |
| File       | CentOS 7.9/Ubuntu 18.04/20.04, 64bit               | >=4       | >=8                      | <p>System partition >=50; </p><p>Data partition >=1000</p>                                              | 1                 | 0                | 3          |
| DB         | CentOS 7.9/Ubuntu 18.04/20.04, 64bit               | >=4       | >=8                      | <p>System partition >=50; </p><p>Data partition >=500</p>                                               | 1                 | 0                | 2          |
| MQ         | CentOS 7.9/Ubuntu 18.04/20.04, 64bit               | >=4       | >=8                      | <p>System partition >=50; </p><p>Data partition >=300</p>                                               | 1                 | 0                | 3          |
| Monitoring | CentOS 7.9/Ubuntu 18.04/20.04, 64bit               | >=4       | >=8                      | <p>System partition >=50; </p><p>Data partition >=500</p>                                               | 1                 | 0                | >=1        |
| Edge Proxy | CentOS 7.9/Ubuntu 18.04/20.04, 64bit               | >=8       | >=8                      | <p>System partition >=50; </p><p>Data partition >=200</p>                                               | 1                 | 1                | >=2        |
| Media      | CentOS 7.9/Ubuntu 18.04/20.04, 64bit               | >=8       | >=8                      | <p>System partition >=50; </p><p>Data partition >=300</p>                                               | 1                 | 1                | >=2        |
| Others     | CentOS 7.9/Ubuntu 18.04/20.04, 64bit               | >=8       | >=8                      | <p>System partition >=50; </p><p>Data partition >=300</p>                                               | 1                 | 0                | >=6        |



### Minimal Deployment (Not recommended)

| Service                                                         | Linux OS                                           | CPU Cores | <p>Memory</p><p>(GB)</p> | Hard Dis Size (GB)                                                                                      | Static Private IP | Public Static IP | Quantities |
| --------------------------------------------------------------- | -------------------------------------------------- | --------- | ------------------------ | ------------------------------------------------------------------------------------------------------- | ----------------- | ---------------- | ---------- |
| K8s Master                                                      | CentOS 7.9/Ubuntu 18.04/20.04, 64bit               | >=4       | >=8                      | <p>System partition >=50; </p><p>Data partition >=300                                              </p> | 1                 | 0                | >=3        |
| <p>File </p><p>DB </p><p>MQ Monitoring and Others</p>           | CentOS 7.9/Ubuntu 18.04/20.04, 64bit               | >=8       | >=8                      | <p>System partition >=100; </p><p>Data partition >=1000</p>                                             | 1                 | 0                | 1          |
| <p>File </p><p>DB </p><p>MQ</p><p>and Others</p>                | CentOS 7.9/Ubuntu 18.04/20.04, 64bit               | >=8       | >=8                      | <p>System partition >=100; </p><p>Data partition >=1000</p>                                             | 1                 | 0                | 1          |
| <p>File </p><p>MQ </p><p>Edge Proxy Media </p><p>and Others</p> | CentOS 7.9/Ubuntu 18.04/20.04, 64bit               | >=8       | >=8                      | <p>System partition >=100; </p><p>Data partition >=1000</p>                                             | 1                 | 1                | 1          |

