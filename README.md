# Midscene.js 完整项目架构分析

## 🏗️ 项目整体架构概览

```mermaid
graph TB
    subgraph "用户层"
        U1[测试开发者]
        U2[自动化工程师]  
        U3[AI 助手用户]
    end
    
    subgraph "应用层 Applications"
        A1[Chrome Extension<br/>浏览器插件]
        A2[Playground<br/>可视化调试]
        A3[Android Playground<br/>移动端调试]
        A4[Recorder Form<br/>录制工具]
        A5[Report Viewer<br/>报告查看器]
        A6[Site Documentation<br/>文档站点]
    end
    
    subgraph "SDK 包层 Packages"
        P1[CLI<br/>命令行工具]
        P2[Core<br/>核心引擎]
        P3[Android<br/>安卓支持]
        P4[iOS<br/>苹果支持]
        P5[Web Integration<br/>Web 集成]
        P6[MCP<br/>模型协议]
        P7[Shared<br/>共享组件]
        P8[Playground<br/>服务组件]
        P9[Recorder<br/>录制组件]
        P10[Visualizer<br/>可视化组件]
    end
    
    subgraph "设备控制层"
        D1[Browser<br/>浏览器控制]
        D2[Android Device<br/>Android 设备]
        D3[iOS Device<br/>iOS 设备]
        D4[WebDriver<br/>Web 驱动]
    end
    
    subgraph "AI 服务层"
        AI1[OpenAI GPT-4V]
        AI2[Anthropic Claude]
        AI3[Qwen-VL]
        AI4[UI-TARS]
        AI5[Gemini Vision]
        AI6[Doubao Vision]
    end
    
    U1 --> A1
    U1 --> P1
    U2 --> A2
    U2 --> A3
    U3 --> P6
    
    A1 --> P2
    A2 --> P8
    A3 --> P3
    A4 --> P9
    A5 --> P10
    
    P1 --> P2
    P2 --> P7
    P3 --> P2
    P4 --> P2
    P5 --> P2
    P6 --> P2
    P8 --> P2
    P9 --> P2
    P10 --> P2
    
    P2 --> D1
    P3 --> D2
    P4 --> D3
    P5 --> D4
    
    P2 --> AI1
    P2 --> AI2
    P2 --> AI3
    P2 --> AI4
    P2 --> AI5
    P2 --> AI6
```

## 📦 包结构与依赖关系

### 核心包依赖图

```mermaid
graph TD
    subgraph "应用程序 Apps"
        APP1[chrome-extension]
        APP2[playground] 
        APP3[android-playground]
        APP4[recorder-form]
        APP5[report]
        APP6[site]
    end
    
    subgraph "SDK 包"
        PKG1[cli]
        PKG2[core] 
        PKG3[android]
        PKG4[ios]
        PKG5[web-integration]
        PKG6[mcp]
        PKG7[shared]
        PKG8[playground-pkg]
        PKG9[recorder]
        PKG10[visualizer]
        PKG11[webdriver]
        PKG12[evaluation]
        PKG13[android-playground-pkg]
        PKG14[ios-playground-pkg]
    end
    
    %% 应用依赖
    APP1 --> PKG2
    APP1 --> PKG9
    APP1 --> PKG7
    
    APP2 --> PKG8
    APP2 --> PKG2
    APP2 --> PKG7
    
    APP3 --> PKG3
    APP3 --> PKG13
    APP3 --> PKG7
    
    APP4 --> PKG9
    APP4 --> PKG2
    
    APP5 --> PKG10
    APP5 --> PKG7
    
    %% SDK 包依赖
    PKG1 --> PKG2
    PKG1 --> PKG7
    
    PKG2 --> PKG7
    PKG2 --> PKG9
    
    PKG3 --> PKG2
    PKG3 --> PKG7
    
    PKG4 --> PKG2  
    PKG4 --> PKG7
    
    PKG5 --> PKG2
    PKG5 --> PKG7
    
    PKG6 --> PKG2
    PKG6 --> PKG3
    PKG6 --> PKG5
    
    PKG8 --> PKG2
    PKG8 --> PKG7
    
    PKG9 --> PKG7
    
    PKG10 --> PKG7
    
    PKG11 --> PKG2
    
    PKG12 --> PKG2
    PKG12 --> PKG7
    
    PKG13 --> PKG3
    
    PKG14 --> PKG4
    
    style PKG2 fill:#e1f5fe
    style PKG7 fill:#fff3e0
```

## 🔧 核心技术架构

### Agent 架构层次

