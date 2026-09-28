# topic

**为真实商业项目创作有目标受众曝光潜力的短视频选题。**

`topic` 是一个以中文为主要工作语言的 Agent Skill，工作版本为 V0.1。它接收项目资料，主动补充会改变判断的关键事实，输出选题、实际讲什么、依据／待补素材，然后结束。用户使用其他语言时跟随用户。

## 解决什么问题

适合商家、创始人和内容团队：已经有一个真实业务，却不确定哪些内容值得拍；手上有产品资料、客户问题或零散经历，却容易写成产品介绍、知识目录或只有刺激标题的选题。

可以用于从零出题或优化已有候选。只给品牌名也可以开始，但对象无法确认或关键信息不足时，Skill会先问问题。它不替代完整商业定位、文案写作、视频制作或发布系统。

## 核心工作方式

1. 确认给谁争取哪一种曝光，区分泛曝光、指定群体、本地曝光与已有购买意向者。
2. 读取现有资料，只追问或核验可能改变选题判断的事实。
3. 还原具体的人、处境、动机及谁在付款或决策。
4. 从真实素材中建立观看理由。
5. 选择适合受众、能够兑现的内容回应，例如答案、演示、比较、故事或体验。
6. 把价值写进观众能理解的选题表达。
7. 校验事实、内容承诺和重复，交付后停止。

不使用固定标题模板、评分公式或题型配额。默认交付10题，也可指定数量。信息足够、关键未知已无法取得，或你要求先按现有资料出题时，会处理剩余未知并交付；不会为了填满字段不停追问。

## 安装

需要一个支持本地 Agent Skills 的宿主。下面以 Codex 为例；本 Skill 本身无脚本、MCP、API Key或额外运行依赖，模型和可选检索工具由宿主提供。

仓库发布后，在此仓库页面选择 **Code → Download ZIP** 并解压。进入包含本 README 与 `SKILL.md` 的目录，不要把仓库下载包再次嵌套为 `topic/topic/`。

### Codex 本地安装（macOS / Linux）

在解压目录中运行以下命令。默认目标为用户级 `~/.agents/skills/topic`；可通过 `TOPIC_SKILL_DIR` 指定其他受宿主支持的位置。命令遇到已有目录会停止，避免覆盖已有版本。

```sh
(
  set -eu
  topic_skill_dir="${TOPIC_SKILL_DIR:-$HOME/.agents/skills/topic}"
  test -f SKILL.md
  test -f references/judgment-guide.md
  test -f references/calibration-cases.md
  test -f agents/openai.yaml
  if [ -e "$topic_skill_dir" ] || [ -L "$topic_skill_dir" ]; then
    printf '%s\n' '目标已存在，请先检查已有版本；未覆盖。' >&2
    exit 1
  fi
  mkdir -p "$topic_skill_dir/references" "$topic_skill_dir/agents"
  cp SKILL.md "$topic_skill_dir/SKILL.md"
  cp references/judgment-guide.md references/calibration-cases.md "$topic_skill_dir/references/"
  cp agents/openai.yaml "$topic_skill_dir/agents/openai.yaml"
  printf '已复制 Skill 到 %s\n' "$topic_skill_dir"
)
```

也可以手动新建 `topic` 文件夹，将 `SKILL.md`、`references/`、`agents/` 原样放入，保持相对结构。Windows 用户可按此方式复制到用户目录下的 `.agents/skills/topic`。

安装后在 Codex 中输入 `$topic`；若未显示，重启 Codex并检查目录结构。安装位置与发现行为依据 [Codex 官方 Skills 文档](https://developers.openai.com/codex/skills)。若已有其他位置的同名安装，先检查已有版本，避免产生重复选项。

其他支持 Agent Skills 的宿主可使用同一套文件，但安装位置和调用语法以该宿主文档为准；本项目不声称已测试所有宿主。

## 最小调用示例

下面使用假设的住宅灯光设计项目，不包含真实客户信息：

```text
$topic
项目：住宅灯光专项设计。
受众：正在装修、选灯的住宅业主。
目标：吸引这类业主的注意。
现有材料：可以讲解厨房台面阴影，并制作示意图或自有小样演示。
没有真实客户案例、改造前后照片、价格或节省金额。
请先按这些信息出3个选题，不再追问。
最终只交付选题、实际讲什么和依据／待补素材。
```

其他项目直接换成自己的真实资料、传播目标和数量即可。允许补充信息时，加上“关键事实缺失时先问我；拿不到的信息如实保留”。不要在资料中提供账号密码或不必要的个人身份信息。

## 输出示例

以下仅演示一条的格式，不是实际客户案例、行为测试结果或效果承诺：

### 01｜切菜时，为什么台面总被自己挡住光？

**实际讲什么：** 用示意图展示人在台面前站立时，身体、灯与操作面的相对位置；拟制作小样比较不同光源位置，观察阴影变化。

**依据／待补素材：** 假设输入允许讲解台面阴影并制作示意图；小样尚未制作，具体比较结果待演示确认，不冒充真实客户改造效果。

## 事实边界与效果说明

- 不编造客户原话、经营数据、社会证明、专业资质或疗效；问句中的事实前提也需要依据。
- 区分用户提供的信息、公开来源、一般知识、假设、拟制作内容与未知。未拍摄的体验可以策划，但不能预写结果。
- 不因为价格、年龄或城市就推断购买动机，也不把所有曝光目标换成成交指导。
- **不保证爆款。** 不预测或保证播放、点赞、本地曝光、获客、成交或收益。V0.1不是已验证跨行业稳定有效的算法。
- 公开校准材料是去标识化后的教学示例，不提供真实客户背书或盲评成绩。
- 文件结构检查、安装复制自检不等于模型行为或市场效果验证；本版未提供行为测试通过或流量验证通过的结论。

## 文件说明

- [SKILL.md](SKILL.md)：核心运行规则。
- [references/judgment-guide.md](references/judgment-guide.md)：观看价值、回应选择与判断边界。
- [references/calibration-cases.md](references/calibration-cases.md)：按问题读取的公开教学示例。
- [agents/openai.yaml](agents/openai.yaml)：Codex界面名称与默认调用提示。

## 许可证

本项目采用 [MIT License](LICENSE)。
