---
title: "Mysql Mysqlslap"
date: 2018-03-02T12:51:05+08:00
draft: false
tags: ["mysql", "mysqlslap"]
categories: ["mysql"]
subtitle: ""
descriptions: ""
bigimg:
---

mysqlslap模拟并发测试数据库性能

安装：简单，装了mysql就有了

作用：模拟并发测试数据库性能。

## reference

[参考网址](1)

[1]: http://www.cnblogs.com/langtianya/p/5177837.html "mysqlslap模拟并发测试数据库性能"

## 命令主要参数, 示例与解析

示例:

`mysqlslap -h10.10.15.240 -P3306 --concurrency=10 --iterations=1 --create-schema='stock' --query=\'"$line"\' --number-of-queries=100000 --debug-info -udeveloper01 -pdeveloper01`

解析:

    mysqlslap 程序名
    -h10.10.15.240 mysql IP地址
    -P3306 端口
    --concurrency=10 并发数
    --iterations=1 迭代数, 多次迭代使得数据有统计意义
    --create-schema='stock' 数据库名
    --query=\'"$line"\' 要测试的sql语句, 因为sql语句, 很可能含有 `select * `, 所以这里必须使用 \'" 和 "\' 把 sql 语句包起来
    --number-of-queries=100000 总query数
    --debug-info
    -udeveloper01 用户名
    -pdeveloper01 密码

## 结果, 示例及解析

示例:

    User time 3.81, System time 2.55
    Maximum resident set size 3620, Integral resident set size 0
    Non-physical pagefaults 1116, Physical pagefaults 0, Swaps 0
    Blocks in 0 out 0, Messages in 0 out 0, Signals 0
    Voluntary context switches 100269, Involuntary context switches 19
    Benchmark
    	Average number of seconds to run all queries: 5.659 seconds
    	Minimum number of seconds to run all queries: 5.659 seconds
    	Maximum number of seconds to run all queries: 5.659 seconds
    	Number of clients running queries: 10
    	Average number of queries per client: 10000


解析:

    User time 3.81, System time 2.55
    Maximum resident set size 3620, Integral resident set size 0
    Non-physical pagefaults 1116, Physical pagefaults 0, Swaps 0
    Blocks in 0 out 0, Messages in 0 out 0, Signals 0
    Voluntary context switches 100269, Involuntary context switches 19
    Benchmark


    Average number of seconds to run all queries: 5.659 seconds : 运行所有的 10000 次请求, 花了 5.659 秒.也就是 10000/5.659 次/秒 的处理能力
    Minimum number of seconds to run all queries: 5.659 seconds : 最少时间
    Maximum number of seconds to run all queries: 5.659 seconds :  最多时间
    Number of clients running queries: 10 : 并发数
    Average number of queries per client: 10000 每一个客户端的请求数

## 程序入口



## files

    [tom@check mm]$ tree ./
    ./
    ├── mysql_strings.txt     存放要测试的 sql 语句
    ├── mysql_test.sh         把 sql 语句, 生成 mysqlslap 语句
    ├── mysql.test.sh         执行 mysqlslap 语句
    ├── mysql.test.sh.log     结果的 log
    └── mysql.tt.sh           把前面几个, 全串在一起, 放在 sh中.

    0 directories, 5 files
    [tom@check mm]$

一个一个来过一下吧

