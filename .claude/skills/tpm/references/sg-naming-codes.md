# 国家电网数据中台命名规范编码对照表

## 文档信息

| 项目 | 内容 |
|-----|------|
| 版本 | 1.0.0 |
| 创建日期 | 2026-03-20 |
| 作者 | 笑江湖 |
| 适用范围 | 国网数据中台数据表命名 |

---

## 1. 分层命名规则

### 1.1 贴源层 (ODS)

#### 1.1.1 全量表命名规则

**格式**: `ODS_{业务系统英文简称}_{源端实例名}_{源端用户名}_{源系统表名}`

**说明**:
- 从源系统全量同步的数据表
- 不包含增量标识

**示例**:
```
ODS_FCM_CD_CWGKAVPD_FMIS9999_ORGRELATIONS
ODS_ERP_CD_P34C_ERP_USER_INFO
```

#### 1.1.2 增量表命名规则

**格式**: `ODS_{增量接入方式}_{业务系统英文简称}_{源端实例名}_{源端用户名}_{源系统表名}`

**增量接入方式说明**:
| 标识 | 说明 |
|-----|------|
| OGG | Oracle GoldenGate，实时增量 |
| CDC | Change Data Capture，变更数据捕获 |
| ETL | 定时ETL抽取 |
| API | 接口增量推送 |

**示例**:
```
ODS_OGG_FCM_CD_CWGKAVPD_FMIS9999_ORGRELATIONS
ODS_CDC_ERP_CD_P34C_ERP_USER_LOG
ODS_ETL_PMS_PMSDB_PMS_USER_ORDER
```

---

### 1.2 共享层 (DWD/UN)

#### 1.2.1 共享层标准表命名规则

**格式**: `UN{系统编号}_{部署方式}_{系统名}_{表名}`

**说明**:
- 系统编号: 见 2.5 系统编码对照表
- 部署方式: 见 2.6 部署方式编码对照表
- 系统名: 业务系统英文简称

**示例**:
```
UN03_03_FCM_CD_ORGRELATIONS
UN11_01_PMS_DEVICE_INFO
UN14_01_CMS_CUSTOMER_INFO
```

#### 1.2.2 共享层模型表命名规则

**格式**: `DWD_{数据域简称}_{表名}`

**说明**:
- 经过清洗、转换、标准化后的业务数据
- 按数据域组织

**示例**:
```
DWD_CST_CSTORG
DWD_FIN_FINANCE_ORDER
DWD_HR_EMPLOYEE_INFO
DWD_GRID_POWER_LINE
```

---

### 1.3 分析层

#### 1.3.1 维度表命名规则

**格式**: `DIM_{数据域简称}_{自定义表名}_{刷新周期编码}{分区增量编码}`

**说明**:
- DIM: Dimension，维度表
- 数据域简称: 见 2.1 数据域编码对照表
- 刷新周期: 见 2.2 刷新周期编码对照表
- 分区增量标识: 见 2.3 分区增量编码对照表

**示例**:
```
DIM_CST_CUST_DF          -- 客户维度表，日级全量
DIM_HR_DEPT_MF           -- 部门维度表，月级全量
DIM_GRID_STATION_YF      -- 变电站维度表，年级全量
DIM_MAT_MATERIAL_DI      -- 物料维度表，日级增量
```

#### 1.3.2 汇总宽表命名规则

**格式**: `DWS_{数据域简称}_{自定义表名}_{刷新周期编码}{分区增量编码}`

**说明**:
- DWS: Data Warehouse Summary，数据仓库汇总层
- 面向特定主题的聚合数据

**示例**:
```
DWS_CST_CUST_INDEX_DF    -- 客户指标汇总表，日级全量
DWS_FIN_REVENUE_MMF      -- 收入汇总表，月级全量
DWS_GRID_LOAD_HF         -- 电网负荷汇总表，小时级全量
```

#### 1.3.3 应用结果表命名规则

**格式**: `ADS_{数据域简称}_{项目名简称}_{自定义表名}_{刷新周期编码}{分区增量编码}`

**说明**:
- ADS: Application Data Store，应用数据层
- 面向特定应用场景的计算结果

**示例**:
```
ADS_CST_YX2_CUST_INDEX_DF     -- 营销2.0客户指标，日级全量
ADS_FIN_DW_REPORT_MMF         -- 财务报表数据，月级全量
ADS_GRID_SCADA_REALTIME_HF    -- SCADA实时数据，小时级全量
```

