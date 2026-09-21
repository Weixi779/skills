---
name: uikit-view-style
description: 按用户确认的 lazy var + then、子视图置于类末尾、setupUI 负责组装的个人习惯整理现有 UIKit View、Cell、Header 和 ViewController。适用于“按我的习惯整理”“这里也整理一下”等 UIKit 风格整理请求，以及明确指定此 skill 的任务；不用于普通功能开发、架构重构、SwiftUI 或单纯格式化。
---

# UIKit 视图整理

这是用户的个人写法偏好，不是适用于所有 UIKit 项目的统一规范。按请求整理现有代码，默认保留行为；当前任务的明确要求和项目规则优先。

## 确认整理范围

- 读完整的目标类型，以及理解其布局、生命周期和交互所需的少量关联代码。有 CodeGraph 时按项目规则查询符号和组件。
- 用户说“这个模块那边也都整理”时，沿用上下文，覆盖其中相关的 View、Cell、Header 等 UIKit 类型；不要顺带重写 Mapper、Model、Presenter、Service。
- 可参考用户指定的类型，但用户明确补充的偏好优先。例如参考类型把 Views 放在前面，这里仍按用户习惯放在类末尾。
- 已明确的本地整理直接完成，不另加方案确认环节。

## 用户确认的写法

### 子视图集中在类末尾

- 所有存储的子视图属性集中在类声明底部，放在方法之后，可用一个 `// MARK: - Views` 分组。
- 优先用 `private lazy var` 声明内部子视图，用 `.then` 配置固定外观：字体、颜色、行数、圆角、布局优先级等。只有构造参数的视图无需空的 `.then`。
- Stack View 也作为底部属性声明，用 `arrangedSubviews` 组合已声明的视图，在 `.then` 设置 axis、alignment、spacing。
- `lazy` 不是机械替换规则。保留协议要求、对外 API 或初始化时机所需的 `let` / `var`；不要把任务、订阅等有副作用的状态也顺手改成 lazy。
- 视图集合也归入这一组；循环创建的子视图可在局部使用 `.then`，不必为了形式改造成新的工厂或数组抽象。
- 不要为了把方法移进主类型而改变访问控制、override、协议派发或 Objective-C selector。已有 extension 承载独立职责时可以保留。

### 组装、数据更新和交互各有位置

- 普通 View / Cell / Header 的初始化调用 `setupUI()`；其中保留子视图组装和 SnapKit 约束。小类型一个方法就够，不机械拆成多层 setup 方法。
- 当 `addSubview` / `addArrangedSubview` 和 `snp.makeConstraints` 较多、混在一起影响阅读时，拆成 `setupViewHierarchy()` 和 `setupConstraints()`：前者集中添加子视图，后者集中安装约束，由 `setupUI()` 依次调用。名称可沿用项目惯例，不设机械的数量门槛；保留视图添加顺序，并确保安装约束前相关视图已进入正确层级。
- ViewController 的组装仍留在原有生命周期阶段，不为统一形式搬进 `init`。
- 固定样式从初始化中的逐项赋值收进对应属性的 `.then`；随模型、主题、trait、尺寸变化的配置仍留在原来的更新路径。
- `configure` 保留数据驱动的内容和状态，例如标题高亮、隐藏条件、订阅/购买文案、加载状态和按钮可用性。不要把这些误当成固定样式搬到初始化。
- 事件安装、回调、观察者和订阅保留原来的阶段和相对顺序；短小的绑定可留在 `setupUI`，已有 `setupInteractions` 等组织也可沿用。
- 可将初始化中连续的组装代码提取成 `setupUI`，保持调用阶段、操作顺序和执行次数。不为行数增加空 helper 或无意义分层。

写法示意（放在类型内对应位置，名称依实际职责选择）：

```swift
private func setupUI() {
    addSubview(contentStack)
    contentStack.snp.makeConstraints { $0.edges.equalToSuperview() }
}

// MARK: - Views

private lazy var titleLabel = UILabel().then {
    $0.numberOfLines = 2
}

private lazy var detailLabel = UILabel()

private lazy var contentStack = UIStackView(arrangedSubviews: [titleLabel, detailLabel]).then {
    $0.axis = .vertical
    $0.spacing = 8
}
```

## 先核实已有组件

整理不应只是把重复实现搬进 `.then`。遇到手写封面、头像、叠图或按钮时，检查项目是否已有对应组件，以及当前类型是否已经使用了它。

- 按具体展示职责区分。例如主封面、共创拼图、覆盖在主图上的节目小封面可能分别由不同组件负责；名字相近不代表能力相同。
- 阅读组件的实际更新、清空、点击、尺寸和布局接口，再判断能否复用。比较外观、图片顺序、占位图、主题、图片加载、空数据和复用行为。
- 单纯风格整理只检查并指出有价值的替换机会；用户已要求复用时直接落实匹配的局部替换，无需重复征求同一授权。
- 复用时删除调用方被组件接管的图片数组、堆叠布局、重复配置和更新逻辑。仍由调用方负责的数据选择、定位约束和事件路由保留。
- 外层 UIButton 可能承担 UIControl 事件和 VoiceOver 激活。组件有 `onTap` 不等于按钮可直接删除；先核对交互语义和无障碍能力。
- 如果组件无法满足所需行为，明确缺口。不要把本地整理扩成共享组件重设计，也不要无声改变外观或交互；已授权采用组件现有外观时，在结果中说明变化。

## 收尾

- 复核 diff：业务表达式、文案、回调捕获、selector、生命周期、订阅清理、复用和无障碍行为没有意外变化。纯整理不顺带修正其他问题。
- 使用项目规定的 formatter、lint 和适当验证；构建限制、主题规范、文件头等以当前项目规则为准，不在此重复固化。
- 只留下本次范围内的修改。共享工作区出现并行更新时，不根据旧快照批量回写文件；先确认当前状态。
- 简短说明整理了哪些类型、是否替换组件、验证结果及未执行的必要检查，不把格式或 lint 通过描述为运行时验证通过。
