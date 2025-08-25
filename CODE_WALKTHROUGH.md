# Magentic-UI 代码导读文档

## 项目概述

Magentic-UI 是由微软研究院开发的一个**人机协作式多智能体系统**的研究原型，专为网页自动化任务而设计。该项目基于 AutoGen 框架构建，提供了一个透明且可控的界面，让用户能够高效地与AI智能体协作完成复杂的网页任务。

### 🎯 核心特点
- **多智能体协作**：通过多个专门化的AI智能体协同工作
- **人机协作**：用户可以随时介入、指导和控制任务执行
- **透明化操作**：所有智能体行为都可见，增强可信度
- **实时网页操作**：支持真实的浏览器交互和网页自动化
- **代码生成与执行**：能够生成、修改和执行代码
- **文件处理**：支持文件分析、修改和生成

### 🏗️ 技术栈
- **后端**：Python 3.10+，FastAPI，AutoGen，Playwright
- **前端**：React，Gatsby，TypeScript，TailwindCSS
- **数据库**：PostgreSQL（通过 SQLModel）
- **容器化**：Docker，支持多容器部署
- **AI模型**：OpenAI GPT-4o，支持 Azure OpenAI 和 Ollama

---

## 系统架构

### 总体架构图

```
┌─────────────────────────────────────────────────────────────┐
│                    Magentic-UI 系统架构                     │
├─────────────────────────────────────────────────────────────┤
│  Frontend (React/Gatsby)                                   │
│  ┌─────────────────┐  ┌─────────────────┐                 │
│  │   Chat UI       │  │   Browser View  │                 │
│  │   - 任务规划     │  │   - 实时网页    │                 │
│  │   - 进度监控     │  │   - 操作预览    │                 │
│  │   - 用户交互     │  │   - VNC 连接    │                 │
│  └─────────────────┘  └─────────────────┘                 │
├─────────────────────────────────────────────────────────────┤
│  Backend API (FastAPI)                                     │
│  ┌─────────────────┐  ┌─────────────────┐                 │
│  │   WebSocket     │  │   REST API      │                 │
│  │   - 实时通信     │  │   - 会话管理    │                 │
│  │   - 事件流       │  │   - 计划存储    │                 │
│  └─────────────────┘  └─────────────────┘                 │
├─────────────────────────────────────────────────────────────┤
│  Multi-Agent System (AutoGen)                              │
│  ┌─────────────────┐  ┌─────────────────┐                 │
│  │   Orchestrator  │  │   Web Surfer    │                 │
│  │   - 任务协调     │  │   - 网页操作    │                 │
│  └─────────────────┘  └─────────────────┘                 │
│  ┌─────────────────┐  ┌─────────────────┐                 │
│  │   Coder         │  │   File Surfer   │                 │
│  │   - 代码生成     │  │   - 文件处理    │                 │
│  └─────────────────┘  └─────────────────┘                 │
├─────────────────────────────────────────────────────────────┤
│  Tools & Services                                          │
│  ┌─────────────────┐  ┌─────────────────┐                 │
│  │   Playwright    │  │   Docker        │                 │
│  │   - 浏览器控制   │  │   - 代码执行    │                 │
│  └─────────────────┘  └─────────────────┘                 │
└─────────────────────────────────────────────────────────────┘
```

---

## 项目结构详解

### 根目录结构

```
magentic-ui/
├── src/magentic_ui/           # Python 后端核心代码
├── frontend/                  # React/Gatsby 前端代码
├── docker/                    # Docker 容器配置
├── experiments/               # 实验和评估代码
├── samples/                   # 示例代码
├── tests/                     # 测试代码
├── docs/                      # 文档和媒体资源
├── pyproject.toml            # Python 项目配置
└── README.md                 # 项目说明
```

### 后端架构 (`src/magentic_ui/`)

#### 1. 核心模块

```python
# 主要入口点
src/magentic_ui/
├── __init__.py              # 主要API导出
├── task_team.py            # 智能体团队创建
├── magentic_ui_config.py   # 配置管理
├── types.py                # 类型定义
└── version.py              # 版本信息
```

#### 2. 智能体系统 (`agents/`)

