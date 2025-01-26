

## 常见前缀

### `feat`(Feature) 新功能

*   含义：表示新增功能（feature）的提交。
*   示例：`feat: add night mode to the application`
*   描述：用于描述新功能的添加，比如增加了一个新的用户界面特性或后端服务功能。


### `fix`(Fix) 修复 bug

*   含义：表示修复错误（bug fix）的提交。
*   示例：`fix: resolve null pointer exception in user login`
*   描述：用于修复代码中的错误或问题，确保程序的行为符合预期。


### `style`(Style) 代码风格变动，不影响代码逻辑

*   含义：表示代码样式修改的提交，不影响程序逻辑。
*   示例：`style: update code formatting according to ESLint rules`
*   描述：用于改进代码风格，例如调整缩进、空格、换行等，通常不会改变代码的功能。


### `revert:` 回滚

*   含义：用于提交回滚之前的提交。
*   示例：`revert: 回滚feat: 增加用户注册功能。`
*   描述：用于直接明确回滚的代码，包含了那些历史功能模块。


### `build:` 构建系统或外部依赖项的变更

*   含义：用于提交影响构建系统的更改。
*   示例：`build: 升级webpack到版本5。`
*   描述：用于打包构建项目的描述，或者更改了相关依赖等。


## 其他常见前缀

### `refactor` (Refactor) 代码重构，即不改变代码功能的情况下优化代码结构。
    
*   含义：表示代码重构，即不改变代码功能的情况下优化代码结构。
*   示例：`refactor: simplify complex function logic`


### `docs` (Documentation) 文档变更

*   含义：表示文档更新，如 README 文件、API 文档等。
*   示例：`docs: update installation guide for v2.0`


### `test` (Test) 修改或添加测试

*   含义：表示测试相关的更改，如添加或修改测试用例。
*   示例：`test: add unit tests for new feature`

### `chore` (Chore) 杂项（构建过程或辅助工具的变动）

*   含义：表示维护任务，如更新依赖项、配置文件等。
*   示例：`chore: update package.json dependencies`


### `perf` (Performance) 性能优化

*   含义：表示性能优化的提交。
*   示例：`perf: optimize database query performance`


### `ci` (Continuous Integration) 持续继承的变更

*   含义：表示持续集成相关的更改，如 CI/CD 配置文件的更新。
*   示例：`ci: configure GitHub Actions for automated testing`
