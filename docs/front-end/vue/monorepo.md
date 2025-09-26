# vue3项目架构

> 以下是基于 pnpm workspace + TypeScript + rollup 的 Monorepo 架构实现步骤：

### 1. 初始化项目结构
```bash
# 创建项目目录
mkdir vue3-monorepo && cd vue3-monorepo

# 初始化 package.json
pnpm init -y

# 创建目录结构
mkdir -p packages/{core,shared,compiler-core}
mkdir -p scripts
```

### 2. 配置 pnpm workspace
创建 `pnpm-workspace.yaml` 定义工作区范围：
```yaml
packages:
  - 'packages/*'  # 所有子包
  - 'scripts/*'   # 脚本目录
```

### 3. 配置根目录 package.json
```json
{
  "name": "vue3-monorepo",
  "private": true,  # 私有仓库，不发布到npm
  "version": "1.0.0",
  "scripts": {
    "dev": "pnpm -r dev",
    "build": "pnpm run -r build",
    "test": "pnpm run -r test",
    "lint": "eslint ."
  },
  "devDependencies": {
    "typescript": "^5.2.2",
    "eslint": "^8.56.0",
    "rollup": "^4.9.6",
    "@rollup/plugin-typescript": "^11.1.6",
    "pnpm": "^8.15.4"
  }
}
```

### 4. 创建基础共享包（shared）
```bash
cd packages/shared
pnpm init -y
```

修改 `packages/shared/package.json`：
```json
{
  "name": "@vue/shared",
  "version": "1.0.0",
  "main": "dist/index.js",
  "module": "dist/index.mjs",
  "types": "dist/index.d.ts",
  "scripts": {
    "build": "rollup -c ../../scripts/rollup.config.js",
    "dev": "rollup -c ../../scripts/rollup.config.js -w"
  }
}
```

创建共享工具函数 `packages/shared/src/index.ts`：
```typescript
export const isObject = (val: unknown): val is object => 
  val !== null && typeof val === 'object'

export const isString = (val: unknown): val is string => 
  typeof val === 'string'
```

### 5. 配置 TypeScript
在根目录创建 `tsconfig.json`：
```json
{
  "compilerOptions": {
    "target": "ESNext",
    "module": "ESNext",
    "moduleResolution": "Node",
    "strict": true,
    "declaration": true,
    "sourceMap": true,
    "resolveJsonModule": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "baseUrl": ".",
    "paths": {
      "@vue/*": ["packages/*/src"]
    }
  },
  "include": ["packages/**/*"],
  "exclude": ["**/dist", "**/node_modules"]
}
```




### 6. 配置 Rollup 构建脚本
创建 `scripts/rollup.config.js`：
```js
import typescript from '@rollup/plugin-typescript';
import { fileURLToPath } from 'url';
import { dirname, resolve } from 'path';

// 获取当前文件路径
const __filename = fileURLToPath(import.meta.url);
const __dirname = dirname(__filename);

// 获取当前包目录
const pkgDir = resolve(__dirname, '..', process.cwd());

export default {
  input: resolve(pkgDir, 'src/index.ts'),
  output: [
    {
      file: resolve(pkgDir, 'dist/index.js'),
      format: 'cjs',
      sourcemap: true
    },
    {
      file: resolve(pkgDir, 'dist/index.mjs'),
      format: 'esm',
      sourcemap: true
    }
  ],
  plugins: [
    typescript({
      tsconfig: resolve(__dirname, '..', 'tsconfig.json')
    })
  ],
  external: (id) => id.startsWith('@vue/')
};
```



### 7. 创建核心包（core）
```bash
cd packages/core
pnpm init -y
# 添加对shared包的依赖
pnpm add @vue/shared@workspace --workspace
```

修改 `packages/core/package.json`：
```json
{
  "name": "@vue/core",
  "version": "1.0.0",
  "main": "dist/index.js",
  "module": "dist/index.mjs",
  "types": "dist/index.d.ts",
  "scripts": {
    "build": "rollup -c ../../scripts/rollup.config.js",
    "dev": "rollup -c ../../scripts/rollup.config.js -w"
  },
  "dependencies": {
    "@vue/shared": "workspace:^1.0.0"
  }
}
```

创建核心代码 `packages/core/src/index.ts`：
```typescript
import { isObject } from '@vue/shared'

export function createApp(rootComponent: any) {
  if (!isObject(rootComponent)) {
    throw new Error('root component must be an object')
  }
  
  return {
    mount(container: string | HTMLElement) {
      // 实现挂载逻辑
      console.log('mounting app to', container)
    }
  }
}
```

### 8. 配置 ESLint（可选）
```bash
pnpm add eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin -D -w
```

创建 `.eslintrc.js`：
```javascript
module.exports = {
  root: true,
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint'],
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/recommended'
  ]
}
```

### 9. 测试构建流程
```bash
# 安装依赖
pnpm install

# 构建所有包
pnpm run build

# 开发模式（监听文件变化）
pnpm run dev
```

### 关键特性说明
1. **包之间的依赖管理**：通过 `workspace:` 协议实现内部包相互引用
2. **统一构建流程**：所有子包共享同一个 Rollup 配置
3. **类型共享**：通过 TypeScript 路径映射实现跨包类型提示
4. **独立发布**：每个子包可以单独发布到 npm
5. **统一脚本**：通过 pnpm 工作区命令批量执行脚本

这种架构与 Vue3 源码的相似之处在于：
- 按功能拆分多个独立包（core、compiler、runtime等）
- 使用 TypeScript 保证类型安全
- 通过 monorepo 管理包之间的依赖关系
- 统一的构建和发布流程

后续可以根据需要添加更多包（如 compiler、runtime-dom 等），并完善构建脚本和测试流程。