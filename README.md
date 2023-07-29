# AWDJavaWebPatch
通过jar包快速生成patch模版

## 食用方式

编辑config.py

```python
javaPath = "/Library/Java/JavaVirtualMachines/zulu-17.jdk/Contents/Home/bin/java" #请填写java绝对路径
jarPath = "design-cms.jar" #如果使用的是war包请先将后缀改为jar
copyConfig=True #是否将IDEA配置进行复制
```

然后python3 main.py即可

## 运行流程

1. 用IDEA的插件jar包进行反编译（这个东东还验证文件扩展名，如果扩展名不是jar就不会反编译，这也是为什么要把jarPath以jar结尾）

2. 将对应Java文件和依赖jar包拷贝到project下（这里以jar包为实例）

   ```python
   shutil.copytree(f"./target/{jarName[0:-4]}/BOOT-INF/classes", f"./project/{jarName[:-4]}/src/main/java")
   shutil.copytree(f"./target/{jarName[0:-4]}/BOOT-INF/lib", f"./project/{jarName[:-4]}/lib")
   shutil.copytree(f"./IdeaTemplate/jar/.idea", f"./project/{jarName[:-4]}/.idea")
   
   
   ```

3. 不过此处有点小问题，在我测试的过程中直接将配置文件移动似乎并不能让IDEA进行自动修复并重构项目，又因为workspace等文件由于每个人机器不同所以无法进行统一的模版复制，所以下面会提供手动配置的方法（如果师傅有好的可以构建配置项目的方式可以提交哦）

   ```
   .idea/
   ├── artifacts
   │   └── NEWAWDJAR.xml		#此处存放对应组件的构建配置
   ├── design-cms.iml
   ├── libraries						#存储依赖配置
   │   └── lib.xml
   ├── misc.xml						#存储其他变量，比如class输出位置
   ├── modules.xml
   └── workspace.xml
   ```

## 手动配置

1. 打开项目，此时IDEA会自动做好默认配置
2. 设置SDK和class输出，在***文件-项目结构-项目设置-项目-SDK***中更改项目SDK，将编译器输出改为当前项目目录下的out文件夹
3. 设置源代码，在***文件-项目结构-项目设置-模块-当前项目***中将**src/main/java**标记为源代码
4. 设置依赖库，在***文件-项目结构-项目设置-库***中将lib文件夹添加为库文件夹
5. 设置jar包或war包打包，在***文件-项目结构-项目设置-工件***中添加jar或者Web应用程序，将清单文件设置为**META-INF/MANIFEST.MF**，将可用元素全部拖入即可

## 最后

个人推荐的用法是不进行jar包或war包的打包，而是通过本项目生成好IDEA项目之后只进行1、2、3步骤的配置，然后通过压缩软件对编译好的class文件直接进行替换，目前测试Bandizip可以保证替换前后jar包和war包的可用性

有部分的jar包war包反编译的class是经过编译器优化的，这也导致反编译后生成的Java文件会有奇怪的语法错误，这时候对症下药即可，也可以直接删除所有java文件只保留你需要进行patch的

最后祝各位师傅在AWD赛场上玩的愉快，不被环境困扰

