```python
agents/
├── _coder.py               # 代码生成智能体
├── _user_proxy.py          # 用户代理智能体
├── web_surfer/            # 网页浏览智能体
│   ├── _web_surfer.py     # 主要实现
│   ├── _prompts.py        # 提示词模板
│   └── _tool_definitions.py # 工具定义
├── file_surfer/           # 文件处理智能体
│   ├── _file_surfer.py    # 主要实现
│   └── _tool_definitions.py # 工具定义
├── mcp/                   # MCP (Model Context Protocol) 智能体
│   ├── _agent.py          # MCP智能体实现
│   └── _config.py         # MCP配置
└── users/                 # 用户相关智能体
    ├── _dummy_user_proxy.py    # 虚拟用户代理
    └── _metadata_user_proxy.py # 元数据用户代理
```

#### 3. 后端服务 (`backend/`)

```python
backend/
├── cli.py                 # 命令行接口
├── web/                   # Web API
│   ├── app.py            # FastAPI 应用
│   ├── routes/           # API 路由
│   └── managers/         # 连接管理
├── database/             # 数据库
│   ├── db_manager.py     # 数据库管理
│   └── schema_manager.py # 模式管理
├── datamodel/            # 数据模型
│   ├── db.py            # 数据库模型
│   └── types.py         # 类型定义
└── teammanager/          # 团队管理
    └── teammanager.py    # 智能体团队管理
```

#### 4. 工具系统 (`tools/`)

```python
tools/
├── playwright/           # 浏览器控制
│   ├── browser.py       # 浏览器管理
│   └── controller.py    # 操作控制器
├── mcp/                 # MCP工具集成
├── bing_search.py       # Bing搜索工具
├── tool_metadata.py     # 工具元数据
└── url_status_manager.py # URL状态管理
```

#### 5. 团队协调 (`teams/`)

```python
teams/
├── orchestrator/         # 编排器
│   ├── orchestrator_config.py # 配置
│   └── orchestrator.py        # 主要实现
└── roundrobin_orchestrator.py # 轮询编排器
```

### 前端架构 (`frontend/`)

#### 1. 组件结构

```typescript
src/
├── components/
│   ├── common/              # 通用组件
│   │   ├── Button.tsx       # 按钮组件
│   │   ├── Icon.tsx         # 图标组件
│   │   └── markdownrender.tsx # Markdown渲染
│   ├── features/            # 功能组件
│   │   ├── Plans/          # 计划管理
│   │   └── McpServersConfig/ # MCP服务器配置
│   ├── settings/           # 设置相关
│   │   ├── SettingsModal.tsx # 设置模态框
│   │   └── tabs/           # 设置标签页
│   ├── views/              # 视图组件
│   │   ├── chat/           # 聊天界面
│   │   ├── manager.tsx     # 管理器视图
│   │   ├── sidebar.tsx     # 侧边栏
│   │   └── session_editor.tsx # 会话编辑器
│   └── types/              # 类型定义
│       ├── app.ts          # 应用类型
│       ├── datamodel.ts    # 数据模型类型
│       └── plan.ts         # 计划类型
├── hooks/                  # React Hooks
│   ├── provider.tsx        # 上下文提供者
│   └── store.tsx          # 状态管理
├── pages/                  # 页面组件
│   ├── index.tsx          # 首页
│   └── 404.tsx            # 404页面
└── styles/                # 样式文件
    └── global.css         # 全局样式
```

#### 2. 状态管理

使用 Zustand 进行状态管理：

```typescript
// store.tsx - 主要状态管理
interface AppState {
  sessions: Session[];          // 会话列表
  activeSession: string | null; // 当前活跃会话
  settings: Settings;           // 应用设置
  connectionStatus: string;     // 连接状态
}
```

---

## 智能体系统详解

### 1. 智能体类型

#### Orchestrator（编排器）
- **职责**：协调其他智能体，制定和管理任务计划
- **特点**：中央协调者，负责任务分解和分配
- **位置**：`src/magentic_ui/teams/orchestrator/`

