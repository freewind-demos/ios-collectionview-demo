# iOS CollectionView Demo

## 简介

展示 UICollectionView 的基本用法：网格布局。

## 启动和使用

在 Xcode 中打开 `ios-collectionview-demo.xcodeproj`，运行即可。

## 教程

### 创建 CollectionView

```swift
let layout = UICollectionViewFlowLayout()
layout.itemSize = CGSize(width: 100, height: 100)
let collectionView = UICollectionView(frame: .zero, collectionViewLayout: layout)
```
