---
layout: post
title: Groovy - Script for dirty performance benchmarking
---
Cooked up this script which, if piped to a .csv file makes it easy to open the results in excel (or equivalent):

    Test Name, Num of Transactions, Num of Iterations, Num of Threads, Total Time (ms), Mean Transaction Time (ms), Transactions per Second

    When Requesting Google Homepage, 30,10,3,535,17.8333333333,56.0747663551

    When Requesting BBC News Homepage, 30,10,3,1357,45.2333333333,22.1075902727



You could extract he header printing and the performanceOf method (+ time conversion methods) to a seperate class or script and then just write as many tests as you like using the performanceOf + closure syntax.

These examples just request an http resource but they can do any operations you like.

Code:

    println "Test Name, Num of Transactions, Num of Iterations, Num of Threads, Total Time (ms), Mean Transaction Time (ms), Transactions per Second"

    performanceOf("When Requesting Google Homepage", times=10 , threads=3){

        "http://www.google.com".toURL().text

     }

    performanceOf("When Requesting BBC News Homepage", times=10 , threads=3){

        "http://news.bbc.co.uk".toURL().text

    }

    def performanceOf ( String test, int times,  int threads,  Closure cl){

        long total = times * threads

        print "$test,"

        print " $total,$times,$threads,"

        long start = System.nanoTime()

        times.times {

            def performanceThreads = []

            threads.times { performanceThreads << new Thread(cl as Runnable) }

            performanceThreads*.start()

            performanceThreads*.join()

        }

        long elapsed = millis(System.nanoTime() - start)

        def mean = elapsed / total

        def tps = total / secs(elapsed)

        println "$elapsed,$mean,$tps"

    }

    def millis(nano){

        nano / 1000000

    }

    def secs(millis){

        millis / 1000

    }
