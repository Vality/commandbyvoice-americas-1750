# 游戏数据 MOD Agent 工作约定

## 绝对完成门槛：这是“全部配置表”改造，不是局部改表

本任务的完成条件是：**逐一审查当前 MOD `Data` 目录中的全部 JSON 配置表，并让其中与本次世界观、地图、人物、玩法相关的内容完成一致改造。** 不得只改 `Data_Country.json`、`Data_CountryProvince.json`、地图或玩家点名的少数系统后就报告完成。

1. `Data` 是当前游戏原版三国配置的完整副本，`Art` 是原版三国国家头像、旗帜、官员与后宫立绘模板；它们是本次 MOD 的起点，**不是罗马示例 MOD，也不是可忽略的参考文件**。
2. 开始 Build 前，必须建立全表审查清单，逐个列出每一个 `Data/**/*.json` 文件的处理结论：`已按本次世界观改造`、`保留原版且说明理由` 或 `不适用且说明理由`。最终回复必须逐表报告该清单；没有清单即视为未完成。
3. 对所有世界相关表，必须保持同一套国家、行省、人物、年代、制度、资源和故事逻辑。尤其必须完整处理国家/行省、默认官员、密信可选人物、后宫及多语言、科技树、阶层、AI/历史/主动剧情、产业/经济/城市、军团、初始资源、政策、成就、特质、王朝风格、年份和所有美术/立绘引用。
4. 任何 `NameKey`、`BioKey`、`TitleTextID`、`DescribeTextID`、角色名、简介、事件标题或说明，只要玩家界面会读取，就必须在 `Data_Language.json` 中有可解析的当前语言文本；不得把裸 Key 展示给玩家。
5. 所有 CountryID、ProvinceID、人物、官职、后宫、产业、军团、事件、成就、多语言 Key 和资源路径引用必须存在且互相匹配。`Data_CountryProvince.json` 中锁定的 `CountryID`、`ProvinceID`、`ProvinceIndex`、`ProvinceName` 不得改动。
6. 必须检查默认国家资源、土地、人口、粮食、金币、兵力、产业产出和军团数值，不能保留无依据的 `0`、`-1`、空字符串或指向旧世界的关联；所有数值必须能支撑开局。
7. 必须检查并清除与本次 MOD 世界观冲突的罗马或原版三国专属人物、事件、国家、地名、美术引用和多语言内容；若因玩法兼容必须保留，需在最终报告逐项说明理由。
8. Build 后必须解析全部 JSON，校验全部外键、地图 SHA-256、所有玩家可见多语言 Key、每个国家的有效官员、默认后宫、剧情事件、产业及非异常初始资源。**任一检查未通过、任一配置表未审查，或任一必需系统仍为空时，不得报告“完成”。**

## 科技树必须与故事背景同步重设计

1. 每个新建 MOD 都必须将 `Data_skillTree.json` 作为世界观改造项，依据 `Prompts/world.md`、`Prompts/mod-brief.json` 和已锁定的地图/国家设定，重新设计科技名称、描述、分支主题、前置关系和数值倾向；不得保留与新故事背景冲突的原版时代、国家、制度或技术叙事。
2. 完整科技树的**总节点数不得少于 200 个**。Plan 必须列出总数、各科技分类的节点数、根节点、分支主题、终局节点、关键跨分支前置关系，以及每类效果类型与数值来源；不得以重复节点、空描述或仅改名的模板节点凑数。
3. Build 后必须解析 `Data_skillTree.json`，校验 `skillID` 唯一、所有前置 ID 存在、依赖图无环、每个节点有名称和描述、`effectType` 处于本文件白名单内，并在最终报告中给出实际总节点数与各分类数量。若未达到 200 个节点或任一校验失败，不得报告完成。

## 中文编码与“????”故障防护

