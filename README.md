# iOS CollectionView Demo

## 简介

本 Demo 展示 UICollectionView 的基本用法，使用网格布局显示颜色块。UICollectionView 是 iOS 中用于展示网格、瀑布流等复杂布局的控件，比 UITableView 更灵活。

## 基本原理

UICollectionView 是 UIKit 框架中用于显示集合视图的控件，其核心机制包括：

1. **布局系统**：使用 UICollectionViewLayout 或其子类（如 UICollectionViewFlowLayout）控制布局
2. **数据源与代理**：类似 UITableView，通过数据源提供数据，代理处理交互
3. **单元格复用**：通过 `dequeueReusableCell` 实现高性能滚动
4. **灵活的布局**：支持网格、瀑布流、分页等多种布局方式

## 教程

### 1. 创建 CollectionView

```swift
let layout = UICollectionViewFlowLayout()
layout.itemSize = CGSize(width: 100, height: 100)

let collectionView = UICollectionView(frame: .zero, collectionViewLayout: layout)
```

### 2. 设置布局参数

```swift
// 单元格大小
layout.itemSize = CGSize(width: 100, height: 100)

// 单元格间距
layout.minimumInteritemSpacing = 10  // 水平间距
layout.minimumLineSpacing = 10       // 垂直间距

// 边距
layout.sectionInset = UIEdgeInsets(top: 20, left: 20, bottom: 20, right: 20)
```

### 3. 注册与数据源

```swift
collectionView.register(UICollectionViewCell.self, forCellWithReuseIdentifier: "Cell")
collectionView.dataSource = self
collectionView.delegate = self
```

### 4. 实现数据源方法

```swift
func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
    return colors.count
}

func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
    let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "Cell", for: indexPath)
    cell.backgroundColor = colors[indexPath.item]
    return cell
}
```

## 关键代码详解

本 Demo 使用 UICollectionViewFlowLayout 实现网格布局：

### 布局配置

```swift
let layout = UICollectionViewFlowLayout()
layout.itemSize = CGSize(width: 100, height: 100)
layout.minimumInteritemSpacing = 10
layout.minimumLineSpacing = 10
layout.sectionInset = UIEdgeInsets(top: 20, left: 20, bottom: 20, right: 20)
```

**参数说明**：
- `itemSize`：每个单元格的大小（100x100）
- `minimumInteritemSpacing`：同一行内单元格之间的最小间距
- `minimumLineSpacing`：行与行之间的最小间距
- `sectionInset`：整个 collectionView 的内边距

### CollectionView 初始化

```swift
let collectionView = UICollectionView(frame: .zero, collectionViewLayout: layout)
collectionView.dataSource = self
collectionView.delegate = self
collectionView.register(UICollectionViewCell.self, forCellWithReuseIdentifier: "Cell")
collectionView.backgroundColor = .systemBackground
```

**注意**：
- `frame = .zero` 配合 Auto Layout 使用
- 必须设置 `backgroundColor`，否则默认透明

### 数据源方法

```swift
func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
    return colors.count
}
```
- `numberOfItemsInSection`：返回每组的项目数
- 注意这里用 `indexPath.item` 而不是 `indexPath.row`

```swift
func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
    let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "Cell", for: indexPath)
    cell.backgroundColor = colors[indexPath.item]
    cell.layer.cornerRadius = 10
    return cell
}
```

**关键点**：
- 使用 `indexPath.item` 获取项索引（与 TableView 的 `row` 区别）
- 为每个单元格设置背景色
- 添加圆角使外观更美观

### 点击事件处理

```swift
func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
    let alert = UIAlertController(title: "提示", message: "选择了第 \(indexPath.item + 1) 个颜色", preferredStyle: .alert)
    alert.addAction(UIAlertAction(title: "确定", style: .default))
    present(alert, animated: true)
}
```
- 用户点击时显示提示框
- 注意索引从 0 开始，加 1 显示从 1 开始的编号

## 运行效果

1. 屏幕显示一个 2x4 的颜色网格，共 8 个颜色块
2. 每个颜色块为 100x100 的圆角方形
3. 颜色块之间有 10pt 间距
4. 网格四周有 20pt 的边距
5. 点击任意颜色块，会弹出提示框显示选择了第几个颜色
