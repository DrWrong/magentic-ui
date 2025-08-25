# Magentic-UI 开发快速入门

## 🚀 5分钟快速开始

### 环境准备
```bash
# 1. 确保安装了必要软件
# - Python 3.10+
# - Docker Desktop
# - Node.js & npm/yarn

# 2. 克隆项目
git clone https://github.com/microsoft/magentic-ui.git
cd magentic-ui

# 3. 设置Python环境
python3 -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -e .

# 4. 设置API密钥
export OPENAI_API_KEY="your-key-here"

# 5. 启动服务
magentic-ui --port 8081
```

## 📂 关键文件路径

### 后端核心
```
src/magentic_ui/
├── task_team.py           # 智能体团队创建入口
├── magentic_ui_config.py  # 主配置文件
├── agents/               # 所有智能体实现
│   ├── web_surfer/       # 网页操作智能体
│   ├── _coder.py         # 代码生成智能体
│   └── file_surfer/      # 文件处理智能体
├── backend/web/app.py    # FastAPI主应用
├── tools/playwright/     # 浏览器控制工具
└── teams/orchestrator/   # 任务编排器
```

### 前端核心
```
frontend/src/
├── components/views/manager.tsx    # 主界面组件
├── components/views/chat/         # 聊天界面
├── components/types/              # TypeScript类型
├── hooks/store.tsx               # 状态管理
└── pages/index.tsx               # 首页入口
```

## 🔧 常用开发任务

### 添加新智能体
```python
# 1. 在 agents/ 下创建新智能体
class MyCustomAgent(AssistantAgent):
    def __init__(self, config):
        super().__init__(
            name="my_agent",
            model_client=config.model_client,
            tools=[my_tool_1, my_tool_2],
            system_message="你是一个专门的智能体..."
        )

# 2. 在 task_team.py 中注册
my_agent = MyCustomAgent(config)
participants.append(my_agent)
```

### 创建自定义工具
```python
# 在 tools/ 下创建工具
from autogen_ext.tools import Tool

@Tool
def my_custom_tool(param: str) -> str:
    """自定义工具描述"""
    # 实现工具逻辑
    return result
```

### 修改前端界面
```typescript
// 在 components/ 下创建组件
export const MyComponent: React.FC = () => {
    const { sessions, activeSession } = useStore();
    
    return (
        <div className="my-component">
            {/* 组件内容 */}
        </div>
    );
};
```

### 添加API端点
```python
# 在 backend/web/routes/ 下添加路由
@router.post("/api/my-endpoint")
async def my_endpoint(request: MyRequest) -> MyResponse:
    # 处理逻辑
    return response
```

## 🧪 调试与测试

### 后端调试
```bash
# 运行测试
pytest tests/

# 代码格式化
ruff format src/

# 类型检查
pyright src/
```

### 前端调试
```bash
cd frontend

# 开发模式（热重载）
npm run develop

# 构建生产版本
npm run build

# 类型检查
npm run typecheck
```

### 日志调试
```python
# 在代码中添加日志
from loguru import logger

logger.info("调试信息")
logger.error("错误信息")
```

## 🔍 重要配置

### 模型配置
```yaml
# config.yaml
gpt4o_client: &gpt4o_client
  provider: OpenAIChatCompletionClient
  config:
    model: gpt-4o-2024-08-06
    api_key: ${OPENAI_API_KEY}

orchestrator_client: *gpt4o_client
```

### Docker配置
```bash
# 重新构建Docker镜像
cd docker
sh build-all.sh

# 不使用Docker运行（功能受限）
magentic-ui --run-without-docker --port 8081
```

### 数据库配置
```python
# 默认使用SQLite，生产环境推荐PostgreSQL
DATABASE_URL = "postgresql://user:pass@localhost/magenticui"
```

## 🐛 常见问题解决

### 1. Docker权限问题
```bash
# Linux下添加用户到docker组
sudo usermod -aG docker $USER
# 重新登录生效
```

### 2. 端口占用
```bash
# 查看端口占用
lsof -i :8081

# 使用其他端口
magentic-ui --port 8082
```

### 3. 前端构建失败
```bash
cd frontend
rm -rf node_modules yarn.lock
yarn install
yarn build
```

### 4. API密钥问题
```bash
# 检查环境变量
echo $OPENAI_API_KEY

# 或在UI中设置
# Settings -> Model Configuration
```

## 📚 学习资源

### 核心技术栈文档
- [AutoGen](https://github.com/microsoft/autogen) - 多智能体框架
- [FastAPI](https://fastapi.tiangolo.com/) - Python Web框架
- [Playwright](https://playwright.dev/) - 浏览器自动化
- [React](https://react.dev/) - 前端框架
- [Gatsby](https://www.gatsbyjs.com/) - 静态站点生成器

### 项目相关
- [技术报告](https://www.microsoft.com/en-us/research/wp-content/uploads/2025/07/magentic-ui-report.pdf)
- [GitHub仓库](https://github.com/microsoft/magentic-ui)
- [问题反馈](https://github.com/microsoft/magentic-ui/issues)

## 🔄 开发工作流

### 1. 功能开发
```bash
# 创建功能分支
git checkout -b feature/my-feature

# 开发 -> 测试 -> 提交
git add .
git commit -m "feat: 添加新功能"
git push origin feature/my-feature
```

### 2. 代码审查
- 确保所有测试通过
- 代码符合项目规范
- 添加必要的文档

### 3. 发布流程
- 通过Pull Request合并
- 自动化CI/CD流程
- 版本标签管理

---

*快速上手完成！遇到问题请查看完整的[代码导读文档](CODE_WALKTHROUGH.md)或提交Issue。*
