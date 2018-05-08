## Swift 编程规范

> 良好的编程规范有利于项目的维护，同时也能避免因人为的疏忽产生的错误。本篇编码规范主要参考了 [swift-style-guide](https://github.com/raywenderlich/swift-style-guide/blob/master/README.markdown)和[The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/swift_programming_language/index.html)当中的规范，并综合了实际开发中总结的经验。




### 正确性 (Correctness)

力争让你的代码没有编译警告(`warning`)。这个规则下延伸出了许多决定，如：使用`#selector`方式，而不是直接使用字符串。



### 命名 (Naming)

描述性和一致性的命名使得代码更易读、更易懂。一些规则如下：

* 追求调用方的清晰性
* 优先使用更清晰的命名，而不是更简洁的
* 使用驼峰样式
* 类型、协议名首字母大写，其他的都首字母小写
* 包含所有需要的单词，省略不必要的
* 使用基于角色的命名，而不是基于类型的
* 工厂方法使用`make`开头
* 对方法的命名
	* 动词方法以`-ed`结尾，对于不可变的(`non-mutating`)动词方法用`-ing`结尾
	* 布尔类型(`boolean`)应该以`is`开头
	* 用于描述事物的协议(`protocols`)名称，用名词命名
	* 用于描述能力的协议(`protocols`)名称，应以`-able` 或 `-ible`结尾
* 不要用生僻的单词
* 通常不要用缩写
* 选用好的参数名来起到描述的作用

#### 类前缀 (Class Prefixes)

Swift中的类型自动使用了其所在的模块(`module`)作为命名空间(`namespace`)，所以你不必给类型加前缀。如果来自不同模块的类型名子冲突，可以显示的使用模块名作为调用前缀来避免冲突。

```swift
import SomeModule
import OtherModule

let someUsefulClass = SomeModule.UsefulClass()
let otherUsefulClass = OtherModule.UsefulClass()
```

#### 代理 (Delegates)

当定义一个代理方法时，第一个匿名参数应该是代理的源对象。(`UIKit`中有许多这样的的例子)

**推荐：**

```swift
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
```

**不推荐：**

```swift
/// `namePicker`应该是匿名的
func didSelectName(namePicker: NamePickerViewController, name: String)

/// 至少应包含`_ namePickerView: NamePickerView`参数，来作为代理的源对象
func namePickerShouldReload() -> Bool
```

#### 使用可类型推导的上下文 (Use Type Inferred Context)

利用好编译器的类型推导特性，来写出简短、清晰的代码。

**推荐：**

```swift
let selector = #selector(viewDidLoad)
view.backgroundColor = .red
let toView = context.view(forKey: .to)
let view = UIView(frame: .zero)
```

**不推荐：**

```swift
// 以下几种写法，都没有用到编译器的类型推导特性，写出来的代码较冗余
let selector = #selector(ViewController.viewDidLoad)
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
let view = UIView(frame: CGRect.zero)
```

#### 泛型 (Generics)

泛型参数(`generic type parameters`)应当使用具有描述性的驼峰样式来命名。当泛型参数不具有明确的关系或角色时，可使用一个大写的字母表示即可。如：`T`，`U`，`V`

#### 使用的语言 (Language)

应使用美式英语的拼写方式以匹配Apple的API。

**推荐：**

```swift
let color = "red"
```

**不推荐：**

```swift
let colour = "red"
```



### 代码组织 (Code Organization)

使用扩展(`extensions`)来组织你的代码，将代码划分到不同的功能中。每个扩展应以`// MARK: -`开头，来更好的区分扩展。

#### 协议的遵守 (Protocol Conformance)

具体来讲，当让一个`Model`遵守某个协议时，推荐添加一个独立的`Model`扩展(`extensions`)来遵守该协议。这样使得相关的协议方法能组织在一起。

**推荐：**

```swift
class MyViewController: UIViewController {
	// class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
	// table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
	// scroll view delegate methods
}
```

**不推荐：**

```swift
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
	// all methods
}
```

对于`UIKit`的视图控制器(`view controllers`)，可以考虑将生命周期(`lifecycle`)相关、自定义访问器(`custom accessors`)、`IBAction`独立不同的到类扩展中。

#### 无用的代码 (Unused Code)

无用的代码包括Xcode产生的模板代码、占位的注释等、方法的默认实现仅仅是调用`super`等，这些都应当移除掉。

**推荐：**

```swift
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
	return Database.contacts.count
}
```

**不推荐：**

```swift
override func didReceiveMemoryWarning() {
	super.didReceiveMemoryWarning()
	// Dispose of any resources that can be recreated.
}

override func numberOfSections(in tableView: UITableView) -> Int {
	// #warning Incomplete implementation, return the number of sections
	return 1
}

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
	// #warning Incomplete implementation, return the number of rows
	return Database.contacts.count
}
```

#### 减小引入 (Minimal Imports)

保存最小的引入(`imports`)。例如, 当只使用`Foundation`时，不要引入`UIKit`。



### 空格 (Spacing)

* 代码折行使用4个空格(`spaces`)
* 方法的大括号和其他大括号(`if`/`else`/`switch`/`while`等)，其左括号必须要和语句在同一行，并且右括号要换行。

**推荐：**

```swift
if user.isHappy {
	// Do something
} else {
	// Do something else
}
```

**不推荐：**

```swift
if user.isHappy
{
	// Do something
}
else {
	// Do something else
}
```

* 各个方法之间必须有一个空行，这使得代码视觉上更清晰。

* 方法的实现中，应当适当的添加空行来划分功能。过多的空行意味着你应该拆分这些功能到不同的方法中，通过这样来避免一个巨大的方法。

* 通常冒号(Colons)的左边应当没有空格，而在右边有一个空格。

	例外：

	1. 三目运算符(`ternary operator`) `? :`

		```swift
		let foo = isEnable ? "A" : "B"
		```

	2. 空字典(`empty dictionary `) `[:]`

		```swift
		let bar: [String: Int] = [:]
		```

	3. `#selector`语法的无参方法`(_:)`

		```swift
		let sel = #selector((_:))
		```

**推荐：**

```swift
/// 注意使用空格的位置
class TestDatabase: Database {
	var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
```

**不推荐：**

```swift
class TestDatabase : Database {
	var data :[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
```



### 注释 (Comments)

* 给方法或属性添加注释，使用`option + command + /`来让Xcode自动生成
* 给关键逻辑添加一些局部注释
* 注释要保持最新状态



### 类和结构体 (Classes and Structures)

**用哪个呢？**

要知道结构体(`struct`)是值类型，当事物不具有唯一性时，使用结构体。比如，一个数组`[a, b, c]`，那么另一个数组`[a, b, c]`就跟它是一样的，他们之间可以互为替换，这时应当使用结构体来表示。

类(`class`)是引用类型，当事物具有唯一性或有明确的生命周期时，使用类。比如，一个电话簿列表，它的每一条记录都是唯一的，这时就要用类来表示。

以下是一个类的定义：

```swift
class Circle: Shape {
    var x: Int
    var y: Int
  	var radius: Double
  	var diameter: Double {
		get {
			return radius * 2
        }
		set {
			radius = newValue / 2
		}
	}
    
    init(x: Int, y: Int, radius: Double) {
		self.x = x
		self.y = y
		self.radius = radius
    }

	convenience init(x: Int, y: Int, diameter: Double) {
		self.init(x: x, y: y, radius: diameter / 2)
	}

	override func area() -> Double {
		return Double.pi * radius * radius
	}
}

extension Circle: CustomStringConvertible {
	var description: String {
		return "center = \(_centerString) area = \(area())"
  	}
	private var _centerString: String {
		return "(\(x),\(y))"
	}
}
```

上面的代码遵循了以下规范：

* 属性、变量、常量、参数等语句的定义，都是在冒号后添加一个空格。如：`x: Int`、 `Circle: Shape`
* 方法之间保留一个空行
* 有默认的修饰符(如：`internal`)时不用重新添加。同样的，重载(`override`)方法时，不用重新添加访问修饰符`access modifier `
* 将功能组织整理到扩展中(`extensions`)
* 隐藏一些不必公开的实现细节，如`_centerString`属性用`private`来修饰。

#### 使用self (Use of Self)

在Swift中，可以省略`self`来访问一个对象的属性或方法。但为了消除使用的歧义，建议使用`self`来访问其属性或方法。

#### 计算属性 (Computed Properties)

为了简洁性，如果一个计算属性是只读(`read-only`)的，那么应省略`get { ... }`语句。`get { ... }`语句只在计算属性是读写(`read-write`)时，才要求使用。

**推荐：**

```swift
var diameter: Double {
	return radius * 2
}
```

**不推荐：**

```swift
var diameter: Double {
	get {
		return radius * 2
	}
}
```

#### 使用final标记 (Final)

出于某些原因，你可能不希望一个类被继承，这时你可以将其标记为`final`来表示它不能被继承。例如：一个单例，可能就不希望被继承。



### 函数声明 (Function Declarations)

将简短的函数声明保留在一行中，包括左括号。

```swift
func reticulateSplines(spline: [Double]) -> Bool {
	/* code goes here */
}
```

无返回值的函数可以省略`-> Void`返回值

```swift
func foo(arg: Int) {
	/* code goes here */
}
```

参数较多的函数可以像`Objective-C`中一样进行折行处理

```swift
func foo(arg1: Int, 
         arg2: Double, 
         arg3: String, 
         arg4: [Bool],
         arg5: () -> Void) {
	/* code goes here */
}
```



### 闭包表达式 (Closure Expressions)

当参数列表末尾只有一个闭包参数时，才应使用尾随闭包(`trailing closure`)语法。

**推荐：**

```swift
// 参数列表尾部只有一个闭包参数
UIView.animate(withDuration: 1.0) {
	self.myView.alpha = 0
}

// 参数列表尾部有多个闭包参数
UIView.animate(withDuration: 1.0, animations: {
	self.myView.alpha = 0
}, completion: { finished in
	self.myView.removeFromSuperview()
})
```

**不推荐：**

```swift
UIView.animate(withDuration: 1.0, animations: {
	self.myView.alpha = 0
})

UIView.animate(withDuration: 1.0, animations: {
	self.myView.alpha = 0
}) { finished in
	self.myView.removeFromSuperview()
}
```

对于单行的闭包，可以利用隐式返回(`implicit returns`)的特性:

```swift
list.sort { a, b in
	a > b
}
```

对于使用闭包的链式调用，应当让代码更清晰、更易读。可以借助空格(`spacing`)、换行(`line breaks`)和匿名参数(`anonymous arguments`)等方法来让代码更清晰、更易读，但这都依赖于你的选择。

```swift
let value = numbers.map { $0 * 2 }.filter { $0 % 3 == 0 }.index(of: 90)

let value = numbers
  .map {$0 * 2}
  .filter {$0 > 50}
  .map {$0 + 10}
```



### 类型 (Types)

应尽量使用`Swift`的原生类型，当然`Swift`提供了对`Objective-C`类型的桥接方法，你可以使用`Objective-C的所有方法。

**推荐：**

```swift
let width = 120.0                                    // Double
let widthString = (width as NSNumber).stringValue    // String
```

**不推荐：**

```swift
let width: NSNumber = 120.0                          // NSNumber
let widthString: NSString = width.stringValue        // NSString
```

#### 常量 (Constants)

常量使用关键字`let`定义，变量使用关键字`var`定义。除非值可变，否则都应使用关键字`let`定义。

在一个类型的内部，通过关键字`static let`来定义静态常量，这样可以更好的组织这些静态常量。

**推荐：**

```swift
/// 将所有的接口都定义在一个类型内部，方便外部使用
struct API {
    static let homeAPI: String = "https://www.example.com/home/"
    static let mineAPI: String = "https://www.example.com/mine/"
}

enum Math {
  static let e = 2.718281828459045235360287
  static let pi = 3.14159265358979323846264
}

let l = 2 * Math.pi * r
```

**不推荐：**

```swift
let homeAPI: String = "https://www.example.com/home/"
let mineAPI: String = "https://www.example.com/mine/"

let e = 2.718281828459045235360287
let pi = 3.14159265358979323846264

let l = 2 * Math.pi * r
```

类内部的静态方法和属性，有点类似于全局方法和属性。但应尽量避免使用全局方法和属性。有一些例外情况，比如当使用到`runtime`的`objc_getAssociatedObject()`函数时，需要定义个全局的key来作为参数：

```swift
extension UIView {
    var foo: Int? {
		return objc_getAssociatedObject(self, &_fooKey) as? Int
    }
}

private var _fooKey: Void?
```

#### 可选类型 (Optionals)

当使用`?`来定义可选类型时，表明它可以接受为`nil`的值。

当使用`!`来定义可选类型时，表明它可以接受为`nil`的值，但必须保证在使用它时，值不为`nil`。就像`ViewController`中的`view`在`viewDidLoad`被调用时就已经创建完成。

使用可选绑定(`optional binding`)来一次性解包单个或多个可选类型值。

**推荐：**

```swift
var foo: Int?
var bar: Bool?

if let foo = foo, let bar = bar {
	// 同时解包foo和bar后才会执行这里
}
```

**不推荐：**

```swift
var foo: Int?
var bar: Bool?

if let foo = foo {
	if let bar = bar {
		// 同时解包foo和bar后才会执行这里
	}   
}
```

#### 懒加载 (Lazy Initialization)

使用懒加载(`lazy initialization`)来延迟初始化，这是一个很好的特性。如：

```swift
// 对象不需要进行配置
lazy var foo: SomeBigType = SomeBigType()

// 对象需要进行配置时，使用{ ... }()这种形式
lazy var bar: OtherBigType = { [unowned self] in
	let obt = OtherBigType()
	obt.delegate = self
	return obt
}()
```

**注意：**

> 上面代码中使用到了`self`，所以需要使用`[unowned self]`来避免产生循环引用(`retain cycle`)

#### 类型推导 (Type Inference)

对于局部变量，尽量使用类型推导来让代码更紧凑。而对于成员变量来说，应尽量不要使用类型推导来让类的定义更清晰。

**推荐：**

```swift
struct Box: Shape {
    var width: Int = 10
    var height: Int = 10
    var foo: [Int] = []
    var bar: [String: Int] = [:]
    
    func scale(rate: Int) {
        let width = self.width * rate
        let height = self.height * rate
        self.width = width
        self.height = height
    }
}
```

**不推荐：**

```swift
struct Box: Shape {
    var width = 10
    var height = 10
    var foo = [Int]()
    var bar = [String: Int]()
    
    func scale(rate: CGFloat) {
        let width: Int = self.width * rate
        let height: Int = self.height * rate
        self.width = width
        self.height = height
    }
}
```

#### 语法糖 (Syntactic Sugar)

利用语法糖，使用更简短的声明方式：

**推荐：**

```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**不推荐：**

```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```



### 内存管理 (Memory Management)

可以通过使用`weak` 和 `unowned`来避免循环引用，但也可以直接使用值类型(`struct`, `enum`)来避免循环引用。

通过`guard let`的形式来产生`strongSelf`:

**推荐：**

```swift
resource.request().onComplete { [weak self] response in
	guard let strongSelf = self else {
		return
	}
                               
	let model = strongSelf.updateModel(response)
	strongSelf.updateUI(model)
}
```

**不推荐：**

```swift
// 当self的释放先于onComplete回调时，可能引起崩溃
resource.request().onComplete { [unowned self] response in
	let model = self.updateModel(response)
	self.updateUI(model)
}
```

**不推荐：**

```swift
// 当self的释放介于updateModel()和updateUI()方法之间时，可能会出现意想不到的情况
resource.request().onComplete { [weak self] response in
	let model = self?.updateModel(response)
	self?.updateUI(model)
}
```



### 访问控制 (Access Control)

一般来说，被标记为`private`、`fileprivate`的属性或方法都应以下划线(`_`)开头。

**推荐：**

```swift
private var _isEnabled: Bool
fileprivate var _isClosed: Bool

private func _foo() {
	// code goes here
}

fileprivate func _bar() {
	// code goes here
}
```

**不推荐：**

```swift
private var isEnabled: Bool
fileprivate var isClosed: Bool

private func foo() {
	// code goes here
}

fileprivate func bar() {
	// code goes here
}
```

但例外情况是，当标记被修饰为`private(set)`、`fileprivate(set)`时，不需要下划线(`_`)开头，因为他们都是可访问的属性，只是他们都是只读属性而已，如：

```swift
private(set) var isEnabled: Bool
fileprivate(set) var isClosed: Bool
```

一般将访问控制标记(`access control annotation`)放在声明的最前面，但例外情况是，当有属性标记`@IBAction`、`@IBOutlet`、`@discardableResult`、`@objc`时，需要将属性标记放在最前面。

```swift
private let _message = "Great Scott!"

class TimeMachine {  
    
	@IBOutlet private var _lbTitle: UILabel!
    
	@objc private func _foo() -> Bool {
        // code goes here
    }
}
```



### 控制流 (Control Flow)

优先使用 `for-in` 的方式而不用`while`。

**推荐：**

```swift
for _ in 0..<3 {
	print("Hello three times")
}

for (index, person) in attendeeList.enumerated() {
	print("\(person) is at position #\(index)")
}

for index in stride(from: 0, to: items.count, by: 2) {
	print(index)
}

for index in (0...3).reversed() {
	print(index)
}
```

**不推荐：**

```swift
var i = 0
while i < 3 {
	print("Hello three times")
	i += 1
}

var i = 0
while i < attendeeList.count {
	let person = attendeeList[i]
	print("\(person) is at position #\(i)")
	i += 1
}
```



### 使用Guard语句

通过使用`guard`来避免使用`if`时代码块嵌套过深的问题。

**推荐：**

```swift
func check(phone: String?, name: String?, age: Int) -> Bool {
	guard let ph = phone, ph.count > 0 else {
        return false
    }
    
    guard let nm = name, nm.count > 0 else {
        return false
    }
    
    guard age > 0 else {
        return false
    }
    
    return true
}
```

**推荐：**

```swift
func check(phone: String?, name: String?, age: Int) -> Bool {
    // 把对phone、name、age的判断放到一个guard里，更简洁
    guard 
    	let ph = phone, ph.count > 0,
    	let nm = name, nm.count > 0,
		age > 0 else {
        return false
    }
    
    return true
}
```



**不推荐：**

```swift
func check(phone: String?, name: String?, age: Int) -> Bool {
    if let ph = phone, ph.count > 0 {
        if let nm = name, nm.count > 0 {
            if age > 0 {
                return true
            } else {
                return false
            }
        } else {
            return false
        }
    } else {
        return false
    }
}
```



### 分号 (Semicolons)

在Swift中不要求必须加分号，一般情况下你不需要使用分号。只有当你希望将多行代码写在一行时，才需要加分号来断句。



### 圆括号 (Parentheses)

在`if`语句中不需要加圆括号。只有当你在进行数学运算时，希望代码可读性更高时才需要适当的添加圆括号。

**推荐：**

```swift
let result = ((x * y) + 2) / h
```

**不推荐：**

```swift
let result = (x * y + 2) / h
```



### 引用

* [swift-style-guide](https://github.com/raywenderlich/swift-style-guide/blob/master/README.markdown)
* [The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/swift_programming_language/index.html)
