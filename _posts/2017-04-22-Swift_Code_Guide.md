---
layout: post
title: "Swift Code Guide (Raywenderlich) 翻译"
categories: journal
tags: [documentation,sample]
image:
  feature: wukong.jpg
  teaser: wukong.jpg
  credit: Death to Stock Photo
  creditlink: ""
---

	·	Correctness 正确性
	◦	多使用#selector
	·	Naming 命名
	◦	清晰 > 简洁
	◦	工厂方法用"make"
	◦	动词方法(mutating)用ed后缀， non-mutating用ing后缀
	◦	布尔值 应该是 像一个断言 也就是要加上一个is前缀
	◦	协议中的描述某种能力 应该用 -able 或者 -ible
	·	Delegates 委托
	◦	协议的第一个没有命名的参数 应该是 委托的来源
	·	Use Type Inferred Context 使用上下文推断
	◦	View.backgroundColor = .red
	◦	Let view = UIView(frame: .zero)
	·	Generics 范型
	◦	使用有描述性的 大写驼峰 没有意义才用T U V
	◦	Element   Target 等等 这些有意义的类型标注
	·	Code Organization 代码组织
	◦	用extension 来为代码划分逻辑块 并且要在前面写上 //MARK: - …
	·	Protocol Conformance 协议的遵从
	◦	遵从协议 要用extension  把相关的方法 放在一个extension中 这样更加清晰 有序
	◦	对于UIKit View Controller 考虑 把重写的 lifecycle 、 IBOutlet 、 IBAction 这三者放在 extension 中
	·	Unused Code
	◦	没用的代码 要移除 比如 placeholder 比如 Xcode 模版代码
	·	Minmal Imports  最小导入
	◦	导入UIKit 就不要导入 Fundation 了
	·	Spacing 间隔
	◦	锁进用两个spaces就好 节省空间 房子代码行跳行 不美观
	◦	If / else / switch / while 的花括号 起始于同一行 结束于新的一行
	◦	CMD + A 全选  CTRL + I 重新对齐
	◦	方法之间要有一行的空白行方便组织和 美观
	◦	冒号的左边没有空格  而 右边需要一个空格 除了三元符 ? : 和空白的指点[:] 和未命名参数的#selector(_:)
	·	Comments 注释
	◦	有必要注释代码 就 注释为什么 这段代码
	◦	不要把注释写到代码中 代码应该是可以自我说明的 
	◦	注释写到代码前面
	·	Classes and Structures  类和结构
	◦	不需要区分identity的用 值类型的 结构
	◦	需要区分identity的用引用类型 类
	◦	如何规范的定义一个类
	·	定义的冒号后要有一个空格  x: Int
	·	相同类型、目的的成员共享一行 var x: Int, y: Int
	·	默认的访问修饰符就不要重写了 等于废话
	·	额外的方法 用extension来组织
	·	不公开的用private
	◦	为了简洁 不要在类里面用self 来调用成员 除非在逃逸闭包中 或者在init 中消除 和 参数重名的歧义
	◦	为了简洁 在计算属性中 如果是只读的 就不要get花括号 直接return
	◦	如果有必要 不然重写派生 用final
	·	函数的声明 
	◦	追求一行解决
	◦	实在一行无法写下去 函数声明太长 就在适合的地方换行 记得第二行要有一个锁进
	·	闭包表达式
	◦	能用尾随闭包的就用尾随闭包 看起来更美观
	◦	闭包参数的命名不要随意 要有意义
	·	Types 类型
	◦	用Swift的原生类型 因为都桥接了 OC 而不要用OC的类型NS前缀的那些
	·	Constants 常量
	◦	常量 就用一个类型常量 而不是实例常量 static let
	·	enum Math {  static let e = 2.718281828459045235360287  static let root2 = 1.41421356237309504880168872}let hypotenuse = side * Math.root2这种写法 没有case  使得避免被意外实例化 而且用这个enum就像是一个命名空间
	·	Optionals 可空类型
	◦	命名不要有 "optional" "maybe" 等字眼 因为它们的类型就可以说明了 不要废话
	·	Lazy Initialization  延迟存储属性
	◦	对于耗费大的对象 考虑用lazy修饰 
	◦	这个属性的可以直接接一个闭包初始化 {} ()
	◦	或者用一个私有工厂方法来初始化
	·	lazy var locationManager: CLLocationManager = self.makeLocationManager()private func makeLocationManager() -> CLLocationManager {  let manager = CLLocationManager()  manager.desiredAccuracy = kCLLocationAccuracyBest  manager.delegate = self  manager.requestAlwaysAuthorization()  return manager}
	·	Type Inference 类型推断
	◦	为了简洁 用类型推断 除非特殊需要 比如 CGRect Int16
	◦	对于空的数组和字典 用类型注释 
	·	提倡用 var names: [String] = [] // 有类型注释
	▪	同理 var lookup: [String: Int] =  [:]
	·	不提倡用 var names = [String]() //没有类型注释
	▪	不提倡 var lookup = [String: Int]()
	·	Syntactic Sugar 语法糖
	◦	提倡var deviceModels: [String]
	◦	反对var deviceModels: Array<String>
	·	Functions VS Methods 函数与方法
	◦	避免使用free 函数 而是要和type 结合 这样的目的是可读性 和 可发现性
	◦	除非没跟type挂钩的 范型函数
	·	Memory Management 内存管理
	◦	即使是教程代码 也不能产生循环引用 使用 Object Graph 来防止循环引用 使用 weak unowned 或者  使用 结构 struct  enum 来完全避免循环引用
	◦	使用 [weak self] 搭配 guard let strongSelf = self else {return} 这样的组合优于[unowned self]  
	·	Access Control 访问控制
	◦	提倡用private 优于 fileprivate(除了 extension)
	◦	提倡  访问控制符放在最前面 除了(static @IBOutlet @IBAction @discardableResult
	·	Control Flow 控制流
	◦	提倡for-in 优于 while
	·	Golden Path 好的路径
	◦	使用guard let  来排除坏的情况 可以多个return
	◦	而不要使用 if 的嵌套 这样可读性不好
	◦	如果有多个Optional要解包 使用连续的guard 复合在一起 这样美观 可读性好
	◦	guard let number1 = number1,       let number2 = number2,       let number3 = number3 else {   fatalError("impossible") }
	◦	在guard的else中要用简单的语句 比如return throw break continue， fatalError() 等 而不要有复杂的逻辑 如果要处理清理工作  在defer block 中处理 这样只有一次
	·	Semicolons 分号
	◦	不要写分号
	·	Parentheses 括号
	◦	条件语句的条件部分 不要写括号(OC是强制要括号的 Swift 不要)
	◦	如果可以增加可读性 不用吝啬用括号 比如 三元表达式
	·	Organization and Bundle Identifier 组织和bundle的id
	◦	Bundle Identifier 用 com.组织名.项目名
	·	Smiley Face 笑脸标志
	◦	提倡用: ] 
	◦	不提倡用 :) 
	◦	因为 这个方括号最大的俘获 括号表示半心半意
	·	           

	

