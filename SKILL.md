---
name: diagram-pro
description: >-
  Fusion diagram skill combining the best of fireworks-tech-graph (22 diagram types, 7 styles),
  cathrynlavery/diagram-design (editorial quality, brand onboarding),
  coleam00/excalidraw-diagram-skill (visual self-review pipeline),
  and Agents365-ai/drawio-skill (OpenClaw native metadata).
  Trigger on: "画图" "帮我画" "生成图" "做个图" "架构图" "流程图"
  "可视化一下" "出图" "generate diagram" "draw diagram" "visualize"
  "画个架构" "出张图" "帮我可视化" "来张时序图" or any system/flow description.
metadata:
  openclaw:
    namespace: diagram-pro
    version: "1.0.0"
    category: visualization
    dependencies:
      system:
        - rsvg-convert
    autoTrigger:
      - keywords: ["画图", "架构图", "流程图", "时序图", "可视化", "diagram", "出图"]
      - patterns: ["帮我画.*图", "生成.*架构", "来张.*图"]
---

# Diagram Pro — 融合版图表技能

> **Fusion Skill**: 融合 fireworks-tech-graph(5000⭐) + diagram-design(2100⭐) + excalidraw-skill(2800⭐) + drawio-skill(800⭐) 四大顶级图表Skill精华。

Generate production-quality SVG technical diagrams exported as PNG via `rsvg-convert`.

## 🎨 五强融合基因

| 来源 | 注入的能力 |
|------|-----------|
| 🔥 fireworks-tech-graph | 22种图表 + 7风格 + 语义形状/箭头 + SVG模板 |
| ✏️ cathrynlavery/diagram-design | 编辑级设计哲学 + 60秒品牌提取 + Annotation原语 |
| 🎨 coleam00/excalidraw-skill | 视觉自检管道 + "Diagrams that argue"方法论 |
| 🔧 Agents365/drawio-skill | OpenClaw原生metadata + Self-check循环 |
| 🇨🇳 axtonliu/obsidian-visual | 中英双语触发 + 多输出模式 |

## 💎 设计哲学 (Design Philosophy)

**每张图都是一篇视觉论述，不是一堆形状的堆砌。**

核心原则（继承自 cathrynlavery/diagram-design）：
1. **删减至上** — 最高质量的操作通常是删除。每个节点必须为存在辩护。
2. **一眼焦点** — 强调色只留给读者应该先看的1-2个元素。
3. **密度控制** — 目标密度4/10，留白不是浪费，是呼吸。
4. **论证而非展示** — 形状映射概念：扇形=一对多，时间线=序列，汇聚=聚合。拒绝统一的卡片网格。
5. **证据制品** — 技术图包含真实代码片段和实际JSON负载，不只是框和箭头。

**画前自问**（来自 diagram-design）：
> 读者从这张图学到的东西，会比一段写得很好的文字更多吗？如果不会，别画。

## Install Source

**For OpenClaw**: Copy this entire directory to your skills folder.

**For Claude Code / Codex**:
```bash
# From local workspace
git clone https://github.com/tutule/diagram-pro.git ~/.claude/skills/diagram-pro
# Or copy directly
cp -r diagram-pro ~/.claude/skills/diagram-pro
```

**Requirement**: `brew install librsvg` (macOS) or `apt install librsvg2-bin` (Linux)

## 🎨 Brand Onboarding (60秒品牌提取)

继承自 cathrynlavery/diagram-design 的品牌自动匹配系统。

### 如何使用
```
User: "onboard diagram-pro 到 https://mysite.com"
Agent: → 抓取首页 → 提取主色调+字体 → 映射到语义角色 → 展示建议diff → 写入brand-tokens.md
```

### 提取映射表

| 从网站检测 | 映射为 |
|-----------|--------|
| `<body>` 背景色 | `paper` token |
| 主文字颜色 | `ink` token |
| 次要文字颜色 | `muted` token |
| 卡片/容器背景 | `paper-2` token |
| 最多使用的品牌色 (CTA/链接) | `accent` token |
| `<h1>` 字体 | `title` font |
| `<body>` 字体 | `node-name` font |
| `<code>/<pre>` 字体 | `sublabel` font |

### 对比度自动检查
写入token前，验证 `ink` 在 `paper` 上的 WCAG AA 对比度。如失败，自动建议调整值。