#### 1.3.4 事实明细表命名规则

**格式**: `FCT_{数据域简称}_{自定义表名}_{刷新周期编码}{分区增量编码}`

**说明**:
- FCT: Fact，事实表
- 存储业务事件明细数据

**示例**:
```
FCT_CST_E_DAY_READ_DI         -- 日电量读取明细，日级增量
FCT_FIN_PAYMENT_RECORD_DI     -- 支付记录明细，日级增量
FCT_AMR_METER_READ_DI         -- 抄表明细，日级增量
```

---

### 1.4 特殊表

#### 1.4.1 临时表命名规则

**格式**: `TEMP_{分层}_{表名}`

**说明**:
- 用于ETL过程或数据处理的临时表
- 应设置合理的生命周期，及时清理

**示例**:
```
TEMP_ODS_CSTORG
TEMP_DWD_FINANCE_CALC
TEMP_DWS_AGGREGATE_TMP
```

#### 1.4.2 自建表命名规则

**格式**: `SELF_{分层}_{表名}`

**说明**:
- 用户自行创建的表，非标准流程生成
- 需谨慎管理，避免与标准表冲突

**示例**:
```
SELF_ODS_CSTORG
SELF_DWD_CUSTOM_MAPPING
SELF_ADS_ANALYSIS_RESULT
```

#### 1.4.3 专项表命名规则

**格式**: `SPECIAL_{分层}_{表名}`

**说明**:
- 用于特定项目或专项分析的数据表
- 应记录项目归属和清理计划

**示例**:
```
SPECIAL_ODS_CSTORG
SPECIAL_DWS_AUDIT_DATA
SPECIAL_ADS_PROJECT_X_RESULT
```

---

### 1.5 交换区表

#### 1.5.1 下发表命名规则

**格式**: `DOWN_{目标系统编码}_{业务标识}_{日期标识}`

**说明**:
- 用于数据下发到下游系统
- 包含目标系统、业务类型、时间标识

**示例**:
```
DOWN_UN14_CUSTINFO_20260320
DOWN_UN11_DEVICE_20260320
DOWN_UN22_LINELOSS_202603
```

#### 1.5.2 上传表命名规则

**格式**: `UP_{源系统编码}_{业务标识}_{日期标识}`

**说明**:
- 用于上游系统上传的数据
- 需进行数据质量校验

**示例**:
```
UP_UN14_PAYMENT_20260320
UP_UN40_MATERIAL_202603
UP_UN05_SERVICE_20260320
```

---

### 1.6 城区/地市表命名规则

#### 1.6.1 城区表命名规则

**格式**: `{分层前缀}_{城区代码}_{业务表名}`

**说明**:
- 城区代码: 国网标准地区编码
- 分层前缀: ODS/DWD/DIM/DWS/ADS/FCT

**示例**:
```
ODS_1101_CUSTOMER_INFO       -- 北京市城区客户信息
DWD_3101_POWER_USAGE         -- 上海市城区用电数据
DWS_4401_REVENUE_DF          -- 广州市城区收入汇总
```

#### 1.6.2 地市表命名规则

**格式**: `{分层前缀}_{地市代码}_{业务表名}`

**说明**:
- 地市代码: 国网标准地市编码
- 用于地市特色业务数据存储

**示例**:
```
ODS_3701_JINAN_CUSTOMER      -- 济南市客户数据
DWD_4201_WUHAN_DEVICE        -- 武汉市设备数据
ADS_5101_CHENGDU_REPORT_DF   -- 成都市报表数据
```

---

## 2. 编码对照表

### 2.1 数据域编码

| 编码 | 英文全称 | 中文名称 | 说明 |
|-----|---------|---------|------|
| HR | Human Resource | 人力资源域 | 员工、组织、薪酬等 |
| FIN | Finance | 财务域 | 财务、资金、预算、成本等 |
| MAT | Material | 物资域 | 物料、库存、采购等 |
| PRJ | Project | 项目域 | 项目、计划、进度等 |
| GRID | Grid | 电网域 | 电网设备、线路、变电站等 |
| AST | Asset | 资产域 | 固定资产、资产运维等 |
| CST | Customer | 客户域 | 客户、用电、营销等 |
| MRT | Meter | 计量域 | 电表、采集、计量等 |
| SAF | Safety | 安全域 | 安全、风控、审计等 |
| ITG | Integration | 集成域 | 跨域数据、主数据等 |

