# 配置 typescript 下的 monorepo

注：

> 根据教程：《How to Set Up a TypeScript Monorepo》配置[https://earthly.dev/blog/setup-typescript-monorepo/]
>
> 添加了 commitizen 功能

## feature

- [x] `git commit` with commitizen/cz-cli
- [x] declare every folder inside `/packages` with a `package.json` file is considered a local package.(by add `"workspaces"` property in `/package.json` )
- [x] typescript
- [x] ts-node
- [x] eslint
- [x] prettier

## git commit

`使用npm run cz命令替代git commit提交代码`
