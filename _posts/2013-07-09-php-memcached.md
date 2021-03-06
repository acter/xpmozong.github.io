---
layout: post
title: "Windows下安装配置PHP Memcached"
description: "Windows下安装配置PHP Memcached"
category: PHP
tags: [PHP,Memcached]
---
{% include JB/setup %}

<ul>
    <li>作者：<a href="http://weibo.com/xpmozong" target="blank">寞踪</a></li>
    <li>本文地址：http://xpmozong.github.io/php/2013/07/09/php-memcached/</li>
    <li>转载请注明出处</li>
</ul>

<p>安装前准备：</p>
<p>
    1. memcached 1.2.1 for Win32 binaries，这个是 Win32 服务器端的 memcached 最新版本，下载
    <a href="http://code.jellycan.com/files/memcached-1.2.6-win32-bin.zip" target="_blank">附件</a>
</p>
<p>2. php 所需的 PECL 扩展，即 php_memcache 扩展 http://windows.php.net/downloads/pecl/releases/memcache/2.2.7/</p>

<p>win7 64 位</p>
<p>php-5.3.4-x64 php_memcache.dll <a href="http://blog.hexu.org/wp-content/uploads/2012/05/php_memcache.zip">下载</a></p>
<p>Memcache-win64 下载：</p>
<p>a. 下载最新版：http://blog.couchbase.com/memcached-windows-64-bit-pre-release-available</p>
<p>b. 直接下载： http://s3.amazonaws.com/downloads.northscale.com/memcached-win64-1.4.4-14.zip</p>
 
<p>安装步骤：</p>
<p>1. 将第一个包解压放某个盘下面，比如在c:\memcached</p>

<p>2. 在终端下输入 'c:\memcached\memcached.exe -d install' , memcached将作为windows的一个服务</p>

<p>3. 再输入 'c:\memcached\memcached.exe -d start' , 这样服务器端已经安装完毕了</p>

<p>4. 解压第二个包包，里面会只有一个 php_memcache.dll 文件，把它放入 php5/ext/ 中</p>
 
<p>5. 在C:\WINDOWS\php.ini 加入一行'extension=php_memcache.dll'</p>

<p>6.接着在 php.ini 文件里加上以下带代码，最好就放在刚才写 "extension=php_memcache.dll" 的下面:</p>

    [Memcache]
    memcache.allow_failover = 1
    memcache.max_failover_attempts=20
    memcache.chunk_size =8192
    memcache.default_port = 11211


<p>试运行：</p>
<p>写一个 test.php 文件：（更多使用方法可以参看 PHP 手册里的 Memcache Functions 使用说明） </p>
<p>复制代码 代码如下:</p>

    header('Content-Type:text/html;charset=utf-8');
    $memcache = new Memcache;
    $memcache->connect('localhost', 11211) or die ("Could not connect");

    $version = $memcache->getVersion();
    echo "Server's version: ".$version."<br/>\n";

    $tmp_object = new stdClass;
    $tmp_object->str_attr = 'test';
    $tmp_object->int_attr = 123;

    $memcache->set('key', $tmp_object, false, 10) or die ("Failed to save data at the server");
    echo "Store data in the cache (data will expire in 10 seconds)<br/>\n";

    $get_result = $memcache->get('key');
    echo "Data from the cache:<br/>\n";
    print_r($get_result);

<p>如果有输出：</p>

    Server's version: 1.2.6
    Store data in the cache (data will expire in 10 seconds)
    Data from the cache:
    stdClass Object ( [str_attr] => test [int_attr] => 123 ) 
