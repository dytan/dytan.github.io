---
layout:     post
title:      "LibSVM 在 Matlab中的使用"
subtitle:   "LibSVM at Matlab"
date:       2016-04-19 12:00:00
author:     "dytan"
header-img: "img/post-bg-re-vs-ng2.jpg"
header-mask: 0.3
catalog:    true
tags:
    - SVM
    - Matlab
    - ML
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

    
>[文章](http://noalgo.info/363.html)来自[NoAlGo](http://noalgo.info)博客原创，感谢NoAlGo.

支持向量机(SVM，Support Vector Machine）是一种基于统计学习理论的模式识别方法，在解决小样本、高维度及非线性的分类问题中应用非常广泛。<br />LIBSVM是一个由台湾大学林智仁(Lin Chih-Jen)教授等开发的SVM模式识别与回归的软件包，使用简单，功能强大，本文主要介绍其在Matlab中的使用。

## 一 安装

### 1. 下载

在LIBSVM的[主页]("http://www.csie.ntu.edu.tw/~cjlin/libsvm/index.html" rel="external nofollow")上下载最新版本的软件包，并解压到合适目录中。

### 2. 编译

如果你使用的是64位的操作的系统和Matlab，那么不需要进行编译步骤，因为自带软件包中已经包含有64位编译好的版本：libsvmread.mexw64、libsvmwrite.mexw64、svmtrain.mexw64、svmpredict.mexw64。

否则，需要自己编译二进制文件。
首先在Mtlab中进入LIBSVM根目录下的matlab目录（如C:\libsvm-3.17\matlab），在命令窗口输入
>`mex –setup`

然后Matlab会提示你选择编译mex文件的C/C++编译器，就选择一个已安装的编译器，如Microsoft Visual C++ 2010。之后Matlab会提示确认选择的编译器，输入y进行确认。

然后可以输入以下命令进行编译:
> `make`

注意，Matlab或VC版本过低可能会导致编译失败，建议使用最新的版本。

编译成功后，当前目录下会出现若干个后缀为mexw64（64位系统）或mexw32（32位系统）的文件。

### 3. 重命名（可选，但建议执行）

编译完成后，在当前目录下回出现svmtrain.mexw64、svmpredict.mexw64（64位系统）或者svmtrain.mexw32、svmpredict.mexw32（32位系统）这两个文件，把文件名svmtrain和svmpredict相应改成libsvmtrain和libsvmpredict。

这是因为Matlab中自带有SVM的工具箱，而且其函数名字就是svmtrain和svmpredict，和LIBSVM默认的名字一样，在实际使用的时候有时会产生一定的问题，比如想调用LIBSVM的变成了调用Matlab SVM。

>*如果有进行重命名的，以后使用LIBSVM时一律使用libsvmtrain和libsvmpredict这两个名字进行调用。*

### 4. 添加路径

为了以后使用的方便，建议把LIBSVM的编译好的文件所在路径（如C:\libsvm-3.17\matlab）添加到Matlab的搜索路径中。具体操作为：（中文版Matlab对应进行）

>HOME -&gt; Set Path -&gt; Add Folder -&gt; 加入编译好的文件所在的路径（如C:\libsvm-3.17\matlab）

当然也可以把那4个编译好的文件复制到想要的地方，然后再把该路径添加到Matlab的搜索路径中。

## 二 测试

LIBSVM软件包中自带有测试数据，为软件包根目录下的heart_scale文件，可以用来测试LIBSVM是否安装成功。这里的heart_scale文件不能用Matlab的load进行读取，需要使用libsvmread读取。

进入LIBSVM的根目录运行以下代码（因为heart_scale文件没有被添加进搜索路径中，其他路径下无法访问这个文件）：

    [heart_scale_label, heart_scale_inst] = libsvmread('heart_scale');
    model = libsvmtrain(heart_scale_label, heart_scale_inst, '-c 1 -g     0.07');
    [predict_label, accuracy, dec_values] = libsvmpredict(heart_scale_label, heart_scale_inst, model);
    
如果LIBSVM安装正确的话，会出现以下的运行结果，显示正确率为86.6667%。
    
    optimization finished, #iter = 134
    nu = 0.433785
    obj = -101.855060, rho = 0.426412
    nSV = 130, nBSV = 107
    Total nSV = 130
    Accuracy = 86.6667% (234/270) (classification)
    
### 三 原理简介

使用SVM前首先得了解SVM的工作原理，简单介绍如下:

SVM(Support Vector Machine，支持向量机）是一种有监督的机器学习方法，可以学习不同类别的已知样本的特点，进而对未知的样本进行预测。

SVM本质上是一个二分类的算法，对于n维空间的输入样本，它寻找一个最优的分类超平面，使得两类样本在这个超平面下可以获得最好的分类效果。这个最优可以用两类样本中与这个超平面距离最近的点的距离来衡量，称为边缘距离，边缘距离越大，两类样本分得越开，SVM就是寻找最大边缘距离的超平面，这个可以通过求解一个以超平面参数为求解变量的优化问题获得解决。给定适当的约束条件，这是一个二次优化问题，可以通过用KKT条件求解对偶问题等方法进行求解。

对于不是线性可分的问题，就不能通过寻找最优分类超平面进行分类，SVM这时通过把n维空间的样本映射到更高维的空间中，使得在高维的空间上样本是线性可分的。在实际的算法中，SVM不需要真正地进行样本点的映射，因为算法中涉及到的高维空间的计算总是以内积的形式出现，而高维空间的内积可以通过在原本n维空间中求内积然后再进行一个变换得到，这里计算两个向量在隐式地映射到高维空间的内积的函数就叫做核函数。SVM根据问题性质和数据规模的不同可以选择不同的核函数。

虽然SVM本质上是二分类的分类器，但是可以扩展成多分类的分类器，常见的方法有一对多（one-versus-rest）和一对一（one-versus-one）。在一对多方法中，训练时依次把k类样本中的某个类别归为一类，其它剩下的归为另一类，使用二分类的SVM训练处一个二分类器，最后把得到的k个二分类器组成k分类器。对未知样本分类时，分别用这k个二分类器进行分类，将分类结果中出现最多的那个类别作为最终的分类结果。而一对一方法中，训练时对于任意两类样本都会训练一个二分类器，最终得到k\*(k-1)/2个二分类器，共同组成k分类器。对未知样本分类时，使用所有的k*(k-1)/2个分类器进行分类，将出现最多的那个类别作为该样本最终的分类结果。

LIBSVM中的多分类就是根据一对一的方法实现的。

## 四 使用

关于LIBSVM在Matlab中的使用，可以参看软件包中matlab目录下的README文件，这里对里面内容做一个翻译和一些细节的讲解。

### 1. 训练

libsvm函数用于对训练集的数据进行训练，得到训练好的模型。
>`model = libsvmtrain(training_label_vector, training_instance_matrix [, 'libsvm_options']);`

这个函数有三个参数，其中
>- training_label_vector:训练样本的类标，如果有m个样本，就是m x 1的矩阵（类型必须为double）。这里可以是二分类和多分类，类标是（-1,1）、（1,2,3）或者其他任意用来表示不同的类别的数字，要转成double类型。
- training_instance_matrix:训练样本的特征，如果有m个样本，每个样本特征是n维，则为m x n的矩阵（类型必须为double）。
- libsvm_options:训练的参数，在第3点详细介绍。

###2. 预测

libpredict函数用于对测试集的数据进行测试，还能对未知样本进行预测。
>`[predicted_label, accuracy, decision_values/prob_estimates]　= libsvmpredict(testing_label_vector, testing_instance_matrix, model [,'libsvm_options'])`;
  
这个函数包括四个参数，其中

>-testing_label_vector:测试样本的类标，如果有m个样本，就是m x 1的矩阵（类型必须为double）。如果类标未知，可以初始化为任意m x 1的double数组。
-testing_instance_matrix:测试样本的特征，如果有m个样本，每个样本特征是n维，则为m x n的矩阵（类型必须为double）。
-model:使用libsvmtrain返回的模型
-libsvm_options:预测的参数，与训练的参数形式一样。

### 3. 训练的参数

LIBSVM训练时可以选择的参数很多，包括：

>- -s svm类型：SVM设置类型（默认0)  
   0  -  C-SVC;   1  -  v-SVC;  2  –  一类SVM;  3  -  e-SVR;  4  -  v-SVR
- -t 核函数类型：核函数设置类型（默认2）  
　　　　0 – 线性核函数：\\(u^T v\\)      
　　　　1 – 多项式核函数：\\((ru^Tv + coef_0)^{degree} \\)  
　　　　2 – RBF(径向基)核函数：\\(e^{-r|u-v|^2}\\)  
　　　　3 – sigmoid核函数：\\(tanh(ru^Tv + coef_0)\\)	
- -d degree：核函数中的degree设置（针对多项式核函数）（默认3）
- -g r(gamma）：核函数中的gamma函数设置（针对多项式/rbf/sigmoid核函数）（默认1/k，k为总类别数)
- -r coef0：核函数中的coef0设置（针对多项式/sigmoid核函数）（（默认0)
- -c cost：设置C-SVC，e -SVR和v-SVR的参数（损失函数）（默认1）
- -n nu：设置v-SVC，一类SVM和v- SVR的参数（默认0.5）
- -p p：设置e -SVR 中损失函数p的值（默认0.1）
- -m cachesize：设置cache内存大小，以MB为单位（默认40）
- -e eps：设置允许的终止判据（默认0.001）
- -h shrinking：是否使用启发式，0或1（默认1）
- -wi weight：设置第几类的参数C为weight*C (C-SVC中的C) （默认1）
- -v n: n-fold交互检验模式，n为fold的个数，必须大于等于2

以上这些参数设置可以按照SVM的类型和核函数所支持的参数进行任意组合，如果设置的参数在函数或SVM类型中没有也不会产生影响，程序不会接受该参数；如果应有的参数设置不正确，参数将采用默认值。

### 4. 训练返回的内容

libsvmtrain函数返回训练好的SVM分类器模型，可以用来对未知的样本进行预测。这个模型是一个结构体，包含以下成员：

>- Parameters: 一个5 x 1的矩阵，从上到下依次表示：  
　　　　-s SVM类型（默认0）； 
　　　　-t 核函数类型（默认2）  
　　　　-d 核函数中的degree设置(针对多项式核函数)(默认3)；  
　　　　-g 核函数中的r(gamma）函数设置(针对多项式/rbf/sigmoid核函数) (默认类别数目的倒数)；<br />
　　　　-r 核函数中的coef0设置(针对多项式/sigmoid核函数)((默认0)	
- nr_class: 表示数据集中有多少类别，比如二分类时这个值即为2。
- totalSV: 表示支持向量的总数。
- rho: 决策函数wx+b中的常数项的相反数（-b）。
- Label: 表示数据集中类别的标签，比如二分类常见的1和-1。
- ProbA: 使用-b参数时用于概率估计的数值，否则为空。
- ProbB: 使用-b参数时用于概率估计的数值，否则为空。
- nSV: 表示每类样本的支持向量的数目，和Label的类别标签对应。如Label=[1; -1],nSV=[63; 67]，则标签为1的样本有63个支持向量，标签为-1的有67个。
- sv_coef: 表示每个支持向量在决策函数中的系数。
- SVs: 表示所有的支持向量，如果特征是n维的，支持向量一共有m个，则为m x n的稀疏矩阵。

另外，如果在训练中使用了-v参数进行交叉验证时，返回的不是一个模型，而是交叉验证的分类的正确率或者回归的均方根误差。

### 5. 预测返回的内容

libsvmtrain函数有三个返回值，不需要的值在Matlab可以用~进行代替。

>- predicted_label：第一个返回值，表示样本的预测类标号。
- accuracy：第二个返回值，一个3 x 1的数组，表示分类的正确率、回归的均方根误差、回归的平方相关系数。
- decision_values/prob_estimates：第三个返回值，一个矩阵包含决策的值或者概率估计。对于n个预测样本、k类的问题，如果指定“-b 1”参数，则n x k的矩阵，每一行表示这个样本分别属于每一个类别的概率；如果没有指定“-b 1”参数，则为n x k*(k-1)/2的矩阵，每一行表示k(k-1)/2个二分类SVM的预测结果。

### 6. 读取或保存

libsvmread函数可以读取以LIBSVM格式存储的数据文件。
>`[label_vector, instance_matrix] = libsvmread("data.txt");`

这个函数输入的是文件的名字，输出为样本的类标和对应的特征。
libsvmwrite函数可以把Matlab的矩阵存储称为LIBSVM格式的文件。
>`libsvmwrite("data.txt", label_vector, instance_matrix]`

这个函数有三个输入，分别为保存的文件名、样本的类标和对应的特征（必须为double类型的稀疏矩阵）。

## 五 更新:svdd扩展安装（2014.10）

 从[libsvm官网]("http://www.csie.ntu.edu.tw/~cjlin/libsvmtools/#libsvm_for_svdd_and_finding_the_smallest_sphere_containing_all_data")下载svdd工具箱，目前使用libsvm3.18以及svdd3.18版本。
svdd工具箱里面有一个matlab文件夹和3个文件svm.cpp、svm.h、svm-train.c。

将matlab文件夹中的文件svmtrain.c覆盖原libsvm的matlab文件夹中的文件。   
将svm.cpp、svm.h、svm-train.c这3个文件覆盖libsvm文件夹下的相同文件。    
按本文刚开始讲述的方法进行mex -setup、make等完成安装，根据需要进行改名以及添加Path。

#  codesnippt 

## install

## use

    # load data
    load heart_scale.mat
    # train
    model=svmtrain(heart_scale_label,heart_scale_inst,'-c 1 -g 0.07'
    # predict
    [predict,accuracy,dec_values]=svmpredict(heart_scale_label,heart_scale_inst,model)
    

