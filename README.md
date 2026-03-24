# web3base-ai

最小可用说明：本仓库仅提供 `web3base-ai` skill。

## What It Does

`web3base-ai/SKILL.md` 定义了一个统一入口技能，用于处理 Web3Base Open API 的内容检索与整合，覆盖：
- chains
- twitter users
- youtube channels
- nav（导航）
- taxonomy（分类）

它会先识别用户意图与参数约束，再按对应路由调用接口；若请求跨多个域，会拆分子任务后合并输出。

## Install

```bash
npx skills add https://github.com/web3baseai/skills --skill web3base-ai
```

## Verify

```bash
npx skills add https://github.com/web3baseai/skills --list
```
# skills