1. 所有 JSON，尤其是 `Data_skillTree.json`、`Data_Language.json` 及玩家可见的名称、描述字段，必须以 UTF-8 写入。禁止将含中文的 JSON 或脚本源码直接通过 Windows 控制台默认代码页管道传递；该行为可能把中文静默替换为 `?`。
2. 需要由脚本批量写入中文时，必须显式指定 UTF-8 编码；若执行环境的管道编码不可信，应使用 UTF-8 Base64/字节内容传递文本，再在目标端解码写入。不得把控制台输出正常误判为文件编码正常。
3. 每次 Build 后，除 JSON 解析外，必须扫描所有本次新增或修改的玩家可见文本字段：不得含有 `?` 占位符或 Unicode 替换字符 `�`。科技树至少应检查 `useless`、`skillName`、`description`，并在游戏内打开科技树抽查新增节点。
4. 若游戏内新增中文显示为 `????`，应先读取对应 JSON 的原始文本确认是否已被写坏；不要将其误判为字体、布局或 UI 问题。必须从可靠的原始中文内容恢复字段，以显式 UTF-8 重写，再重新执行文本扫描与 JSON/前置依赖校验。

## 官方模板参考

- 当前 MOD 根目录的 `Data` 已含当前游戏原版三国的全部 JSON 配置表；`Art` 已含原版三国国家头像、旗帜、官员与后宫立绘。它们既是字段、资源路径和制作参考，也是玩家与 AI 应直接替换、补全的工作文件。
- `Maps/DefaultVoronoiMap.json`、`Data/Data_Country.json` 与 `Data/Data_CountryProvince.json` 的地图 ID/名称约束仍以手绘地图摘要为准。

## 最高优先级：手绘地图不可变

1. 开始前必须读取 `Prompts/map-brief.json`、`manifest.json`、`Prompts/world.md`、`Prompts/mod-brief.json`。
2. `Maps/DefaultVoronoiMap.json`、`Prompts/map-brief.json` 只读；不得格式化、重排、增删或修改。
3. 地图摘要中的 `CountryID`、`ProvinceID`、国家/行省名称、归属关系和顺序是唯一权威。不得重编、交换、增删。
4. `Data_CountryProvince.json` 中的 `CountryID`、`ProvinceID`、`ProvinceIndex`、`ProvinceName` 只读；仅可补充人口、经济、城市、军团等玩法字段。
5. 若确实需要改地图，停止数据修改并要求玩家回游戏内地图编辑器重新保存。

## 工作流

1. 先用 **Plan** 模式列出：将改动的文件、每个外键、引用的地图 ID、风险和验证方法。Plan 必须以本 MOD 根目录的 `AGENTS.md` 为最高执行约束；不得以通用经验、角色设定或表名猜测替代本文件和当前 JSON 表头。
2. 等待玩家明确确认 Plan 后，才用 **Build** 写入。
3. 只允许写入当前提示词给出的 MOD 绝对路径及其子目录；禁止触碰游戏安装目录、其他 MOD、C#、DLL、EXE 或网络。
4. 写入前将目标 JSON 备份为同目录 `.bak`；保留 `num_columns`、`headers`、`field_types`、`rows` 的表格格式，新增行从 `row_number: 4` 起。
5. Build 完成后解析每个 JSON，校验外键和地图 SHA-256，并报告改动文件、ID、风险与游戏内验证步骤。

## 执行状态机与零写入调查约束

本节用于杜绝“调查时偷改、只改一半就汇报、把未完成说成已完成”。所有操作只能处于下列四个状态之一；状态切换条件必须满足，不能自行跳过。