### 首次使用门控
在项目中首次使用时，检查 brand-tokens.md 是否已自定义。如未，暂停询问：
> "这是此项目的首张图。品牌色板还是默认值。要运行 onboarding、手动粘贴token、还是用默认值继续？"

详见 `references/onboarding.md`。

## Helper Scripts (Recommended)

Four helper scripts in `scripts/` directory provide stable SVG generation and validation:

### 1. `generate-diagram.sh` - Validate SVG + export PNG
```bash
./scripts/generate-diagram.sh -t architecture -s 1 -o ./output/arch.svg
```
- Validates an existing SVG file
- Exports PNG after validation
- Example: `./scripts/generate-diagram.sh -t architecture -s 1 -o ./output/arch.svg`

### 2. `generate-from-template.py` - Create starter SVG from template
```bash
python3 ./scripts/generate-from-template.py architecture ./output/arch.svg '{"title":"My Diagram","nodes":[],"arrows":[]}'
```
- Loads a built-in SVG template
- Renders nodes, arrows, and legend entries from JSON input
- Escapes text content to keep output XML-valid

### 3. `validate-svg.sh` - Validate SVG syntax
```bash
./scripts/validate-svg.sh <svg-file>
```
- Checks XML syntax
- Verifies tag balance
- Validates marker references
- Checks attribute completeness
- Validates path data

### 4. `test-all-styles.sh` - Batch test all styles
```bash
./scripts/test-all-styles.sh
```
- Tests multiple diagram sizes
- Validates all generated SVGs
- Generates test report

**When to use scripts:**
- Use scripts when generating complex SVGs to avoid syntax errors
- Scripts provide automatic validation and error reporting
- Recommended for production diagrams

**When to generate SVG directly:**
- Simple diagrams with few elements
- Quick prototypes
- When you need full control over SVG structure

## Workflow (Always Follow This Order)

1. **Classify** the diagram type (see Diagram Types below)
2. **Extract structure** — identify layers, nodes, edges, flows, and semantic groups from user description
3. **Plan layout** — apply the layout rules for the diagram type
4. **Load style reference** — always load `references/style-1-flat-icon.md` unless user specifies another; load the matching `references/style-N.md` for exact color tokens and SVG patterns
5. **Map nodes to shapes** — use Shape Vocabulary below
6. **Check icon needs** — load `references/icons.md` for known products
7. **Write SVG** with adaptive strategy (see SVG Generation Strategy below)
8. **Validate**: Run `rsvg-convert file.svg -o /dev/null 2>&1` to check syntax
9. **Export PNG**: `rsvg-convert -w 1920 file.svg -o file.png`
10. **Report** the generated file paths
11. **🔍 Visual Self-Review (MANDATORY when image reading available)** — 继承自 coleam00/excalidraw-skill 的视觉验证管道。Load the exported PNG back and inspect it. Syntactic validity does not guarantee visual correctness. Common issues and fixes:
    - **Arrow-component collision**: Route arrows through gaps between boxes, not through box interiors
    - **Text collision**: Add background rects behind arrow labels (opacity 0.95, matching canvas color)
    - **Overcrowding**: Widen inter-row/inter-column gutters so same-layer arrows have clear corridors
    - **Cross-layer congestion**: Collapse repeated cross-layer arrows into a single "delegates down" rail outside the content area
    - **Legend collision**: Move legend/notes out of any region where arrows or labels land
    - **Spacing issues**: Increase viewBox height/width rather than packing elements tighter
  **Self-review loop**: revise → re-export → re-inspect, up to 3 rounds. If issues persist after 3 rounds, note the remaining issues in the output report and proceed. Skip this step silently if image reading is unavailable.

### 🔧 反重叠规则 (Anti-Collision Rules)

**每个生成阶段都必须检查，这是最常见的视觉质量问题。**

#### 文字与线条重叠
1. **箭头跨文字**: 箭头路径不得穿过文字区域。检查每个 `<text>` 的包围盒与每条 `<line>/<path>` 的交集。
2. **标签背景**: 所有在箭头上的文字标签必须有背景矩形（`<rect>` with `fill` matching canvas color, `opacity="0.9"`）。
3. **线间距**: 平行箭头之间至少保持20px间距，避免标签互相覆盖。

