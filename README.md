# sudoku-myrepetition
my repetition of sudoku, to implement how a game or a software is constructed and how the structure looks like

# display_symbol.h
- 定义了绘画数独方格、指向格子指针等元素的样式
- 使用Unicode编码打印
# system_env.hpp
- 在windows平台下使用utf-8字符

# color.h
- 定义了一个用于在终端中输出彩色文本的工具。它使用了 ANSI 转义序列来控制终端的文本颜色和样式
``` c++
eunm Code{...}
class Modefier
	Code md, bg, fg;

public:
	Modefier(): //重置终端字体样式md、背景色bg、前景色fg
	Modefier(Mode,BackGround,FrontGround): //重置终端字体样式md、背景色bg、前景色fg
	operator<<()重载：使用ANSI转义序列控制终端文本样式颜色如
	`\033[1;31;42m` 表示加粗、红色前景、绿色背景
```

# common.h
- 定义了一些游戏里需要使用到的常量、枚举、结构体和类
- 为数独游戏提供通用的数据结构和配置
```C++
UNSELECTED //数字未被选择
enum class Difficulty:int // 游戏难度 1-3
enum class State:int //单元格的状态值
enum class KeyMode:int //键盘输入模式

struct KeyMap//键盘映射的结构 一部分固定，一部分由派生类初始化(UP\DOWN等)
struct Normal:KeyMap Vim:KeyMap //两种不同的键盘映射方式

point_t //二维坐标点 包含x,y
point_value_t //带状态的值，包含具体值及其状态(用户输入还是初始生成)

class CPointSort //用来比较两个点坐标是否一样的operator()重载
```


# utility.lni
- 一个内联工具文件
```C++
random() //生成范围内的随机整数
get_unit()  //返回一个包含1-9的整数向量
shuffle_unit() //将1-9的整数向量的顺序打乱
message() //向控制台打印msg
cls() //在windows和linux下分别调用cls或clear清屏
getch() //从控制台读取一个字符，无需回车键 (windows下#include <conio.h>)
```

# block.h

- 定义 `CBlock` 类，用于管理数独棋盘中的一个区块一行/一列/或是3*3
```C++ 
MAX_COUNT = 9
_count //已存储的单元格数量
point_value_t *_numbers[MAX_COUNT] //指针数组表示每个单元格
```

# block.cpp
- 对3*3方格的一些操作
```C++
CBlock() 初始化_count=0
isValid() //检测是否有重复的值
isFull() //检查是否有方格是未填入数字的或者没有值的
print()  //打印当前区块内容
push_back() //向区块中添加一个单元格
```

# i18n.h\i18n.cpp
- 实现 **国际化（Internationalization，简称 i18n）** 功能的文件
- 用于实现多语言
- 根据用户语言的偏好显示不同的文本内容
```C++

class Language uint32_t
	//语言类型
enum class Key //要翻译的文本键
Dict map<Key,string> 翻译字典
Dict english,chinese //分别定义在英文和中文语言下Key的值
Instance() //获得I18n的实例
SetLanguage() 
Get()   //获取对应字符串
```

# command.h ,command.cpp
- 用于实现 **命令模式（Command Pattern）** 的文件
- 将请求封装为一个对象，从而使你可以用不同的请求对客户进行参数化，并支持请求的排队、记录日志、撤销操作等功能。

```C++
CComand 用于封装一个操作

CScene* _pOwner 和CScene场景对象联系，形成命令对象-接收并执行者的联系
point_t _stPoint; 表示命令操作的目标单元格的坐标
int _nPreValue; 命令执行前的单元格值（撤销操作使用）
int _nCurValue; 命令执行后的单元格值


execute() 设置当前坐标的值(填数字)
undo() 撤销操作

```

# scene.h \scene.cpp
- 负责管理游戏的场景、状态和逻辑
```C++
class CScene //管理场景和状态
KeyMap //键盘映射
_max_column  棋盘大小
_cur_point当前选中的单元格坐标
_column_block 存储数独棋盘的列区块
_row_block 存储行区块
_xy_block  3*3的小格子区块
_map  所有单元格的数据(值和状态)
std::vector<CCommand> _vCommand; 存储命令历史，支持撤销

setMode() 设置键盘
show() 打印整个棋盘，按行打印
init() 初始化场景
printUnderline() 打印一行地形框，若是
generate()生成一个新的数独谜题
setValue() 填入数字
eraseRandomGrids() //随机选择一定个数的格子清空
isComplete() 检查游戏是否完成
getCurPoint() 获取当前所在格子
save()\load()保存，载入地图
play()游玩时的按键检测逻辑



```


# input.h \ input.cpp
管理输入的难度、语言、键盘映射
```C++
inputDIfficulty()
inputKeyMode()
InputLanguage()

```