1. **调查 / Plan 状态 = 零写入。** 在玩家明确发送 `plan ok`、`确认 Plan` 或等价的逐表 Plan 批准语句之前，只允许读取、列目录、计算摘要、解析校验和生成报告。不得创建、编辑、格式化、重命名、复制、删除任何 MOD 文件或目录；这包括 `Data`、`Art`、`Maps`、`Prompts`、`manifest.json`、`mod-creation-progress.json`、`.bak`、临时文件和资源占位文件。不得以“修复非法 JSON”“先做一小部分验证”“测试写入”作为例外。
2. **Plan 的固定产物与批准门槛。** Plan 必须在同一份报告中给出：MOD 根目录绝对路径、读取到的 `AGENTS.md` 摘要校验、全部 Data JSON 的去重清单、每表唯一结论、每个拟写入字段及其值来源、完整人物/资源/外键链、地图摘要结论、所有现有非法 JSON 的修复方案、以及 Build 后的逐项验收命令或方法。上述任一项缺失，只能继续调查，不能索取批准，更不能写入。
3. **明确批准才可进入 Build。** “继续调查”“再看看”“输出 Plan”“可以”“没问题”“开始准备”“阅读完成”等都不是写入授权。只有玩家明确批准该份 Plan 后，才可开始 Build；如 Plan 内容、MOD 路径、地图摘要或数据文件在批准后发生变化，必须重新调查并重新获得批准。
4. **Build 前冻结与差异基线。** 进入 Build 的第一步必须列出并保存（只在获得批准后）全部目标文件的 SHA-256、文件清单和每张表原始行数；同时确认当前目录没有来自此前未获批准的写入。若发现未获批准的脏改、半成品、陌生 `.bak` 或不完整资源复制，立即停止 Build，只读报告具体路径、摘要和差异，不得把它们混入本次结果或覆盖掉。
5. **Build 必须原子化、可回退。** 先对每个将写文件创建同目录 `.bak`，再按已批准的逐表清单完整执行；不得把“已改造”与“仍待改造”混在同一轮 Build 中。任一表写入、JSON 解析、外键、资源路径、枚举、地图摘要或计数校验失败时，立即停止，恢复本轮已写文件的 `.bak`，并报告失败项；不得留下部分世界观数据后声称调查或 Build 已完成。
6. **不得擅自扩大工作。** Plan 未列出且玩家未批准的文件、字段、资源复制、数值重平衡、修复原模板数据，都不得在 Build 中顺手改动。需要新增范围时，先回到调查 / Plan 状态，写出变更原因、影响和验证方法，获得新的明确批准。
7. **报告状态必须诚实可核。** 只有“本轮 Build 全部验收通过”才可称为“完成”。仍处于调查时只能称“调查中/Plan 待批准”；只读检查发现旧文件或他人写入时必须标为“现状”，不得表述为自己已完成的改造。不得用“已读取全部文件”“可开始执行”“仍需改造 N 个”替代逐表 Plan 或验收结果。
8. **禁止假设与伪修复。** 不得因看见旧三国文本、路径缺失、字段含义不明或 JSON 损坏，就猜测要替换的字段、创建虚构 Key、混用文本与稳定 ID，或把原资源路径改成不存在的新路径。所有修复必须是已批准 Plan 中具有表头、白名单、消费契约和外键证据的精确操作。
9. **禁止新增或臆造 JSON 配置表。** 当前 MOD `Data` 目录中原本不存在的 `.json` 文件，游戏不会因为 Agent 新建它而自动加载；因此无论名称看似合理（例如 `Data_DefaultLeader.json`、`Data_CountryResources.json`），都不得创建、复制、补造或纳入“完成”依据。只允许修改本轮调查开始时 `Data` 目录中实际存在的原有 JSON 文件；确需解决君主、资源或其他数据入口时，必须先从现有 JSON、既有资源引用和运行时加载路径追到真实入口，在该入口内按本文件约束修改。找不到入口时只能报告为阻塞项，绝不能用新 JSON 伪造解决方案。

## 表意、枚举与资源的强制约束

