# 分布式爬虫系统

## 项目简介
这是一个基于Python实现的分布式爬虫系统，旨在高效地抓取和处理网页数据。系统通过多个worker进行任务并行处理，并使用Redis作为任务队列进行URL分发。系统支持多种URL分发策略，包括轮询、哈希分配和负载均衡，能够自动化地管理和分发URL任务。

## 功能特性
- **URL管理**：系统可以管理URL的去重，并支持将URLs添加到Redis数据库。
- **多种分发策略**：支持轮询、哈希分配和负载均衡等任务分发策略。
- **分布式处理**：通过多个worker并行抓取和处理网页内容，提升了系统的抓取效率。
- **任务重试机制**：当某个worker任务失败时，系统会自动将任务重新放入队列以便重试。
- **关键词提取**：支持使用TF-IDF或TextRank算法从网页内容中提取关键词。
- **数据存储**：抓取的网页内容和提取的关键词会被保存为JSON文件，便于后续分析和存档。

## 系统架构
该系统由以下几个主要组件构成：
- **URLManager**：负责管理URL，支持将URL添加到Redis并选择不同的分发策略来分配给worker。
- **Worker**：负责从任务队列中获取URL，抓取网页内容，进行分析并将结果保存为文件。
- **Redis**：作为任务队列，用于存储待处理的URL以及每个worker的任务状态。

## 目录结构

为了帮助理解项目结构，以下是详细的目录树展示：

```plaintext
/project
│
├── /workers               # Worker代码目录
│   ├── worker.py          # Worker的主要实现
│
├── /src                   # 核心代码目录
│   ├── url_manager.py     # URL管理器
│   ├── proxy_pool.py      # 代理池
│   ├── dns_cache.py       # DNS缓存
│   ├── task_distribute.py # 任务分发模块
│
├── /data                  # 存储抓取的数据
│   └── output.json        # 抓取数据的存储文件
├── README.md              # 项目的说明文件
└── requirements.txt       # 依赖包列表

## 安装与运行

### 1. 克隆项目
首先，将该项目克隆到本地：
```bash
git clone https://github.com/your-username/distributed-crawler.git
cd distributed-crawler
2. 安装依赖
确保你已经安装了Python 3.x，并且可以使用pip进行包管理。然后安装所需的依赖：

bash
pip install -r requirements.txt
3. 配置Redis
本系统依赖Redis作为任务队列，确保你已经安装并启动了Redis服务。你可以在本地机器上启动Redis，或者使用远程Redis服务。默认情况下，系统会连接到localhost:6379。

4. 启动URL管理器
首先，启动URL管理器来管理URL的添加和分发：

bash
python src/url_manager.py
5. 启动Worker
接下来，启动多个worker来处理分配给它们的任务：

bash
python workers/worker.py
6. 添加URL
你可以通过URLManager添加待抓取的URL。例如：

python
url_list = ["http://example.com", "https://www.baidu.com"]
for url in url_list:
    if manager.add_url(url):
        assigned_worker = manager.distribute(url, method='load_balanced')
        print(f'URL: {url} -> 分配给: {assigned_worker}')
7. 查看结果
抓取的数据将被保存在/data目录下的JSON文件中。你可以查看每个URL对应的抓取内容以及提取的关键词。

分发策略
轮询（Round Robin）：每个worker轮流接受任务，适用于任务量相对均衡的情况。
哈希分配（Hash-Based）：根据URL的哈希值将任务固定分配给特定worker。
负载均衡（Load-Balanced）：将任务分配给任务最少的worker，以确保负载均匀分配。
扩展与优化
代理池：可以通过ProxyPool类配置代理池，在爬取过程中使用代理IP，提高抓取的稳定性。
DNS缓存：通过DNSCache类缓存域名解析结果，减少重复的DNS请求。
多种关键词提取算法：支持使用TF-IDF或TextRank算法提取网页中的关键词，提升数据的分析能力。
许可证
本项目采用MIT许可证，详细内容请查看LICENSE文件。

贡献
欢迎提出问题或贡献代码！如果您有任何建议或遇到问题，欢迎提交Issues或Pull Requests。
---

这个`README.md`文件包括了项目的功能、安装步骤、目录结构和使用说明。你可以直接将其复制并放入你的GitHub项目中。如果有任何进一步的调整或定制化需求，请随时告诉我！
