# Lunar

> Yet another AI powered code reviewer

Lunar is short for 'LUnar is Not an Ai Reviewer'.

## Featuers

- Review Pull Requests with AI and directly comment on each file.
  ([example pr](https://github.com/0xWelt/test-action/pull/2))

  ![review](./docs/review.png)

## Use Lunar as Github Actions

1.  Add the `OPENAI_API_KEY` to your github actions secrets.

    ![actions_secrets](./docs/actions_secrets.png)

2.  create `.github/workflows/lunar.yml`

    ```yaml
    name: Lunar Code Review

    permissions:
      contents: read
      pull-requests: write

    on:
      pull_request_target:
        types: [opened, reopened, synchronize]

    jobs:
      test:
        runs-on: ubuntu-latest
        steps:
          - uses: 0xWelt/Lunar@main
            env:
              GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
              OPENAI_API_KEY: ${{ secrets.DEEPSEEK_API_KEY }}
              # Optional, change as you wish or comment out to use default
              OPENAI_BASE_URL: https://api.deepseek.com/v1
              MODEL: deepseek-chat
              REVIEW_PROMPT: |
                我将提供给你一段github pull request的file diff。请你扮演一位专业的开源社区开发者，帮我进行code review。
                [[代码审核原则]]
                [代码质量]
                1. 该修改是否有必要，是否相比旧代码有改进或添加了新功能？
                2. 是否有逻辑错误或潜在的运行时错误？
                3. 是否有与其他代码部分的兼容性问题，会不会破坏与修改无关的现有逻辑？
                4. 是否有可以优化的代码结构或逻辑，是否有更简洁或更高效的方式来实现相同功能？
                5. 是否有潜在的性能问题？
                [安全性]
                6. 是否有敏感信息泄露的风险（如硬编码的密钥、密码等）？
                7. 是否有可能运行来路不明的外部代码？

                [[结果返回格式]]
                如果发现任何问题或改进建议，请分点详细列出并说明；如果没有问题，请直接输出“LGTM”四个字母表示通过，不用再输出任何其他解释。
              temperature: 1.0
              max_tokens: 8192
    ```

## Develop Plan

- [x] Review Pull Requests with AI and directly comment on each file.
- [ ] Add **icons** and **model names** for popular LLMs.
- [ ] Multi-turn conversation support. Context aware code suggestions.