1. **先读后写，禁止按名称猜表义。** 每张拟修改表必须先读取当前文件的 `headers`、`field_types`、至少两条实际 `rows`，再通过项目内 `AGENTS.md` 的表职责和相关 C# 加载/消费代码确认字段语义。Plan 中须写出该表的真实字段、准备保留的受限字段和拟改字段；不得出现“保留或轻改”“审阅后决定”等未决结论。
2. **子嗣与宗亲严格分表。** `Data_DefaultRelationGraph.json` 是开局子嗣关系表，字段 `ChildName`、`MotherConcubineName`、`CountryID`、`Gender`、`Age`、`Military`、`Political`、`Charisma` 必须共同有效；子嗣必须引用同国、已存在的默认后宫。`Data_ClanRelatives.json` 仅用于非子嗣的宗亲/远亲，不能用它替代子嗣关系，也不能把它误作国家外交关系图。
3. **人物分层与数量必须可追溯。** `Prompts/mod-brief.json` 中的 `rulerName` 是君主独立输入，`characterBrief` 是玩家权威的人物自由设定；Build 前必须完整读取并解析其中的官员、后宫、子嗣及关系。必须追调用链确认“君主”对应的运行时配置入口；不得未经验证把君主当作普通官员、把后宫当作官员，或用一张表替代另一类人物。Plan 与最终报告都必须分别列出各国君主/官员/后宫/子嗣人数、姓名、所在表、稳定 ID/Key 及关联外键。
4. **后宫必须按真实两表契约写入。** `Data_Concubine.json` 是通用后宫池，必须沿用它实际的 `id`、`name`、`description`、`rarity`、`tag`、`baseBuffValue`、`portraitPath`、`hasDynamicEffect`、`acquisitionType` 等字段及已有合法枚举；它不因表名而天然拥有 CountryID。`Data_DefaultConcubines.json` 才负责开局国家后宫，必须校验 `CountryID`、`Rank`、`SocialClassType`、画像与家庭官员字段。两表名称、画像和多语言 Key 的关系必须逐项验证。
5. **资源类型不得为了世界观而改枚举。** `Data_Money.json` 的 `MoneyType`、`Data_DefaultIndustry.json` 的 `OutputGoodsType`、以及所有 `ClassType`、`Type`、`Rank`、`Gender`、官职、政策、建筑、产业、资源等枚举/ID，只能复用当前 JSON 或项目 C# 中已存在的精确值。需要“灵石”等世界观表达时，优先改已有资源的显示文本、`NameLanguageID`、描述、经济数值和玩法叙事；在确认加载代码支持前，严禁新造或改写底层枚举值。
6. **国家、行省、产业和军团必须按加载契约关联。** `Data_DefaultIndustry.json` 的 `ProvinceIndex` 必须以当前 `Data_CountryProvince.json` 的真实索引含义为准，不能把它当作 `ProvinceID`；每条产业要验证 CountryID、ProvinceIndex、GoodsType 和产出。开局军团必须追溯并校验 `Data_CountryProvince.json` 的 `DefaultProvinceLegions` 与实际将领/行省的解析格式，不能只笼统修改 `Data_LegionConfig.json` 后宣称已有开局军团。
7. **画像与美术采用可替换占位资源策略。** 不得引用不存在的路径，也不得虚构资源。为新角色/国家需要新文件名时，只能在当前 MOD 的 `Art` 内复制一份已存在、已验证可加载的官方模板资源，改为稳定的新文件名并更新引用；保留原始模板不覆盖、不删除。复制后的占位资源须在最终报告列出“目标路径 -> 来源路径 -> 引用表/字段”，玩家可在之后用同名生成图片替换该文件。不得修改地图资源或游戏安装目录资源。
8. **本 MOD 的 `AGENTS.md` 优先于模型既有知识。** 若角色设定、通用常识或此前 Plan 与当前表头/本文件冲突，必须停止猜测，报告冲突并要求玩家决定；不得静默补字段或捏造默认值。
9. **每表必须给出确定结论。** Build 前的 Plan 与 Build 后报告都要覆盖当前 `Data` 目录实际存在的每一个 JSON，且每一张只能标记为“改造（列出字段与原因）”“保留（列出兼容理由）”或“不适用（列出理由）”。对发现的原模板无效 JSON，必须在 Plan 中单列其语法错误、修复方式、备份路径和解析验收方法。
10. **Plan 不得留下人物链断点。** 若 `Data_DefaultConcubines.json` 写入某个默认后宫，Plan 必须证明该人物在其实际依赖的后宫定义、画像和多语言中均可解析；不得用一批与默认后宫名单无关的人物替换通用后宫池后，再把名单外人物写入默认后宫。`Data_DefaultRelationGraph.json` 的子嗣性别必须使用当前表和消费代码认可的精确值（例如 `Male` / `Female`），不得缩写为 `M` / `F`。
11. **稳定 ID 只能复用，不能改名伪装成显示文本。** `cardId`、`traitId`、`MoneyType`、`OutputGoodsType`、`ClassType`、`CountryID`、`ProvinceID`、`ProvinceIndex`、`tierId`、`slotId`、`functionId` 等稳定 ID/枚举不得因世界观更名；显示名只能通过该表已有的显示字段或其引用的多语言 Key 修改。任何 Plan 中出现当前表头不存在的“人物 ID”“默认 ID”等字段时，必须先删除该设想或追到真实存储字段后重写。
12. **君主是独立验收项。** 当 `mod-brief.json` 提供君主而当前输入表未直接标明其存储位置时，必须先追踪原君主的运行时创建路径、配置文件与画像/旗帜引用，再提出精确改动。仅复制国君画像、旗帜或在报告中标记“入口未明确”均不足以进入 Build。
13. **美术映射必须精确且单义。** Plan 必须使用当前 MOD `Art` 根目录下实际存在的相对来源路径，并为每个新增占位文件给出唯一的“目标相对路径 -> 来源相对路径 -> 引用表/字段”。“新建”和“覆盖原文件”是互斥操作；默认策略为保留原文件、复制为新文件名。目标路径、目录层级或引用字段未验证时不得写入。
14. **Plan 提交前的完整性门槛。** 先枚举当前 `Data` 目录的实际 JSON 数量和文件名，Plan 各分类的数量之和必须一致。所有“待核查”“可能修改”“后续决定”项必须在请求玩家确认前变成确定结论；无效 JSON 必须先写明其语法修复方案。没有通过这些门槛的 Plan 只能继续调查，不能请求确认或 Build。
15. **逐表清单必须一一对应真实文件。** 每个实际 JSON 文件只能出现一次；不得通过重复列出同一文件凑数量，也不得遗漏 `PendingTaskConfigData.json`、`Sheet1.json` 等边缘文件。文件分类标题、序号和合计数必须可由当前目录清单复算。若某文件的作用尚未确认，继续读取其真实表头和消费代码，直到给出确定的“改造 / 保留 / 不适用”结论。
16. **受地图锁定表的精确边界。** 当前地图草稿的 `Data_Country.json` 只有 `CountryID` 和 `CountryName`，两者均受地图摘要锁定；不得虚构 `CivilizationId` 等不存在字段。`Data_CountryProvince.json` 仅锁定 `CountryID`、`ProvinceID`、`ProvinceIndex`、`ProvinceName` 四字段，必须在不改变这四项的前提下规划并校验人口、城市、繁荣、产业和 `DefaultProvinceLegions` 等开局玩法数据，禁止把整表错误地视为只读。
17. **枚举必须逐字段落表，不得只写“保留”。** 对每张拟改表，Plan 必须列出其实际会写入的枚举/稳定字段、可用精确值来源和本次采用值。例如官员的 `Type`/`DefaultPosition`，默认后宫的 `Rank`/`SocialClassType`，子嗣的 `Gender`，产业的 `Category`/`OutputGoodsType`，后宫事件的 `eventType`，政策的 `category`/`isInSlot`，政府的稳定 ID，技能的 `effectType`，资源的 `MoneyType`。不能证明合法值时继续读取，不得请求确认。
18. **不得给纯数值或系统表臆造显示 Key。** `Data_EconomyBase.json` 是年度收支数值表；任何“改 Key、改 Goods 名称、改资源类型”的建议都必须先由真实 headers 和消费代码证明。对 `Data_PlayerPicture.json`、`Data_RedDot.json`、`Data_PrestigeEffect.json`、教程、任务、加载等系统表，同样须先证明存在世界文本或资源引用，才可修改；否则保留并说明兼容理由。
19. **非法 JSON 与空/遗留表不是可选项。** 已发现的 `Data_Language_Ranking.json` 必须在 Plan 中写明具体语法修复、同目录 `.bak`、修复后的 Newtonsoft 全表解析；`Sheet1.json`、空表和遗留表不得使用“补列名或保持原样”之类二选一描述，必须基于实际消费路径给出唯一决定。
20. **未通过门槛时继续自查。** 如果当前 Plan 存在人物链断裂、君主入口未追到、表重复/遗漏、枚举未落表、资源映射缺失、非法 JSON 未处理或表结论未定，必须继续在当前 MOD 工作区读取文件和项目规则并修订 Plan；不得把这些调查任务转交给玩家，不得请求 `plan ok`，更不得进入 Build。

