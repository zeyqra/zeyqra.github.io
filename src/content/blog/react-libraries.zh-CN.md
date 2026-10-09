---
title: "2026 年 React 库"
description: "Robin Wieruch《React Libraries for 2026》完整中文译文，保留原文链接。"
pubDate: 2026-05-26
tags:
  - React
  - 前端开发
  - JavaScript
---

> 原文：[React Libraries for 2026 — Robin Wieruch](https://www.robinwieruch.de/react-libraries/)，作者分类：[React](https://www.robinwieruch.de/categories/react/)。作者：Robin Wieruch。原文页面标注更新于 2026 年 5 月 26 日。以下为用户提供 HTML 文件中文章主体的完整中文译文，文章中的链接均予以保留。

# 2026 年 React 库

React 已经存在相当长一段时间了，多年来，围绕它发展出了一个庞大（但有时也令人眼花缭乱）的库生态系统。从其他语言或框架过渡到 React 的开发者常常难以驾驭构建 Web 应用程序所需的 各种库 。

React 的核心在于允许开发者使用 [函数组件](https://www.robinwieruch.de/react-function-component/) 构建组件驱动的用户界面。虽然它内置了 [React Hooks](https://www.robinwieruch.de/react-hooks/) 等解决方案，用于管理本地状态、处理副作用和优化性能，但最终一切都归结为使用函数（包括组件和 Hooks）来构建用户界面。

在本篇教程中，我们将探索 2026 年必备的 React 库。这些库是使用 React 开发大型应用程序的基础。无论您是初学者还是经验丰富的开发者，本指南都能帮助您轻松驾驭庞大的 React 生态系统。

让我们深入了解一下你可以在下一个 React 应用中使用的库。

## 目录

- [开始一个新的 React 项目](https://www.robinwieruch.de/react-libraries/#starting-a-new-react-project)

- [React 的包管理器](https://www.robinwieruch.de/react-libraries/#package-manager-for-react)

- [React 状态管理](https://www.robinwieruch.de/react-libraries/#react-state-management)

- [React 数据获取](https://www.robinwieruch.de/react-libraries/#react-data-fetching)

- [使用 React Router 进行路由](https://www.robinwieruch.de/react-libraries/#routing-with-react-router)

- [React 中的 CSS 样式](https://www.robinwieruch.de/react-libraries/#css-styling-in-react)

- [React UI 库](https://www.robinwieruch.de/react-libraries/#react-ui-libraries)

- [React动画库](https://www.robinwieruch.de/react-libraries/#react-animation-libraries)

- [React 中的可视化和图表库](https://www.robinwieruch.de/react-libraries/#visualization-and-chart-libraries-in-react)

- [React中的表单库](https://www.robinwieruch.de/react-libraries/#form-libraries-in-react)

- [React 中的代码结构](https://www.robinwieruch.de/react-libraries/#code-structure-in-react)

- [React 身份验证](https://www.robinwieruch.de/react-libraries/#react-authentication)

- [React后端](https://www.robinwieruch.de/react-libraries/#react-backend)

- [React数据库](https://www.robinwieruch.de/react-libraries/#react-database)

- [React Hosting](https://www.robinwieruch.de/react-libraries/#react-hosting)

- [React 测试](https://www.robinwieruch.de/react-libraries/#testing-in-react)

- [React 和不可变数据结构](https://www.robinwieruch.de/react-libraries/#react-and-immutable-data-structures)

- [React国际化](https://www.robinwieruch.de/react-libraries/#react-internationalization)

- [React 中的富文本编辑器](https://www.robinwieruch.de/react-libraries/#rich-text-editor-in-react)

- [React 中的支付](https://www.robinwieruch.de/react-libraries/#payments-in-react)

- [React 中的时间](https://www.robinwieruch.de/react-libraries/#time-in-react)

- [React桌面应用程序](https://www.robinwieruch.de/react-libraries/#react-desktop-applications)

- [React 中的文件上传](https://www.robinwieruch.de/react-libraries/#file-upload-in-react)

- [React 中的邮件](https://www.robinwieruch.de/react-libraries/#mails-in-react)

- [拖放](https://www.robinwieruch.de/react-libraries/#drag-and-drop)

- [使用 React 进行移动开发](https://www.robinwieruch.de/react-libraries/#mobile-development-with-react)

- [React VR/AR](https://www.robinwieruch.de/react-libraries/#react-vrar)

- [React 设计原型](https://www.robinwieruch.de/react-libraries/#design-prototyping-for-react)

- [React 组件文档](https://www.robinwieruch.de/react-libraries/#react-component-documentation)

## 开始一个新的 React 项目

React 初学者经常会问的第一个问题是：如何搭建一个 React 项目？市面上工具众多，选择合适的工具可能会让人不知所措。React 社区中最受欢迎的工具是 [Vite](https://vitejs.dev/) ，它可以轻松创建包含不同库（例如 React）的项目，并且可选地支持 TypeScript。

Vite 也提供 [卓越的性能](https://twitter.com/rwieruch/status/1491093471490412547) 。

如果您已经熟悉 React，不妨考虑使用其流行的（元）框架之一，而不是 Vite。Next.js [是](https://nextjs.org/) 一个广泛使用的选择，它基于 React 构建，因此理解 [React 的基础知识至关重要](https://www.roadtoreact.com/) 。它开箱即用，提供了许多功能，例如不同的渲染技术、基于文件的路由和 API 路由。

Next.js 最初用于服务器端渲染（Web 应用程序），但它也可以用于静态网站生成（网站），并支持其他渲染模式（例如 ISR）。Next.js 的最新成员是 React 服务器组件 (RSC) 和 React 服务器函数 (RSF)，自 2023 年以来，它们通过将 React 组件从客户端移至服务器端，推动了 React 渲染范式的重大转变。

迅速崛起的 Next.js 替代方案是 [TanStack Start](https://tanstack.com/start) ，它于 2026 年 3 月发布了 v1.0 版本。它将 TanStack Router 的类型安全路由与基于文件的路由、服务器函数和 SSR 相结合。RSC 支持目前处于实验阶段，但已列入开发计划。如果您追求成熟度和完善的生态系统，Next.js 仍然是首选；如果您想要一流的类型安全性和 TanStack 的开发者体验，那么 TanStack Start 值得关注（并最终选择）。

另一个选择是 [React Router](https://reactrouter.com/) v7，它在 2024 年末与 Remix 合并，现在既作为路由库也作为完整的框架（加载器、操作、可选的服务器渲染、实验性的 RSC）发布。

如果您优先考虑静态内容的性能，不妨了解一下 [Astro](https://astro.build/) 。作为一个与框架无关的工具，它能与 React 无缝协作，即使使用 React 构建组件，也只会向浏览器发送 HTML 和 CSS。JavaScript 仅在组件需要交互时才会加载，从而确保最佳性能。

如果你只是想了解像 Vite 这样的工具是如何工作的，不妨自己 [搭建一个 React 项目](https://www.robinwieruch.de/minimal-react-webpack-babel-setup/) 。你可以从一个简单的 HTML 和 JavaScript 项目开始，然后自己添加 React 及其配套工具（例如 Webpack、Babel）。虽然这并非你日常工作中需要处理的内容，尤其是在 Vite 已经取代 Webpack 之后，但这仍然是一次绝佳的学习机会，可以帮助你了解底层工具的工作原理。

如果您是一位 React 老手，并且想尝试一些新的东西，可以看看 [RedwoodSDK](https://rwsdk.com/) （Cloudflare 优先、服务器优先的 React，带有 RSC；1.0 beta 版于 2025 年底发布；旧版 RedwoodJS 已更名为 Redwood GraphQL）或 [Waku](https://github.com/dai-shi/waku) ，后者由 Zustand 背后的开发者创建，具有一流的 RSC 支持，目前处于 1.0 alpha 版本。

对于任何新项目来说，还有一点值得了解： [React Compiler](https://react.dev/blog/2025/10/07/react-compiler-1) 于 2025 年 10 月发布了 v1.0 版本，并且 Vite、Next.js 和 Expo 都已开箱即用地支持它。它会自动记忆组件和 Hooks，因此大部分手动操作 `useMemo` 和 `useCallback` 工作量都消失了。

建议：

- Vite 用于客户端渲染的 React 应用程序

- Next.js 用于全栈 React 应用开发（TanStack Start 是一个值得关注的强劲新秀）

- Astro 用于静态端生成的 React 应用程序

## React 的包管理器

在 JavaScript 生态系统（包括 React）中，最广泛使用的包管理器是 [npm](https://www.npmjs.com/) ，因为它随每个 Node.js 安装包一起提供。pnpm [是](https://pnpm.io/) 一个强大的替代方案，凭借其速度和磁盘效率，已成为许多新项目的默认选择。Bun [是](https://bun.sh/) 快速增长的第三种选择：它是一个集包管理器、运行时、打包器和测试运行器于一体的工具链，拥有目前为止最快的安装速度和迅速增长的用户群体。

如果您创建了多个相互依赖或共享一组自定义 UI 组件的 React 应用，不妨了解一下 monorepo 的概念。npm 和 pnpm 都支持通过工作区 (workspace) 实现 monorepo，其中 pnpm 在这方面表现尤为出色。结合 [Turborepo](https://turborepo.org/) 等 monorepo 流水线工具，monorepo 的使用体验将更加完美。对于规模更大或涉及多种编程语言的应用， [Nx](https://nx.dev/) 作为更全面的 monorepo 平台，能够提供更丰富的解决方案。

建议：

- 选择一款软件包管理器并坚持使用。 默认且最广泛使用的 -> npm

- 性能提升但受欢迎程度下降 -> pnpm

- 如果需要单体仓库，请查看 Turborepo（参见教程）。

## React 状态管理

React 提供了两个内置的 hook 来管理局部状态： [useState](https://www.robinwieruch.de/react-usestate-hook) 和 [useReducer](https://www.robinwieruch.de/react-usereducer-hook/) 。对于全局状态管理，内置的 [useContext](https://www.robinwieruch.de/react-usecontext-hook/) hook 允许你将数据从顶层组件传递到更深层的组件，而无需依赖 [props](https://www.robinwieruch.de/react-pass-props-to-component/) ，从而有效地防止了 prop 传递。

React 的这三个 Hook 都允许开发者在 React 中实现强大的状态管理，这些状态管理既可以通过使用 React 的 useState/useReducer Hook 在组件中实现，也可以通过将它们与 React 的 useContext Hook 结合使用来实现全局管理。

如果你发现自己过于频繁地使用 React 的 Context 来管理共享/全局状态，那么你绝对应该了解一下 [Zustand](https://github.com/pmndrs/zustand) 。它允许你管理全局应用程序状态，任何连接到其 store 的 React 组件都可以读取和修改这些状态​​。

如今，Zustand 已成为 React 社区全局状态管理的实际标准。就我个人而言，自 2021 年以来，我再也没有使用过任何状态管理库。有了 TanStack Query 用于服务器端状态管理，React 内置的 hooks 用于本地状态管理，以及 URL 用于共享 UI 状态管理，真正需要专用 store 的应用场景已经大大减少。当然，你仍然会遇到很多使用 Redux 构建的旧版 React 应用，而当你确实需要一个全局 store 时，Zustand 无疑是明智之选。

如果你碰巧在使用 Redux，那么你也绝对应该了解一下 [Redux Toolkit](https://redux-toolkit.js.org/) 。如果你对状态机感兴趣， [XState](https://github.com/statelyai/xstate) 是最成熟的选择。作为替代方案，如果你需要一个全局 store，但又不喜欢 Zustand 或 Redux，可以看看其他流行的状态管理方案，例如 [Jotai](https://github.com/pmndrs/jotai) （原子级）或 [MobX](https://github.com/mobxjs/mobx) （仍在维护，但现在主要用于遗留代码库）。

还有一个值得了解的模式：对于 URL 中存储的状态（例如筛选器、标签页、分页）， [nuqs](https://nuqs.dev/) 提供带有 Hook 式 API 的类型化搜索参数。它与框架无关，并被 Sentry、Supabase、Vercel 和 Clerk 等工具所使用。

建议：

- useState/useReducer 用于共置或共享状态（参见教程）

- 启用 useContext 以启用 少量 全局状态（参见教程）

- 大量 全局状态 的 Zustand（或其替代方案）

## React 数据获取

React 内置的 hooks 非常适合 UI 状态，但对于远程数据（以及数据获取）的状态管理（即缓存），我建议使用专门的数据获取库，例如 [TanStack Query](https://tanstack.com/query) （以前称为 React Query）。

虽然 TanStack Query 本身并不被视为状态管理库，因为它主要用于从 API 获取远程数据，但它会为您处理所有远程数据的状态管理（例如缓存、乐观更新）。

TanStack Query 最初是为调用 [REST API](https://www.robinwieruch.de/node-express-server-rest-api/) 而设计的。不过，现在它也支持 [GraphQL](https://www.roadtographql.com/) 。但是，如果您正在寻找更专用的 React 前端 GraphQL 库，可以考虑 [Apollo Client](https://www.apollographql.com/docs/react/) （v4 版本更精简，与 React 解耦，并且支持 React 编译器）、 [urql](https://formidable.com/open-source/urql/) （轻量级）或 [Relay](https://github.com/facebook/relay) （专为 Meta 内部规模构建，外部应用较为小众）。

如果您已经在使用 Redux，并且想要在 Redux 中添加集成状态管理的数据获取功能，那么与其添加 TanStack Query，不如看看 [RTK Query](https://redux-toolkit.js.org/rtk-query/overview) ，它将数据获取功能完美地集成到了 Redux 中。

如果你掌控着前端和后端（两者都使用 TypeScript），不妨了解一下 [tRPC](https://trpc.io/) ，它能提供端到端的类型安全 API。这能极大地提升开发效率和用户体验。你还可以将它与 TanStack Query 结合使用，在享受数据获取带来的便利的同时，仍然可以通过类型化函数从前端调用后端。

最后，如果您使用的（元）框架支持 React 服务器组件/服务器函数 (RSC/RSF)（例如 Next.js），则可以使用它们进行数据获取。它们允许您在服务器端获取数据并将其传递给客户端。这样，您就可以避免使用客户端数据获取库。

建议：

- 服务器端数据获取 React 服务器组件/函数（如果（元）框架支持）

- 客户端数据获取 TanStack 查询（REST API 或 GraphQL API） 结合 axios 或 [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

- Apollo Client（GraphQL API） 为了获得更完善的 GraphQL 体验

- 适用于紧耦合客户端-服务器架构的tRPC

## 使用 React Router 进行路由

如果使用 Next.js 这样的 React 框架，通常已经内置了路由。独立 React 应用中最常见的路由库是 [React Router](https://reactrouter.com/)。它现在有两种模式：传统的库模式用于客户端路由（例如 Vite 单页应用），框架模式则是在原 Remix 基础上发展而来，提供 loaders、actions、服务端渲染和实验性的 React Server Components（RSC）支持。

另一个强劲的选择是 [TanStack Router](https://tanstack.com/router)。它已经稳定，尤其适合使用 TypeScript 的项目；TanStack 系列注重易用的 API 和完善的类型推断。

React Router 和 TanStack Router（配合 TanStack Start）都在推进 RSC 支持。RSC 可以让部分组件在服务器端运行，适用于缩小客户端资源、在服务端获取数据等场景。

刚接触 React 时，可以先学习[条件渲染](https://www.robinwieruch.de/conditional-rendering-react/)，了解如何根据状态切换页面级组件，再决定是否需要路由。

建议：
- 服务端路由：Next.js、React Router 框架模式或 TanStack Start
- 客户端路由：最常用的是 React Router，也可以选择 TanStack Router

## React 中的 CSS 样式

React 中的 CSS 有很多写法和不同观点，本节只介绍常见选择。本节介绍几种常见的样式选择。

初学 React 时，可以先在 JSX 中使用 style 对象理解样式如何工作；实际应用中，通常把样式放在单独的 CSS 文件中。

~~~jsx
const Headline = ({ title }) => <h1 style={{ color: 'blue' }}>{title}</h1>;
~~~

~~~jsx
import './Headline.css';

const Headline = ({ title }) => (
  <h1 className="headline" style={{ color: 'blue' }}>{title}</h1>
);
~~~

应用规模变大后，可以按需求选择不同方案。条件组合 className 时，可以使用轻量工具 [clsx](https://github.com/lukeed/clsx)。

[Tailwind CSS](https://tailwindcss.com/) 是最流行的实用类 CSS 方案之一。它提供一组可组合的预定义类，便于快速构建界面和统一设计规则；代价是 JSX 中会出现较长的 class 列表，团队也需要熟悉这些类。

~~~jsx
const Headline = ({ title }) => <h1 className="text-blue-700">{title}</h1>;
~~~

Tailwind v4 于 2025 年 1 月发布。它把主题配置转向 CSS 中的 @theme，并以新的 Oxide 引擎提升构建速度，还能自动发现内容源。新项目可以从 v4 开始。

CSS Modules 是另一种常见选择。它把 CSS 限定在模块范围内，降低样式意外影响其他组件的风险。

~~~jsx
import styles from './style.module.css';

const Headline = ({ title }) => (
  <h1 className={styles.headline}>{title}</h1>
);
~~~

[styled-components](https://www.robinwieruch.de/react-styled-components/) 和 [Emotion](https://emotion.sh/) 属于 CSS-in-JS 方案，可把样式与 React 组件放在一起。

~~~jsx
import styled from 'styled-components';

const BlueHeadline = styled.h1`
  color: blue;
`;

const Headline = ({ title }) => <BlueHeadline>{title}</BlueHeadline>;
~~~

作者不建议新项目采用这两个运行时 CSS-in-JS 库：[styled-components 自 2025 年 3 月起进入维护模式](https://github.com/orgs/styled-components/discussions/5657)，其维护者也不建议新项目使用。Emotion 仍在维护，但其使用很大程度上与 MUI v5 及更新版本的依赖关系相连。两者在运行时使用 React Context，这与 React Server Components 的方向不容易配合，因此新方案更多转向构建时生成样式。

实用类 CSS 已成为主流。希望采用类似 CSS-in-JS 的写法、同时避免运行时开销的团队，可以了解 [PandaCSS](https://panda-css.com/)（由 Chakra/Zag 作者开发，Chakra UI v3 以此为基础）或 [vanilla-extract](https://vanilla-extract.style/)（TypeScript 优先、提供类型安全）。Meta 的 [StyleX](https://stylexjs.com/) 采用原子化 CSS-in-JS；[UnoCSS](https://unocss.dev/) 则是更灵活的实用类 CSS 方案。项目地址：[StyleX](https://stylexjs.com/)、[UnoCSS](https://unocss.dev/)。

React 19 对原生 style 标签的优先级和去重处理，也减少了过去采用 CSS-in-JS 的一些理由。因此，纯 CSS Modules 或 Tailwind 现在同样是很有吸引力的选择。

建议：
- 实用类 CSS：最流行的选择之一，例如 Tailwind CSS
- CSS-in-CSS：例如 CSS Modules
- 构建时 CSS-in-JS：例如 PandaCSS 或 vanilla-extract，适合需要 JS 驱动样式但不想承担运行时开销的项目

## React UI 库

对于初学者来说，从零开始构建可复用组件是一种非常棒且值得推荐的学习体验。无论是 [下拉菜单](https://www.robinwieruch.de/react-dropdown/) 、 [选择框](https://www.robinwieruch.de/react-select/) 、 [单选按钮](https://www.robinwieruch.de/react-radio-button/) 还是 [复选框](https://www.robinwieruch.de/react-checkbox/) ，最终你都应该掌握如何自己创建这些 UI 组件。

但是，如果您没有足够的资源自己设计所有组件，则需要使用 UI 库，该库可让您访问许多预构建的组件，这些组件共享相同的设计系统、功能和可访问性规则：

- [Material UI (MUI)](https://material-ui.com/) （在自由职业项目中仍然很常见）

- [Mantine](https://mantine.dev/) （v8；2026 年最强的全能选手之一）

- [Chakra UI](https://chakra-ui.com/) （v3，在 PandaCSS 上重写）

- [Hero UI](https://www.heroui.com/) （原名 Next UI）

- [Park UI](https://park-ui.com/) （基于 Ark UI）

- [PrimeReact](https://primereact.org/)

- [Ant Design](https://ant.design/) （仍然是企业/数据密集型 B2B 领域的默认选择）

不过，目前的趋势是转向无头 UI 库。它们不包含样式，但具备现代组件库所需的所有功能和可访问性。大多数情况下，它们会与 Tailwind 等实用优先 CSS 解决方案结合使用：

- [shadcn/ui](https://ui.shadcn.com/) （自 2023 年起成为事实上的标准；2026 年起，您可以将模块安装到 Radix 或 Base UI 原语上）

- [Radix](https://www.radix-ui.com/) （shadcn/ui 的原始基础）

- [Base UI](https://base-ui.com/) （2026 年发布 v1.0 版本，由 Radix/Floating UI/MUI 的作者开发；现在与 Radix 一起成为一流的 shadcn 目标）

- [React Aria](https://react-spectrum.adobe.com/react-aria/)

- [Ark UI](https://ark-ui.com/) （由 Chakra UI 的开发者制作，基于 Zag 状态机构建）

- [Ariakit](https://ariakit.org/)

- [Daisy UI](https://daisyui.com/) （v5，兼容 Tailwind v4；悄然成为下载量最高的 UI 库之一）

- [Headless UI](https://headlessui.com/)

- [Tailwind UI](https://www.tailwindui.com/) （非免费）

Bootstrap 的粉丝们仍然可以使用 [React Bootstrap，](https://react-bootstrap.github.io/) 如果你需要的就是这种设计语言的话。

## React 动画库

Web 应用中的所有动画都始于 CSS。然而，您很快就会发现 CSS 动画可能无法完全满足您的需求。一些流行的 React 动画库包括：

- [Motion](https://motion.dev/) （事实上的标准，以前称为 Framer Motion；v12 完全支持 React 19）

- [react-spring](https://www.react-spring.dev/) （专为基于物理的动画而设计的利基框架）

- [GSAP](https://gsap.com/) （Webflow 收购后现已采用 MIT 许可证；非常适合复杂的基于时间轴的动画）

## React 中的可视化和图表库

如果要从底层自行构建图表，[D3](https://d3js.org/) 提供了完整的可视化能力，但学习成本较高。许多开发者会选择 React 图表库，以较少的灵活度换取更快的开发速度。

- [Recharts](http://recharts.org/) 是作者的个人推荐；shadcn/ui 的图表组件也基于 Recharts。它提供现成图表，也允许组合和定制。
- [Tremor](https://tremor.so/) 提供预设样式的图表组件，视觉风格与 shadcn/ui 接近，常用于 SaaS 仪表板。
- [visx](https://github.com/airbnb/visx) 更接近底层 D3，组合能力强，但学习曲线较陡，需要自行处理更多图表细节。
- [nivo](https://nivo.rocks/)、[react-chartjs](https://github.com/reactchartjs/react-chartjs-2) 和 [Victory](https://formidable.com/open-source/victory/) 也是可选方案。原文指出，Victory 的开发节奏已放缓，目前更多工作集中在 Victory Native XL。

## React 中的表单库

[目前为止， React Hook Form](https://react-hook-form.com/) 是 React 中最流行的表单库 。它包含了所有必需的功能：验证（最常用的是 [zod](https://github.com/colinhacks/zod) 验证器）、表单提交、表单状态管理等等。对于想要在 React 中快速入门表单开发的用户来说，这是一个非常棒的库。

两种值得了解的替代方案：

- [TanStack Form](https://tanstack.com/form) 稳定且发展迅速，尤其适合需要类型安全的复杂表单或已经投资于 TanStack 生态系统其他部分的用户。

- [Conform](https://conform.guide/) 最适合 Next.js App Router 或 React Router 框架模式下的逐步增强型服务器操作表单。

您可能仍然会在现有代码库中遇到 [Formik](https://github.com/jaredpalmer/formik) 或 [React Final Form](https://final-form.org/react) ，但目前它们实际上已经停止维护，不建议在新项目中使用。

建议：

- React Hook 表单 通过 Zod 集成进行验证

## React 中的代码结构

如果你想采用统一且符合常理的代码风格，请在 React 项目中使用 [ESLint](https://eslint.org/) 。像 ESLint 这样的代码检查工具可以强制执行特定的代码风格。例如，你可以使用 ESLint 将遵循某个流行的风格指南作为一项要求。你还可以将 [ESLint 集成到你的 IDE/编辑器中](https://www.robinwieruch.de/vscode-eslint/) ，它会指出所有错误。目前默认版本是带有扁平化配置的 ESLint v9，TypeScript-ESLint 生态系统也已跟进。

如果你想采用统一的代码格式，可以在 React 项目中使用 [Prettier](https://github.com/prettier/prettier) 。它是一款带有明确规范的代码格式化工具，只有少量可选配置。你可以 [将其集成到编辑器或 IDE 中](https://www.robinwieruch.de/how-to-use-prettier-vscode/) ，这样每次保存文件时它都会自动格式化代码。Prettier 并不能取代 ESLint，但它可以 [很好地](https://www.robinwieruch.de/prettier-eslint/) 与之集成。

2026 年最值得关注的是基于 Rust 和 Go 的替代方案。oxlint 是一个速度更快的 ESLint 替代方案，它复用了现有的 ESLint 插件生态系统（插件 API 目前处于 alpha 测试阶段，但已支持约 99.5% 的现有插件）。其配套的格式化工具 [oxfmt](https://oxc.rs/) [则](https://oxc.rs/docs/guide/usage/formatter.html) 是一个与 Prettier 兼容的替代方案，速度比 Prettier 快约 30 倍。类型感知型 oxlint 规则由 [tsgolint](https://github.com/oxc-project/tsgolint) 提供支持，tsgolint 基于微软的 [tsgo](https://github.com/microsoft/typescript-go) 构建（自 2026 年 4 月起支持 TypeScript 7.0 Beta 版，速度比 tsgo 快 7-10 倍 `tsc` ）。我建议 2026 年的新项目使用 oxlint + oxfmt，但需要注意的是，oxfmt 目前处于 beta 测试阶段，而类型感知型 oxlint 则处于 alpha 测试阶段。它们虽然可以用于生产环境，但您应该了解自己选择的工具的现状。

如果您希望今天就拥有一个稳定的、单一二进制工具链， [Biome](https://biomejs.dev/) v2 在一个工具中涵盖了代码检查和格式化，具有类型感知规则和稳定的插件 API。

建议：

- 新项目（展望未来）：oxlint + oxfmt，稳定版发布后将使用 tsgo 进行快速类型检查。

- 今天就想拥有一个稳定的单二进制工具？试试 Biome v2 吧！

- 成熟、插件丰富、保守：ESLint + Prettier

## React 身份验证

在 React 应用中，你可能需要引入身份验证功能，例如注册、登录和注销。密码重置和密码修改等其他功能也经常需要。这些功能远非 React 本身所能涵盖，因为后端应用会为你处理这些事务。

最佳的学习体验是自己实现一个包含身份验证的全栈应用程序（例如 [《通往下一步之路](https://www.road-to-next.com/) 》）。由于身份验证存在许多安全风险和并非人人了解的细节，我建议在生产环境中使用第三方身份验证服务：

- [Better Auth](https://www.better-auth.com/) （2026 年自托管身份验证的事实上的开源选择； [Arctic](https://arcticjs.dev/) 是与之配合良好的独立 OAuth 助手）

- [Auth.js](https://authjs.dev/) （v5，原名 NextAuth；维护者现在建议新项目使用 Better Auth）

- [Clerk](https://clerk.com/) （付费）或 [Kinde](https://kinde.com/) （付费；捆绑包还具有标记和计费功能）

- [WorkOS 的 AuthKit](https://www.authkit.com/) （托管登录，如果以后需要 B2B 功能，还可提供企业级 SSO/SCIM）

- [Stack Auth](https://stack-auth.com/) （开源 Clerk 替代方案，可自托管）

- [Supabase Auth](https://supabase.com/docs/guides/auth/overview) 、 [Convex Auth](https://labs.convex.dev/auth) 或 Firebase Auth（如果您已经在使用这些后端，这通常是最简单的途径）

- [Auth0](https://auth0.com/) 或 [AWS Cognito](https://aws.amazon.com/cognito/) （企业/AWS 环境）

## React 后端

由于 React 向服务器端迁移的趋势十分强劲，因此 React 项目最理想的部署环境是像 Next.js（主要用于动态 Web 应用）或 Astro（主要用于静态网站）这样的（元）框架。React [Router](https://reactrouter.com/) v7（Remix 和 React Router 合并后的版本）和 [TanStack Start](https://tanstack.com/start) （自 2026 年 3 月起发布 v1.0）也是同样可靠的全栈框架选择。

如果你无法使用全栈框架，但仍然想继续使用 JS/TS， [Hono](https://hono.dev/) 是 2026 年领先的运行时优先框架，而 [tRPC](https://trpc.io/) (v11) 则为所有这些框架提供了端到端的类型安全。Express仍然是传统的实用框架，并在 2024 年底发布了 v5 [版本](https://expressjs.com/) ，其中包含了现代化的 async/await 中间件。其他替代方案包括 [Fastify](https://fastify.dev/) 、 [NestJS](https://nestjs.com/) 和 [Elysia](https://elysiajs.com/) （Bun-first）。

值得一提的是 [Koa](https://koajs.com/) 和 [Hapi](https://hapi.dev/) ，它们偶尔还会出现在一些较老的代码库中。

## React 数据库

虽然并非完全依赖于 React，但由于全栈 React 应用如今越来越流行，React 与数据库层的联系也比以往任何时候都更加紧密。在开发任何 Next.js 应用时，您很可能需要用到数据库 ORM。目前两大热门选择是 [Drizzle ORM](https://orm.drizzle.team/) （在 2025 年底的周下载量超过了 Prisma；是无服务器/边缘计算的默认选择）和 [Prisma](https://www.prisma.io/) （仍然是最成熟的生态系统，对于希望使用开箱即用的团队来说是一个稳妥的选择）。

[Kysely](https://kysely.dev/) 是一个更轻量级的替代方案 ，它是一个查询构建器，而不是一个完整的 ORM。

在选择数据库时， [Supabase](https://supabase.com/) （Postgres）和 [Firebase](https://firebase.google.com/) 仍然是流行的全功能后端。如果您想要将数据库和后端集成在一个软件包中， [Convex](https://www.convex.dev/) 凭借其一流的 React Hooks 功能，已成为一个强大的响应式替代方案。

流行的无服务器数据库替代方案包括 [Neon](https://neon.tech/) （已于 2025 年被 Databricks 收购，目前仍在运营）、 [Turso](https://turso.tech/) （边缘端使用 libSQL，2026 年默认的堆栈组合是 Hono + Drizzle，运行在 Cloudflare Workers 上）以及 [Xata](https://xata.io/) （已于 2025 年转向开源 Postgres，用于 AI/代理工作负载）。PlanetScale仍在运营，但在 2024 年停止面向业余用户的服务后 [，](https://planetscale.com/) 目前仅面向企业用户。

## React 托管与部署

部署 React 应用与部署其他 Web 应用类似。若希望完全掌控环境，可以使用 [DigitalOcean](https://m.do.co/c/fb27c90322f3) 或 [Hetzner](https://www.hetzner.com/) 等服务，自行管理基础设施。

如果希望由平台代管，[Vercel](https://vercel.com/) 为 Next.js 项目提供顺畅的部署体验。需要留意其限制：免费套餐不允许商业用途，流量和函数调用超额费用在规模扩大后可能累积。[Netlify](https://www.netlify.com/) 是直接竞争者，按席位计价更友好。

若希望自行托管又需要平台化管理，[Coolify](https://coolify.io/) 已发展为这一领域领先的开源 PaaS，可一键部署许多常见服务。

[Cloudflare](https://www.cloudflare.com/) 也越来越适合托管全栈 React 应用，可使用 Workers、Pages、R2 和 D1；其免费流量额度较有竞争力。

如果已经使用 Firebase 或 Supabase 等后端即服务，可以看看它们是否也提供托管。其他常见平台包括 [Render](https://render.com/)、[Fly.io](https://fly.io/)、[Railway](https://railway.app/)，也可以直接使用 AWS、Azure 或 Google Cloud。

## React 测试

React 应用测试通常以测试框架为基础。[Vitest](https://vitest.dev/) 是原文对新项目的推荐；v3 提供了成熟的浏览器模式，并由 Playwright/WebdriverIO 支持。[Jest](https://github.com/facebook/jest) v30 仍适用于已有的 CommonJS 项目和 React Native 项目，而 Vitest 对 React Native 的支持有限。两者都提供测试运行器、断言、间谍、模拟和桩等能力。

[React Testing Library（RTL）](https://www.robinwieruch.de/react-testing-library/) 通常与测试框架配合使用。它可以渲染组件、模拟 HTML 元素上的用户操作，再由测试框架执行断言；测试重点放在用户能观察到的行为上。

对于端到端测试，原文认为 [Playwright](https://playwright.dev/) 是 2026 年最主流的新选择。[Cypress](https://www.robinwieruch.de/react-testing-cypress/) 仍适合已经采用它的团队，或看重其组件测试开发体验的团队。两者都能自动化浏览器中的交互，从用户视角检查应用行为。

建议：
- 单元与集成测试：Vitest + React Testing Library
- 端到端测试：Playwright，或 Cypress
- 需要时可使用 Vitest 快照测试

## React 与不可变数据结构

原生 JavaScript 已提供多种处理不可变数据的方式。如果团队希望更明确地维护不可变数据结构，常见选择是 [Immer](https://github.com/immerjs/immer)。[Mutative](https://github.com/unadlib/mutative) 是可直接替换的方案，采用相同的 draft API；如果性能很重要，它的运行时速度更快。

React Compiler 会自动执行记忆化，因此在组件代码中手动追求不可变更新的必要性有所降低。原文认为，Immer 现在更适合用于 Redux Toolkit 和 reducer 模式。

## React 国际化

为 React 应用实现[国际化（i18n）](https://www.robinwieruch.de/react-internationalization/)时，除了翻译文字，还要处理复数规则、日期与货币格式等问题。原文列出了以下工具：

- [react-i18next](https://github.com/i18next/react-i18next)：生态最大、稳妥的默认选择
- [next-intl](https://next-intl.dev/)：Next.js 项目的常见选择
- [Paraglide JS](https://inlang.com/m/gerre34r/library-inlang-paraglideJs)：编译时生成类型安全的 m.key() 函数，可 tree-shake；与运行时方案相比，通常能显著缩小资源包
- [Lingui](https://lingui.dev/)：编译时处理消息，适合使用 PO 文件的团队
- [FormatJS](https://github.com/formatjs/formatjs)：仍在维护，但原文认为它现在主要用于已有项目

## React 富文本编辑器

原文在 2026 年会优先考虑以下 React 富文本编辑器：

- [Tiptap](https://tiptap.dev/)：默认推荐；v3 支持 SSR/JSX，核心扩展采用 MIT 许可证，也可选用 Tiptap Cloud 的 AI 与协作功能
- [BlockNote](https://www.blocknotejs.org/)：基于 Tiptap 的 Notion 风格块编辑器，内置 Yjs 协作；适合需要开箱即用的 Notion 式体验
- [Plate](https://platejs.org/)：v49 及以上版本，是原生适配 shadcn/ui 的选择
- [Lexical](https://lexical.dev/)：由 Meta 支持，用于 Facebook、Instagram 和 WhatsApp Web
- [Slate](https://www.slatejs.org/)：底层编辑器引擎，也是 Plate 的基础

## React 支付

在 React 应用中集成支付时，常见服务商是 Stripe 和 PayPal；两者都提供面向 React 的集成方式。

- [PayPal](https://developer.paypal.com/docs/checkout/)
- Stripe 的 [React Stripe Elements](https://github.com/stripe/react-stripe-js)
- [Stripe Checkout](https://stripe.com/docs/payments/checkout)

如果需要 Merchant of Record（由服务商代为处理销售税、增值税和合规事项），2026 年值得了解的选择包括：

- [Paddle](https://www.paddle.com/)：SaaS 领域成熟的 MoR
- [Polar.sh](https://polar.sh/)：开源，在独立开发者和开发工具创业团队中发展很快
- [Lemon Squeezy](https://www.lemonsqueezy.com/)：2024 年被 Stripe 收购，目前仍独立运营；Stripe 自有的 Managed Payments 与其功能已有重叠
- [Braintree](https://www.braintreepayments.com/)：PayPal 旗下服务

## React 中的时间处理

如果 React 应用经常处理日期、时间和时区，可以使用专门的工具：

- [date-fns](https://github.com/date-fns/date-fns)：v4 增加了原生时区支持
- [Day.js](https://github.com/iamkun/dayjs)

新代码值得关注的是 [Temporal API](https://github.com/tc39/proposal-temporal)。它于 2026 年 3 月达到 TC39 Stage 4，并纳入 ES2026。新版 Chrome、Firefox 和 Edge 已原生支持，Safari 仍在跟进。等目标运行环境都支持后，Temporal 将成为基于标准的日期时间方案；在此之前，date-fns 和 Day.js 仍是实用的过渡选择。

## React 桌面应用

原文将 [Tauri](https://tauri.app/) 列为 2026 年新建跨平台桌面应用的默认推荐。Tauri 2.0 正式支持 iOS 和 Android，生成的程序体积约为 Electron 的十分之一，内存占用也更低。

如果需要成熟的生态和可预测的 Chromium 行为，[Electron](https://www.electronjs.org/) 仍是稳妥选择；VS Code、Slack 和 Discord 都是这类桌面应用的例子。

## React 文件上传

简单的文件上传可以从原生的 file 类型 input 开始。若需要拖放区域，[react-dropzone](https://react-dropzone.js.org/) 仍是常见的无头 dropzone 方案。原文指出它近期维护较轻，但目前还没有真正取代它的方案。

## React 邮件

只用 HTML 编写邮件很麻烦。有些库可以让开发者用 React 组件生成响应式 HTML 邮件：

- [react-email](https://react.email/)：原文的推荐，也是 Resend 团队推动的事实标准
- [jsx-email](https://jsx.email/)：优先支持 Tailwind 插件架构，是 react-email 的主要替代方案

若需要邮件服务，原文在 2026 年优先推荐 [Resend](https://resend.com/)，理由是开发体验好，并且与 react-email 紧密集成。[Postmark](https://postmarkapp.com/) 长期专注于事务性邮件；[Plunk](https://www.useplunk.com/) 是可自行托管的开源方案；[Twilio SendGrid](https://sendgrid.com/) 和 [Mailgun](https://www.mailgun.com/) 等老牌服务仍适用于企业级发送量。

## 拖放交互

作者用过 [react-beautiful-dnd 的后继项目](https://github.com/hello-pangea/dnd)，认为它维护活跃，适合列表重排。

功能更灵活的替代方案是 [dnd kit](https://dndkit.com/)，但学习曲线更陡。它在 2023 至 2024 年间一度较为沉寂，随后于 2025 至 2026 年重新发布，并重做了核心包，因此原文认为其是否仍在维护的疑虑已经消除。若需要与框架无关的高性能拖放能力，也可以了解 [Atlassian Pragmatic drag-and-drop](https://github.com/atlassian/pragmatic-drag-and-drop)。

## 使用 React 进行移动开发

将 React 用于移动应用时，首选方案是 [React Native](https://reactnative.dev/)。到了 2026 年，许多团队会直接采用 [Expo](https://www.robinwieruch.de/react-native-expo/)，它实际上已成为 React Native 的默认框架。React Native 新架构现已默认启用，也是未来的发展方向。

若希望 Web 和移动端共用组件，可以考虑 [Tamagui](https://tamagui.dev/)。[Solito](https://solito.dev/) 则连接 React Navigation 与 Next.js，适合希望用一套代码覆盖不同平台的项目。

## React 虚拟现实与增强现实

React 也可以用于虚拟现实和增强现实。作者说明自己没有实际使用过这些库，但在 2026 年会优先考虑：

- [react-three-fiber](https://github.com/pmndrs/react-three-fiber)：Poimandres 项目，为 Three.js 提供 React 渲染器
- [@react-three/drei](https://github.com/pmndrs/drei)：与 react-three-fiber 配套的一组实用工具
- [@react-three/xr](https://github.com/pmndrs/xr)：Poimandres 的 XR 方案，支持命中测试、锚点、网格/平面检测和 Quest 模拟器

## React 设计原型

对 React 开发者而言，AI 界面生成器已成为从想法快速得到可运行组件的方式。[Vercel 的 v0](https://v0.dev/) 是原文的默认推荐：它可以生成使用 Tailwind 和 shadcn/ui 的 React 界面，并提供完整编辑器、Git 集成和数据库连接。

[Lovable](https://lovable.dev/) 适合端到端构建 React + Supabase 全栈 MVP；如果希望选择框架并在浏览器中运行代码，可以了解 [Bolt.new](https://bolt.new/)。在对话中快速生成单个组件时，[Claude Artifacts](https://www.anthropic.com/news/artifacts) 也很方便。

与设计师协作时，[Figma](https://www.figma.com/) 仍是设计交接的主流工具。Figma Make 可以根据提示生成 React，Code Connect 则能把 Figma 组件关联到真实代码库，因此设计到代码的流程较过去更顺畅。

## React 组件文档

如果负责维护组件文档，可以考虑多种 React 文档工具。作者在不少项目中使用过 [Storybook](https://storybook.js.org/)，评价中性。Storybook 9 内置基于 Vitest 的测试，安装体积也比早期版本小得多。

更轻量、面向 React 的替代方案是 [Ladle](https://ladle.dev/)，冷启动更快。[React Cosmos](https://reactcosmos.org/) 仍适合已经使用它的团队；[Docusaurus](https://github.com/facebook/docusaurus) 则适用于更通用的文档网站。

## 原文中的延伸阅读

原文还在各章节间插入了以下阅读卡片。这里集中列出，保留其链接：

- [2026 年 React 技术栈](https://www.robinwieruch.de/react-tech-stack/)
- [了解网站与 Web 应用](https://www.robinwieruch.de/web-applications/)
- [把 React 作为库或框架来学习](https://www.robinwieruch.de/learning-react/)
- [The Road to Next](https://www.road-to-next.com/)
- [如何启动 React 项目](https://www.robinwieruch.de/react-starter/)
- [我的 2026 年 Web 开发 Mac 配置](https://www.robinwieruch.de/mac-setup-web-development/)
- [如何用 TypeScript/JavaScript 构建 Monorepo](https://www.robinwieruch.de/javascript-monorepos/)
- [何时使用 useState 或 useReducer](https://www.robinwieruch.de/react-usereducer-vs-usestate/)
- [如何结合 useState/useReducer 与 useContext](https://www.robinwieruch.de/react-state-usereducer-usestate-usecontext/)
- [React 中的状态是什么](https://www.robinwieruch.de/react-state/)
- [Redux 教程（不使用 Redux Toolkit）](https://www.robinwieruch.de/react-redux-tutorial/)
- [了解 TanStack Query 的底层工作原理](https://www.robinwieruch.de/react-hooks-fetch-data/)
- [React 本地状态与远端数据状态详解](https://www.robinwieruch.de/react-state/)
- [创建第一个端到端类型安全的 tRPC 应用](https://www.robinwieruch.de/react-trpc/)
- [学习使用 React Router](https://www.robinwieruch.de/react-router/)
- [React CSS 样式完整指南](https://www.robinwieruch.de/react-css-styling/)
- [styled-components 最佳实践](https://www.robinwieruch.de/styled-components/)
- [智能体编码：押注原语（D3 与 Recharts）](https://www.robinwieruch.de/agentic-coding-bet-on-primitives/)
- [如何在 React 中校验表单](https://www.robinwieruch.de/react-form-validation/)
- [React 文件夹结构五步指南](https://www.robinwieruch.de/react-folder-structure/)
- [按功能组织 React 架构](https://www.robinwieruch.de/react-feature-architecture/)
- [如何使用 React Router 实现身份验证](https://www.robinwieruch.de/react-router-authentication/)
- [Web 开发趋势](https://www.robinwieruch.de/web-development-trends/)
- [React Native 导航入门](https://www.robinwieruch.de/react-native-navigation/)

React 生态是一个围绕 React 构建的灵活体系。你可以根据实际问题选择需要的库，从小规模开始，再逐步添加工具来解决具体挑战。如果 React 核心能力已经满足需求，也可以只使用 React，保持项目轻量。