### 2.2 刷新周期编码

| 编码 | 英文全称 | 中文名称 | 说明 |
|-----|---------|---------|------|
| MIN | Minute | 分钟级 | 分钟级刷新，如MIN05表示5分钟 |
| H | Hour | 小时级 | 小时级刷新，如H01表示1小时 |
| D | Day | 日级 | 日级刷新 |
| W | Week | 周级 | 周级刷新 |
| M | Month | 月级 | 月级刷新，如M01表示1个月 |
| Q | Quarter | 季度级 | 季度级刷新 |
| Y | Year | 年级 | 年级刷新 |

### 2.3 分区增量编码

| 编码 | 英文全称 | 中文名称 | 说明 |
|-----|---------|---------|------|
| I | Incremental | 增量 | 数据按增量方式分区 |
| F | Full | 全量 | 数据按全量方式分区 |

### 2.4 分层编码

| 编码 | 英文全称 | 中文名称 | 说明 |
|-----|---------|---------|------|
| ODS | Operational Data Store | 贴源层 | 原始数据，未经处理 |
| BUF | Buffer | 缓冲层 | 临时缓冲数据 |
| DWD | Data Warehouse Detail | 明细层 | 清洗后的明细数据 |
| EXE | Exchange | 交换层 | 数据交换区 |
| ADS | Application Data Store | 应用层 | 应用结果数据 |
| DIM | Dimension | 维度层 | 维度数据 |
| OTH | Other | 其他 | 其他类型表 |

### 2.5 系统编码 (UN01-UN76)

| 编码 | 系统名称 | 英文缩写 |
|------|----------|----------|
| UN01 | 集中部署企业资源管理系统 | ERP_CD |
| UN02 | 企业资源管理系统 | ERP |
| UN03 | 集中部署财务管控系统 | FCM_CD |
| UN04 | 财务管控系统 | FCM |
| UN05 | 95598业务支持系统 | 95598 |
| UN06 | 电子商务平台1.0 | ECP |
| UN07 | 规划计划信息管理系统 | PPS |
| UN08 | 基建管理信息系统 | ICMS |
| UN09 | 经济法律管理业务系统 | ELMS |
| UN10 | 人力资源管理系统 | HRCS |
| UN11 | 设备（资产）运维精益管理系统2.0 | PMS |
| UN12 | 统一车辆管理平台 | UVM |
| UN13 | 统一项目储备库管理系统 | PRS |
| UN14 | 营销业务应用系统 | CMS |
| UN15 | 用电信息采集系统 | AMR |
| UN16 | 主数据管理平台 | MDM |
| UN17 | 电网调度管理系统 | OMS |
| UN18 | 调度电能量管理系统 | EMS |
| UN19 | 配电自动化系统 | PDAS |
| UN20 | 营销分析与辅助决策系统 | AADMM |
| UN21 | 设备（资产）运维精益管理系统2.5 | PMS25 |
| UN22 | 一体化电量与线损管理系统 | PLS |
| UN23 | 电子商务平台2.0 | ECP20 |
| UN24 | 物流管理平台 | ELP |
| UN25 | 国网商城 | ESG |
| UN26 | 网上国网 | WSGW |
| UN27 | 电网运检智能化分析管控系统 | MCSAS |
| UN28 | 电能质量在线监测系统 | PQMM |
| UN29 | 基于大数据分析的配电网可靠性评估系统 | PWFX |
| UN30 | 信息通信业务管理 | IRS |
| UN31 | 网上电网2.0 | PIS20 |
| UN32 | 人力资源综合报表 | HRCR |
| UN33 | 后勤管理信息系统 | LMS |
| UN34 | 营销基础数据平台 | MBDP |
| UN35 | 电网GIS平台 | GIS |
| UN36 | 互联网+发展辅助决策系统 | IPD |
| UN37 | 省地调控云 | POMC |
| UN38 | 电能量采集系统 | TMR |
| UN39 | 大数据征信系统 | BDCS |
| UN40 | 物资辅助工具 | WZFZGJ |
| UN41 | 计量生产调度平台 | MCPSP |
| UN42 | 计量资产全寿命周期管理系统 | MALMS |
| UN43 | 营销远程实时费控 | RRCCS |
| UN44 | 营销业务管控平台 | MBCP |
| UN45 | 95598运营管理系统 | 95598OM |
| UN46 | 国家电力需求侧管理平台 | NPRSMP |
| UN47 | 电网谐波监测分析系统 | HMAS |
| UN48 | 国家电网人力资源招聘平台 | HRRP |
| UN49 | 网络大学平台 | NU |
| UN50 | 党建信息化综合管理系统 | PCIIMS |
| UN51 | 协同办公系统 | OA |
| UN52 | 科技工作管理系统 | SWMS |
| UN53 | 员工报销系统 | SRM |
| UN54 | 竣工决算分级审批系统 | GRAS |
| UN55 | 原始凭证电子化系统 | ODES |
| UN56 | 凭证协同系统 | FDCM |
| UN57 | 智能分析决策五位一体风控子系统 | FIOSS |
| UN58 | 一体化规划设计平台 | PDS |
| UN59 | 新一代电网发展信息系统 | NGDIS |
| UN60 | 安监管理一体化平台 | ISS |
| UN61 | 安全工器（机）具管控 | STCM |
| UN62 | 业务外包安全资信管理 | OSCM |
| UN63 | 电网工程数字化管理应用 | PGEDM |
| UN64 | 企业架构管理系统 | EAS |
| UN65 | 集体企业管控系统 | MCEM |
| UN66 | 产业集约管控系统 | IICS |
| UN67 | 数字档案馆系统 | DAS |
| UN68 | 电子文件管理系统 | ERMS |
| UN69 | 办公业务安全保障系统 | OBSS |
| UN70 | 研究决策支持系统 | RDSS |
| UN71 | 国际业务综合管理子系统 | IBMS |
| UN72 | 国际合作管理子系统 | ICMS |
| UN73 | 审计综合管理系统 | ACMM |
| UN74 | 数字化审计平台 | DAP |
| UN75 | 离退休管理子系统 | RMSS |
| UN76 | 企协管理信息系统 | EAMIS |