#### 文字与形状重叠
4. **框内padding**: 所有rect内的文字必须至少距边框8px（x方向）和12px（y方向）。
5. **中文额外上移**: 中文标签比英文标签视觉上重，在rect内居中时y坐标上移2-3px。
6. **注释文字位置**: Annotation原语的文字必须完全在核心图区域之外，或在专属边栏（x<100 或 x>860）。
7. **边栏对比度**: 注释边栏文字必须满足 WCAG AA 标准（≥4.5:1）。
   - 标题文字: `#334155` (slate-700, 9.9:1 on #fafafa)
   - 正文文字: `#475569` (slate-600, 7.3:1) 或 `#64748b` (slate-500, 4.6:1)
   - 引导虚线: `#94a3b8` (slate-400) 或更浅，避免与文字争注意力

#### 层间间距
7. **层间最小间距**: 虚线框层之间至少30px空白缓冲区，确保箭头在此区域通过时不与任何文字重叠。
8. **密集区警告**: 如果某100×100px区域内超过3个元素，考虑拆分或扩大viewBox。

#### 检查清单（每张图生成后必须运行）
```
□ 箭头是否穿过任何文字？ → 添加背景rect或改道路径
□ 框内文字是否紧贴边框？ → 增加padding，调整坐标
□ 是否有两条箭头共享同一y轴且间距<20px？ → 错开y坐标
□ Annotation是否与主图元素重叠？ → 移至边栏
□ 层间缓冲区是否足够？ → 增加30px间距
```

## Diagram Types & Layout Rules

### Architecture Diagram
Nodes = services/components. Group into **horizontal layers** (top→bottom or left→right).
- Typical layers: Client → Gateway/LB → Services → Data/Storage
- Use `<rect>` dashed containers to group related services in the same layer
- Arrow direction follows data/request flow
- ViewBox: `0 0 960 600` standard, `0 0 960 800` for tall stacks

### Data Flow Diagram
Emphasizes **what data moves where**. Focus on data transformation.
- Label every arrow with the data type (e.g., "embeddings", "query", "context")
- Use wider arrows (`stroke-width: 2.5`) for primary data paths
- Dashed arrows for control/trigger flows
- Color arrows by data category (not just Agent/RAG — use semantics)

### Flowchart / Process Flow
Sequential decision/process steps.
- Top-to-bottom preferred; left-to-right for wide flows
- Diamond shapes for decisions, rounded rects for processes, parallelograms for I/O
- Keep node labels short (≤3 words); put detail in sub-labels
- Align nodes on a grid: x positions snap to 120px intervals, y to 80px

### Agent Architecture Diagram
Shows how an AI agent reasons, uses tools, and manages memory.
Key conceptual layers to always consider:
- **Input layer**: User, query, trigger
- **Agent core**: LLM, reasoning loop, planner
- **Memory layer**: Short-term (context window), Long-term (vector/graph DB), Episodic
- **Tool layer**: Tool calls, APIs, search, code execution
- **Output layer**: Response, action, side-effects
Use cyclic arrows (loop arcs) to show iterative reasoning. Separate memory types visually.

### Memory Architecture Diagram (Mem0, MemGPT-style)
Specialized agent diagram focused on memory operations.
- Show memory **write path** and **read path** separately (different arrow colors)
- Memory tiers: Working Memory → Short-term → Long-term → External Store
- Label memory operations: `store()`, `retrieve()`, `forget()`, `consolidate()`
- Use stacked rects or layered cylinders for storage tiers

### Sequence Diagram
Time-ordered message exchanges between participants.
- Participants as vertical **lifelines** (top labels + vertical dashed lines)
- Messages as horizontal arrows between lifelines, top-to-bottom time order
- Activation boxes (thin filled rects on lifeline) show active processing
- Group with `<rect>` loop/alt frames with label in top-left corner
- ViewBox height = 80 + (num_messages × 50)

### Comparison / Feature Matrix
Side-by-side comparison of approaches, systems, or components.
- Column headers = systems, row headers = attributes
- Row height: 40px; column width: min 120px; header row height: 50px
- Checked cell: tinted background (e.g. `#dcfce7`) + `✓` checkmark; unsupported: `#f9fafb` fill
- Alternating row fills (`#f9fafb` / `#ffffff`) for readability
- Max readable columns: 5; beyond that, split into two diagrams

### Timeline / Gantt
Horizontal time axis showing durations, phases, and milestones.
- X-axis = time (weeks/months/quarters); Y-axis = items/tasks/phases
- Bars: rounded rects, colored by category, labeled inside or beside
- Milestone markers: diamond or filled circle at specific x position with label above
- ViewBox: `0 0 960 400` typical; wider for many time periods: `0 0 1200 400`

### Mind Map / Concept Map
Radial layout from central concept.
- Central node at `cx=480, cy=280`
- First-level branches: evenly distributed around center (360/N degrees)
- Second-level branches: branch off first-level at 30-45° offset
- Use curved `<path>` with cubic bezier for branches, not straight lines

### Class Diagram (UML)
Static structure showing classes, attributes, methods, and relationships.
- **Class box**: 3-compartment rect (name / attributes / methods), min width 160px
  - Top compartment: class name, bold, centered (abstract = *italic*)
  - Middle: attributes with visibility (`+` public, `-` private, `#` protected)
  - Bottom: method signatures, same visibility notation
- **Relationships**:
  - Inheritance (extends): solid line + hollow triangle arrowhead, child → parent
  - Implementation (interface): dashed line + hollow triangle, class → interface
  - Association: solid line + open arrowhead, label with multiplicity (1, 0..*, 1..*)
  - Aggregation: solid line + hollow diamond on container side
  - Composition: solid line + filled diamond on container side
  - Dependency: dashed line + open arrowhead
- **Interface**: `<<interface>>` stereotype above name, or circle/lollipop notation
- **Enum**: compartment rect with `<<enumeration>>` stereotype, values in bottom
- Layout: parent classes top, children below; interfaces to the left/right of implementors
- ViewBox: `0 0 960 600` standard; `0 0 960 800` for deep hierarchies

### Use Case Diagram (UML)
System functionality from user perspective.
- **Actor**: stick figure (circle head + body line) placed outside system boundary
  - Label below figure, 13-14px
  - Primary actors on left, secondary/supporting on right
- **Use case**: ellipse with label centered inside, min 140×60px
  - Keep names verb phrases: "Create Order", "Process Payment"
- **System boundary**: large rect with dashed border + system name in top-left
- **Relationships**:
  - Include: dashed arrow `<<include>>` from base to included use case
  - Extend: dashed arrow `<<extend>>` from extension to base use case
  - Generalization: solid line + hollow triangle (specialized → general)
- Layout: system boundary centered, actors outside, use cases inside
- ViewBox: `0 0 960 600` standard

### State Machine Diagram (UML)
Lifecycle states and transitions of an entity.
- **State**: rounded rect with state name, min 120×50px
  - Internal activities: small text `entry/ action`, `exit/ action`, `do/ activity`
  - **Initial state**: filled black circle (r=8), one outgoing arrow
  - **Final state**: filled circle (r=8) inside hollow circle (r=12)
  - **Choice**: small hollow diamond, guard labels on outgoing arrows `[condition]`
- **Transition**: arrow with optional label `event [guard] / action`
  - Guard conditions in square brackets
  - Actions after `/`
- **Composite/nested state**: larger rect containing sub-states, with name tab
- **Fork/join**: thick horizontal or vertical black bar (synchronization)
- Layout: initial state top-left, final state bottom-right, flow top-to-bottom
- ViewBox: `0 0 960 600` standard

### ER Diagram (Entity-Relationship)
Database schema and data relationships.
- **Entity**: rect with entity name in header (bold), attributes below
  - Primary key attribute: underlined
  - Foreign key: italic or marked with (FK)
  - Min width: 160px; attribute font-size: 12px
- **Relationship**: diamond shape on connecting line
  - Label inside diamond: "has", "belongs to", "enrolls in"
  - Cardinality labels near entity: `1`, `N`, `0..1`, `0..*`, `1..*`
- **Weak entity**: double-bordered rect with double diamond relationship
- **Associative entity**: diamond + rect hybrid (rect with diamond inside)
- Line style: solid for identifying relationships, dashed for non-identifying
- Layout: entities in 2-3 rows, relationships between related entities
- ViewBox: `0 0 960 600` standard; wider `0 0 1200 600` for many entities

### Network Topology
Physical or logical network infrastructure.
- **Devices**: icon-like rects or rounded rects
  - Router: circle with cross arrows
  - Switch: rect with arrow grid
  - Server: stacked rect (rack icon)
  - Firewall: brick-pattern rect or shield shape
  - Load Balancer: horizontal split rect with arrows
  - Cloud: cloud path (overlapping arcs)
- **Connections**: lines between device centers
  - Ethernet/wired: solid line, label bandwidth
  - Wireless: dashed line with WiFi symbol
  - VPN: dashed line with lock icon
- **Subnets/Zones**: dashed rect containers with zone label (DMZ, Internal, External)
- **Labels**: device hostname + IP below, 12-13px
- Layout: tiered top-to-bottom (Internet → Edge → Core → Access → Endpoints)
- ViewBox: `0 0 960 600` standard

## UML Coverage Map

Full mapping of UML 14 diagram types to supported diagram types:

| UML Diagram | Supported As | Notes |
|-------------|-------------|-------|
| Class | Class Diagram | Full UML notation |
| Component | Architecture Diagram | Use colored fills per component type |
| Deployment | Architecture Diagram | Add node/instance labels |
| Package | Architecture Diagram | Use dashed grouping containers |
| Composite Structure | Architecture Diagram | Nested rects within components |
| Object | Class Diagram | Instance boxes with underlined name |
| Use Case | Use Case Diagram | Full actor/ellipse/relationship |
| Activity | Flowchart / Process Flow | Add fork/join bars |
| State Machine | State Machine Diagram | Full UML notation |
| Sequence | Sequence Diagram | Add alt/opt/loop frames |
| Communication | — | Approximate with Sequence (swap axes) |
| Timing | Timeline | Adapt time axis |
| Interaction Overview | Flowchart | Combine activity + sequence fragments |
| ER Diagram | ER Diagram | Chen/Crow's foot notation |

## Shape Vocabulary

Map semantic concepts to consistent shapes across all diagram types:

| Concept | Shape | Notes |
|---------|-------|-------|
| User / Human | Circle + body path | Stick figure or avatar |
| LLM / Model | Rounded rect with brain/spark icon or gradient fill | Use accent color |
| Agent / Orchestrator | Hexagon or rounded rect with double border | Signals "active controller" |
| Memory (short-term) | Rounded rect, dashed border | Ephemeral = dashed |
| Memory (long-term) | Cylinder (database shape) | Persistent = solid cylinder |
| Vector Store | Cylinder with grid lines inside | Add 3 horizontal lines |
| Graph DB | Circle cluster (3 overlapping circles) | |
| Tool / Function | Gear-like rect or rect with wrench icon | |
| API / Gateway | Hexagon (single border) | |
| Queue / Stream | Horizontal tube (pipe shape) | |
| File / Document | Folded-corner rect | |
| Browser / UI | Rect with 3-dot titlebar | |
| Decision | Diamond | Flowcharts only |
| Process / Step | Rounded rect | Standard box |
| External Service | Rect with cloud icon or dashed border | |
| Data / Artifact | Parallelogram | I/O in flowcharts |

## Arrow Semantics

Always assign arrow meaning, not just color:

| Flow Type | Color | Stroke | Dash | Meaning |
|-----------|-------|--------|------|---------|
| Primary data flow | blue `#2563eb` | 2px solid | none | Main request/response path |
| Control / trigger | orange `#ea580c` | 1.5px solid | none | One system triggering another |
| Memory read | green `#059669` | 1.5px solid | none | Retrieval from store |
| Memory write | green `#059669` | 1.5px | `5,3` | Write/store operation |
| Async / event | gray `#6b7280` | 1.5px | `4,2` | Non-blocking, event-driven |
| Embedding / transform | purple `#7c3aed` | 1px solid | none | Data transformation |
| Feedback / loop | purple `#7c3aed` | 1.5px curved | none | Iterative reasoning loop |

Always include a **legend** when 2+ arrow types are used.

## Annotation Primitive (编辑标注原语)

继承自 cathrynlavery/diagram-design 的 editorial annotation 系统。

用于在图旁添加编辑级注释——不是技术标注，是"编者按"式的旁白。

**何时使用**：
- 指出反直觉的设计选择（"为什么这里不用缓存？"）
- 标注需要读者注意的细节
- 添加幽默或人性化的旁注

**SVG实现**：
```xml
<!-- Annotation: italic serif + dashed Bézier leader -->
<g class="annotation">
  <path d="M x1,y1 C cx1,cy1 cx2,cy2 x2,y2" stroke="#6b7280" stroke-dasharray="4,3" fill="none" stroke-width="1"/>
  <circle cx="x2" cy="y2" r="3" fill="#6b7280"/>
  <text x="x1+10" y="y1" font-family="Georgia, serif" font-style="italic" font-size="12" fill="#6b7280">
    注释文字
  </text>
</g>
```

**规则**：
- 字体：衬线斜体（Georgia/Noto Serif SC），与技术图的等宽字体形成对比
- 颜色：灰色 `#6b7280`，不与主图争注意力
- 位置：图的边缘，不进入核心内容区
- 数量限制：每张图最多2个annotation

## Layout Rules & Validation

**Spacing**:
- Same-layer nodes: 80px horizontal, 120px vertical between layers
- Canvas margins: 40px minimum, 60px between node edges
- Snap to 8px grid: horizontal 120px intervals, vertical 120px intervals

**Arrow Labels** (CRITICAL):
- MUST have background rect: `<rect fill="canvas_bg" opacity="0.95"/>` with 4px horizontal, 2px vertical padding
- Place mid-arrow, ≤3 words, stagger by 15-20px when multiple arrows converge
- Maintain 10px safety distance from nodes

**Arrow Routing**:
- Prefer orthogonal (L-shaped) paths to minimize crossings
- Anchor arrows on component edges, not geometric centers
- Route around dense node clusters, use different y-offsets for parallel arrows
- Jump-over arcs (5px radius) for unavoidable crossings

**Line Overlap Prevention** (CRITICAL - most common bug on Codex):
When two arrows must cross each other, ALWAYS use jump-over arcs to prevent visual overlap:
- Crossing horizontal arrows: add a small semicircle arc (radius 5px, stroke same color as arrow, fill none) that "jumps over" the other line
- SVG pattern for jump-over: use a white/matching-background arc on the lower layer, then draw the upper arc on top
- Multiple crossings: stagger arc radii (5px, 7px, 9px) so arcs don't overlap each other
- Never let two arrows' straight-line segments cross without a jump-over arc

**Validation Checklist** (run before finalizing):
1. **Arrow-Component Collision**: Arrows MUST NOT pass through component interiors (route around with orthogonal paths)
2. **Text Overflow**: All text MUST fit with 8px padding (estimate: `text.length × 7px ≤ shape_width - 16px`)
3. **Arrow-Text Alignment**: Arrow endpoints MUST connect to shape edges (not floating); all arrow labels MUST have background rects
4. **Container Discipline**: Prefer arrows entering and leaving section containers through open gaps between components, not through inner component bodies

## SVG Technical Rules

- ViewBox: `0 0 960 600` default; `0 0 960 800` tall; `0 0 1200 600` wide
- Fonts: embed via `<style>font-family: ...</style>` — no external `@import` (breaks rsvg-convert)
- `<defs>`: arrow markers, gradients, filters, clip paths
- Text: minimum 12px, prefer 13-14px labels, 11px sub-labels, 16-18px titles
- All arrows: `<marker>` with `markerEnd`, sized `markerWidth="10" markerHeight="7"`
- Drop shadows: `<feDropShadow>` in `<filter>`, apply sparingly (key nodes only)
- Curved paths: use `M x1,y1 C cx1,cy1 cx2,cy2 x2,y2` cubic bezier for loops/feedback arrows
- Clip content: use `<clipPath>` if text might overflow a node box

## SVG Generation & Error Prevention

**MANDATORY: Python List Method** (ALWAYS use this):
```python
python3 << 'EOF'
lines = []
lines.append('<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 960 700">')
lines.append('  <defs>')
# ... each line separately
lines.append('</svg>')

with open('/path/to/output.svg', 'w') as f:
    f.write('\n'.join(lines))
print("SVG generated successfully")
EOF
```

**Why mandatory**: Prevents character truncation, typos, and syntax errors. Each line is independent and easy to verify.

**Pre-Tool-Call Checklist** (CRITICAL - use EVERY time):
1. ✅ Can I write out the COMPLETE command/content right now?
2. ✅ Do I have ALL required parameters ready?
3. ✅ Have I checked for syntax errors in my prepared content?

**If ANY answer is NO**: STOP. Do NOT call the tool. Prepare the content first.

**Error Recovery Protocol**:
- **First error**: Analyze root cause, apply targeted fix
- **Second error**: Switch method entirely (Python list → chunked generation)
- **Third error**: STOP and report to user - do NOT loop endlessly
- **Never**: Retry the same failing command or call tools with empty parameters

**Validation** (run after generation):
```bash
rsvg-convert file.svg -o /tmp/test.png 2>&1 && echo "✓ Valid" && rm /tmp/test.png
```

**If using `generate-from-template.py`**:
- Prefer `source` / `target` node ids in arrow JSON so the generator can snap to node edges
- Keep `x1,y1,x2,y2` as hints or fallback coordinates, not the main routing primitive
- Let the generator choose orthogonal routes; avoid hardcoding center-to-center straight lines unless the path is guaranteed clear

**Common Syntax Errors to Avoid**:
- ❌ `yt-anchor` → ✅ `y="60" text-anchor="middle"`
- ❌ `x="390` (missing y) → ✅ `x="390" y="250"`
- ❌ `fill=#fff` → ✅ `fill="#ffffff"`
- ❌ `marker-end=` → ✅ `marker-end="url(#arrow)"`
- ❌ `L 29450` → ✅ `L 290,220`
- ❌ Missing `</svg>` at end

## Output

- **Default**: `./[derived-name].svg` and `./[derived-name].png` in current directory
- **Custom**: user specifies path with `--output /path/` or `输出到 /path/`
- **PNG export**: `rsvg-convert -w 1920 file.svg -o file.png` (1920px = 2x retina)

## Styles

| # | Name | Background | Best For |
|---|------|-----------|----------|
| 1 | **Flat Icon** (default) | White | Blogs, docs, presentations |
| 2 | **Dark Terminal** | `#0f0f1a` | GitHub, dev articles |
| 3 | **Blueprint** | `#0a1628` | Architecture docs |
| 4 | **Notion Clean** | White, minimal | Notionnce |
| 5 | **Glassmorphism** | Dark gradient | Product sites, keynotes |
| 6 | **Claude Official** | Warm cream `#f8f6f3` | Anthropic-style diagrams |
| 7 | **OpenAI Official** | Pure white `#ffffff` | OpenAI-style diagrams |

Load `references/style-N.md` for exact color tokens and SVG patterns.

## 🇨🇳 中文用户体验优化

继承自 axtonliu/obsidian-visual-skills 的中文生态经验。

### 触发词扩展
不仅支持英文 trigger，还支持中文自然语言：
- "帮我把这个系统画出来" → Architecture Diagram
- "来张时序图看看调用链" → Sequence Diagram
- "画个对比矩阵比较下这几个方案" → Comparison Matrix
- "把这个流程可视化一下" → Flowchart
- "出张思维导图" → Mind Map
- "帮我出个架构拓扑" → Network Topology

### 中文排版规则
- 中文字体栈：`"PingFang SC", "Microsoft YaHei", "Noto Sans SC", system-ui, sans-serif`
- 中文字号+1px（中文笔画密度高，同字号显小）
- 标签长度：中文3-6字，英文2-4词
- 数字/英文混排：中英文之间加半角空格（`RAG 检索` 而非 `RAG检索`）

### 中文品牌模板
常用中文技术品牌图标适配：
- 飞书/Lark, 钉钉, 企业微信 → 使用蓝色系rect+文字缩写
- 阿里云/腾讯云/华为云 → 品牌色填充
- 文心一言/通义千问/智谱 → AI模型色系（紫/蓝/绿）

## Style Selection

**Default**: Style 1 (Flat Icon) for most diagrams. Load `references/style-diagram-matrix.md` for detailed style-to-diagram-type recommendations.

These patterns appear frequently — internalize them:

**RAG Pipeline**: Query → Embed → VectorSearch → Retrieve → Augment → LLM → Response
**Agentic RAG**: adds Agent loop with Tool use between Query and LLM
**Agentic Search**: Query → Planner → [Search Tool / Calculator / Code] → Synthesizer → Response
**Mem0 / Memory Layer**: Input → Memory Manager → [Write: VectorDB + GraphDB] / [Read: Retrieve+Rank] → Context
**Agent Memory Types**: Sensory (raw input) → Working (context window) → Episodic (past interactions) → Semantic (facts) → Procedural (skills)
**Multi-Agent**: Orchestrator → [SubAgent A / SubAgent B / SubAgent C] → Aggregator → Output
**Tool Call Flow**: LLM → Tool Selector → Tool Execution → Result Parser → LLM (loop)
