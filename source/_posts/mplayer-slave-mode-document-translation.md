---
layout: post
title: 'mplayer slave 模式文档翻译'
date: 2010-1-22
categories: [Others]
tags: [MPlayer]
keywords: "MPlayer"
description: 
comments: true
---
slave模式协议
-------------------

在关于slave模式，MPlayer为后台运行其他程序。不再截获键盘事件，MPlayer会从标准输入读一个换行符（/n）分隔开的命令。

要动手尝试slave模式，运行

mplayer -slave -quiet <movie>

并在控制台窗口输入slave命令。

您也可以使用一个fifo文件（命名管道）：

```
mkfifo </tmp/fifofile>
mplayer -slave -input file=</tmp/fifofile> <movie>
```
大多数slave模式命令相当于命令行选项，但并非一定要在相同的名称。详细说明中可以在手册中找到

所有命令都可以以前缀“pausing”，“pausing_keep”，或“pausing_toggle”为前缀。“pausing”告诉MPlayer暂停尽快正在处理的命令。 “pausing_keep”告诉MPlayer暂定保持只要它已在暂停模式。“pausing_toggle”告诉MPlayer暂定保持只要它尚未暂停模式。请注意，“尽可能“可以在命令完全执行之前。
作为一项临时黑客，也有个实验性“pausing_keep_force”前缀，与之MPlayer不退出了所有的暂停循环。
这样你能避免“frame stepping”由于“pausing_keep”的影响。但大多数命令将要么不执行或是按照令人意外的方式。
对于“set_mouse_pos”和“key_down_event”，“pausing_keep_force”是默认
因为其他值不为他们作出多大的意义。


各种提示和技巧（帮助扩展！）：

- 尝试使用例如
pausing_keep_force pt_step 1
get_property pause
切换到下一个文件。它避免在转换到新的音频文件之前旧文件播放一小段时间

可用的命令（'mplayer -input cmdlist的'会打印出一份清单）：

alt_src_step <value> (ASX playlist only)
当有一个以上的源可以有选择下一个/前一个。

audio_delay <value> [abs]
设置/调整音频延迟。
如果[abs]不提供或为零，调整迟延<value>秒。
如果[abs]不为零，将延迟到<value>秒。

[brightness|contrast|gamma|hue|saturation] <value> [abs]
设置/调整视频参数。
如果[abs]不提供或为零，修改参数为<value>。
如果[abs]不为零，参数设置为<value>。
<value>的范围是[-100，100]。

change_rectangle <val1> <val2>
更改矩形滤波器矩形的坐标。
<val1>
必须是下列之一：
0 =宽度
1 =高度
2 = x坐标
3 = y坐标
<val2>
如果<val1>为0或1：
整数加/减去宽/高。
正值宽度/高度和负值减去它。
如果<val1>是2或3：
相对矩形左上角的整数值。正值移动矩形向右/向下和负值移动矩形向左/向上。

dvb_set_channel <channel_number> <card_number>
设置的DVB通道。

dvdnav <button_name>
给定dvdnav按钮。
up
down
left
right
menu
select
prev
mouse


edl_mark
将当前位置写入EDL文件。

frame_drop [value]
切换/设置帧的模式。

get_audio_bitrate
打印出当前文件音频比特率。

get_audio_codec
打印出的音频当前文件的编解码器的名称。

get_audio_samples
打印出的音频和当前文件的声道数。

get_file_name
打印出当前文件名。

get_meta_album
打印出当前文件的'专辑'的元数据。

get_meta_artist
打印出当前文件的'艺术家'的元数据。

get_meta_comment
打印出当前文件的'评论'的元数据。

get_meta_genre
打印出当前文件的'流派'的元数据。

get_meta_title
打印出当前文件的'标题'的元数据。

get_meta_track
打印出当前文件的'音轨的数量'的元数据。

get_meta_year
打印出当前文件的'年份'的元数据。

