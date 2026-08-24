[English](https://github.com/tercel/tercel/blob/main/README.md) · **中文**

# Tercel

**面向 agent 可调用软件的受治理能力标准 · schema 强制约束的模块 · 协议中立的执行边界**

我的工作集中在一条边界上——agent 在这里停止推理，开始真正让事情发生。

> **MCP 告诉 agent 它*能*调用什么。[apcore](https://github.com/aiperceivable/apcore) 决定这次调用——带着这些参数、在这个环境里、以这个身份——究竟*该不该*发生，并留下它确实发生过的证据。**

今天的 agent 工具栈，大多把发现问题解决了两遍，把授权问题解决了零遍。一份 tool schema 告诉模型如何组装一次调用，却对以下问题只字不提：谁有权发起它、它可以触及什么、是否必须先经过人工批准、事后又留下了什么记录。这些问题不属于模型的推理循环——它们属于执行边界，并且无论请求由哪种协议送达，都必须以完全相同的方式被强制执行。

这就是 [**apcore**](https://github.com/aiperceivable/apcore) 所持的立场：

```text
                  ┌──────────────────────────────────────────┐
                  │     Agent  ·  Human  ·  Application       │
                  └────────────────────┬─────────────────────┘
                                       │
          ┌──────────┬──────────┬──────┴────┬──────────┬──────────┐
          ▼          ▼          ▼           ▼          ▼          ▼
       ┌─────┐   ┌───────┐  ┌───────┐   ┌──────┐   ┌───────┐  ┌────────┐
       │ MCP │   │  A2A  │  │  CLI  │   │ HTTP │   │OpenAI │  │ direct │
       └──┬──┘   └───┬───┘  └───┬───┘   └──┬───┘   └───┬───┘  └───┬────┘
          └──────────┴──────────┴─────┬────┴───────────┴──────────┘
                                      ▼
              ╔═══════════════════════════════════════════════╗
              ║              apcore runtime                   ║
              ║   schema · ACL · approval gate · middleware    ║
              ║          observability · audit trail          ║
              ╚═══════════════════════┬═══════════════════════╝
                                      ▼
                  ┌──────────────────────────────────────────┐
                  │      your existing business logic        │
                  │           (unchanged)                    │
                  └──────────────────────────────────────────┘
```

一次定义受治理的能力，然后把它投射到任意表面。**传输协议变了，保证不会随之削弱。**

---

## 生态

以下全部位于 [**@aiperceivable**](https://github.com/aiperceivable) 组织下。

**核心标准** —— 一份协议规范，配三份彼此独立的实现；把它们约束在一起的是跨语言 conformance 测试夹具，而不是共享代码。[完整文档 →](https://apcore.aiperceivable.com/)

| | 仓库 | 安装 |
|---|---|---|
| 规范 | [apcore](https://github.com/aiperceivable/apcore) | — |
| Python | [apcore-python](https://github.com/aiperceivable/apcore-python) | `pip install apcore` |
| TypeScript | [apcore-typescript](https://github.com/aiperceivable/apcore-typescript) | `npm install apcore-js` |
| Rust | [apcore-rust](https://github.com/aiperceivable/apcore-rust) | `cargo add apcore` |

**表面适配器** —— 同一个模块，投射到不同协议之上。每个表面只规范一次、实现三次，由一套共享的 conformance 套件钉死那些必须保持一致的行为。

| 表面 | 规范 | Python | TypeScript | Rust |
|---|---|---|---|---|
| Model Context Protocol | [apcore-mcp](https://github.com/aiperceivable/apcore-mcp) | [py](https://github.com/aiperceivable/apcore-mcp-python) | [ts](https://github.com/aiperceivable/apcore-mcp-typescript) | [rs](https://github.com/aiperceivable/apcore-mcp-rust) |
| Agent2Agent | [apcore-a2a](https://github.com/aiperceivable/apcore-a2a) | [py](https://github.com/aiperceivable/apcore-a2a-python) | [ts](https://github.com/aiperceivable/apcore-a2a-typescript) | [rs](https://github.com/aiperceivable/apcore-a2a-rust) |
| 命令行 | [apcore-cli](https://github.com/aiperceivable/apcore-cli) | [py](https://github.com/aiperceivable/apcore-cli-python) | [ts](https://github.com/aiperceivable/apcore-cli-typescript) | [rs](https://github.com/aiperceivable/apcore-cli-rust) |

**到代码已经在的地方去找它** —— 这些集成会扫描既有代码库，把已经存在的东西暴露出来，而不是要求任何人重写。

[fastapi-apcore](https://github.com/aiperceivable/fastapi-apcore) · [django-apcore](https://github.com/aiperceivable/django-apcore) · [flask-apcore](https://github.com/aiperceivable/flask-apcore) · [nestjs-apcore](https://github.com/aiperceivable/nestjs-apcore) · [axum-apcore](https://github.com/aiperceivable/axum-apcore) · [tiptap-apcore](https://github.com/aiperceivable/tiptap-apcore)

至于那些根本没有框架可言的代码——[**apexe**](https://github.com/aiperceivable/apexe) 通过扫描 help 输出，把一个现成的 CLI 二进制文件包装成受治理的模块。于是一个存在了四十年的工具，抵达 agent 时携带的 ACL 与审批语义，和手写模块完全相同。

**编排** —— [apflow](https://github.com/aiperceivable/apflow)，分布式任务编排引擎，图中的每一项能力本身都是一个 apcore 模块。

---

## 开发者工具链

此外还有一组 Claude Code skill，针对工程中那些"照检查清单办事胜过临场发挥"的环节：

| | |
|---|---|
| [**code-forge**](https://github.com/tercel/code-forge) | TDD 驱动的实现流程 —— 计划、执行、调试、评审、worktree 与分支生命周期、并行 agent 调度 |
| [**spec-forge**](https://github.com/tercel/spec-forge) | PRD、SRS、技术设计与测试计划 —— 可单独使用，也可串成一条完整的可追溯链 |
| [**theory-forge**](https://github.com/tercel/theory-forge) | 学术严谨性审计 —— 引用真实性、可证伪性、Toulmin 论证结构、范围纪律、对立观点交锋 |
| [**research-forge**](https://github.com/tercel/research-forge) | 对开源项目与产品做技术和商业尽职调查 |
| [**prompt-coach**](https://github.com/tercel/prompt-coach) | 实时 prompt 与目标语言辅导，以 `UserPromptSubmit` hook 的形式运行 |
| [**agent-skill-bundler**](https://github.com/tercel/agent-skill-bundler) | 把 Claude Code skill 移植到其他 agent 平台 |
| [**tercel-claude-plugins**](https://github.com/tercel/tercel-claude-plugins) | 承载它们的市场仓库 |

---

## 其他

[aiperceivable.com](https://aiperceivable.com/) · [@tercelyi](https://x.com/tercelyi) · [LinkedIn](https://www.linkedin.com/in/tercel-yi)

以上任何一个仓库都欢迎 issue 与设计讨论 —— 尤其欢迎那些棘手的部分：授权语义、跨语言 conformance，以及当一次调用被拒绝返回时，agent 究竟有权假设什么。
