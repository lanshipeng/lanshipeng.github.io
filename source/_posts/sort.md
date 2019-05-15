---
title: 排序
categories: 
- 算法
tags:     
- 堆排序
- 归并排序 
comments: true
---

#### golang 实现归并和堆排序  

<!-- more -->  

1.归并排序  

```
    package main

    import "fmt"

    func main(){
        a := make([]int,0,0)
        r := make([]int,0,0)
        a = []int{4,1,3,2,6,5,7}
        r = []int{0,0,0,0,0,0,0}
        mergesort(a,0,6,r)
        fmt.Println(a)
        fmt.Println(r)
    }
    func Swap(a *int,b *int){
        temp := *a
        *a = *b
        *b = temp
    }

    func mergesort(a []int,start,end int64,result []int){

        if end - start == 0{
            return
        }
        if end - start == 1{
            if a[start] > a[end]{
                Swap(&a[start],&a[end])
            }
        }else{
            mergesort(a,start,(end-start+1)/2 + start,result)
            mergesort(a,(end-start+1)/2 +1 + start,end,result)
            merge(a,start,end,result)
            var i int64 =0
            for i=start;i<=end;i++{
                a[i] = result[i]
            }
        }

    }

    func merge(a []int,start,end int64,result []int){

        left_index  := start
        left_length := (end-start+1)/2 +1
        right_index := start + left_length
        result_index := start

        for  left_index < start + left_length && right_index < end+1 {

            if a[left_index] <= a[right_index]{
                result[result_index] = a[left_index]
                result_index++
                left_index++
            }else{
                result[result_index] = a[right_index]
                right_index++
                result_index++
            }

        }
        for left_index < start +left_length{
            result[result_index] = a[left_index]
            result_index++
            left_index++
        }
        for right_index < end+1{
            result[result_index] = a[right_index]
            right_index++
            result_index++
        }
    }
```




2.堆排序  

```   
    package main

    import "fmt"

    func main(){
        a := make([]int,0,0)
        a = []int{1,4,2,3,7,9,8}
        heapsort(a)
        fmt.Println(a)
    }

    func swap(a *int,b *int){
        temp := *a
        *a = *b
        *b = temp
    }
    func heapsort(a []int){

        //创建大顶堆
        var i int = 0
        var j int = 0
        for i= len(a)/2;i>=0;i--{
            heapadjust(a,i,len(a))
        }
        for j= len(a) -1 ;j>=1;j--{
            swap(&a[0],&a[j])
            heapadjust(a,0,j)
        }

    }
    func heapadjust(a []int,i,length int){

        var left int = 2*i +1
        if left < length{
            maxIndex := left
            right := left + 1
            if a[right] > a[left] && right < length{
                maxIndex = right
            }
            if a[maxIndex] > a[i]{
                swap(&a[maxIndex],&a[i])
                heapadjust(a,maxIndex,length)
            }
        }
    }
```
