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

## add a Local Package

mkdir in `/packages:`

- cd packages
- mkdir utils

initialize a `package.json` file inside utils:

- npm init --scope @monorepo --workspace ./packages/utils
- confirm `package.json` own this content

  ```json
  {
    "name": "@monorepo/utils",
    "version": "1.0.0",
    "description": "The package containing some utility functions",
    "main": "build/index.js",
    "scripts": {
      "build": "tsc --build"
    }
  }
  ```

- add tsconfig.json inside utils:

  ```json
  {
    "compilerOptions": {
      "target": "es2022",
      "module": "commonjs",
      "moduleResolution": "node",
      "declaration": true,
      "strict": true,
      "incremental": true,
      "esModuleInterop": true,
      "skipLibCheck": true,
      "forceConsistentCasingInFileNames": true,
      "rootDir": "./src",
      "outDir": "./build",
      "composite": true
    }
  }
  ```

## build Local Package Works

通过 `--workspace`指明命令的工作路径，从而执行对应 `package.json`中定义的script

```powershell
npm run build --workspace ./packages/utils
```

## tsconfig.json全局管理

```json
{
  "compilerOptions": {
    "incremental": true,
    "target": "es2022",
    "module": "commonjs",
    "declaration": true,
    "strict": true,
    "moduleResolution": "node",
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "rootDir": "./src",
    "outDir": "./build"
  },
  "files": [],
  "references": [
    {
      "path": "./packages/utils"
    }
  ]
}
```

在根目录执行 `tsc --build` 命令时，编译器会通过全局 `tsconfig.json`文件中的 `references`字段中定义的路径找到所有可编译的独立项目，并编译

## 在pacakges下的库中引入其他库

在packages/ui下使用packages/utils的库

```
npm install @monorepo/utils --workspace ./packages/ui
```

## npm安装依赖命令

```
npm install moment --workspace ./packages/ui
```

## 全局声明packages下的项目作为依赖

```
npm install @monorepo/utils @monorepo/ui
```