1. mysql_strings.txt

        [tom@check mm]$ cat mysql_strings.txt
        SELECT * FROM diag_stocka_zhongtai WHERE stock_code="000001.SZ"
        SELECT research_report_str FROM diag_stocka_zhongtai WHERE stock_code="000001.SZ"
        SELECT inst_num,mai_ru,zeng_chi,zhong_xing,jian_chi,mai_chu FROM diag_stocka_zhongtai WHERE stock_code="000001.SZ"
        SELECT factor_name,factor_type,factor_id,is_buy_sell FROM diag_factor_zhongtai WHERE stock_code ="000001.SZ"
        SELECT trade_date,stock_code,main_money_net FROM diag_quot_zhongtai WHERE data_type="stocka" AND stock_code="000001.SZ" ORDER BY trade_date DESC LIMIT 5
        SELECT rating_inst,trade_date,recent_rating,pre_rating FROM diag_quot_zhongtai  WHERE data_type="rating" AND stock_code="000001.SZ" AND trade_date>=(SELECT DISTINCT trade_date FROM diag_quot_zhongtai ORDER BY trade_date DESC LIMIT 59,1) ORDER BY trade_date DESC LIMIT 5
        SELECT trade_date,stock_code,price_change FROM diag_quot_zhongtai  WHERE data_type="index" AND stock_code="000001.SH" AND trade_date>=(SELECT DISTINCT trade_date FROM diag_quot_zhongtai ORDER BY trade_date DESC LIMIT 15,1)
        SELECT trade_date,stock_code,price_change FROM diag_quot_zhongtai WHERE data_type="stocka" AND stock_code="000001.SZ" AND trade_date>=(SELECT DISTINCT trade_date FROM diag_quot_zhongtai ORDER BY trade_date DESC LIMIT 15,1)
        SELECT a.trade_date,a.stock_code,a.price_change FROM diag_quot_zhongtai a,diag_stocka_zhongtai b  WHERE data_type="indus" AND a.stock_code=b.indus_name AND b.stock_code="000001.SZ" AND a.trade_date>=(SELECT DISTINCT trade_date FROM diag_quot_zhongtai ORDER BY trade_date DESC LIMIT 15,1)

2. mysql_test.sh

        [tom@check mm]$ cat mysql_test.sh
        #! /bin/bash

        echo "#! /bin/bash" > mysql.test.sh

        while read line
        do
            #echo $line
            #echo "$line" "haha"
            #echo mysqlslap -h10.10.15.240 -P3306 --concurrency=10 --iterations=1 --create-schema='stock' --query=\'$line\' --number-of-queries=100000 --debug-info -udeveloper01 -pdeveloper01
            #echo mysqlslap -h10.10.15.240 -P3306 --concurrency=10 --iterations=1 --create-schema='stock' --query=\'$line\' --number-of-queries=100000 --debug-info -udeveloper01 -pdeveloper01 >> mysql.test.sh
            #echo mysqlslap -h10.10.15.240 -P3306 --concurrency=10 --iterations=1 --create-schema='stock' --query=\'"$line"\' --number-of-queries=100000 --debug-info -udeveloper01 -pdeveloper01
            echo mysqlslap -h10.10.15.240 -P3306 --concurrency=10 --iterations=1 --create-schema='stock' --query=\'"$line"\' --number-of-queries=100000 --debug-info -udeveloper01 -pdeveloper01
            #echo mysqlslap -h10.10.15.240 -P3306 --concurrency=10 --iterations=1 --create-schema='stock' --query=\'"$line"\' --number-of-queries=100000 --debug-info -udeveloper01 -pdeveloper01 >> mysql.test.sh

        done

        chmod +x mysql.test.sh
        [tom@check mm]$

