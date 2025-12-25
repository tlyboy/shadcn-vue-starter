# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 开发命令

```bash
pnpm dev      # 启动开发服务器
pnpm build    # 类型检查 + 构建生产版本
pnpm preview  # 预览生产构建
```

## 技术栈

- **Vue 3** + TypeScript + Vite (使用 rolldown-vite)
- **Tailwind CSS v4** - 通过 `@tailwindcss/vite` 插件集成
- **Reka UI** - 无头组件库（替代 Radix Vue）
- **shadcn-vue** 风格的组件系统

## 架构

### 路径别名
- `@/` 映射到 `./src/`

### UI 组件模式
组件位于 `src/components/ui/`，每个组件目录包含：
- `index.ts` - 导出组件和使用 `class-variance-authority` 定义的变体
- `Component.vue` - 使用 Reka UI 的 `Primitive` 组件实现

示例：
```typescript
// 使用 cva 定义变体
export const buttonVariants = cva("base-classes", {
  variants: { variant: {...}, size: {...} },
  defaultVariants: {...}
})
```

组件使用 `cn()` 工具函数合并 Tailwind 类（位于 `src/lib/utils.ts`）。

### 主题系统
- 深色模式通过 `.dark` class 切换（使用 `@vueuse/core` 的 `useColorMode`）
- 颜色变量使用 OKLCH 色彩空间定义在 `src/index.css`
- 支持 View Transitions API 的主题切换动画

### 布局
- 布局组件位于 `src/layouts/`
- 使用 slot 模式传递内容