## 配置表职责

| 配置表 | 用途与不可破坏的关联 |
|---|---|
| `Data_Country.json` | 国家基本资料；`CountryID` 必须与地图 StateID 一致，`CountryName` 必须与地图和摘要一致。 |
| `Data_CountryProvince.json` | 国家—行省玩法数据；地图四字段只读：`CountryID`、`ProvinceID`、`ProvinceIndex`、`ProvinceName`。 |
| `Data_DefaultOfficials.json` | 开局官员；必须沿用真实字段和官职/类型枚举，`CountryID`、人物、画像、多语言与官职引用都必须有效。君主是否在本表创建必须先追运行时调用链确认。 |
| `Data_Concubine.json` | 通用后宫人物池；按本表真实字段和合法枚举维护，不能假设其直接持有 CountryID。 |
| `Data_DefaultConcubines.json` | 开局国家后宫名单；只能引用已定义、资源存在的后宫人物，并校验 `CountryID`、品阶、社会阶层与家庭官员关联。 |
| `Data_DefaultRelationGraph.json` | 开局子嗣关系；`ChildName` 必须与同国的 `MotherConcubineName`、性别和属性共同有效，是子嗣的权威来源。 |
| `Data_ClanRelatives.json` | 非子嗣的宗亲/远亲；不能代替 `Data_DefaultRelationGraph.json`，也不是国家关系图。 |
| `Data_CountryCenter.json`、`Data_defaultCell.json`、`Data_GridConfig.json`、`Data_ProvinceConfig.json` | 地图相关数据；手绘地图 MOD 中默认只读，不能猜测坐标、格子或 ID。 |
| `Data_DefaultIndustry.json`、`Data_EconomyBase.json`、`Data_CityConfig.json` | 产业、经济与城市；产业需按真实 `ProvinceIndex` 契约绑定国家行省，`GoodsType` 与资源枚举不得猜测。 |
| `Data_CountryProvince.json` 的 `DefaultProvinceLegions`、`Data_LegionConfig.json` | 开局军团；必须先确认实际加载入口和字符串/外键格式，再配置有效国家、将领和行省。 |
| `Data_Language.json`、`Data_Language_Ranking.json`、`Data_LanguageType.json` | 多语言；只追加本 MOD 所需 key，不覆盖无关 key。 |
| `Data_AIEvents.json`、`Data_ProactiveStoryEvents.json`、`Data_HaremEvent.json` | AI/剧情/后宫事件；沿用已有 schema，不臆造解析字段。 |
| `Data_other.json` | 杂项参数；仅修改已存在或玩家确认新增的键。 |
| `Data_Achievements.json`、`Data_CharacterTraits.json`、`Data_civilization.json`、`Data_DefaultPolicyCard.json`、`Data_DifficultyConfig.json`、`Data_GovernmentStructure.json`、`Data_HaremCollection.json`、`Data_homeLand.json`、`Data_ImperialConsumption.json`、`Data_KingStyle.json`、`Data_Money.json`、`Data_PendingTask.json`、`Data_PlayerPicture.json`、`Data_PolicyStyle.json`、`Data_PrestigeEffect.json`、`Data_RankingScore.json`、`Data_RedDot.json`、`Data_skillTree.json`、`Data_skillTree_new.json`、`Data_SocialClass.json`、`Data_TutorialTask.json`、`Data_Whitelist.json`、`Data_YearName.json` | 各自负责成就、角色、时代、政策、外交、难度、朝廷、后宫、家园、消耗、君主风格、资源、任务、头像、威望、排行榜、红点、天赋、阶层、教程、白名单与年号；修改前先读取原表头、既有枚举和消费代码。 |

