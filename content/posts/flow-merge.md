---
title: "Coroutines Flow で merge する"
date: 2020-07-05T16:59:53+09:00
draft: false
---

Coroutines Flow を使ったときに RxJava の Single を [merge](http://reactivex.io/documentation/operators/merge.html) するオペレーションと同じことをする処理を書いたメモです。

`flow` ビルダーの中でmergeしたいApiの flow を emit するだけです。

{{< highlight kotlin >}}
fun main() {
    runBlocking {
        val api1 = flowOf(1)
        val api2 = flowOf(2)

        flow {
            emit(api1.single())
            emit(api2.single())
        }.collect {
            println(it) 
            // output
            // 1 
            // 2
        }
    }
}
{{< / highlight >}}