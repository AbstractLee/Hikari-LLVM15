# Hikari-LLVM15
 A fork of HikariObfuscator.
 
 ## 原项目链接
 [https://github.com/HikariObfuscator/Hikari](https://github.com/HikariObfuscator/Hikari)

## 使用

下载后编译

### Swift混淆支持

编译Swift Toolchain的时间非常长。建议使用[Hanabi](https://github.com/NeHyci/Hanabi)

需要注意的是添加混淆参数的位置是在**Swift Compiler - Other Flags**中的**Other Swift Flags**，并且是在前面加-Xllvm，而不是-mllvm。

每次编译前需要先Shift+Command+K(Clean Build Folder)，因为Swift并不会像OC一样检测flag的修改就会重新编译

###  混淆选项

-aesSeed

指定cryptoutils的随机数生成种子。默认为0x1337

-enable-allobf

同时启用AntiClassDump, BogusControlFlow(虚假控制流), Flattening(控制流平坦化), FunctionCallObfusate(混淆函数调用), FunctionWrapper(封装函数调用), IndirectBranch(间接跳转), SplitBasicBlocks(切割基本块), StringEncryption(字符串加密), Substitution(指令替换)。默认关闭

#### AntiClassDump

-enable-acdobf

启用AntiClassDump。默认关闭

-acd-use-initialize

将动态注册代码添加到+initialize。默认开启

-acd-rename-methodimp

重命名在IDA中显示的方法函数(修改为ACDMethodIMP)。默认关闭

#### FunctionCallObfuscate

-enable-fco

启用FunctionCallObfuscate。默认关闭

-fcoconfig

FunctionCallObfuscate的配置文件路径，参照Hikari原项目的wiki

#### AntiHooking (修改过)

整体开启这个功能会使生成的二进制文件大小急剧膨胀，建议只在部分函数开启这个功能(toObfuscate)

支持检测Objective-C运行时Hook(通过dladdr检测，同时会检测dladdr有没有被inline hook)

InlineHook检测目前只支持ARM64平台。自动在函数中插入代码检测当前函数是否被Hook(通过判断BR X16和BR X17实现)，如果检测到就会调用AHCallBack函数(从PreCompiled IR获取)，如果不存在AHCallBack，就会退出程序。

-enable-antihook

启用AntiHooking。默认关闭

-adhexrirpath

AntiHooking PreCompiled IR文件的路径

#### AntiDebugging (修改过)

自动在函数中进行反调试，如果有InitADB和ADBCallBack函数(从PreCompiled IR获取)，就会调用ADBInit函数，如果不存在InitADB和ADBCallBack函数并且是Apple ARM64平台，就会自动在void返回类型的函数中插入内联汇编反调试，否则不做处理。

-enable-adb

启用AntiDebugging。默认关闭

-adb_prob

每个函数被添加反调试的概率。默认为40

-adbextirpath

AntiDebugging PreCompiled IR文件的路径

#### StringEncryption (修改过)

-enable-strcry

启用StringEncryption。默认关闭

-strcry-encrypt-global-string

如果发现原Hikari字符串加密无法处理的字符串，就插入函数到llvm.global_ctors来解密，存在缺陷，可能会在未来修改一下。默认关闭

#### SplitBasicBlocks

-enable-splitobf

启用SplitBasicBlocks。默认关闭

-split_num

每个基本块切割的数量。默认为2

#### BogusControlFlow (修改过)

-enable-bcfobf

启用BogusControlFlow。默认关闭

-bcf_prob

每个基本块被添加虚假控制流的概率。默认为70

-bcf_loop

虚假控制流在每个函数混淆的次数。默认为1

-bcf_cond_compl

生成分支条件的表达式复杂程度。默认为3

-bcf_junkasm

在虚假块中插入花指令，干扰IDA对函数的识别。默认关闭

-bcf_junkasm_minnum

在虚假块中花指令的最小数量。默认为1

-bcf_junkasm_maxnum

在虚假块中花指令的最大数量。默认为3

#### Flattening (修改过)

经过修改，支持混淆存在C++异常处理的函数

-enable-cffobf

启用Flattening。默认关闭

#### Substitution

-enable-subobf

启用Substitution。默认关闭

-sub_loop

Substitution在每个函数混淆的次数。默认为1

-sub_prob

每个基本块被Substitution混淆的概率。默认为50

#### IndirectBranch (修改过)

-enable-indibran

启用IndirectBranch。默认关闭

-indibran-use-stack

将跳转表地址和索引加载到栈中，再从栈中读取

-indibran-enc-jump-target

加密跳转表和索引

#### FunctionWrapper

-enable-funcwra

启用FunctionWrapper。默认关闭

-fw_prob

每个函数调用被FunctionWrapper混淆的概率。默认为30

-fw_times

FunctionWrapper在每个函数调用混淆的次数。默认为2

## 移植过程

暂无