## MOD 自包含枚举白名单（无源码环境的唯一依据）

本节从当前 MOD 模板 JSON 提取，是 OpenCode 在无法读取游戏源码时填写枚举字段的**封闭白名单**。列出的值可复用；未列出的枚举字段和值一律不得新增、翻译、改名或猜测，必须保留该行原值并在 Plan 中说明。布尔字段仅允许 `True` / `False`。稳定 ID 字段即使看似可读也必须保留原值。

| 表 | 字段 | 本 MOD 允许的枚举值 / 固定值 |
|---|---|---|
| `Data_DefaultOfficials.json` | `Type` | `Civil`、`Military` |
|  | `DefaultPosition` | `sangong_1`、`sangong_2`、`sangong_3`、`jiuqing_1`、`jiuqing_2`；不得创建其他职位 ID。 |
|  | `IsDefault` | `True` |
| `Data_DefaultConcubines.json` | `Rank` | `Empress`、`Zhaoyi`、`Meiren`、`Liangren` |
|  | `SocialClassType` | `Royal`、`Bureaucrat`、`Military`、`Merchant`、`Scholar`、`Commoner` |
|  | `IsDefault` | `True` |
| `Data_Concubine.json` | `rarity` | `Rare`、`Epic`、`Legendary` |
|  | `tag` | `Cultivator` |
|  | `hasDynamicEffect` | `True`、`False` |
|  | `acquisitionType` | `婚配` |
| `Data_DefaultRelationGraph.json` | `Gender` | `Male`、`Female` |
| `Data_ClanRelatives.json` | `Gender` | `男`、`女` |
| `Data_DefaultIndustry.json` | `Category` | `1`、`2`、`3` |
|  | `OutputGoodsType` | `Grain`、`Metal`、`DailyGoods` |
| `Data_Money.json` | `MoneyType` | `land`、`food`、`wood`、`stone`、`human`、`coin`、`book`、`soldier`、`military`、`control`、`prestige`、`corruption`、`order`、`favor` |
|  | `SceneType` | `BigWorld`、`Homeland`、`Both` |
|  | `ShowPerYearRate` | `True`、`False` |
| `Data_DefaultPolicyCard.json` | `category` | `Military`、`Political`、`Other` |
|  | `isInSlot` | `False`；不得为世界观改造新增其他布尔状态。 |
| `Data_SocialClass.json` | `ClassType` | `Royal`、`Bureaucrat`、`Military`、`Merchant`、`Scholar`、`Commoner` |
| `Data_HaremEvent.json` | `eventType` | `Family`、`Art`、`Conflict` |
| `Data_AIEvents.json` | `eventType` | `军事`、`外交`、`政治` |
| `Data_Achievements.json` | `Type` | `StorySuccess`、`StoryFailRewrite`、`StoryRewrite`、`WarVictory`、`CharacterAlive`、`Diplomacy`、`Unification` |
|  | `IsHidden` | `True` |
| `Data_GovernmentStructure.json` | `tierId` | `sangong`、`jiuqing`、`other`（稳定 ID，只读） |
|  | `functionId` | `prime_minister`、`grand_commandant`、`finance`、`ritual`、`justice`、`transport`、`garrison`、`diplomacy`、`royal_clan`、`censor_in_chief`、`imperial_supply`、`palace_guard`、`-`（稳定 ID，只读） |
|  | `groupId` | `prime_minister_group`、`grand_commandant_group`、`finance_group`、`ritual_group`、`justice_group`、`transport_group`、`garrison_group`、`diplomacy_group`、`royal_clan_group`、`censor_in_chief_group`、`imperial_supply_group`、`palace_guard_group`、`-`（稳定 ID，只读） |
| `Data_PolicyStyle.json` | `Style` | `武功`、`文治`、`重商`、`仁政`、`法治`、`防御`、`扩张`、`重农`（稳定值，只改显示文本引用时不得改本字段） |
| `Data_civilization.json` | `CivilizationID` | `10100`、`10200`、`10300`、`10400`、`10500`（稳定 ID，只读） |
| `Data_homeLand.json` | `buildType` | `1001`、`1002`、`1003`、`1004`、`1005`、`1006`、`1007`、`1008`、`1009`（稳定 ID，只读） |
| `Data_PendingTask.json`、`PendingTaskConfigData.json` | `TaskType` | `active_story_event_attention`、`book_surplus`、`class_tension_high`、`corruption_high`、`extended_stat_low`、`industry_balance_blocked`、`province_economy_warning`、`province_no_governor`、`storyline_in_progress` |
| `Data_ProactiveStoryEvents.json` | `phase` | `1`、`2`、`3`、`4`、`5` |
| `Data_CountryProvince.json` | `ProvinceNameAlwaysVisible` | `True`、`False` |
|  | `EconomyProfile` | `Balanced`；不得自行增加其他 Profile。 |

