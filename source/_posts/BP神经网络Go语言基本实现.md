---
title: BP神经网络Go语言基本实现
date: 2017-05-16 15:43:55
tags:
---



BP神经网络的原理表现得挺清楚的。

1.初始化一个BPNN

2.输入训练集和预期输出，训练网络

3.网络训练好了，用来预测数据

	package main
	import (
	  "fmt"
	  "math"
	  "math/rand"
	)
	const MAX_H_COUNT = 100
	//bp人工神经网络结构
	type BPNN struct {
	  //样本数量
	  Sample_count int
	  //输入向量维数
	  Input_count int
	  //输出向量维数
	  Output_count int
	  //实际使用隐层神经元数量
	  Hide_count int
	  //学习率
	  Study_rate float64
	  //精度控制参数
	  Precision float64
	  //循环次数
	  Loop_count int
	  //隐藏层权矩阵i,隐层节点最大数量为100
	  v [][]float64
	  //输出层权矩阵
	  w [][]float64
	}
	//配置一个BP网络
	func NewBPNN(sc int, ic int, oc int, hc int, sr float64, p float64, lc int) (bp BPNN) {
	}
	
	//使用bp网络进行计算
	
	func (bp *BPNN) UseBp(input []float64) (output []float64) {
	  var O1 = make([]float64, MAX_H_COUNT)
	  var i, j int
	  var temp float64
	
	  output = make([]float64, bp.Output_count)
	
	  for i = 0; i < bp.Hide_count; i++ {
	      temp = 0
	      for j = 0; j < bp.Input_count; j++ {
	          temp = temp + input[j]*bp.v[j][i]
	      }
	      O1[i] = sigmoid(temp)
	  }
	  for i = 0; i < bp.Output_count; i++ {
	      temp = 0
	      for j = 0; j < bp.Hide_count; j++ {
	          temp = temp + O1[j]*bp.w[j][i]
	      }
	      output[i] = sigmoid(temp)
	  }
	  fmt.Println("结果：   ")
	  for i = 0; i < bp.Output_count; i++ {
	      fmt.Printf("%f     ", output[i])
	  }
	  fmt.Println(" ")
	  return
	}
	
	//Sigmoid函数,神经网络激活函数
	
	func sigmoid(net float64) float64 {
	  return 1 / (1 + math.Exp(-net))
	}
	
	func copyMatrix(copiedMatrix *float64, originMatrix float64) {
	  *copiedMatrix = make([][]float64, len(originMatrix))
	  for i := 0; i < len(originMatrix); i++ {
	      (*copiedMatrix)[i] = make([]float64, len(originMatrix[i]))
	      for j := 0; j < len(originMatrix[i]); j++ {
	          (*copiedMatrix)[i][j] = originMatrix[i][j]
	      }
	  }
	}
	
	//训练bp网络，样本为x，理想输出为y
	
	func (bp *BPNN) TrainBp(x float64, y int) {
	  //精度控制参数
	  f := bp.Precision
	  //学习率
	  a := bp.Study_rate
	  //隐层节点数
	  h := bp.Hide_count
	
	  //权矩阵
	  var v [][]float64
	  //输出层权矩阵
	  var w [][]float64
	
	  // fmt.Printf("1v:\n%v\nbp.v\n%v\n", v, bp.v)
	  // copy(v, bp.v)
	  copyMatrix(&v, bp.v)
	  // fmt.Printf("2v:\n%v\nbp.v\n%v\n", v, bp.v)
	  // copy(w, bp.w)
	  copyMatrix(&w, bp.w)
	
	  //修改量矩阵
	  var ChgH = make([]float64, bp.Hide_count)
	  var ChgO = make([]float64, bp.Output_count)
	
	  //隐层和输出层输出量
	  var O1 = make([]float64, bp.Hide_count)
	  var O2 = make([]float64, bp.Output_count)
	
	  //最大循环次数
	  LoopCount := bp.Loop_count
	
	  //循环变量
	  var i, j, m, n int
	  fmt.Println(i, j, m, n)
	  //临时变量
	  var temp float64
	  fmt.Println(temp)
	
	  e := f + 1
	  //循环直到达到精度或者到达循环最大次数
	  for n = 0; e > f && n < LoopCount; n++ {
	      e = 0
	      //对每个样本训练网络
	      for i = 0; i < bp.Sample_count; i++ {
	          // fmt.Printf("range:\n%v\n%v\n%v\n", x, v, bp.Input_count)
	          //计算隐层输出向量
	          for m = 0; m < h; m++ {
	              temp = 0
	              for j = 0; j < bp.Input_count; j++ {
	                  // fmt.Printf("%v,%v,%v,%v\n", temp, i, j, m)
	                  temp = temp + x[i][j]*v[j][m]
	              }
	              O1[m] = sigmoid(temp)
	          }
	          //计算输出层输出向量
	          for m = 0; m < bp.Output_count; m++ {
	              temp = 0
	              for j = 0; j < h; j++ {
	                  temp = temp + O1[j]*w[j][m]
	              }
	              O2[m] = sigmoid(temp)
	          }
	          //计算输出层的权修改量
	          for j = 0; j < bp.Output_count; j++ {
	              ChgO[j] = O2[j] * (1 - O2[j]) * (float64(y[i][j]) - O2[j])
	          }
	          //计算输出误差
	          for j = 0; j < bp.Output_count; j++ {
	              e = e + (float64(y[i][j])-O2[j])*(float64(y[i][j])-O2[j])
	          }
	          //计算隐层权修改量
	          for j = 0; j < h; j++ {
	              temp = 0
	              for m = 0; m < bp.Output_count; m++ {
	                  temp = temp + w[j][m]*ChgO[m]
	              }
	              ChgH[j] = temp * O1[j] * (1 - O1[j])
	          }
	          //修改输出层权矩阵
	          for j = 0; j < h; j++ {
	              for m = 0; m < bp.Output_count; m++ {
	                  w[j][m] = w[j][m] + a*O1[j]*ChgO[m]
	              }
	          }
	          for j = 0; j < bp.Input_count; j++ {
	              for m = 0; m < h; m++ {
	                  v[j][m] = v[j][m] + a*x[i][j]*ChgH[m]
	              }
	          }
	      }
	      //每循环十次输出一次误差
	      if n%10 == 0 {
	          fmt.Println("误差 : %f", e)
	      }
	  }
	  fmt.Println("总共循环次数：%d", n)
	  fmt.Println("调整后的隐层权矩阵：")
	  for i = 0; i < bp.Input_count; i++ {
	      for j = 0; j < h; j++ {
	          fmt.Printf("%f     ", v[i][j])
	      }
	      fmt.Println(" ")
	  }
	  fmt.Println("调整后的输出层权矩阵：")
	  for i = 0; i < h; i++ {
	      for j = 0; j < bp.Output_count; j++ {
	          fmt.Printf("%f     ", w[i][j])
	      }
	      fmt.Println(" ")
	  }
	
	  fmt.Println("bp网络训练结束!")
	}
	
	func main() {
	  //样本数量
	  var Sample_count int = 6
	  //输入向量维数
	  var Input_count int = 3
	  //输出向量维数
	  var Output_count int = 2
	  //实际使用隐层神经元数量
	  var Hide_count int = 10
	  //学习率
	  var Study_rate float64 = 0.01
	  //精度控制参数
	  var Precision float64 = 0.001
	  //循环次数
	  var Loop_count int = 1000000
	
	  //训练样本
	  x := [][]float64{
	      []float64{0.8, 0.5, 0},
	      []float64{0.9, 0.7, 0.3},
	      []float64{1, 0.8, 0.5},
	      []float64{0, 0.2, 0.3},
	      []float64{0.2, 0.1, 1.3},
	      []float64{0.2, 0.7, 0.8}}
	
	  //理想输出
	  y := [][]int{
	      []int{0, 1},
	      []int{0, 1},
	      []int{0, 1},
	      []int{1, 0},
	      []int{1, 0},
	      []int{1, 0}}
	  var bp BPNN = NewBPNN(Sample_count, Input_count, Output_count, Hide_count, Study_rate, Precision, Loop_count)
	  bp.TrainBp(x, y)
	  input := []float64{0.8, 0.8, 0}
	  bp.UseBp(input)
	}