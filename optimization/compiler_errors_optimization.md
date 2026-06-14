# 博客编译器警告与构建报错优化计划

本篇文档记录了对博客在本地编译、GitHub Actions (CI) 构建以及本地 IDE 编辑器校验中发现的警告与隐患的排查计划，并给出了具体的优化步骤。

---

## 1. 消除 Ruby 3.4/3.5 下的 Logger 弃用警告

### 问题分析
在本地执行 `bundle exec jekyll serve` 时，Ruby 终端频繁抛出：
```text
warning: logger was loaded from the standard library, but will no longer be part of the default gems starting from Ruby 3.5.0.
```
在 Ruby 3.4+ 中，`logger` 被移出了默认核心库。到 Ruby 3.5 发布时，如果不做显式声明，警告将升级为 `LoadError`（找不到该库的致命报错）。

### 优化方案
在 `Gemfile` 中显式声明依赖，让 Bundler 自动拉取最新的 `logger` Gem 并注入编译环境：
1. 在 [Gemfile](file:///Users/xuanbobby/Documents/Git/BobbyBBY.github.io/Gemfile) 底部追加：
   ```ruby
   gem "logger"
   ```
2. 运行环境配置命令：
   ```bash
   bundle install
   ```

---

## 2. 升级 GitHub Actions 自动化编译流以消除版本冲突

### 问题分析
目前 [.github/workflows/ci.yml](file:///Users/xuanbobby/Documents/Git/BobbyBBY.github.io/.github/workflows/ci.yml#L11) 中的 Docker 镜像版本硬编码为 `JEKYLL_VERSION=3.8`，而用户本地编译运行在最新的 `Jekyll 4.4.1`。
- **配置选项废弃风险**：Jekyll 3.x 使用 `gems` 配置插件，而 4.x 废弃并统一为 `plugins`。版本不一致极易导致 CI 报错并阻断 GitHub Pages 的构建。
- **Docker 容器权限冲突**：在 `ci.yml` 中运行的 `chmod 777` 在某些 Runner 权限规则下可能被安全策略阻断，导致工作流直接中断。

### 优化方案
更新 CI 执行规则，确保本地编译环境与云端 Runner 100% 对齐：
1. 将 `ci.yml` 中的环境变量 `JEKYLL_VERSION=3.8` 升级为 `4.4.1`。
2. 优化 `docker run` 参数或改用 GitHub Pages 官方推荐的免 Docker 的原生 Action：
   ```yaml
   - name: Upload artifact
     uses: actions/upload-pages-artifact@v1
     with:
       path: ./_site
   ```

---

## 3. 解决本地编辑器大面积 Liquid 误报红线问题

### 问题分析
由于博客在 `.html`（如 `tags.html`）、`.json`（如 `cb-search.json`）等文件中高度内嵌了 Jekyll 的 Liquid 循环和条件控制语句（`{% for %}`、`{% if %}`）以及 YAML 头部的 Front Matter（`---`），本地编辑器的 HTML/JSON 编译器由于没有对应的模板引擎解析插件，会将其误判为格式语法错误而标红。

### 优化方案
不破坏文件代码，通过编辑器环境增强来解除编辑器误报：
1. **VS Code 环境**：
   - 在应用商店搜索并安装插件：`Liquid Languages Support` 或 `Jekyll Snippets`。
   - 安装后，编辑器将正确识别 Liquid 语法高亮，并消除红线。
2. **JSON 误报修复**：
   - 临时在编辑器设置中将 `cb-search.json` 关联的文件类型指定为 `Liquid` 而不是标准的 `JSON`，从而避免语法错误。

---

## 4. 优化 macOS ARM64 架构下 Gem Native Extension 编译环境

### 问题分析
在 macOS 搭载 M 系列芯片的电脑上，在安装某些依赖 C 扩展的 Gem 库（如 `sass-c`、`eventmachine` 等）时，常常因为缺少系统库路径或 CPU 架构没有完美匹配，导致 Bundler 抛出原生编译器构建失败（`Failed to build gem native extension`）。

### 优化方案
为本地的 Ruby/Gem 编译器明确指定系统库的绝对寻址路径（基于 Homebrew）：
1. 确保安装了 Xcode 命令行工具：
   ```bash
   xcode-select --install
   ```
2. 通过 Bundler 为编译失败的 Gem 指定 clang/gcc 构建路径变量（以 `sass-c` 为例）：
   ```bash
   bundle config build.sass-c --with-cflags="-Wno-error=implicit-function-declaration"
   ```
3. 确保安装并链接了 openssl 等基础 C 库，以防 `eventmachine` 编译失败：
   ```bash
   brew install openssl
   bundle config build.eventmachine --with-cppflags=-I/opt/homebrew/opt/openssl/include --with-ldflags=-L/opt/homebrew/opt/openssl/lib
   ```