### 技能效果白名单

- `Data_skillTree.json` 的 `effectType` 仅允许：`ArmorDamageReduction`、`ArmyMaintenanceCostReduction`、`AttackRange`、`BuildingProductionCycleReduction`、`CasualtyRecoveryRate`、`ClassSatisfaction_All`、`ClassSatisfaction_Commoner`、`ControlStabilityBonus`、`CorruptionGrowthReduction`、`CriticalHitChance`、`CriticalHitDamage`、`LegionCountMax`、`LegionSoldierCapacity`、`MarchSpeed`、`MeleeDamageBonus`、`Null`、`OfficialAppointmentCapacity`、`OfficialRecruitQualityBonus`、`PrimaryIndustryBonus`、`RangedDamageBonus`、`RebellionBaseRateReduction`、`RecruitCostReduction`、`ResourceProductionEfficiency`、`SecondaryIndustryBonus`、`SiegeEfficiency`、`SoldierCombatPower`、`TertiaryIndustryBonus`、`TradeIncomeBonus`、`TradeMaxCount`、`TransportSpeed`。
- `Data_skillTree_new.json` 的 `effectType` 仅允许：`AllianceProposalSuccess`、`ArmyMaintenanceCostReduction`、`AttackFailChanceReduction`、`BarracksTrainingSpeed`、`BuildingOutputFlatBonus`、`BuildingProductionCycleReduction`、`BuildingQueueCapacityBonus`、`CasualtyRecoveryRate`、`ClassInfluenceBalance`、`ControlStabilityBonus`、`CorruptionGrowthReduction`、`DamageVarianceControl`、`DiplomaticFriendshipGain`、`DistanceDefenseBypass`、`InternalEventRiskReduction`、`LegionForceAllocationBonus`、`MarchSpeed`、`MarketTradeTaxBonus`、`NPCDeterrenceAura`、`NPCTradeAcceptance`、`OfficialAppointmentCapacity`、`OfficialLoyaltyDecayReduction`、`OfficialRecruitQualityBonus`、`PrestigeGrowthBonus`、`RandomEventPositiveWeight`、`RebellionBaseRateReduction`、`ResearchInstituteOutputMultiplier`、`ResourceMaxCapacity`、`ResourceProductionEfficiency`、`SiegeEfficiency`、`SocialClassSatisfactionGain`、`SocialClassTensionDecay`、`SoldierCombatPower`、`SupplyConsumptionReduction`、`TradeBonusAmplifier`、`TransportDispatchIntervalReduction`、`TransportSpeed`。