3. mysql.test.sh

        [tom@check mm]$ cat mysql.test.sh
        #! /bin/bash
        mysqlslap -h10.10.15.240 -P3306 --concurrency=10 --iterations=1 --create-schema=stock --query='SELECT * FROM diag_stocka_zhongtai WHERE stock_code="000001.SZ"' --number-of-queries=100000 --debug-info -udeveloper01 -pdeveloper01
        mysqlslap -h10.10.15.240 -P3306 --concurrency=10 --iterations=1 --create-schema=stock --query='SELECT research_report_str FROM diag_stocka_zhongtai WHERE stock_code="000001.SZ"' --number-of-queries=100000 --debug-info -udeveloper01 -pdeveloper01
        mysqlslap -h10.10.15.240 -P3306 --concurrency=10 --iterations=1 --create-schema=stock --query='SELECT inst_num,mai_ru,zeng_chi,zhong_xing,jian_chi,mai_chu FROM diag_stocka_zhongtai WHERE stock_code="000001.SZ"' --number-of-queries=100000 --debug-info -udeveloper01 -pdeveloper01
        mysqlslap -h10.10.15.240 -P3306 --concurrency=10 --iterations=1 --create-schema=stock --query='SELECT factor_name,factor_type,factor_id,is_buy_sell FROM diag_factor_zhongtai WHERE stock_code ="000001.SZ"' --number-of-queries=100000 --debug-info -udeveloper01 -pdeveloper01
        mysqlslap -h10.10.15.240 -P3306 --concurrency=10 --iterations=1 --create-schema=stock --query='SELECT trade_date,stock_code,main_money_net FROM diag_quot_zhongtai WHERE data_type="stocka" AND stock_code="000001.SZ" ORDER BY trade_date DESC LIMIT 5' --number-of-queries=100000 --debug-info -udeveloper01 -pdeveloper01
        mysqlslap -h10.10.15.240 -P3306 --concurrency=10 --iterations=1 --create-schema=stock --query='SELECT rating_inst,trade_date,recent_rating,pre_rating FROM diag_quot_zhongtai  WHERE data_type="rating" AND stock_code="000001.SZ" AND trade_date>=(SELECT DISTINCT trade_date FROM diag_quot_zhongtai ORDER BY trade_date DESC LIMIT 59,1) ORDER BY trade_date DESC LIMIT 5' --number-of-queries=100000 --debug-info -udeveloper01 -pdeveloper01
        mysqlslap -h10.10.15.240 -P3306 --concurrency=10 --iterations=1 --create-schema=stock --query='SELECT trade_date,stock_code,price_change FROM diag_quot_zhongtai  WHERE data_type="index" AND stock_code="000001.SH" AND trade_date>=(SELECT DISTINCT trade_date FROM diag_quot_zhongtai ORDER BY trade_date DESC LIMIT 15,1)' --number-of-queries=100000 --debug-info -udeveloper01 -pdeveloper01
        mysqlslap -h10.10.15.240 -P3306 --concurrency=10 --iterations=1 --create-schema=stock --query='SELECT trade_date,stock_code,price_change FROM diag_quot_zhongtai WHERE data_type="stocka" AND stock_code="000001.SZ" AND trade_date>=(SELECT DISTINCT trade_date FROM diag_quot_zhongtai ORDER BY trade_date DESC LIMIT 15,1)' --number-of-queries=100000 --debug-info -udeveloper01 -pdeveloper01
        mysqlslap -h10.10.15.240 -P3306 --concurrency=10 --iterations=1 --create-schema=stock --query='SELECT a.trade_date,a.stock_code,a.price_change FROM diag_quot_zhongtai a,diag_stocka_zhongtai b  WHERE data_type="indus" AND a.stock_code=b.indus_name AND b.stock_code="000001.SZ" AND a.trade_date>=(SELECT DISTINCT trade_date FROM diag_quot_zhongtai ORDER BY trade_date DESC LIMIT 15,1)' --number-of-queries=100000 --debug-info -udeveloper01 -pdeveloper01
        [tom@check mm]$

4. cat mysql.tt.sh

        [tom@check mm]$ cat mysql.tt.sh
        #! /bin/bash

        cat mysql_strings.txt | ./mysql_test.sh >> mysql.test.sh
        cat ./mysql.test.sh
        ./mysql.test.sh | tee -a ./mysql.test.sh.log
        [tom@check mm]$

5. mysql.test.sh.log

        [tom@check mm]$ cat mysql.test.sh.log
        Benchmark
        	Average number of seconds to run all queries: 1.716 seconds
        	Minimum number of seconds to run all queries: 1.716 seconds
        	Maximum number of seconds to run all queries: 1.716 seconds
        	Number of clients running queries: 10
        	Average number of queries per client: 10000

        Benchmark
        	Average number of seconds to run all queries: 2.109 seconds
        	Minimum number of seconds to run all queries: 2.109 seconds
        	Maximum number of seconds to run all queries: 2.109 seconds
        	Number of clients running queries: 10
        	Average number of queries per client: 10000

        Benchmark
        	Average number of seconds to run all queries: 2.577 seconds
        	Minimum number
