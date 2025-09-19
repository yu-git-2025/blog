# 在 Vite.js+React 项目中渲染本地 Markdown 文件

##### 推荐方法三

## 方法一

### 使用 Vite 原生 `?raw` 导入（简单直接）

Vite 原生支持通过 `?raw` 后缀将文件作为原始字符串导入[1](https://juejin.cn/post/7108960087154098206?from=search-suggest)[2](https://devpress.csdn.net/vue/66cd49dfc618435984a26d45.html)。这意味着你可以直接获取到 Markdown 文件的文本内容，然后使用一个 Markdown 解析库将其转换为 HTML，最后使用 React 渲染出来。

#### 具体步骤：

1. **安装解析库**：你需要一个库将 Markdown 文本转换成 HTML。常用的有 `marked`（快速轻量）或 `remarkable`。
   这里以 `marked` 为例，你也可以选择其他库。

   ```bash
   npm install marked

   #添加代码语法高亮
   npm install highlight.js
   ```

2. **创建 Markdown 渲染组件**：创建一个 React 组件（例如 `MarkdownRenderer.jsx`）来处理 Markdown 的导入和渲染。

   ```jsx
   // MarkdownRenderer.jsx
   import React, { useEffect, useRef } from "react";
   import { marked } from "marked";
   import hljs from "highlight.js";
   import "highlight.js/styles/github.css";

   const MarkdownRenderer = ({ content }) => {
     const markdownRef = useRef(null);

     useEffect(() => {
       // 配置 marked 选项
       marked.setOptions({
         highlight: function (code, lang) {
           if (lang && hljs.getLanguage(lang)) {
             try {
               return hljs.highlight(code, { language: lang }).value;
             } catch (err) {
               console.error("Error highlighting code:", err);
             }
           }
           return hljs.highlightAuto(code).value;
         },
         langPrefix: "hljs language-", // 添加语言类前缀
       });

       // 渲染 Markdown 内容
       if (markdownRef.current && content) {
         markdownRef.current.innerHTML = marked.parse(content);

         // 手动触发代码高亮（确保所有代码块都被高亮）
         markdownRef.current.querySelectorAll("pre code").forEach((block) => {
           hljs.highlightElement(block);
         });
       }
     }, [content]);

     return <div ref={markdownRef} />;
   };

   export default MarkdownRenderer;
   ```

   - **说明**：`dangerouslySetInnerHTML` 是 React 中渲染 HTML 字符串的方式，需确保 Markdown 内容可信，以防 XSS 攻击。`marked` 默认输出是安全的（对原生 HTML 进行转义），但如果你在其选项中允许了 HTML 输出，则需要谨慎。

3. **使用组件**：在你的其他组件中使用这个 `MarkdownRenderer` 组件。

   ```jsx
   // App.jsx
   import React from "react";

   import MarkdownReader from "./components/markdownRenderer";
   import text from "./assets/React.md?raw";

   function App() {
     return (
       <div className="App">
         <MarkdownReader content={text} />
       </div>
     );
   }

   export default App;
   ```

#### Highlight.js 主题选择

**官方主题展示库 (最推荐)**：访问 Highlight.js 的官方主题展示页面，这里可以**直观地看到所有主题**应用到不同语言代码上的实际效果：https://highlightjs.org/static/demo/。

## 方法二

### 使用 `vite-plugin-markdown` 插件（功能丰富）

`vite-plugin-markdown` 这类插件可以帮你更高效地处理 Markdown 文件，它们通常能直接返回解析后的 HTML 字符串、甚至 React 组件，还可能支持 Front Matter（元数据）提取等高级功能[7](https://www.yisu.com/jc/607692.html)。

#### 具体步骤：

1. **安装插件**：

   ```bash
   npm install vite-plugin-markdown
   ```

2. **配置 `vite.config.js`**：

   ```js
   // vite.config.js
   import { defineConfig } from "vite";
   import react from "@vitejs/plugin-react";
   // 注意导入方式，2.2.0 版本可能需要这样引入
   import { vitePluginMarkdown } from "vite-plugin-markdown"; // [!code focus]

   export default defineConfig({
     plugins: [
       react(),
       vitePluginMarkdown({
         mode: ["react"],
       }),
     ],
   });
   ```

   - `mode` 选项可以是数组，支持 `'html'`、`'toc'`（目录）、`'react'`、`'vue'` 等[7](https://www.yisu.com/jc/607692.html)。

3. **创建和使用 Markdown 文件作为 React 组件**：

   - 假设你有一个 `example.md` 文件：

     ```

     ```

   ***

   title: 示例文档

   ***

   # 你好，Markdown！

   这是一段**加粗**的文字。

   ````js
   console.log('Hello, World!');
   \```
   ````

   - 在你的 React 组件中直接导入 `example.md`，它已经是一个 React 组件了[7](https://www.yisu.com/jc/607692.html)：

     jsx

     ```jsx
     // App.jsx
     import React from "react";
     // 从具名导出中引入 ReactComponent
     import { ReactComponent } from "./assets/example.md";

     function App() {
       return (
         <div className="App">
           <ReactComponent />
         </div>
       );
     }

     export default App;
     ```

   - 插件通常也会提供其他导入方式，例如获取元数据 (`attributes`)、原始 HTML (`html`) 等[7](https://www.yisu.com/jc/607692.html)：

     jsx

     ```jsx
     import { attributes, html } from "./path/to/example.md";

     console.log(attributes); // { title: "示例文档" }
     console.log(html); // 包含解析后 HTML 的字符串
     ```

   插件内部通常会帮你处理好 Markdown 到 HTML 的转换以及语法高亮等，你可能只需要引入其提供的 CSS 文件即可。

## 方法三(推荐)

### 使用 react-markdown 替代插件

### 基本使用

1. **安装**
   在你的 React 项目目录下，通过 npm 或 yarn 安装 `react-markdown`：

   ```bash
   npm install react-markdown
   # 或
   yarn add react-markdown
   ```

2. **基本用法**
   安装完成后，你就可以在组件中引入并使用它了：

   ```jsx
   import React from "react";
   import ReactMarkdown from "react-markdown";

   const markdownContent = `
   # 你好，React Markdown！
   
   这是一段 **加粗** 的文字和一个 [链接](https://reactjs.org/)。
   `;

   function App() {
     return (
       <div>
         <ReactMarkdown>{markdownContent}</ReactMarkdown>
       </div>
     );
   }

   export default App;
   ```

   在上面的例子中，我们将 Markdown 字符串直接作为子组件（`children`）传递给 `ReactMarkdown` 组件，它就会自动将其渲染为对应的 HTML 元素[1](https://calpa.me/blog/react-markdown-render-markdown-as-component/)。

### 处理本地 Markdown 文件

通常，你的 Markdown 内容可能保存在一个 `.md` 文件中。在 Vite 构建的 React 项目中，你可以使用 `?raw` 查询参数来导入文件的原始内容：

```jsx
import React from "react";
import ReactMarkdown from "react-markdown";
import rawMD from "./assets/your-document.md?raw"; // 使用 ?raw 导入原始字符串

function App() {
  return (
    <div>
      <ReactMarkdown>{rawMD}</ReactMarkdown>
    </div>
  );
}

export default App;
```

### 增强功能（插件与代码高亮）

`react-markdown` 的强大之处在于它的插件生态系统，可以让你支持更多的 Markdown 语法（如表格、任务列表等）并为代码块添加语法高亮。

1. **安装常用插件**

   ```bash
   npm install remark-gfm rehype-highlight
   # 或
   yarn add remark-gfm rehype-highlight
   ```

   - `remark-gfm`: 支持 **GitHub Flavored Markdown** (GFM)，例如表格、删除线、任务列表和自动链接[1](https://calpa.me/blog/react-markdown-render-markdown-as-component/)[7](https://www.uudwc.com/A/RxPye)。
   - `rehype-highlight`: 用于**代码块的语法高亮**[1](https://calpa.me/blog/react-markdown-render-markdown-as-component/)。

2. **使用插件**
   安装后，将它们作为 `remarkPlugins` 和 `rehypePlugins` 属性传递给 `ReactMarkdown` 组件：

   ```jsx
   import React from "react";
   import ReactMarkdown from "react-markdown";
   import remarkGfm from "remark-gfm";
   import rehypeHighlight from "rehype-highlight";
   import "highlight.js/styles/github-dark.css"; // 引入一个代码高亮样式
   import rawMD from "./assets/your-document.md?raw";

   function App() {
     return (
       <div>
         <ReactMarkdown
           remarkPlugins={[remarkGfm]}
           rehypePlugins={[rehypeHighlight]}
         >
           {rawMD}
         </ReactMarkdown>
       </div>
     );
   }

   export default App;
   ```

对于样式，可以直接引入现成的 CSS，比如 `github-markdown-css`[7](https://www.uudwc.com/A/RxPye)：

```bash
npm install github-markdown-css
# 或
yarn add github-markdown-css
```

然后在组件中引入：

```jsx
import "github-markdown-css";
// 然后在包裹 ReactMarkdown 的容器元素上添加 className="markdown-body"
<div className="markdown-body">
  <ReactMarkdown>{rawMD}</ReactMarkdown>
</div>;
```

### 安全考虑

默认情况下，`react-markdown` 是安全的，它会**忽略 Markdown 中的原生 HTML 标签**，这可以有效防止 XSS 攻击[1](https://calpa.me/blog/react-markdown-render-markdown-as-component/)[2](https://www.jb51.net/javascript/338937vsh.htm)。如果你确实需要渲染原始 HTML，可以使用 `rehype-raw` 插件[2](https://www.jb51.net/javascript/338937vsh.htm)[7](https://www.uudwc.com/A/RxPye)，但请务必谨慎使用，并确保内容来源可信。

```bash
npm install rehype-raw
# 或
yarn add rehype-raw
```

```jsx
import React from "react";
import ReactMarkdown from "react-markdown";
import rehypeRaw from "rehype-raw";
import rawMD from "./assets/your-document.md?raw"; // 假设此 MD 文件包含 HTML

function App() {
  return (
    <div>
      <ReactMarkdown rehypePlugins={[rehypeRaw]}>{rawMD}</ReactMarkdown>
    </div>
  );
}

export default App;
```

### 最终成果

```jsx
import React from "react";
import ReactMarkdown from "react-markdown";
import remarkGfm from "remark-gfm";
import rehypeHighlight from "rehype-highlight";
import "highlight.js/styles/github.css"; // 引入一个代码高亮样式
import rawMD from "./assets/React.md?raw";
import "github-markdown-css";

function App() {
  return (
    <div className="markdown-body">
      <ReactMarkdown
        remarkPlugins={[remarkGfm]}
        rehypePlugins={[rehypeHighlight]}
      >
        {rawMD}
      </ReactMarkdown>
    </div>
  );
}

export default App;
```

### 常用插件与用途

| 插件名称           | 用途描述                                                                                                                                                           | 安装命令                       |
| ------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------- |
| `remark-gfm`       | 支持 GitHub 风格的 Markdown（如表格、任务列表、删除线等）[1](https://calpa.me/blog/react-markdown-render-markdown-as-component/)[7](https://www.uudwc.com/A/RxPye) | `npm install remark-gfm`       |
| `rehype-highlight` | 为代码块添加语法高亮（通常配合 `highlight.js` 的样式）[1](https://calpa.me/blog/react-markdown-render-markdown-as-component/)                                      | `npm install rehype-highlight` |
| `rehype-raw`       | 允许解析 Markdown 中的原生 HTML 标签[2](https://www.jb51.net/javascript/338937vsh.htm)[7](https://www.uudwc.com/A/RxPye)                                           | `npm install rehype-raw`       |
| `rehype-katex`     | 支持渲染数学公式（LaTeX）[3](https://avoid.overfit.cn/post/be191aff038f4b59a54395b4457a967a)                                                                       | `npm install rehype-katex`     |
| `remark-math`      | 与 `rehype-katex` 搭配使用，用于处理数学公式[3](https://avoid.overfit.cn/post/be191aff038f4b59a54395b4457a967a)                                                    | `npm install remark-math`      |

## 目录渲染

```bash
npm install react-toc
```

```jsx
import React from "react";
import ReactMarkdown from "react-markdown";
import remarkGfm from "remark-gfm";
import rehypeHighlight from "rehype-highlight";
import "highlight.js/styles/github.css"; // 引入一个代码高亮样式
import rawMD from "./assets/React.md?raw";
import "github-markdown-css";
import TOC from "react-toc"; // 引入 TOC 组件

function App() {
  return (
    <div className="markdown-body" style={{ display: "flex" }}>
      <aside style={{ width: "250px", flexShrink: 0 }}>
        {/* TOC 组件会自动查找正文中的标题并生成目录 */}
        <TOC
          markdownText={rawMD}
          title="文章目录" // 自定义目录标题
        />
      </aside>
      <main style={{ flexGrow: 1 }}>
        <ReactMarkdown
          remarkPlugins={[remarkGfm]}
          rehypePlugins={[rehypeHighlight]}
        >
          {rawMD}
        </ReactMarkdown>
      </main>
    </div>
  );
}

export default App;
```

## 最终成品

````jsx
//App.jsx
import React, { useState, useEffect, useRef, useCallback } from "react";
import ReactMarkdown from "react-markdown";
import remarkGfm from "remark-gfm";
import rehypeHighlight from "rehype-highlight";
import "highlight.js/styles/github.css";
import rawMD from "./assets/React.md?raw";
import "github-markdown-css";
import "./App.css";

// 自定义目录组件
const TocComponent = ({ headings, activeId, onItemClick }) => {
  return (
    <div className="toc-container">
      <h3 className="toc-title">文章目录</h3>
      <div className="toc-content">
        {headings.map((heading, index) => (
          <div
            key={index}
            className={`toc-item toc-level-${heading.level} ${
              activeId === heading.id ? "active" : ""
            }`}
            onClick={() => onItemClick(heading.id)}
          >
            {heading.text}
          </div>
        ))}
      </div>
    </div>
  );
};

// 生成唯一ID - 处理特殊字符
const generateUniqueId = (text, index) => {
  if (typeof text !== "string") {
    if (Array.isArray(text)) {
      text = text.join("");
    } else {
      text = String(text);
    }
  }

  // 移除Markdown代码标记
  text = text.replace(/`/g, "");

  const baseId = text
    .toLowerCase()
    .replace(/\s+/g, "-")
    .replace(/[^\w-]+/g, "")
    .replace(/--+/g, "-")
    .replace(/^-+/, "")
    .replace(/-+$/, "");

  // 添加索引确保唯一性
  return baseId ? `${baseId}-${index}` : `heading-${index}`;
};

// 从Markdown文本中提取标题（排除代码块中的内容）
const extractHeadingsFromMarkdown = (markdownText) => {
  const headings = [];
  let inCodeBlock = false;
  let index = 0;

  // 按行分割Markdown文本
  const lines = markdownText.split("\n");

  for (let i = 0; i < lines.length; i++) {
    const line = lines[i].trim();

    // 检测代码块开始和结束
    if (line.startsWith("```")) {
      inCodeBlock = !inCodeBlock;
      continue;
    }

    // 如果不在代码块中，检查是否为标题
    if (!inCodeBlock && line.startsWith("#")) {
      // 匹配标题格式
      const match = line.match(/^(#+)\s+(.+)$/);
      if (match) {
        const level = match[1].length;
        let text = match[2].trim();

        // 排除特殊情况（如注释中的#号）
        if (level <= 6 && text.length > 0) {
          // 移除Markdown代码标记，但保留文本内容
          text = text.replace(/`/g, "");
          const id = generateUniqueId(text, index);
          headings.push({ level, text, id, index });
          index++;
        }
      }
    }
  }

  return headings;
};

// 防抖函数
const debounce = (func, wait) => {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
};

function App() {
  const [activeId, setActiveId] = useState("");
  const [headings, setHeadings] = useState([]);
  const contentRef = useRef(null);
  const isScrollingRef = useRef(false);
  const scrollEndTimerRef = useRef(null);
  const headingElementsRef = useRef(new Map());

  // 从Markdown文本中提取标题
  useEffect(() => {
    const foundHeadings = extractHeadingsFromMarkdown(rawMD);
    setHeadings(foundHeadings);
  }, []);

  // 处理ReactMarkdown渲染的标题
  const renderHeading = useCallback(
    (level, props) => {
      // 将children转换为纯文本
      const getTextContent = (children) => {
        if (typeof children === "string") {
          return children;
        } else if (Array.isArray(children)) {
          return children.map((child) => getTextContent(child)).join("");
        } else if (children && children.props) {
          return getTextContent(children.props.children);
        }
        return "";
      };

      const textContent = getTextContent(props.children);
      const cleanText = textContent.replace(/`/g, ""); // 移除反引号

      // 查找匹配的标题
      const heading = headings.find(
        (h) => h.text === cleanText || h.text.replace(/`/g, "") === cleanText
      );

      const id = heading ? heading.id : generateUniqueId(cleanText, 0);

      const HeadingTag = `h${level}`;
      return React.createElement(HeadingTag, { id, ...props });
    },
    [headings]
  );

  // 平滑滚动到指定锚点
  const scrollToAnchor = useCallback((id) => {
    // 设置滚动标记
    isScrollingRef.current = true;

    const element = document.getElementById(id);
    if (element) {
      // 考虑可能存在的固定导航栏遮挡问题，添加偏移量
      const offset = 80;
      const elementPosition =
        element.getBoundingClientRect().top + window.pageYOffset;
      const offsetPosition = elementPosition - offset;

      window.scrollTo({
        top: offsetPosition,
        behavior: "smooth",
      });

      setActiveId(id);

      // 清除之前的滚动结束计时器
      if (scrollEndTimerRef.current) {
        clearTimeout(scrollEndTimerRef.current);
      }

      // 监听滚动结束
      const handleScrollEnd = () => {
        isScrollingRef.current = false;
        window.removeEventListener("scroll", scrollEndHandler);
      };

      // 创建防抖的滚动结束处理器
      const scrollEndHandler = debounce(handleScrollEnd, 100);
      window.addEventListener("scroll", scrollEndHandler);

      // 设置超时作为备用，确保滚动标记最终会被清除
      scrollEndTimerRef.current = setTimeout(() => {
        isScrollingRef.current = false;
        window.removeEventListener("scroll", scrollEndHandler);
      }, 1500);
    } else {
      // 如果找不到元素，可能是ID不匹配，尝试查找近似ID
      console.warn(
        `Element with id ${id} not found, trying to find similar...`
      );
      const allHeadings = document.querySelectorAll("h1, h2, h3, h4, h5, h6");
      let foundElement = null;

      for (let i = 0; i < allHeadings.length; i++) {
        const heading = allHeadings[i];
        if (heading.id && heading.id.includes(id.replace(/\d+$/, ""))) {
          foundElement = heading;
          break;
        }
      }

      if (foundElement) {
        const offset = 80;
        const elementPosition =
          foundElement.getBoundingClientRect().top + window.pageYOffset;
        const offsetPosition = elementPosition - offset;

        window.scrollTo({
          top: offsetPosition,
          behavior: "smooth",
        });

        setActiveId(foundElement.id);
      }
    }
  }, []);

  // 监听滚动，自动高亮当前章节
  useEffect(() => {
    // 获取所有标题元素
    const updateHeadingElements = () => {
      headingElementsRef.current.clear();
      headings.forEach((heading) => {
        const element = document.getElementById(heading.id);
        if (element) {
          headingElementsRef.current.set(heading.id, element);
        }
      });
    };

    // 初始更新
    updateHeadingElements();

    // 延迟再次更新，确保DOM已渲染
    setTimeout(updateHeadingElements, 300);

    const handleScroll = debounce(() => {
      // 如果正在手动滚动，则不处理自动高亮
      if (isScrollingRef.current) return;

      const scrollPosition = window.scrollY + 100; // 添加偏移量，使标题在接近顶部时触发
      let closestHeading = null;
      let minDistance = Infinity;

      // 遍历所有标题元素，找到最接近视口顶部的标题
      headingElementsRef.current.forEach((element, id) => {
        const rect = element.getBoundingClientRect();
        const elementTop = rect.top + window.pageYOffset;
        const distance = Math.abs(elementTop - scrollPosition);

        if (distance < minDistance) {
          minDistance = distance;
          closestHeading = id;
        }
      });

      if (closestHeading && closestHeading !== activeId) {
        setActiveId(closestHeading);
      }
    }, 50);

    window.addEventListener("scroll", handleScroll);

    return () => {
      window.removeEventListener("scroll", handleScroll);
    };
  }, [headings, activeId]);

  return (
    <div className="markdown-body app-container">
      <aside className="sidebar">
        <TocComponent
          headings={headings}
          activeId={activeId}
          onItemClick={scrollToAnchor}
        />
      </aside>
      <main ref={contentRef} className="content">
        <ReactMarkdown
          remarkPlugins={[remarkGfm]}
          rehypePlugins={[rehypeHighlight]}
          components={{
            // 自定义标题渲染，确保有id属性
            h1: (props) => renderHeading(1, props),
            h2: (props) => renderHeading(2, props),
            h3: (props) => renderHeading(3, props),
            h4: (props) => renderHeading(4, props),
            h5: (props) => renderHeading(5, props),
            h6: (props) => renderHeading(6, props),
          }}
        >
          {rawMD}
        </ReactMarkdown>
      </main>
    </div>
  );
}

export default App;
````

```css
/* App.css */
.app-container {
  display: flex;
  min-height: 100vh;
}

.sidebar {
  width: 280px;
  flex-shrink: 0;
  position: sticky;
  top: 20px;
  align-self: flex-start;
  max-height: calc(100vh - 40px);
  overflow-y: auto;
  padding: 20px;
  border-right: 1px solid #eaecef;
  background-color: #f6f8fa;
  transition: all 0.3s ease;
}

.content {
  flex-grow: 1;
  padding: 20px 40px;
  max-width: calc(100% - 280px);
  box-sizing: border-box;
}

/* 目录样式 */
.toc-container {
  padding: 10px 0;
}

.toc-title {
  margin-top: 0;
  margin-bottom: 15px;
  font-size: 18px;
  font-weight: 600;
  color: #24292e;
  padding-bottom: 10px;
  border-bottom: 1px solid #e1e4e8;
}

.toc-content {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.toc-item {
  padding: 6px 12px;
  cursor: pointer;
  border-radius: 3px;
  transition: all 0.2s ease;
  color: #586069;
  font-size: 14px;
  line-height: 1.4;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.toc-item:hover {
  color: #0366d6;
  background-color: rgba(3, 102, 214, 0.05);
}

.toc-item.active {
  color: #0366d6;
  font-weight: 600;
  background-color: rgba(3, 102, 214, 0.1);
  box-shadow: 0 0 0 2px rgba(3, 102, 214, 0.2);
  transform: translateX(5px);
}

.toc-level-1 {
  margin-left: 0;
  font-size: 15px;
  font-weight: 600;
}

.toc-level-2 {
  margin-left: 16px;
  font-size: 14px;
}

.toc-level-3 {
  margin-left: 32px;
  font-size: 13px;
}

.toc-level-4 {
  margin-left: 48px;
  font-size: 13px;
}

.toc-level-5 {
  margin-left: 64px;
  font-size: 12px;
}

.toc-level-6 {
  margin-left: 80px;
  font-size: 12px;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .app-container {
    flex-direction: column;
  }

  .sidebar {
    width: 100%;
    position: relative;
    max-height: none;
    border-right: none;
    border-bottom: 1px solid #eaecef;
  }

  .content {
    max-width: 100%;
    padding: 20px;
  }
}

/* 平滑滚动和标题偏移 */
html {
  scroll-behavior: smooth;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  scroll-margin-top: 100px;
}

/* Markdown内容样式调整 */
.markdown-body {
  box-sizing: border-box;
}

.markdown-body h1,
.markdown-body h2,
.markdown-body h3,
.markdown-body h4,
.markdown-body h5,
.markdown-body h6 {
  margin-top: 24px;
  margin-bottom: 16px;
  font-weight: 600;
  line-height: 1.25;
  position: relative;
}

.markdown-body h1::before,
.markdown-body h2::before,
.markdown-body h3::before,
.markdown-body h4::before,
.markdown-body h5::before,
.markdown-body h6::before {
  content: "";
  display: block;
  height: 100px;
  margin-top: -100px;
  visibility: hidden;
  pointer-events: none;
}

.markdown-body h1 {
  padding-bottom: 0.3em;
  font-size: 2em;
  border-bottom: 1px solid #eaecef;
}

.markdown-body h2 {
  padding-bottom: 0.3em;
  font-size: 1.5em;
  border-bottom: 1px solid #eaecef;
}

.markdown-body h3 {
  font-size: 1.25em;
}

.markdown-body h4 {
  font-size: 1em;
}

.markdown-body h5 {
  font-size: 0.875em;
}

.markdown-body h6 {
  font-size: 0.85em;
  color: #6a737d;
}

/* 优化滚动性能 */
.content {
  transform: translateZ(0);
  will-change: transform;
}

/* 添加平滑过渡效果 */
.toc-item {
  transition: all 0.3s ease;
}

.sidebar {
  transition: all 0.3s ease;
}
```

## 安装插件

```bash
npm install react-markdown

npm install remark-gfm

npm install rehype-highlight

npm install github-markdown-css

```

```bash
npm install react-markdown remark-gfm rehype-highlight github-markdown-css
```