```mermaid
graph TB
    subgraph "Agent 抽象层"
        A1[Agent<T>]
        A2[AndroidAgent]
        A3[PuppeteerAgent] 
        A4[PlaywrightAgent]
        A5[iOSAgent]
    end
    
    subgraph "Interface 抽象层"
        I1[AbstractInterface]
        I2[AndroidDevice]
        I3[PuppeteerPage]
        I4[PlaywrightPage] 
        I5[iOSDevice]
    end
    
    subgraph "核心组件层"
        C1[TaskExecutor<br/>任务执行器]
        C2[Insight<br/>AI 理解引擎]
        C3[ModelConfigManager<br/>模型配置管理]
        C4[TaskCache<br/>任务缓存]
        C5[ScriptPlayer<br/>YAML 执行器]
    end
    
    subgraph "AI 处理层"
        AI1[AiLocateElement<br/>元素定位]
        AI2[AiExtractElementInfo<br/>信息提取]
        AI3[plan/uiTarsPlanning<br/>操作规划]
        AI4[callAI<br/>模型调用]
    end
    
    subgraph "设备控制层"
        D1[ADB Bridge]
        D2[Puppeteer API]
        D3[Playwright API]
        D4[iOS WebDriverAgent]
        D5[YADB Input Tool]
    end
    
    A1 --> I1
    A2 --> I2
    A3 --> I3
    A4 --> I4
    A5 --> I5
    
    A1 --> C1
    A1 --> C2
    A1 --> C3
    A1 --> C4
    
    C1 --> AI1
    C1 --> AI2
    C1 --> AI3
    C2 --> AI1
    C2 --> AI2
    C5 --> C1
    
    AI1 --> AI4
    AI2 --> AI4
    AI3 --> AI4
    
    I2 --> D1
    I2 --> D5
    I3 --> D2
    I4 --> D3
    I5 --> D4
    
    style A1 fill:#e1f5fe
    style C2 fill:#f3e5f5
    style AI4 fill:#fff3e0
```

### AI 模型集成架构

```mermaid
graph TB
    subgraph "AI 调用入口"
        E1[aiAction]
        E2[aiQuery]
        E3[aiAssert]
        E4[aiLocate]
        E5[aiWaitFor]
    end
    
    subgraph "AI 处理引擎"
        P1[TaskExecutor.action]
        P2[Insight.locate]
        P3[Insight.extract] 
        P4[setupPlanningContext]
        P5[createPlanningTask]
    end
    
    subgraph "AI 服务调用"
        S1[callAI]
        S2[callAIWithObjectResponse]
        S3[callAIWithStringResponse]
        S4[createChatClient]
    end
    
    subgraph "模型适配层"
        M1[OpenAI Adapter]
        M2[Anthropic Adapter]
        M3[Qwen-VL Adapter]
        M4[UI-TARS Adapter]
        M5[Gemini Adapter]
    end
    
    subgraph "消息格式化"
        F1[UIContext Builder]
        F2[Screenshot Processor]
        F3[Prompt Constructor]
        F4[Response Parser]
    end
    
    subgraph "外部 AI 服务"
        X1[OpenAI API]
        X2[Anthropic API]
        X3[Qwen API]
        X4[Google AI API]
        X5[ByteDance API]
    end
    
    E1 --> P1
    E2 --> P3
    E3 --> P3
    E4 --> P2
    E5 --> P3
    
    P1 --> P4
    P1 --> P5
    P2 --> S2
    P3 --> S2
    
    P4 --> F1
    P5 --> S1
    
    S1 --> S4
    S2 --> S1
    S3 --> S1
    
    S4 --> M1
    S4 --> M2
    S4 --> M3
    S4 --> M4
    S4 --> M5
    
    F1 --> F2
    F1 --> F3
    S1 --> F4
    
    M1 --> X1
    M2 --> X2
    M3 --> X3
    M4 --> X3
    M5 --> X4
    
    style P1 fill:#e1f5fe
    style F2 fill:#fff3e0
    style X1 fill:#f3e5f5
```

## 🖥️ 平台支持架构

### 多平台设备控制

