# BIM 三维知识库 (BIM 3D Knowledge Hub)

一个专业的企业级BIM三维知识库系统，为工程人员提供标准化的构造做法、管控要点、3D模型和CAD图纸在线查看功能。

## 产品特性

### 核心功能

#### 1. 知识库管理
- 结构化的构造做法、施工工艺和管控要点
- 按专业、构造部位、工序、风险等级分类
- 支持标签体系和版本管理
- 草稿 → 审核 → 发布的内容流转机制

#### 2. 二三维展示
- **3D模型查看**: 支持Revit模型轻量化展示，包含爆炸视图和施工步骤动画
- **CAD图纸预览**: 在线查看CAD图纸，支持缩放、测量、图层控制、批注
- **图片对比**: 标准示例与现场实际的左右对照展示

#### 3. 智能搜索
- 全局关键词搜索
- 多维度筛选（专业、部位、工序、风险等级）
- 构件点击定位到知识条目

#### 4. AI智能助手
- 自然语言提问，获取专业工程技术解答
- 支持上传现场照片进行节点识别
- 智能推荐相关构造做法

#### 5. 移动端支持
- 完全响应式设计
- 底部Tab导航（首页/搜索/收藏/我的）
- 二维码扫码快速访问
- 离线包下载功能

#### 6. 内容管理系统
- 创建和编辑知识条目
- 文件上传进度显示
- 版本历史和差异对比
- 权限分级（部门/项目/企业）

## 技术栈

- **前端框架**: Next.js 16 (App Router)
- **样式系统**: TailwindCSS v4
- **UI组件库**: shadcn/ui
- **AI服务**: EvoLink Chat Completions API
- **3D渲染**: Three.js + glTF Loader (前端)
- **模型转换**: Revit API / Autodesk Forge / Speckle (后端)
- **CAD预览**: Autodesk Viewer / 轻量CAD组件
- **类型检查**: TypeScript
- **状态管理**: React Hooks + SWR

## 快速开始

### 环境要求
- Node.js 18+
- npm 或 yarn

### 安装依赖