### 未列出的字段

- `Data_EconomyBase.json`、`Data_CityConfig.json`、`Data_Language*.json`、`Data_LegionConfig.json` 及未在上表出现的字段：本 MOD 未向 Agent 授权新增枚举值。仅可保留原值，或只修改已明确为显示文本/数值的字段；若仍需改变其枚举，必须停止并请求玩家提供新的白名单。

## 验收清单

- 所有 JSON 可解析，不能有注释、尾逗号或重复键。
- 每个外键可在当前 MOD 或官方基础数据中解析。
- `Maps/DefaultVoronoiMap.json` 的 SHA-256 与 `map-brief.json` 一致。
- 最终回复必须报告：修改文件、创建/复用 ID、外键验证、SHA-256、未处理风险、建议的游戏内验证步骤。

## 历史人物立绘技能

- 制作或替换国家君主、官员、武将、默认后宫立绘时，必须先完整读取 `Skills/historical-ink-portrait/SKILL.md` 并严格执行。
- `Skills/historical-ink-portrait/assets/style-reference-zongze.png` 是随新 Mod 一起复制的风格基准；生图时必须作为 reference image 输入，但只能继承水墨媒介、低饱和配色、洁净度与虚实层次，禁止复用宗泽的面貌、红袍、姿势、地图或汴京背景。
- 立绘生成母版与交付文件固定为 672×1008 PNG（严格 2:3），采用干净、低饱和彩色水墨；不得批量换脸或复用千篇一律的服饰、姿态和背景。
- 每个人必须依据本 MOD 的人物简介、时代、身份及公开历史典故分别设计，并闭合 Art 文件、CountryID 目录和配置字段引用。
- 必须先完成一张最小闭环并交由玩家确认；未获确认不得批量生成剩余立绘。

## 开局年份：必须由世界书驱动

- 在 Plan 阶段，先读取 `Prompts/world.md` 与 `Prompts/mod-brief.json`，明确本 MOD 唯一的开局年份（公元或公元前），并把年份与证据写入 Plan。
- Build 时，必须把这个年份同时写入 `Data/Data_other.json` 的既有键 `gameStartYear` 与 `defaultGameStartYear`；两项必须完全一致。前者控制顶部年份和年号换算，后者控制新开局、经济相对年份与朝会时间。
- 随后同步审查并按世界书修正 `Data_AIEvents.json`、`Data_ProactiveStoryEvents.json`、`Data_YearName.json` 以及所有 `StartYear`、`EndYear`、`PhaseUnlockYear`、人物年龄/生卒、剧情年表字段；每个可选国家必须有发生在该年份的 Phase 1 开局事件。
- Build 后必须新开局截图核验顶部年份。若显示三国模板的“公元219年”或其他不属于世界书的年份，MOD 未完成，不得发布。
