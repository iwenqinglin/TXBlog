
##### 工具封装
  一、封装的目的
 代码复用、提升内聚度

 二、封装的原则
 把尽可能多的东西藏起来，对外提供简捷的接口，单一职责原则


三、封装规范

封装类protocol的命名【必须】
 类名+Delegate
 eg：UITableView的协议UITableViewDelegate

协议参数规范【推荐】
 协议方法参数需要返回封装类的对象，如果有多个参数，那么第一个参数是封装类的对象
 多个参数的方法
 eg：
 - (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section
 单个参数的方法
 eg：
 - (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView

协议方法的命名规范【推荐】
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

协议关键字【必须】
 非必须实行的协议要用@optional修饰，否则实现的类会有警告

协议归类【必须】
 如果协议方法的职责不一样，需要按功能职责分类，不能全部写在一个协议里
 eg：
 UITableView的UITableViewDataSource和UITableViewDelegate协议


封装类的初始化方法【推荐】
 初始化一个对象所必须的信息，需要在初始化方法通过参数传进去，不建议通过属性赋值。
 eg：
 初始化UITableView必要的UITableViewStyle
 - (instancetype)initWithFrame:(CGRect)frame style:(UITableViewStyle)style


方法的参数【推荐】
 封装类方法参数数量不宜过多，原则上不超过4个，参数比较多的可以包装成NSDictionary或者对象model传值。
 eg：
 - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions;


方法的暴露【必须】
封装类内部使用的方法不能声明在头文件，只有对外提供的接口才声明在头文件，尽可能对外只暴露方法，而不是属性。

属性【推荐】
 封装类内部使用的属性，不在头文件暴露出来；在外部只读的属性需要用readonly修饰
 eg：
 @property (nonatomic, weak, readonly, nullable) id<UIPointerInteractionDelegate> delegate;
 对于BOOL值的属性，不建议用isXXX命名，而是修改get方法，以便代码阅读
 eg：
 @property (nonatomic, getter=isEnabled) BOOL enabled;
 
 容器类型的参数或者属性，建议标明它元素的类型【推荐】
  eg：
  @property(nonatomic, copy) NSArray <QMUIAlertAction *> *actions;
  @property(nonatomic, copy) NSDictionary<NSAttributedStringKey, id> *sheetCancelButtonAttributes;
  
 接口需要传图片的参数类型用UIImage 而不是图片的路径NSString
  eg：
  - (void)setImageForState:(UIControlState)state withURL:(NSURL *)url  placeholderImage:(nullable UIImage *)placeholderImage;
  
 接口需要传url的参数类型用NSURL类型  而不是NSString类型
 eg：
- (void)setImageForState:(UIControlState)state  withURL:(NSURL *)url;
 
 接口参数可以传nil的建议用nullable/__nullable/_Nullable修饰，不能传nil的建议用_Nonnull或者方法声明在NS_ASSUME_NONNULL_BEGIN/NS_ASSUME_NONNULL_END之间
  eg：
  + (instancetype)actionWithTitle:(nullable NSString *)title style:(UIAlertActionStyle)style handler:(void (^ __nullable)(UIAlertAction *action))handler;
  - (NSDictionary *_Nullable)paramsForApi:(LAApiBaseManager *_Nonnull)manager
 
接口参数有多种状态的必须定义成枚举，不能定义为int类型
  eg：
  typedef void(^IAPCompletionHandle)(IAPResultType type,NSData *data);
  
 typedef NS_ENUM(NSUInteger,IAPResultType) {
     IAPResultSuccess = 0,       // 购买成功
     IAPResultFailed = 1,        // 购买失败
     IAPResultCancle = 2,        // 取消购买
     IAPResultVerFailed = 3,     // 订单校验失败
     IAPResultVerSuccess = 4,    // 订单校验成功
     IAPResultNotArrow = 5,      // 不允许内购
     IAPResultIDError = 6,       // 项目ID错误
 };


 
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
 
 
 
 