#### Web Surfer（网页浏览者）
- **职责**：执行网页操作，如点击、填表、导航
- **工具**：Playwright 浏览器控制
- **特点**：支持实时网页交互，可视化操作
- **位置**：`src/magentic_ui/agents/web_surfer/`

#### Coder（编程者）
- **职责**：生成、修改和执行代码
- **支持语言**：Python、JavaScript等
- **特点**：代码执行在隔离的Docker容器中
- **位置**：`src/magentic_ui/agents/_coder.py`

#### File Surfer（文件处理者）
- **职责**：读取、分析和修改文件
- **支持格式**：文本、代码、文档等多种格式
- **特点**：智能文件内容理解和处理
- **位置**：`src/magentic_ui/agents/file_surfer/`

#### User Proxy（用户代理）
- **职责**：代表用户参与对话，处理用户输入
- **类型**：
  - `DummyUserProxy`：自动化场景
  - `MetadataUserProxy`：带元数据的用户交互
- **位置**：`src/magentic_ui/agents/users/`

#### MCP Agent（MCP智能体）
- **职责**：通过MCP协议集成外部工具和服务
- **特点**：可扩展性，支持自定义工具集成
- **位置**：`src/magentic_ui/agents/mcp/`

### 2. 智能体协作模式

```python
# 团队创建示例
async def get_task_team(
    magentic_ui_config: MagenticUIConfig,
    input_func: InputFuncType,
    paths: RunPaths,
) -> GroupChat:
    # 创建各类智能体
    orchestrator = create_orchestrator(config)
    web_surfer = WebSurfer(config)
    coder = CoderAgent(config)
    file_surfer = FileSurfer(config)
    user_proxy = create_user_proxy(config)
    
    # 组建团队
    team = GroupChat(
        participants=[orchestrator, web_surfer, coder, file_surfer, user_proxy],
        group_chat_manager=orchestrator,
    )
    
    return team
```

---

## 核心功能模块

### 1. 任务规划与执行

#### Co-Planning（协作规划）
```python
# 计划创建和管理
class Plan:
    id: str
    title: str
    steps: List[PlanStep]
    status: PlanStatus
    created_at: datetime
    updated_at: datetime
```

#### Co-Tasking（协作任务执行）
- 用户可以随时介入任务执行
- 支持实时修改计划和指导智能体
- 透明的执行过程可视化

### 2. 安全与控制

#### Action Guards（操作守护）
```python
# 敏感操作需要用户确认
class ApprovalGuard:
    async def requires_approval(self, action: Action) -> bool:
        # 判断是否需要用户批准
        return action.is_sensitive()
    
    async def request_approval(self, action: Action) -> bool:
        # 请求用户批准
        return await self.input_func.request_approval(action)
```

#### 权限控制
- 敏感操作必须经过用户确认
- 支持自定义操作权限级别
- 完整的操作审计日志

### 3. 学习与优化

#### Plan Learning（计划学习）
```python
# 从历史执行中学习
class PlanLearner:
    def learn_from_execution(self, plan: Plan, result: ExecutionResult):
        # 分析执行结果，优化计划
        pass
    
    def retrieve_similar_plans(self, task: str) -> List[Plan]:
        # 检索相似的历史计划
        pass
```

#### Memory Provider（记忆提供者）
- 存储和检索历史任务信息
- 智能计划推荐
- 持续学习和改进

---

## API 接口设计

### WebSocket 接口

```python
# 实时通信接口
@app.websocket("/ws/{session_id}")
async def websocket_endpoint(websocket: WebSocket, session_id: str):
    # 处理实时消息
    await manager.connect(websocket, session_id)
    try:
        while True:
            data = await websocket.receive_text()
            await process_message(session_id, data)
    except WebSocketDisconnect:
        manager.disconnect(websocket, session_id)
```

### REST API 接口

```python
# 会话管理
@app.post("/api/sessions")
async def create_session(session: SessionCreate) -> Session:
    """创建新会话"""
    
@app.get("/api/sessions/{session_id}")
async def get_session(session_id: str) -> Session:
    """获取会话详情"""

# 计划管理
@app.post("/api/plans")
async def create_plan(plan: PlanCreate) -> Plan:
    """创建新计划"""
    
@app.get("/api/plans")
async def list_plans() -> List[Plan]:
    """获取计划列表"""

# 设置管理
@app.post("/api/settings")
async def update_settings(settings: Settings) -> Settings:
    """更新系统设置"""
```