```mermaid
graph TB
    subgraph "统一 Agent 接口"
        UA[Agent<AbstractInterface>]
    end
    
    subgraph "Web 平台"
        W1[PuppeteerAgent]
        W2[PlaywrightAgent] 
        W3[ChromeExtensionAgent]
        W4[WebDriverAgent]
    end
    
    subgraph "移动平台"
        M1[AndroidAgent]
        M2[iOSAgent]
    end
    
    subgraph "设备抽象层"
        D1[PuppeteerPage]
        D2[PlaywrightPage]
        D3[ChromeBridgePage]
        D4[AndroidDevice]
        D5[iOSDevice]
    end
    
    subgraph "底层驱动"
        L1[Puppeteer SDK]
        L2[Playwright SDK]
        L3[Chrome Extension API]
        L4[ADB + YADB]
        L5[WebDriverAgent + XCTest]
    end
    
    subgraph "操作系统"
        O1[Chrome/Edge/Safari]
        O2[Firefox]
        O3[Android OS]
        O4[iOS/iPadOS]
    end
    
    UA --> W1
    UA --> W2  
    UA --> W3
    UA --> W4
    UA --> M1
    UA --> M2
    
    W1 --> D1
    W2 --> D2
    W3 --> D3
    M1 --> D4
    M2 --> D5
    
    D1 --> L1
    D2 --> L2
    D3 --> L3
    D4 --> L4
    D5 --> L5
    
    L1 --> O1
    L2 --> O1
    L2 --> O2
    L3 --> O1
    L4 --> O3
    L5 --> O4
    
    style UA fill:#e1f5fe
    style D4 fill:#fff3e0
    style D5 fill:#f3e5f5
```

## 🔄 完整数据流架构

### 端到端执行流程

```mermaid
sequenceDiagram
    participant User as 用户脚本
    participant Agent as Agent
    participant TaskExec as TaskExecutor
    participant Insight as Insight Engine
    participant Device as Device Layer
    participant AI as AI Service
    participant OS as 操作系统
    
    User->>Agent: agent.aiAction("点击登录按钮")
    
    Agent->>TaskExec: taskExecutor.action(prompt)
    TaskExec->>TaskExec: createPlanningTask()
    TaskExec->>Insight: setupPlanningContext()
    
    Insight->>Device: screenshotBase64()
    Device->>OS: 系统截图调用
    OS-->>Device: PNG Buffer
    Device-->>Insight: Base64 截图
    
    Insight->>Insight: 构建 UIContext
    Insight->>AI: plan(instruction, context)
    
    AI->>AI: 图像分析和理解
    AI-->>Insight: 操作计划 JSON
    
    Insight-->>TaskExec: PlanningActions[]
    TaskExec->>TaskExec: convertPlanToExecutable()
    
    loop 执行每个操作
        TaskExec->>Device: 执行具体操作
        Device->>OS: 系统操作调用
        OS-->>Device: 操作结果
        Device-->>TaskExec: 执行完成
    end
    
    TaskExec-->>Agent: ExecutionResult
    Agent-->>User: 操作完成
    
    Note over Agent: 生成执行报告
```

### 截图与AI处理详细流程

```mermaid
graph TB
    subgraph "截图获取层"
        S1[用户调用 aiAction]
        S2[Agent.getUIContext]
        S3[commonContextParser]
        S4[Device.screenshotBase64]
        S5[ADB/WebDriver 调用]
        S6[系统截图 API]
    end
    
    subgraph "数据处理层"
        D1[PNG Buffer 验证]
        D2[Base64 编码]
        D3[UIContext 构建]
        D4[截图缩放处理]
        D5[格式标准化]
    end
    
    subgraph "AI 调用层"
        A1[消息格式化]
        A2[模型选择]
        A3[HTTP API 调用]
        A4[响应解析]
        A5[结果验证]
    end
    
    subgraph "操作执行层"
        E1[坐标解析]
        E2[操作规划]
        E3[设备控制]
        E4[结果验证]
        E5[报告生成]
    end
    
    S1 --> S2 --> S3 --> S4 --> S5 --> S6
    S6 --> D1 --> D2 --> D3 --> D4 --> D5
    D5 --> A1 --> A2 --> A3 --> A4 --> A5
    A5 --> E1 --> E2 --> E3 --> E4 --> E5
    
    style S4 fill:#e1f5fe
    style A3 fill:#fff3e0
    style E3 fill:#f3e5f5
```

## 🏢 开发工具链架构

### 构建和开发环境

```mermaid
graph TB
    subgraph "开发工具"
        T1[TypeScript]
        T2[Rslib 构建工具]
        T3[Biome 代码格式化]
        T4[Vitest 测试框架]
        T5[NX Monorepo 管理]
        T6[PNPM 包管理]
    end
    
    subgraph "CI/CD"
        C1[GitHub Actions]
        C2[自动化测试]
        C3[包发布]
        C4[文档生成]
    end
    
    subgraph "质量控制"
        Q1[ESLint 代码检查]
        Q2[CommitLint 提交规范]
        Q3[TypeScript 类型检查]
        Q4[单元测试覆盖率]
        Q5[集成测试]
    end
    
    subgraph "发布流程"
        P1[NPM Registry]
        P2[版本管理]
        P3[Release Notes]
        P4[文档站点部署]
    end
    
    T1 --> T2
    T2 --> C2
    T3 --> Q1
    T4 --> Q4
    T5 --> T6
    T6 --> C3
    
    C1 --> C2
    C2 --> C3
    C3 --> C4
    
    Q1 --> Q3
    Q3 --> Q4
    Q4 --> Q5
    
    C3 --> P1
    P1 --> P2
    P2 --> P3
    C4 --> P4
    
    style T2 fill:#e1f5fe
    style C1 fill:#fff3e0
```

