转载请注明插件作者：ENA

闲得没事搞的插件，拙技见笑

------
**Github**：https://github.com/Enashpinal/GPTforMC

# 简介

GPTforMC是一个基于 Spigot 服务端的插件，支持1.13+，为你的服务器接入DeepSeek或ChatGPT，支持自定义 API 接口地址、系统提示词、控制 AI 行为等。玩家在聊天时发送带有指定触发词的消息即可触发对话。

管理员可以创建不同模型、身份的 AI，根据设定的触发词响应玩家的消息，允许每个 AI 使用不同的配置。

# 使用方法

将插件放入plugins文件夹，启动一次服务器。

编辑config.yml，设置 API 接口和 Key（免费Key获取：https://github.com/chatanywhere/GPT_API_free）

# 命令

创建AI（权限：gpt.create）          /gpt create <名称>

删除AI（权限：gpt.admin或gpt.remove）          /gpt remove <名称>

AI列表（权限：gpt.use 或 gpt.admin）                   /gpt list

清除记忆（权限：gpt.edit 或 gpt.admin）       /gpt clearmemory <名称>

设置AI配置（权限：gpt.edit 或 gpt.admin）    /gpt set <名称> <选项> <值>

>     示例：/gpt set Deepseek top_p 0.5 —— 将名称为Deepseek的AI核采样设为0.5
> 
>                /gpt set Deepseek trigger ds 2 —— 为Deepseek添加触发词“ds”及其优先级
> 
>                 /gpt set Deepseek prompt system 0 You are a friendly AI assistant —— 
> 
> 在Deepseek的提示词列表索引0插入提示词，内容为 You are a friendly AI assistant。
> 
>                /gpt set Deepseek model gpt-4o —— 将Deepseek的模型ID改为gpt-4o

设置全局配置（权限：gpt.config 或 gpt.admin） /gpt set <名称> <选项> <值> （使用方法同上，但无全局触发词和提示词）

重载配置（权限：gpt.admin）        /gpt reload（若无法重载请尝试/reload）

显示帮助（无权限节点）                 /gpt help

# 配置文件

## config.yml（全局配置）
```yml
# GPTforMC 插件配置文件
# 配置插件的全局设置，AI 独立配置默认继承全局配置

# OpenAI 配置
openai:
  # API 请求的 URL 地址
  # 推荐使用Chatanywhere免费转发API，支持deepseek-1/deepseek-v3/gpt-3.5-turbo/gpt-4o-mini等模型
  api-url: "https://api.openai.com/v1/chat/completions"

  # OpenAI API 密钥
  api-key: "sk-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"

  # 默认使用的 OpenAI 模型英文 ID
  model: "gpt-4o-mini"

  # API 请求的超时时间（ms）
  # 范围: 1000 - 30000，建议值： 10000
  timeout: 10000

  # 最大记忆轮次
  # 控制 AI 保留的对话历史记录条数（每条记录包括用户输入和 AI 回复）
  # 范围: 0 - 20，0 表示不保留记忆
  memory_rounds: 4

  # 控制 AI 回答的随机性
  # 范围: 0.0 - 2.0，值越高回答越随机，值越低回答越固定
  temperature: 0.7

  # 核采样参数，控制生成内容的多样性
  # 范围: 0.0 - 1.0，值越低越倾向于高概率词汇
  top_p: 1.0

  # 最大生成 Token 数
  # 控制 AI 回答的长度，1 Token 约为 0.75 个英文单词或 1-2 个中文字符
  max_tokens: 500

  # 话题新鲜度惩罚
  # 范围: -2.0 - 2.0，值越高越
  presence_penalty: 0.0

  # 频率惩罚度
  # 范围: -2.0 - 2.0，值越高越减少重复词汇
  frequency_penalty: 0.0

  # 是否强制添加提示词
  force_system_prompt: true

  # AI 消息的默认颜色代码
  # 使用 Minecraft 颜色代码
  # 参考: https://minecraft.fandom.com/wiki/Formatting_codes
  message_color: "&a"

  # 玩家消息的格式化模板
  # 占位符: {player} 表示玩家名称，{message} 表示玩家消息
  message_format: "用户{player}说：{message}"

# 权限组配置
permission-groups:
  # 默认玩家组（普通玩家）
  default:
    # 允许创建的最大 AI 数量
    # 范围: 0 - 无限制，0 表示禁止创建
    max-ai: 0

    # 是否允许创建 AI
    gpt.create: false

    # 是否允许删除 AI
    gpt.remove: false

    # 是否允许编辑 AI 设置
    gpt.edit: false

    # 是否允许修改全局配置
    gpt.config: false

    # 是否允许使用 AI（触发 AI 回复）
    gpt.use: true

    # 是否具有管理员权限（可操作所有 AI）
    gpt.admin: false

    # 是否绕过 AI 数量限制
    gpt.bypass: false

  # 管理员组
  admin:
    # 允许创建的最大 AI 数量
    max-ai: 100

    # 是否允许创建 AI
    gpt.create: true

    # 是否允许删除 AI
    gpt.remove: true

    # 是否允许编辑 AI 设置
    gpt.edit: true

    # 是否允许修改全局配置
    gpt.config: true

    # 是否允许使用 AI
    gpt.use: true

    # 是否具有管理员权限
    gpt.admin: true

    # 是否绕过 AI 数量限制
    gpt.bypass: true
```
## ai_data.yml（AI独立配置，默认有一个AI）
```yml
# GPTforMC AI 单独配置文件

GPT-4o-mini:
  # AI名称，唯一标识此AI配置
  message_format: 用户{player}说：{message}
  # 玩家消息的格式化模板，{player} 表示玩家名称，{message} 表示消息内容
  max_tokens: 500
  presence_penalty: 0.0
  max_memory: 4
  timeout: 300000
  top_p: 1.0
  force_system_prompt: true
  message_color: '&a'
  frequency_penalty: 0.0
  # 以上字段同全局配置
  trigger_words:
    # 触发 AI 回复的关键词列表
    - priority: 1
      # 触发优先级，值越大越优先调用此 AI（1 - 100）
      word: gpt
      # 触发词，玩家消息中包含此词触发 AI 对话
    - priority: 5
      word: ChatGPT
  temperature: 0.7
  # 控制 AI 回答随机性
  model: gpt-4o-mini
  # 使用的 AI 模型 ID（如 gpt-4o-mini, gpt-4）
  prompts:
    # 预设提示词列表，若 force_system_prompt 为 true 则将系统提示词强制添加到记忆
    - role: system
      # 系统提示词，定义 AI 行为
      content: 你是一个AI智能助手，名叫gpt
    - role: user
      # 用户消息，模拟用户输入
      content: Hello, gpt!
    - role: assistant
      # AI 回复，模拟 AI 输出
      content: Hello! How can I assist you today?
  memories: []
  # AI 的对话记忆，存储用户和 AI 的历史对话，默认为空
  allow_default_edit: false
  # 是否允许默认权限组（普通玩家）编辑或删除此AI，默认为false（仅管理员创建的AI）
```
# 下载插件

https://enanetdisk.pages.dev/?file=%2Fdisk%2FMinecraftPlugins%2FGPTforMC-1.0.jar