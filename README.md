# Sootup-Paths-Analysis
使用sootup工具解析java方法，拆解路径并筛选关键路径

# SootUp 项目结构

## 目录结构

```
SootUp/
├── README.md                    # 项目说明文档
├── pom.xml                      # Maven配置文件
├── SootUpAnalyzer.java          # 核心分析器类
├── sootup_integration.py        # Python集成脚本
├── TestClass.java               # 测试用Java类
├── build.sh                     # Linux/Mac构建脚本
├── build.bat                    # Windows构建脚本
├── USAGE_GUIDE.md               # 详细使用指南
├── PROJECT_STRUCTURE.md         # 项目结构说明
└── target/                      # Maven构建输出目录
    └── sootup-analyzer-1.0.0.jar
```

## 文件说明

### 核心文件

1. **SootUpAnalyzer.java**
   - 主要的Java分析器类
   - 提供类分析、方法分析、调用图分析功能
   - 支持JSON格式输出

2. **sootup_integration.py**
   - Python集成脚本
   - 用于在AliPathMaker项目中调用SootUp
   - 提供文件编译和分析功能

3. **pom.xml**
   - Maven项目配置文件
   - 包含SootUp相关依赖
   - 配置构建和打包选项

### 测试文件

1. **TestClass.java**
   - 用于测试SootUp分析功能的示例Java类
   - 包含多种方法调用和语句类型

## 依赖关系

### Maven依赖
- sootup.core (2.0.0)
- sootup.java.bytecode.frontend (2.0.0)
- sootup.java.core (2.0.0)
- sootup.callgraph (2.0.0)
- sootup.analysis.intraprocedural (2.0.0)
- jackson-databind (2.15.2)
- slf4j-simple (2.0.7)

### 系统要求
- Java 11+
- Maven 3.6+
- Python 3.8+ (用于集成脚本)

大家重点关注一下CFGGeneration中的AllCFGToCSV方法，我们的关键路径筛选要加入到这个方法中。
简单解释一下这个AllCFGToCSV方法：
这个方法是为了将我们找到的CFG路径/依赖信息等写入一个csv文件（表格文件），我们要在它写入之前删掉不关键的路径，保留关键路径。
重点关注的两个数据结构：
private ArrayList<ArrayList<Integer>> routeList;
private transient Map<Integer,Stmt> Line2StmtMap;

第一个数据结构元素为数组的数组，sootup会给每个jimlple语句进行编号（这个大家应该懂，因为你们用过sootup，如果不懂的话再查一下），那么想记录目标方法的多条路径，只需要记录多个数组即可，例如：路径1是【1，3，6，8】，路径2是【1，2，4，6】，路径3是【2，5，6，7】等等，routelist就是用来记录这个的。

第二个数据结构是记录上述编号和stmt语句的对应关系，是一个map（int和stmt的映射关系），因为编号和stmt语句是一对一关系，具体使用看AllCFGToCSV中看csv写入时候是怎么用的就行。

大家的任务：
利用这两个数据结构，进行关键路径筛选，Line2StmtMap 中的值类型是sootup支持的Stmt类型，很多sootup相关方法可以对其使用。具体而言，我们需要先获得路径数目，然后设置一个默认值（比如40%，意味着最终筛选出的关键路径是原路径总数的40%），然后对每条路径计算加权评分，最终得到关键路径，将关键路径写入，非关键路径不写入。有问题随时联系！

