# 设计模式 Design Pattern

### 代码质量 - 常见评价标准

#### 可维护性\(maintainability\)

代码的可维护性是由很多因素协同作用的结果 . 代码的可读性好、简洁、可扩展性好 , 就会使得代码易维护 ;

相反就会使得代码不易维护 . 更细化地讲 , 如果代码分层清晰 , 模块化好 , 高内聚低耦合 , 遵从基于接口而非实现编程的设计原则等等 .

#### 可读性\(readability\)

代码的可读性应该是评价代码质量最重要的指标之一 , 需要看代码是否符合编码规范、命名是否达意、注释是否详尽、函数是否长短合适、模块划分是否清晰、是否符合高内聚低耦合等等 . Code Review是一个很好的测验代码可读性的手段 .

#### 可扩展性\(extensibility\)

它表示我们的代码应对未来需求变化的能力 . 在不修改或少量修改原有代码的情况下 , 通过扩展的方式添加新的功能代码 .

#### 灵活性\(flexibility\)

* 当添加一个新的功能代码的时候 , 原有的代码已经预留好了扩展点 , 不需要修改原有的代码 , 只要在扩展点上添加新的代码即可 . 
* 当要实现一个功能的时候 , 发现原有代码中 , 已经抽象出了很多底层可以复用的模块、类等代码，可以拿来直接使用 . 
* 当使用某组接口的时候 , 如果这组接口可以应对各种使用场景 , 满足各种不同的需求 . 

#### 简洁性\(simplicity\)

尽量保持代码简单 . 代码简单、逻辑清晰 , 也就意味着易读、易维护 . 例如著名的KISS原则 , Keep It Simple , Stupid .

#### 可复用性\(reusability\)

代码的可复用性可以简单地理解为 , 尽量减少重复代码的编写 , 复用已有的代码 . 可复用性也是一个非常重要的代码评价标准 , 是很多设计原则、思想、模式等所要达到的最终效果 . 跟 DRY\(Don’t Repeat Yourself\)设计原则的关系挺紧密 .

#### 可测试性\(testability\)

代码可测试性的好坏 , 能从侧面上非常准确地反应代码质量的好坏 . 代码的可测试性差 , 比较难写单元测试 , 那基本上就能说明代码设计得有问题 . 

![](/assets/bianxiegaozholiangdaima.png)

