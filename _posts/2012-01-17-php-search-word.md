---
layout: post
title: "php查找一个单词在一个文件中的第几行第几列"
description: "php查找一个单词在一个文件中的第几行第几列"
category: PHP
tags: [PHP]
---
{% include JB/setup %}

<ul>
    <li>作者：<a href="http://weibo.com/xpmozong" target="blank">寞踪</a></li>
    <li>本文地址：http://xpmozong.github.io/php/2012/01/17/php-search-word/</li>
    <li>转载请注明出处</li>
</ul>

<p>在写这个程序之前，先回忆相关的代码。</p>
1、

    $file = "aa.txt "; 
    $file_array = file($file); 
    $file_line_num = count($file_array); 
    echo $file_line_num;die;    //获得这个文件有多少行

2、

    $txt = file('aa.txt');
    echo $txt[2];die;   //输出第三行

<p>3、有一个很大的日志文件，要读出最后100行，怎么办？</p>

    $commandstr="tail -n 100 aa.txt>online.txt"; //把server.log的最后100行重定向到online.txt中
    system($commandstr); //调用系统命令
    $file_handle = fopen("online.txt", "r+"); //以只读的方式打开online.txt文件
    while (!feof($file_handle)) {
       $line = fgets($file_handle);
            echo nl2br($line); //输出文件
    }
    fclose($file_handle); //关闭文件

<p>注意，要把online.txt文件的属性设为777，还有上面程序只对linux系统下有效。</p>

<p>4、现在开始标题的程序吧</p>

    $txt = file('aa.txt');
    foreach($txt as $key=>$value) {
        if(preg_match('/heaven/',$value)) {
            $strarr = explode(' ',$value);
            foreach($strarr as $k => $v) {
                if(preg_match('/heaven/',$v)) {
                    echo ($key+1).','.($k+1).' ';
                }
            }
        }
    }

