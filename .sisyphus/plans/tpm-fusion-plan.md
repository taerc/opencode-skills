# tpm 技能融合计划

## TL;DR

> **目标**：将 pm 和 po 深度融合为单一技能 tpm（技术产品经理）
> **命名**：tpm
> **风格**：专业严谨
> **po处理**：复制到 workspace 合并，用户目录标记弃用

---

## Context

### 当前状态
- `pm` 在 workspace: `D:\wkspace\git\opencode-skill\.claude\skills\pm`
- `po` 在用户目录: `C:\Users\rotae\.claude\skills\po`
- 决策：复制 po 到 workspace，与 pm 合并为 tpm

### 融合决策
- 融合方案：深度融合
- 角色风格：专业严谨
- 技能命名：tpm
- DBA 能力：完整保留
- L1-L5 评估：删除

---

## TODOs

### Wave 1: 目录重命名与复制

- [ ] 1. 重命名目录 pm → tpm

  **What to do**:
  - 使用 mv 重命名目录
  - 验证目录结构完整
  
  **Acceptance Criteria**:
  - [ ] 目录已重命名为 tpm
  - [ ] 原内容完整保留

  **Parallelization**: Wave 1
  **Recommended Agent**: quick

- [ ] 2. 更新 SKILL.md 的 name 字段

  **What to do**:
  - 将 name: tech-pm-architect 改为 name: tpm
  - 更新标题从 Tech PM Architect 改为 TPM
  
  **Acceptance Criteria**:
  - [ ] name 字段为 tpm
  - [ ] 标题已更新

  **Parallelization**: Wave 1 (with Task 1)
  **Recommended Agent**: quick

- [ ] 3. 复制 po 的模板文件到 tpm

  **What to do**:
  - 从 C:\Users\rotae\.claude\skills\po\templates\ 复制所有模板
  - 目标: D:\wkspace\git\opencode-skill\.claude\skills\tpm\templates\
  - 文件列表:
    - user-story-template.md
    - requirement-template.md
    - product-spec-template.md
    - feature-design-template.md
    - dba-design-template.md
    - changelog-template.md
    - sg-naming-codes.md
  
  **Acceptance Criteria**:
  - [ ] 所有模板文件已复制
  - [ ] 文件内容完整

  **Parallelization**: Wave 1
  **Recommended Agent**: quick

---

### Wave 2: SKILL.md 内容整合

- [ ] 4. 整合角色定位

  **What to do**:
  - 合并 pm 的"资深技术产品经理"定位
  - 融入 po 的数据库架构师能力
  - 统一为专业严谨风格
  - 参考原文:
    - pm: 第6-15行
    - po: 第6-21行
  
  **Acceptance Criteria**:
  - [ ] 角色定位包含技术+数据库双能力
  - [ ] 风格统一为专业严谨

  **Parallelization**: Wave 2
  **Recommended Agent**: unspecified-high

- [ ] 5. 整合方法论

  **What to do**:
  - 保留"层层逼问"作为主方法论（pm 第188-291行）
  - 添加"五维度分析"作为验证方法（po 第979-1009行）
  - 明确两者的使用场景
  - 删除重复的追问策略（保留统一的版本）
  
  **Acceptance Criteria**:
  - [ ] 主方法论：层层逼问
  - [ ] 验证方法：五维度分析
  - [ ] 使用场景清晰
  - [ ] 无重复内容

  **Parallelization**: Wave 2 (with Task 4)
  **Recommended Agent**: unspecified-high

- [ ] 6. 删除 L1-L5 评估等级内容

  **What to do**:
  - 删除六大能力的 L1-L5 详细等级表格
  - 保留能力描述（每项核心技能、行为指标）
  - 位置: pm SKILL.md 第35-186行
  
  **Acceptance Criteria**:
  - [ ] L1-L5 等级表格已删除
  - [ ] 能力描述保留完整

  **Parallelization**: Wave 2 (with Task 4)
  **Recommended Agent**: quick

---

### Wave 3: 工作模式整合

- [ ] 7. 整合九种工作模式

  **What to do**:
  - 需求分析模式（合并两者的流程）
    - pm: 第293-325行
    - po: 第366-402行
  - 用户故事模式（来自po 第296-332行）
  - 禅道需求模式（来自po 第336-402行）
  - 产品规格模式（来自po 第405-460行）
  - 技术评审模式（来自pm 第327-358行）
  - DBA设计模式（来自po 第500-924行）
  - 功能设计模式（来自po 第463-499行）
  - 可行性分析模式（来自pm 第360-389行）
  - 表名规范检查模式（来自po 第108-247行）
  
  **Acceptance Criteria**:
  - [ ] 九种模式全部整合
  - [ ] 无重复流程
  - [ ] 模式间边界清晰

  **Parallelization**: Wave 3
  **Recommended Agent**: deep