### 2.6 部署方式编码

| 编码 | 部署方式 | 说明 |
|-----|---------|------|
| 01 | 集中部署 | 总部统一部署和管理 |
| 02 | 省级部署 | 各省公司独立部署 |
| 03 | 混合部署 | 集中+省级混合模式 |

---

## 3. 约束条件

### 3.1 表名长度限制

- **最大长度**: 128个字符
- **建议长度**: 不超过64个字符，便于阅读和维护

### 3.2 字符规范

| 平台 | 规范 | 说明 |
|-----|------|------|
| **阿里中台** | 英文字母统一**大写** | 例: `ODS_FCM_CD_ORGRELATIONS` |
| **华为中台** | 英文字母统一**小写** | 例: `ods_fcm_cd_orgrelations` |

### 3.3 命名一致性

1. **统一分隔符**: 使用下划线 `_` 作为分隔符
2. **禁止使用**: 中文、空格、特殊字符（除下划线）
3. **数字位置**: 数字应放在有意义的编码位置，如周期编码 `D`、`H01`
4. **避免保留字**: 表名不得使用数据库保留字

### 3.4 版本控制

- 表结构变更应通过版本号管理
- 历史版本表可添加版本后缀，如 `_V1`、`_V2`

---

## 4. 示例库

### 4.1 贴源层示例

| 表名 | 说明 | 分层 |
|-----|------|-----|
| `ODS_FCM_CD_CWGKAVPD_FMIS9999_ORGRELATIONS` | 财务管控组织机构全量表 | ODS |
| `ODS_OGG_FCM_CD_CWGKAVPD_FMIS9999_ORGRELATIONS` | 财务管控组织机构增量表 | ODS |
| `ODS_ERP_CD_P34C_ERP_USER_INFO` | ERP用户信息全量表 | ODS |
| `ODS_CDC_PMS_PMSDB_PMS_DEVICE` | PMS设备信息增量表 | ODS |
| `ODS_CMS_CMSDB_CMS_CUSTOMER` | 营销客户信息全量表 | ODS |

### 4.2 共享层示例

| 表名 | 说明 | 分层 |
|-----|------|-----|
| `UN03_03_FCM_CD_ORGRELATIONS` | 财务管控组织机构标准表 | UN |
| `UN11_01_PMS_DEVICE_INFO` | PMS设备信息标准表 | UN |
| `UN14_01_CMS_CUSTOMER_INFO` | 营销客户信息标准表 | UN |
| `DWD_CST_CSTORG` | 客户域组织机构模型表 | DWD |
| `DWD_FIN_FINANCE_ORDER` | 财务域订单模型表 | DWD |
| `DWD_HR_EMPLOYEE_INFO` | 人力资源域员工模型表 | DWD |