---

## 开发与部署

### 开发环境设置

```bash
# 1. 克隆项目
git clone https://github.com/microsoft/magentic-ui.git
cd magentic-ui

# 2. 设置Python环境
uv venv --python=3.12 .venv
source .venv/bin/activate
uv sync --all-extras

# 3. 构建前端
cd frontend
yarn install
yarn build

# 4. 启动服务
magentic-ui --port 8081
```

### Docker 部署

```bash
# 构建容器
cd docker
sh build-all.sh

# 运行服务
magentic-ui --port 8081
```

### 配置文件

```yaml
# config.yaml
gpt4o_client: &gpt4o_client
  provider: OpenAIChatCompletionClient
  config:
    model: gpt-4o-2024-08-06
    api_key: your-api-key
    max_retries: 5

orchestrator_client: *gpt4o_client
coder_client: *gpt4o_client
web_surfer_client: *gpt4o_client
file_surfer_client: *gpt4o_client

mcp_agent_configs:
  - name: custom_agent
    description: "自定义MCP智能体"
    model_client: *gpt4o_client
    mcp_servers:
      - server_name: CustomServer
        server_params:
          type: StdioServerParams
          command: your-mcp-server
```

---

## 扩展与定制

### 1. 添加新智能体

```python
# 创建自定义智能体
class CustomAgent(AssistantAgent):
    def __init__(self, config: AgentConfig):
        super().__init__(
            name="custom_agent",
            model_client=config.model_client,
            tools=[custom_tool_1, custom_tool_2],
        )
```

### 2. 集成MCP服务器

```python
# 配置MCP服务器
mcp_config = {
    "name": "my_service",
    "server_params": {
        "type": "StdioServerParams",
        "command": "my-mcp-server",
        "args": ["--config", "config.json"]
    }
}
```

### 3. 自定义工具

```python
# 创建自定义工具
@register_tool
def custom_search_tool(query: str) -> str:
    """自定义搜索工具"""
    # 实现搜索逻辑
    return search_results
```

---

## 最佳实践

### 1. 任务设计
- **明确任务目标**：清晰定义期望结果
- **分解复杂任务**：将大任务拆分为小步骤
- **设置检查点**：在关键步骤设置人工确认

### 2. 智能体使用
- **选择合适智能体**：根据任务类型选择专门智能体
- **监控执行过程**：及时介入和调整
- **学习和优化**：保存成功的执行计划

### 3. 安全考虑
- **敏感操作确认**：重要操作需要人工确认
- **权限最小化**：给予智能体最小必要权限
- **操作可撤销**：确保操作可以安全回退

---

## 故障排除

### 常见问题

1. **Docker连接问题**
   - 确保Docker服务正在运行
   - 检查Docker权限设置
   - 参考 `TROUBLESHOOTING.md`

2. **API密钥配置**
   - 设置正确的环境变量
   - 检查配置文件格式
   - 验证密钥有效性

3. **端口冲突**
   - 使用 `--port` 参数指定其他端口
   - 检查端口占用情况

4. **浏览器操作失败**
   - 检查Playwright安装
   - 确认网站可访问性
   - 查看浏览器日志

---

## 贡献指南

### 代码规范
- 遵循PEP 8 Python代码规范
- 使用TypeScript进行前端开发
- 编写完整的文档和注释

### 测试要求
- 为新功能编写单元测试
- 确保所有测试通过
- 测试覆盖率达到要求

### 提交流程
1. Fork项目仓库
2. 创建功能分支
3. 编写代码和测试
4. 提交Pull Request
5. 代码审查和合并

---

## 许可与支持

- **许可证**：MIT License
- **官方仓库**：https://github.com/microsoft/magentic-ui
- **问题反馈**：GitHub Issues
- **技术文档**：[技术报告](https://www.microsoft.com/en-us/research/wp-content/uploads/2025/07/magentic-ui-report.pdf)

---

*本文档持续更新中，如有疑问请参考官方文档或提交Issue。*
