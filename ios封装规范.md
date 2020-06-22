 ##### 单例【推荐】
 头文件声明
 + (instancetype)shareManager

 源文件
 + (instancetype)shareManager {
     static RMYouthModelManager *instance;
     static dispatch_once_t onceToken;
     dispatch_once(&onceToken, ^{
         instance = [[super allocWithZone:NULL] init];
     });
     return instance;
 }

 + (id)allocWithZone:(struct _NSZone *)zone {
     return [RMYouthModelManager shareManager];
 }

 -(id) copyWithZone:(struct _NSZone *)zone
 {
     return [RMYouthModelManager shareManager];
 } 
 
 
  ##### 工具封装
 一、封装的目的
代码复用、提升内聚度

二、封装的原则
把尽可能多的东西藏起来，对外提供简捷的接口，单一职责原则


#####封装规范
#####封装类协议规范
一、封装类protocol的命名
类名+Delegate
eg：UITableView的协议UITableViewDelegate

二、协议参数
协议方法参数需要返回封装类的对象，如果有多个参数，那么第一个参数是封装类的对象
多个参数的方法
eg：
- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section
单个参数的方法
eg：
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView

三、协议方法的命名
多个参数的
控件作为方法名 +will/did事件作为参数名
eg：
- (void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath;
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath；
单个参数
控件+事件
- (void)scrollViewDidScroll:(UIScrollView *)scrollView

BOOL返回值的
should+表意字符
- (BOOL)tableView:(UITableView *)tableView shouldHighlightRowAtIndexPath:(NSIndexPath *)indexPath

四、协议关键字
非必须实行的协议要用@optional修饰，否则实现的类会有警告

五、协议归类
如果协议方法的职责不一样，需要按功能职责分类，不能全部写在一个协议里
eg：
UITableView的UITableViewDataSource和UITableViewDelegate协议


#####封装类规范
一、封装类的初始化方法
初始化一个对象所必须的信息，需要在初始化方法通过参数传进去，不能通过属性赋值。
eg：
初始化UITableView必要的UITableViewStyle
- (instancetype)initWithFrame:(CGRect)frame style:(UITableViewStyle)style

二、方法的参数
封装类方法参数数量不宜过多，原则上不超过4个，参数比较多的可以包装成NSDictionary或者对象model传值。
eg：
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions

三、属性
尽可能对外暴露方法，而不是属性。封装类内部使用的属性，不在头文件暴露出来；在外部只读的属性需要用readonly修饰
eg：
@property (nonatomic, weak, readonly, nullable) id<UIPointerInteractionDelegate> delegate;
对于BOOL值的属性，不建议用isXXX命名，而是修改get方法，以便代码阅读
eg：
@property (nonatomic, getter=isEnabled) BOOL enabled;