---

### Wave 4: 参考资料整理

- [ ] 8. 创建 references 目录并整理内容

  **What to do**:
  - 保留 pm 现有的 references/ 目录内容
  - 创建 ai-capability-boundary.md（从 SKILL.md 提取 AI 能力边界清单）
  - 创建 decision-frameworks.md（从 SKILL.md 提取决策框架）
  - 创建 red-flags.md（从 SKILL.md 提取红旗识别）
  - 移动 sg-naming-codes.md 到 references/
  
  **Acceptance Criteria**:
  - [ ] references 目录结构完整
  - [ ] 内容从 SKILL.md 提取并独立

  **Parallelization**: Wave 4
  **Recommended Agent**: unspecified-high

---

### Wave 5: 协作关系与弃用标记

- [ ] 9. 更新协作关系说明

  **What to do**:
  - 在 tpm/SKILL.md 添加与 product-spec-builder 的边界说明
  - 删除原 po 中的 pm 协作说明（已融合）
  - 添加 tpm 自身的完整能力说明
  
  **Acceptance Criteria**:
  - [ ] 与 product-spec-builder 边界清晰
  - [ ] 无冗余的协作说明

  **Parallelization**: Wave 5
  **Recommended Agent**: unspecified-high

- [ ] 10. 更新用户目录的 po/SKILL.md 添加弃用警告

  **What to do**:
  - 文件: C:\Users\rotae\.claude\skills\po\SKILL.md
  - 在 description 开头添加 [已弃用]
  - 添加说明引导用户使用 workspace 中的 tpm
  - 保留功能完整但标注不再维护
  
  **Acceptance Criteria**:
  - [ ] description 含 [已弃用]
  - [ ] 包含引导使用 tpm 的说明

  **Parallelization**: Wave 5 (with Task 9)
  **Recommended Agent**: quick

---

### Wave FINAL: 验证

- [ ] 11. 最终验证

  **What to do**:
  - 验证 tpm 技能目录结构完整
  - 验证九种工作模式描述完整
  - 验证模板文件路径正确
  - 验证 po 显示弃用警告
  - 检查无重复内容
  
  **Acceptance Criteria**:
  - [ ] tpm 目录结构完整
  - [ ] 九种模式完整
  - [ ] 模板可加载
  - [ ] po 显示弃用

  **Parallelization**: Wave FINAL
  **Recommended Agent**: oracle

---

## 文件变更清单

### workspace (D:\wkspace\git\opencode-skill\.claude\skills\)

| 操作 | 文件路径 | 说明 |
|------|---------|------|
| 重命名 | pm/ → tpm/ | 目录重命名 |
| 修改 | tpm/SKILL.md | name、角色、方法论、模式整合 |
| 删除 | tpm/SKILL.md 中的 L1-L5 内容 | 能力评估等级 |
| 创建 | tpm/templates/user-story-template.md | 来自 po |
| 创建 | tpm/templates/requirement-template.md | 来自 po |
| 创建 | tpm/templates/product-spec-template.md | 来自 po |
| 创建 | tpm/templates/feature-design-template.md | 来自 po |
| 创建 | tpm/templates/dba-design-template.md | 来自 po |
| 创建 | tpm/templates/changelog-template.md | 来自 po |
| 移动 | tpm/templates/sg-naming-codes.md → references/ | 命名规范 |
| 创建 | tpm/references/ai-capability-boundary.md | 从 SKILL.md 提取 |
| 创建 | tpm/references/decision-frameworks.md | 从 SKILL.md 提取 |
| 创建 | tpm/references/red-flags.md | 从 SKILL.md 提取 |

### 用户目录 (C:\Users\rotae\.claude\skills\po\)

| 操作 | 文件路径 | 说明 |
|------|---------|------|
| 修改 | po/SKILL.md | 添加弃用警告 |

---

## Commit Strategy

- **Commit 1**: 重命名 pm → tpm + 复制模板
- **Commit 2**: 整合角色、方法论、删除 L1-L5
- **Commit 3**: 整合九种工作模式
- **Commit 4**: 整理 references + 更新协作说明
- **Commit 5**: 标记用户目录 po 为弃用（需手动确认）

---

## Success Criteria

- [ ] tpm 技能包含完整的技术产品经理能力
- [ ] tpm 技能包含完整的数据库设计能力
- [ ] 九种工作模式完整可用
- [ ] 模板文件可正确加载
- [ ] 无重复内容
- [ ] po 显示弃用警告
