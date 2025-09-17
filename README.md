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
