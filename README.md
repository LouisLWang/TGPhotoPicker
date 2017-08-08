# TGPhotoPicker
the best photo picker plugin in swift

![Swift](https://img.shields.io/badge/Swift-3.0-orange.svg)
![Build](https://img.shields.io/badge/build-passing-green.svg)
![License MIT](https://img.shields.io/badge/license-MIT-green.svg?style=flat)
![Platform](https://img.shields.io/cocoapods/p/Pastel.svg?style=flat)
![Cocoapod](https://img.shields.io/badge/pod-v0.0.1-blue.svg)

## Recently Updated
- 0.0.1 丰富的参数，DIY你满意的一款photo picker

## Features
- [x] 支持链式编程配置，程序员的最爱
- [x] 支持Cocoapods
- [x] 支持2种遮罩模式（直接在选择的照片cell上显示遮罩、选择到最大照片数量后其余照片cell显示遮罩）
- [x] 支持2种删除模式（选择完成后直接点每个照片cell上的删除按钮删除、选择完成后预览单个照片大图时点工具栏上的删除按钮删除）
- [x] 支持选择指示器选择时的顺序数字显示（每个照片cell的状态有5种状态:未选择、选中状态、数字选中状态、删除状态、按住删除按钮时的状态）
- [x] 支持2种选择模式（直接选择、预览选择）
- [x] 预置weibo、wechat 2种成组配置模式，省去多个参数配置，简化为一句代码配置
- [x] 支持8种选择样式（类型）单勾、圈、方块、带、斜带、三角、心、星
- [x] 支持4种选择位置（左上、左下、右上、右下）
- [x] 支持tinColor统一设置风格
- [x] 支持选择指示器大小调节
- [x] 轻量级、使用超灵活、功能超强大
- [x] 用例丰富，快速上手

## Usage
总体分为2种使用方式，有界面的话，用TGPhotoPicker实例化（即多选照片选择完成后把数据呈现在控件上），不需要界面的话用TGPhotoPickerManager.shared.takePhotoModels单例方法取多选照片数据（这个又分两种，用模型或不用模型（直接用分开的数组））

#### 使用默认（有界面）
```swift
lazy var picker: TGPhotoPicker = TGPhotoPicker(self, frame: CGRect(x: 0, y: 50, width: UIScreen.main.bounds.width, height: 200))

override func viewDidLoad() {
        super.viewDidLoad()
        //放到界面中去
        self.view.addSubview(picker)
}
```

#### 带配置（有界面）
```swift
    lazy var picker: TGPhotoPicker = TGPhotoPicker(self, frame: CGRect(x: 0, y: 50, width: UIScreen.main.bounds.width, height: 200)) { (config) in
        config.type = .weibo
        //更多配置在这里添加
    }
```

#### 其他使用方式（无界面） 模型数组
```swift
TGPhotoPickerManager.shared.takePhotoModels(true, true) { (array) in
            //示例代码
            self.picker.tgphotos.removeAll()
            self.picker.tgphotos.append(contentsOf: array)
            DispatchQueue.main.async {
                self.picker.reloadData()
            }
        }
```

#### 其他使用方式（无界面） 4个分开的数组
```swift
TGPhotoPickerManager.shared.takePhotos(true, true, { (config) in
            //链式配置
            config.tg_type(type: TGPhotoPickerType.weibo)
                .tg_confirmTitle("我知道了")
                .tg_maxImageCount(max: 12)
        }) { (asset, smallImg, bigImg, data) in
            //示例代码
            self.picker.tgphotos.removeAll()
            for i in 0..<smallImg.count {
                let model = TGPhotoM()
                model.asset = asset[i]
                model.smallImage = smallImg[i]
                model.bigImage = bigImg[i]
                model.imageData = data[i]
                self.picker.tgphotos.append(model)
            }
            DispatchQueue.main.async {
                self.picker.reloadData()
            }
        }
```

#### 使用控件中的数据
```swift
func upLoadData(){
        var dataArray = [Data]()
        for model in picker.tgphotos {
            dataArray.append(model.imageData!)
        }
        //上传Data数组
    }
```

#### 可以配置的属性（以下为部分可以配置的参数,完整配置参数见TGPhotoPickerConfig.swift（其中官方Demo中列出了主要的24个参数的使用效果））
``` swift
    /** 与useCustomSmartCollectionsMask结合使用,当useCustomSmartCollectionsMask为true时过滤需要显示smartAlbum的Album类型*/
    customSmartCollections 
    
    /** 使用自定义的PHAssetCollectionSubtype集合来过滤显示自己想要的相册夹,如想显示慢动作和自拍,那么上面的useCustomSmartCollectionsMask数组中设置为（或添加）
    useCustomSmartCollectionsMask: Bool = true
    
    /** 是否使用中文名称显示smartAlbum的Album名*/
    useChineseAlbumName: Bool = false
    
    /** 空内容的相册夹是否显示 */
    isShowEmptyAlbum: Bool = false
    
    /** 升序排列照片*/
    ascending: Bool = false
    
    /** 预置的成组配置, 微博 微信*/
    type: TGPhotoPickerType = .normal
    
    /** 在选择类型为方 带时用到的Corner*/
    checkboxCorner: CGFloat = 0
    
    /** 选择框显示的位置*/
    checkboxPosition: TGCheckboxPosition = .topRight
    
    /** 移除按钮显示的位置*/
    removePosition: TGCheckboxPosition = .topRight
    
    /** 移除类型,同选择类型*/
    removeType: TGCheckboxType = .diagonalBelt
    
    /** 是否显示选择顺序*/
    isShowNumber: Bool = true
    
    /** 纯数字模式下显示选择顺序时的数字阴影宽,不需要阴影设置为0*/
    shadowW:CGFloat = 1.0
    
    /** 纯数字模式下显示选择顺序时的数字阴影高,不需要阴影设置为0*/
    shadowH:CGFloat = 1.0
    
    /** 选择框类型（样式） 8种 */
    checkboxType: TGCheckboxType = .diagonalBelt
    
    /** 显示在工具栏上的选择框的大小*/
    checkboxBarWH: CGFloat = 30
    
    /** 显示在照片Cell上的选择框的大小*/
    checkboxCellWH: CGFloat = 20
    
    /** 选择框起始透明度*/
    checkboxBeginngAlpha: CGFloat = 1
    
    /** 选择框的结束透明度, 两者用于选择框渐变效果*/
    checkboxEndingAlpha: CGFloat = 1
    
    /** 选择框的画线宽度, 工具栏上返回、删除按钮的画线宽度*/
    checkboxLineW: CGFloat = 1.5
    
    /** 选择框的Padding*/
    checkboxPadding: CGFloat = 1
    
    /** 选择时是否动画效果*/
    checkboxAnimate: Bool = true
    
    /** 选择时或选择到最大照片数量时，当前或其他Cell的遮罩的透明度*/
    maskAlpha: CGFloat = 0.6
    
    /** 使用选择遮罩: false,当选择照片数量达到最大值时,其余照片显示遮罩; true,其余照片不显示遮罩,而是已经选择的照片显示遮罩 */
    useSelectMask: Bool = false
    
    /** 工具条的高度*/
    toolBarH: CGFloat = 44.0
    
    /** 相册类型列表Cell的高度*/
    albumCellH: CGFloat = 60.0
    
    /** 照片Cell的高宽,即选择时的呈现的宽高*/
    selectWH: CGFloat = 80
    
    /** 控件本身的Cell的宽高,即选择后的呈现的宽高*/
    mainCellWH: CGFloat = 80
    
    /** 自动宽高,用于控件本身Cell的宽高自动计算*/
    autoSelectWH: Bool = false
    
    /** true,在选择照片界面,点击照片（非checkbox区域）时,不跳转到大图预览界面,而是直接选择或取消选择当前照片; false, 点击照片checkbox区域选择或取消选择当前照片,点击非checkbox区域跳转到大图预览界面*/
    immediateTapSelect: Bool = false
    
    /** 控件或Cell之间布局时的padding*/
    padding: CGFloat = 1
    
    /** 左右没有空白,即选择时呈现的UICollectionView没有contentInset中的左右Inset*/
    leftAndRigthNoPadding: Bool = true
    
    /** 选择时呈现的UICollectionView的每行列数*/
    colCount: CGFloat = 4
    
    /** 选择后控件本身呈现的UICollectionView的每行列数*/
    mainColCount: CGFloat = 4
    
    /** 完成按钮标题*/
    doneTitle = "完成"
    
    /** 完成按钮的宽*/
    doneButtonW: CGFloat = 70
    
    /** 完成按钮的高*/
    doneButtonH: CGFloat = 30.8
    
    /** 导航工具栏返回按钮图标显示圆边 及 星（star）样式显示圆边*/
    isShowBorder: Bool = false
    
    /** 分多次选择照片时,剩余照片达到上限时的提示文字*/
    leftTitle = "剩余"
    
    /** 相册类型界面的标题*/
    albumTitle = "照片"
    
    /** 确定按钮的标题*/
    confirmTitle  = "确定"
    
    /** 选择数量达到上限时的提示文字, #为占位符*/
    errorImageMaxSelect = "图片选择最多不能超过#张"
    
    /** 拍摄标题*/
    cameraTitle = "拍摄"
    
    /** 选择标题*/
    selectTitle = "从手机相册选择"
    
    /** 取消标题*/
    cancelTitle = "取消"
    
    /** 选择时显示的数字的字体大小等*/
    fontSize: CGFloat = 15.0
    
    /** 预览照片的最大宽度*/
    previewImageFetchMaxW:CGFloat = 600
    
    /** 工具栏的背景色,有透明部分则全屏穿透效果*/
    barBGColor = UIColor(red: 40/255, green: 40/255, blue: 40/255, alpha: 0.9)
    
    /** 选择框 、按钮的颜色*/
    tinColor = UIColor(red: 7/255, green: 179/255, blue: 20/255, alpha: 1)
    
    /** 删除按钮的颜色*/
    removeHighlightedColor: UIColor = .red
    
    /** 删除按钮是否隐藏*/
    isRemoveButtonHidden: Bool = false
    
    /** 按钮无效时的文字颜色*/
    disabledColor: UIColor = .gray
    
    /** 最大照片选择数量上限*/
    maxImageCount: Int = 9
    
    /** 压缩比,0(most)..1(least) 越小图片就越小*/
    compressionQuality: CGFloat = 0.5
```
#### 使用链式编程配置时，请在所有属性前加tg_前缀即可

### 更多使用配置组合效果请download本项目或fork本项目查看

## diagonalBelt运行效果
![](https://github.com/targetcloud/TGPhotoPicker/blob/master/gif/diagonalBelt.gif) 


## circle运行效果
![](https://github.com/targetcloud/TGPhotoPicker/blob/master/gif/circle.gif) 


## Installation
- 下载并拖动TGPhotoPicker到你的工程中

- Cocoapods
```
pod 'TGPhotoPicker'
```

## Reference
- http://blog.csdn.net/callzjy
- https://github.com/targetcloud/TGImage <img src="https://github.com/targetcloud/TGImage/blob/master/snapShot/Banners.png" width = "12%" hight = "12%"/>

如果你觉得赞，请Star