## 🌐 生态系统架构

### 外部集成和扩展

```mermaid
graph TB
    subgraph "Midscene 核心"
        MC[Midscene Core]
    end
    
    subgraph "官方扩展"
        E1[Chrome Extension]
        E2[Playground UI]
        E3[MCP Protocol]
        E4[CLI Tools]
    end
    
    subgraph "社区扩展"
        CE1[midscene-ios<br/>社区iOS支持]
        CE2[Midscene-Python<br/>Python SDK]  
        CE3[midscene-java<br/>Java SDK]
        CE4[其他语言绑定]
    end
    
    subgraph "外部工具集成"
        I1[Jest/Vitest]
        I2[Cypress]
        I3[Selenium]
        I4[CI/CD 系统]
        I5[IDE 插件]
    end
    
    subgraph "AI 服务生态"
        AI1[OpenAI]
        AI2[Anthropic]
        AI3[开源模型]
        AI4[企业私有模型]
    end
    
    subgraph "用户应用场景"
        U1[Web 自动化测试]
        U2[移动端测试]
        U3[RPA 业务流程]
        U4[AI 助手工具]
        U5[回归测试]
    end
    
    MC --> E1
    MC --> E2
    MC --> E3
    MC --> E4
    
    MC --> CE1
    MC --> CE2
    MC --> CE3
    MC --> CE4
    
    MC --> I1
    MC --> I2
    MC --> I3
    MC --> I4
    MC --> I5
    
    MC --> AI1
    MC --> AI2
    MC --> AI3
    MC --> AI4
    
    MC --> U1
    MC --> U2
    MC --> U3
    MC --> U4
    MC --> U5
    
    style MC fill:#e1f5fe
    style AI1 fill:#fff3e0
    style U1 fill:#f3e5f5
```

## 📊 技术栈总结

### 核心技术选型

| 层次 | 技术栈 | 说明 |
|------|--------|------|
| **语言** | TypeScript | 全栈类型安全 |
| **构建** | Rslib | 现代化构建工具 |
| **包管理** | PNPM + Monorepo | 高效依赖管理 |
| **测试** | Vitest | 快速单元测试 |
| **AI 调用** | OpenAI SDK, Anthropic SDK | 多模型支持 |
| **设备控制** | ADB, WebDriver, Puppeteer | 跨平台设备操作 |
| **服务端** | Express, Socket.IO | 轻量级服务支持 |
| **协议** | MCP, HTTP, WebSocket | 标准协议集成 |

### 关键设计模式

| 模式 | 应用场景 | 实现位置 |
|------|---------|---------|
| **抽象工厂** | 多平台 Agent 创建 | `Agent<T>` |
| **策略模式** | 不同 AI 模型适配 | `callAI()` |
| **观察者模式** | 任务执行监听 | `TaskExecutor` |
| **适配器模式** | 设备控制统一 | `AbstractInterface` |
| **建造者模式** | UIContext 构建 | `commonContextParser` |
| **单例模式** | 配置管理 | `ModelConfigManager` |

## 🎯 架构优势总结

### 核心优势

1. **📱 跨平台统一**: 一套 API 适配 Web/Android/iOS
2. **🤖 AI 原生**: 深度集成多种视觉语言模型
3. **🔧 模块化**: 清晰的分层和组件化设计
4. **⚡ 高性能**: 多层缓存和优化策略
5. **🛠️ 可扩展**: 插件化架构支持自定义扩展
6. **🎨 开发友好**: 完整的工具链和调试支持

### 技术创新点

- **视觉优先**: 摆脱传统 DOM/控件树依赖
- **自然语言**: 用户友好的操作描述方式  
- **智能缓存**: 多层缓存提升执行效率
- **实时调试**: 可视化 Playground 调试环境
- **标准协议**: MCP 协议支持 AI 助手集成

这个架构展现了 **Midscene.js 作为下一代 AI 驱动自动化测试框架的完整技术蓝图**，从底层设备控制到上层 AI 集成，形成了一个功能完整、技术先进的自动化测试生态系统。