### 4.3 分析层示例

| 表名 | 说明 | 分层 |
|-----|------|-----|
| `DIM_CST_CUST_DF` | 客户维度表，日级全量 | DIM |
| `DIM_HR_DEPT_MF` | 部门维度表，月级全量 | DIM |
| `DIM_GRID_STATION_YF` | 变电站维度表，年级全量 | DIM |
| `DWS_CST_CUST_INDEX_DF` | 客户指标汇总表，日级全量 | DWS |
| `DWS_FIN_REVENUE_MMF` | 收入汇总表，月级全量 | DWS |
| `DWS_GRID_LOAD_HF` | 电网负荷汇总表，小时级全量 | DWS |
| `ADS_CST_YX2_CUST_INDEX_DF` | 营销2.0客户指标，日级全量 | ADS |
| `ADS_FIN_DW_REPORT_MMF` | 财务报表数据，月级全量 | ADS |
| `FCT_CST_E_DAY_READ_DI` | 日电量读取明细，日级增量 | FCT |
| `FCT_FIN_PAYMENT_RECORD_DI` | 支付记录明细，日级增量 | FCT |

### 4.4 特殊表示例

| 表名 | 说明 | 分层 |
|-----|------|-----|
| `TEMP_ODS_CSTORG` | ODS层临时表 | TEMP |
| `TEMP_DWD_FINANCE_CALC` | DWD层临时计算表 | TEMP |
| `SELF_ODS_CSTORG` | ODS层自建表 | SELF |
| `SELF_DWD_CUSTOM_MAPPING` | DWD层自定义映射表 | SELF |
| `SPECIAL_ODS_CSTORG` | ODS层专项表 | SPECIAL |
| `SPECIAL_DWS_AUDIT_DATA` | DWS层审计专项表 | SPECIAL |

### 4.5 交换区表示例

| 表名 | 说明 | 类型 |
|-----|------|------|
| `DOWN_UN14_CUSTINFO_20260320` | 下发营销系统客户信息 | 下发表 |
| `DOWN_UN11_DEVICE_20260320` | 下发PMS设备数据 | 下发表 |
| `UP_UN14_PAYMENT_20260320` | 上传营销系统缴费数据 | 上传表 |
| `UP_UN40_MATERIAL_202603` | 上传物资数据 | 上传表 |

### 4.6 城区/地市表示例

| 表名 | 说明 | 区域 |
|-----|------|------|
| `ODS_1101_CUSTOMER_INFO` | 北京市城区客户信息 | 城区 |
| `DWD_3101_POWER_USAGE` | 上海市城区用电数据 | 城区 |
| `DWS_4401_REVENUE_DF` | 广州市城区收入汇总 | 城区 |
| `ODS_3701_JINAN_CUSTOMER` | 济南市客户数据 | 地市 |
| `DWD_4201_WUHAN_DEVICE` | 武汉市设备数据 | 地市 |

---

## 5. 命名检查清单

在创建新表前，请确认以下事项:

- [ ] 表名长度不超过128字符
- [ ] 表名符合目标平台大小写规范（阿里大写/华为小写）
- [ ] 使用了正确的分层前缀
- [ ] 数据域编码正确
- [ ] 系统编码正确（来自2.5节）
- [ ] 刷新周期编码正确
- [ ] 分区增量标识正确（I/F）
- [ ] 不包含中文、空格、特殊字符
- [ ] 不是数据库保留字
- [ ] 表名具有业务含义，易于理解

---

## 6. 附录

### 6.1 常用缩写对照

| 缩写 | 全称 | 中文 |
|-----|------|------|
| ODS | Operational Data Store | 贴源层 |
| DWD | Data Warehouse Detail | 数据仓库明细层 |
| DWS | Data Warehouse Summary | 数据仓库汇总层 |
| ADS | Application Data Store | 应用数据层 |
| DIM | Dimension | 维度 |
| FCT | Fact | 事实 |
| UN | Unified | 统一/共享层 |
| CST | Customer | 客户 |
| HR | Human Resource | 人力资源 |
| FIN | Finance | 财务 |
| GRID | Grid | 电网 |
| MAT | Material | 物资 |

### 6.2 变更记录

| 日期 | 版本 | 变更内容 | 作者 |
|-----|------|---------|------|
| 2026-03-20 | 1.0.0 | 初始版本 | 笑江湖 |
