---
layout: post
title: '右键复制下载解析迅雷协议的地址脚本'
date: 2008-12-2
wordpress_id: 250
categories: [Others]
tags: []
keywords: ""
description: 
comments: true
---

需要xsel、zenity和base64  

``` 
sudo apt-get install xsel
sudo apt-get install ael 
sudo apt-get install zenity 
sudo apt-get install mkvtoolnix
```

代码：

``` bash
save_dir0=~/Download  #最后面千万不要加“/”，不然保存文件的时候可能找不到路径。
max_num=20            #在此设定axel的最大连接数
[ ! -d $save_dir0 ] && mkdir -p $save_dir0
#下载链接，给出一个下载进度条，并且当点击“取消”的时候将该axel废掉武功。
DOWNLOAD() {
   axel -n $max_num "$1" -o "$2/$3" |while read a;do
      echo "$a" |grep "^[/[D]" |sed 's/^/[ *//;s/%.*$//;s/^Download.*$/100/'
   done |zenity --progress --auto-close --text="下载 $true_url 至 $2" --width="350" 2>/dev/null &
   axel_info=`ps ax |grep "axel.*$1" |awk '{print $1"-"$2}'`
   axel_tty=`echo $axel_info |sed 's/^.*-//'`
   axel_pid=`echo $axel_info |sed 's/-.*$//'`
   while true;do 
      if ! [ "`ps ax |grep "$axel_tty.*zenity"`" ];then
         [ "`ps -A |grep "$axel_pid"`" ] && kill $axel_pid
         break
      fi
      sleep 1   
   done   &
}
#出来一个动作选择菜单，选择下一步动作。
UI() {
   choice=$(zenity --list --title "默认保存目录为：$save_dir0" --text "解析得URL：$true_url" /
   --column "选项" --column "动作" /
   A 下载至默认目录 B 选择目录并下载 C 保存链接到剪贴板 2>/dev/null);
   case $choice in
   'A')
      file_name=`zenity --entry --title="重命名文件" --text="请输入一个文件名（取消则按链接默认命名）" 2>/dev/null`
      DOWNLOAD $true_url $save_dir0 $file_name
      file_name=""
      ;;
   'B')
      save_dir=`zenity --file-selection --directory 2>/dev/null`
      file_name=`zenity --entry --title="重命名文件" --text="请输入一个文件名（取消则按链接默认命名）" 2>/dev/null`
      DOWNLOAD $true_url $save_dir $file_name
      file_name=""
      ;;
   'C')
      printf "$true_url" |xsel -i -b
      ;;
   esac
}
#从剪贴板获取迅雷地址，并将其解码成http的。
DECODE() {
   str0="`xsel -b`"
   if [ `echo "$str0" |grep "^thunder"` ] && [ "$str" != "$str0" ];then
      str="$str0"
      true_url="`printf "$str" |sed 's/^thunder://////' |base64 -d |sed 's/^AA//;s/ZZ$//'`"
      [ ! -z "$true_url" ] && UI
      true_url=""
   fi
}

while true;do
   DECODE
   sleep 1
done
```