get_percent_pos
打印出文件中的当前位置为整数百分比[0-100）。

get_property <property>
打印出的属性的当前值。

get_sub_visibility
打印出字幕能见度（1 ==开启，0 ==关闭）。

get_time_length
打印出当前文件的长度用秒表示。

get_time_pos
打印出在文件的当前位置用秒表示，采用浮点数。

get_vo_fullscreen
全屏状态打印出来（1 == 全屏，0 ==窗口）。

get_video_bitrate
打印出当前文件的视频比特率。

get_video_codec
打印出当前视频文件的编解码器的名称。

get_video_resolution
打印出当前文件的视频分辨率。

screenshot <value>
截屏。要求屏幕过滤器加载。
0以一个单独的截图。
1启动/停止服用，每帧画面。

gui_[about|loadfile|loadsubtitle|play|playlist|preferences|skinbrowser|stop]
图形用户界面行动

key_down_event <value>
注入<value>到MPlayer的关键代码的事件。

loadfile <file|url> <append>
加载给定的文件/网址，停止当前文件的播放/网址。
如果是<append>非零继续播放和文件/网址
追加到当前播放列表代替。

loadlist <file> <append>
加载给定的播放列表文件，停止当前文件的播放。
如果<append>是非零和继续播放文件,文件追加到当前播放列表。

loop <value> [abs]
调整/设置怎样的电影应该是循环多次。 -1代表不循环，永远的0。

菜单命令>
执行上显示OSD菜单命令。
up 移动光标向上。
down 移动光标向下。
ok 接受的选择。
cancel 取消选择。
hide 隐藏的OSD菜单。

set_menu <menu_name>
显示菜单命名<menu_name>。

mute [value]
切换声音输出静音或将其设置为[value]（value>=0）
（1 ==开启，0 ==关闭）。

osd [level]
切换OSD模式或将其设置为[level]在[level]>= 0。

osd_show_property_text <string> [duration] [level]
显示一项关于OSD扩展属性的字符串，看到-playing-msg 用于描述可用的扩展。如果[duration]>=0,显示为[duration]ms。 [level]设置所需的最低水平OSD该消息可见（默认是：0 -始终显示）。

osd_show_text <string> [duration] [level]
查看OSD的<string>。

panscan <-1.0 - 1.0> | <0.0 - 1.0> <abs>
增加或减少pan-and-scan的<value>的范围，1.0是最高的。
负值降低pan-and-scan范围。
如果<abs>！= 0，那么pan-and-scan范围被解释为绝对的范围。

pause
暂停/取消暂停播放。

frame_step
播放一帧，然后暂停。

pt_step <value> [force]
转到下一个/上的播放树项。标志的<value>讲述
该方向。如果没有项目可在给定的方向不会做任何事，除非[force]不为零。

pt_up_step <value>[部队]
类似pt_step，但跳转到下一个/父列表中的前一个项目。
有助于摆脱在播放树内部循环。

quit [value]
退出MPlayer。可选的整数[value]的值作为返回代码
为mplayer的进程（默认值：0）。

radio_set_channel <channel>
切换到<channel>。在‘channel’的广播参数需要设置。

radio_set_freq <frequency in MHz>
设置广播频率调谐器。

radio_step_channel <-1|1>
步向前（1）或向后（-1频道列表）。只有当'channel'的广播参数设置。

radio_step_freq <value>
调整频率的<value>（正数 - 向上，负数 - 向下）。

seek <value> [type]
定位电影的某些地方。
0 是一个相对定位+/- <value>（默认值）。
1 是定位<value>％在电影里。
2 是寻求一个绝对位置的<value>秒。

seek_chapter <value> [type]
定位一章的开始。
0 是一个相对寻求+/- <value>章节（默认）。
1 定位到<value>章。

switch_angle <value>
转换ID为角度[value]。通过循环如果用角度[value]省略或负数。

set_mouse_pos的<X> <y>
告诉MPlayer的窗口中鼠标坐标。
此命令不移动鼠标！

set_property <property> <value>
设置属性。

speed_incr <value>
增加<value>当前回放速度。

speed_mult <value>
目前速度乘以<value>。

speed_set <value>
设定速度为<value>。

step_property <property> [value] [direction]
通过value来改变属性，或者，如果没给定或为0则增加默认值。如果小于零，方向是相反的方向。

stop
停止播放。

sub_alignment [value]
切换/设置对齐字幕。
0 顶部对齐
1 居中对齐
2 底部对齐

sub_delay <value> [abs]
调整了字幕延迟+/- <value>秒或将其设置<value>
秒时[abs]不为零。

sub_load <subtitle_file>
从<subtitle_file>加载字幕。

sub_log
当前日志上显示的字幕或连同文件名和时间信息的〜/.mplayer/subtitle_log。

sub_pos <value> [abs]
调整/设置字幕的位置。

sub_remove [value]
如果[value]参数是当前和非负，并取消了字幕文件的[value]索引。如果参数省略或负，除去
所有的字幕文件。

sub_select [value]
显示字幕的索引[value]。关闭字幕显示，如果关闭[value]的值为-1或比更高可用的字幕指数更大。
可用的字幕周期，如果[value]省略或低于-1。支持字幕来源是 -sub 选项在命令行，VOBsubs，DVD字幕和Ogg和Matroska文本流。
这主要是循环所有字幕命令，如果要设置一个特定的字幕，使用sub_file，sub_vob，或sub_demux。

sub_source [source][/source]
显示第一个字幕，从[source][/source]。这里[source][/source]是一个整数：
SUB_SOURCE_SUBS（0）用于文件字幕
SUB_SOURCE_VOBSUB（1） VOBSub文件
SUB_SOURCE_DEMUX（2）在媒体文件或DVD嵌入字幕。
如果[source][/source]为-1，将关闭字幕显示。如果[source][/source]低于-1，将循环每个之间的现有资源第一个字幕。

sub_file [value]
显示字幕specifid由[value]的文件subs。在[value]的值
通过相应的ID_FILE_SUB_ID'-identify'报告的值。
如果[value]的值-1，将关闭字幕显示。如果[value]小于-1，
将循环的所有文件subs。

sub_vob [value]
显示字幕specifid由[value]的vobsubs。在[value]的值
通过相应的ID_VOBSUB_ID'-identify'报告的值。
如果[value]的值-1，将关闭字幕显示。如果[value]小于-1，
将循环的所有vobsubs。

sub_demux [value]
显示字幕specifid由[value]从DVD字幕或嵌入在媒体文件。在[value]的值对应ID_SUBTITLE_ID值'-identify'，。如果[value]的值-1，将关闭字幕显示。
如果[value]小于-1，将循环所有的DVD字幕或嵌入字幕。

sub_scale <value> [abs]
调整字幕大小+/- <value>或将其设置为<value>时，[abs]
不为零。

vobsub_lang
这是与sub_select为了向后兼容。

sub_step<value>
在字幕列表前进<value>步，如果<value>
是为负，倒退<value>步。

sub_visibility [value]
切换/设置字幕。

forced_subs_only [value]
强制切换/设置字幕。

switch_audio [value]（目前的MPEG*，AVI，的Matroska和libav库处理流）
切换到音频文件通过ID[value]。循环
歌曲，如果[value]省略或负数。

switch_angle [value]（DVD光盘只）
切换到DVD的角度通过ID[value]。循环
如果可用角度，如果[value]省略或负数。

switch_ratio [value]
在运行时改变长宽比。 [value]是表示新的长宽比
作为浮动16 / 9（例如1.77778）。
这可能与某些视频过滤器的问题。

switch_title [value]（DVDNAV only）
切换到DVD标题通过ID[value]。循环可用标题，如果[value]的值省略或负数。

switch_vsync [value]
切换场同步（1 ==开启，0 ==关闭）。如果[value]的值没有提供，刷新同步状态反转。

teletext_add_digit <value>
进入/离开字幕的页面号编辑模式，并追加提供的以前输入的数字。
0 .. 9 - 附加apropriate数字。 （启用编辑模式，如果从一般要求模式，并切换到正常模式时。）
-     - 删除最后的页码数字。 （退格仿真，只能在页码编辑模式。）

teletext_go_link <1-6>
按照目前的字幕的页面给出链接。

tv_start_scan
电视频道开始自动扫描。

tv_step_channel <channel>
选择下一个/上一个电视频道。

tv_step_norm
更改电视制式。

tv_step_chanlist
改变频道列表。

tv_set_channel <channel>
设置当前的电视频道。

tv_last_channel
设置当前电视频道到最后一个。

tv_set_freq <frequency in MHz>
设置电视调谐器的频率。

tv_step_freq <frequency offset in MHz>
设置电视调谐器的频率相对于当前值。

tv_set_norm <norm>
电视调谐器设置规范（包括PAL，SECAM，NTSC制式，...).

tv_set_brightness <-100 - 100> [abs]
设置电视调谐器的亮度或调整，如[abs]设置为0。

tv_set_contrast <-100 -100> [abs]
设置电视调谐器的对比或调整，如[abs]设置为0。

tv_set_hue <-100 - 100> [abs]
设置电视调谐器色调或调整，如[abs]设置为0。

tv_set_saturation <-100 - 100> [abs]
设置电视调谐器饱和或调整，如[abs]设置为0。

use_master
主之间切换和PCM音量控制。

vo_border [value]
切换/设置边界显示。

vo_fullscreen [value]
切换/设置全屏模式。

vo_ontop [value]
切换/设置保持在最上层。

vo_rootwin [value]
切换/设置在根窗口播放。

volume <value> [abs]
增大/减小音量，或将其设置为<value>，如果[abs]不为零。


下面的命令，实际上只可用于OSD菜单控制台模式：

help
帮助文本显示，目前还是空的。

exit
从OSD菜单退出控制台。不像'quit'，不退出MPlayer的。

hide
隐藏了OSD菜单控制台。点击菜单命令unhides它。其他按键绑定的行为一切如常。

run <value>
运行<value>的shell命令。在OSD菜单控制台模式标准输出和标准输入
是通过视频输出。
