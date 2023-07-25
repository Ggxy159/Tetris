## 项目结构
Tetris
|---main.cpp    
|---src   
|    |---common.cpp   
|    |---common.h   
|    |---ID_generator.cpp   
|    |---ID_generator.h   
|    |---cursor.cpp   
|    |---cursor.h   
|    |---dmap.cpp   
|    |---dmap.h   
|    |---rankList.cpp   
|    |---rankList.h   
|    |---ui.cpp   
|    |---ui.h   
|
|---README.md    
|---CMakeLists.txt   
|---tetrisRanking.dat   

## 函数
1. `rotate(int)`：对应方块形状的旋转函数，将传入的方块ID（0-27）表示的方块进行旋转，并返回旋转后的方块ID。
2. `setCursor(int, int)`：设置控制台光标位置，传入x和y坐标。
3. `isCursorDisplay(bool)`：控制控制台光标的显示与隐藏，传入true表示显示，false表示隐藏。
4. `setColor(int)`：设置控制台输出的文本颜色，传入0-15的颜色代码。
5. `drawScore()`：绘制当前得分。
6. `drawBlock()`：绘制俄罗斯方块方块块。
7. `drawSpace()`：绘制空格。
8. `drawWelcome()`：绘制游戏欢迎界面，包括游戏说明和玩家输入名字等操作。
9. `drawUI()`：绘制游戏的基本界面，包括游戏区域边框、得分区域、下一个方块区域等。
10. `drawTetris(int, int, int, bool)`：在指定位置绘制俄罗斯方块。
11. `drawPrediction(int, bool)`：绘制下一个方块的预测区域。
12. `readRankList()`：从文件中读取排行榜数据。
13. `writeRankList()`：将排行榜数据写入文件。
14. `updateRankList()`：更新排行榜数据。
15. `drawRankList(int, int, int)`：在指定位置绘制排行榜。
16. `placeJudge(int, int, int)`：判断方块是否可以放置在指定位置。
17. `endGame(bool)`：游戏结束处理函数，包括保存排行榜数据、显示游戏结果等。


# main函数
1. 引入头文件：`main()`函数开头包含了一系列的头文件引入，这些头文件包含了程序所需的各种库函数和数据类型的定义。
2. 定义宏和全局变量：在`main()`函数之前，有一些宏定义和全局变量的声明，这些定义了一些常量和数据结构。
3. 初始化：程序进行了一些初始化操作，如设置随机数种子、设置控制台窗口大小和隐藏光标等。
4. 创建ID生成器：定义了一个名为`idGenerator`的`ID_generator`类的对象。
5. 读取排行榜数据：通过调用`readRankList()`函数读取排行榜数据，并将排行榜数据存储在`rankList`数组中。
6. 绘制UI和欢迎界面：调用`drawUI()`和`drawWelcome()`函数绘制游戏的用户界面和欢迎界面，引导玩家输入用户名。
7. 生成方块：使用`idGenerator`对象生成两个不同的随机方块标识`currBlock`和`nextBlock`，并绘制在游戏区域。
8. 进入游戏循环：进入一个无限循环，游戏会在该循环中持续进行，直到游戏结束。
9. 处理用户输入：在游戏循环中，程序不断检查用户的键盘输入，并根据输入执行相应的操作。例如，按上箭头键旋转方块，按左箭头键和右箭头键移动方块，按下箭头键加速下落等。
10. 方块下落和消除：游戏会定时触发方块的下落，当方块无法下落时，会进行消除判断，如果有可消除的行，则消除该行并增加得分。
11. 更新游戏数据：根据玩家的操作和方块的下落和消除，更新游戏的得分、速度等数据。
12. 绘制游戏界面：根据更新后的游戏数据，绘制更新后的游戏界面，包括方块的位置、得分、速度等信息。
13. 判断游戏结束：根据特定条件判断游戏是否结束，如果游戏结束，则进行最终的处理，包括更新排行榜和显示游戏结束信息。
14. 清理资源：释放动态分配的资源，并进行必要的清理操作。

## 分数
1. 消除行数线性加分
   - 每消除1行,分数增加 (300 - 下落间隔) x 消除行数 
   - 所以消除的越多,加分越多
2. 快速下落加分
   - 当块下落加速时,cntDown会比interval下落间隔值大
   - 此时以cntDown的对数作为加分数额
   - 引导玩家进行快速操作
3. 随着关卡增加的指数级加分
   - 每固定一块,会增加当前关卡计数
   - 这个计数的对数就是加分数额
   - 使得后期分数增长越来越快
4. 使用R键交换块时扣分
   - 每使用R键,分数扣除当前加分底数的2倍
5. 间隔缩短加分
   - 消除行时间隔缩短,然后给于加分