\`\`\`bash
npm install
\`\`\`

### 配置环境变量

创建 `.env.local` 文件：

\`\`\`env
# AI功能配置（必需）
EVOLINK_API_KEY=your_evolink_api_key_here
NEXT_PUBLIC_AI_ENABLED=true

# Supabase重定向配置（开发环境，可选）
NEXT_PUBLIC_DEV_SUPABASE_REDIRECT_URL=http://localhost:3000
\`\`\`

**重要说明：**
- `EVOLINK_API_KEY` 是必需的，用于调用EvoLink Chat Completions API
- 如果未配置此密钥，AI智能助手功能将无法使用
- 获取API密钥请访问 [EvoLink官网](https://evolink.ai)
- 配置后需要重启开发服务器才能生效

### 运行开发服务器

\`\`\`bash
npm run dev
\`\`\`

访问 [http://localhost:3000](http://localhost:3000)

### 构建生产版本

\`\`\`bash
npm run build
npm start
\`\`\`

## 项目结构

\`\`\`
app/
├── api/              # API路由
│   └── ai/chat/      # AI聊天接口
├── cms/              # 内容管理系统
│   ├── create/       # 创建知识条目
│   └── edit/[id]/    # 编辑知识条目
├── knowledge/        # 知识库浏览
│   └── [id]/         # 知识条目详情
├── search/           # 搜索
│   └── ai/           # AI智能助手
├── qr/[id]/          # 二维码扫码访问
├── admin/            # 系统管理
└── page.tsx          # 首页

components/
├── header.tsx                    # 导航头部
├── mobile-nav.tsx                # 移动端底部导航
├── model-viewer.tsx              # 3D模型查看器
├── cad-viewer.tsx                # CAD图纸查看器
├── image-gallery.tsx             # 图片画廊（支持对比）
├── knowledge-entry-form.tsx      # 知识条目表单
└── admin/                        # 管理组件
    ├── user-management.tsx
    ├── organization-management.tsx
    ├── permission-management.tsx
    └── analytics-dashboard.tsx

lib/
├── types.ts          # TypeScript类型定义
├── mock-data.ts      # 模拟数据
└── utils.ts          # 工具函数
\`\`\`

## 核心功能详解

### 1. Revit模型转换流程

\`\`\`
用户上传.rvt文件 
  ↓
后端接收并处理
  ↓
使用Revit API / Autodesk Forge / Speckle转换
  ↓
生成glTF轻量化模型
  ↓
存储到云端（Vercel Blob）
  ↓
前端使用Three.js加载展示
\`\`\`

**转换方案选择:**
- **Autodesk Forge**: 官方云服务，稳定可靠，需要Forge账号
- **Speckle**: 开源方案，支持自托管
- **Revit API**: 本地转换，需要Windows服务器和Revit软件

### 2. CAD图纸在线预览

支持功能:
- 图层控制（显示/隐藏）
- 缩放、平移、旋转
- 测量工具
- 批注标记
- 多图对照

### 3. 施工步骤动画

3D模型支持:
- 逐步显示构件
- 爆炸视图分解
- 施工工序播放
- 构件级别交互

### 4. AI图片识别

流程:
1. 用户上传现场照片
2. 调用EvoLink API (支持图片输入)
3. AI识别构造节点类型
4. 推荐相关知识条目

### 5. 离线包功能

内容包含:
- 知识条目文字内容
- CAD缩略图
- 精简版glTF模型
- 关键图片

## AI集成说明

### 获取EvoLink API密钥

1. 访问 [EvoLink官网](https://evolink.ai) 注册账号
2. 进入控制台创建API密钥
3. 将密钥添加到 `.env.local` 文件中

### EvoLink API配置

\`\`\`typescript
// app/api/ai/chat/route.ts
const response = await fetch('https://api.evolink.ai/v1/chat/completions', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${process.env.EVOLINK_API_KEY}`,
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    model: 'gpt-4',
    messages: [
      { role: 'user', content: '楼板裂缝如何预防？' }
    ],
    temperature: 0.7,
  }),
});
\`\`\`

### 支持图片识别

\`\`\`typescript
messages: [
  { 
    role: 'user', 
    content: '请识别这个构造节点',
    image: 'data:image/jpeg;base64,...' 
  }
]
\`\`\`

## 多语言支持

框架已支持国际化，目前提供:
- 简体中文 (默认)
- 繁体中文
- English

## 数据结构

### 知识条目 (KnowledgeEntry)

\`\`\`typescript
{
  id: string
  title: string
  description: string
  
  // 分类
  professional: 'architecture' | 'structure' | 'mep' | 'decoration'
  componentType: 'slab' | 'wall' | 'beam' | 'column' | 'node'
  workProcess: 'formwork' | 'rebar' | 'concrete' | 'installation'
  riskLevel: 'normal' | 'important' | 'high-risk'
  
  // 内容
  textContent: string
  usageArea?: string          // 使用区域
  precautions?: string        // 注意事项
  typicalIssues?: string      // 典型问题与风险
  
  // 附件
  cadFiles: FileAttachment[]
  images: FileAttachment[]
  rvtModel?: FileAttachment
  
  // 元数据
  status: 'draft' | 'review' | 'published'
  accessScope: 'department' | 'project' | 'company'
  version: number
  versionNotes?: string
  tags: string[]
}
\`\`\`

## 部署指南

### Vercel部署

1. 连接GitHub仓库
2. 设置环境变量
3. 一键部署

### 环境变量配置

生产环境需要配置:
\`\`\`env
EVOLINK_API_KEY=your_api_key
NEXT_PUBLIC_AI_ENABLED=true
\`\`\`

## 后续扩展计划

- [ ] 集成真实数据库（Supabase / Neon）
- [ ] 实现用户认证和权限管理
- [ ] 添加Revit到glTF的后端转换服务
- [ ] 集成真实CAD在线预览组件
- [ ] 完整实现Three.js 3D交互
- [ ] 离线包下载和缓存
- [ ] 数据统计和分析看板
- [ ] 移动端原生App（React Native）

## 技术支持

如需帮助，请：
1. 查看项目文档
2. 提交GitHub Issue
3. 联系技术团队

## License

MIT License
