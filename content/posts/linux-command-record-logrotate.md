---
title: Linux记录之logrotate
date: 2017-04-25
lastmod: 2017-04-28
draft: false
summary: 在Linux中使用logrotate的详细记录
description: 文章描述
categories: ["Linux"]
tags: ["logrotate"]
---

## 名称

**logrotate** - 轮询，压缩，并且邮件系统日志

## 概要

```
logrotate [-dv] [-f|--force] [-s|--state file] config_file+
```

## 描述

**logrotate**旨在简化生成大量日志文件的系统的管理。 它允许自动旋转，压缩，删除和邮寄日志文件。 每个日志文件可能会每天，每周，每月或更长时间处理。

通常，**logrotate**作为日常的cron作业运行。 除非该日志的标准基于日志的大小，并且**logrotate**每天运行多次，否则不会在一天内多次修改日志，或者除非使用**-f**或**-force**选项。

可以在命令行中给出任意数量的配置文件。 稍后的配置文件可能会覆盖早期文件中给出的选项，因此**logrotate**配置文件的列出顺序很重要。 通常，应该使用包含所需的任何其他配置文件的单个配置文件。 有关如何使用include指令来完成此操作的更多信息，请参阅下文。 如果在命令行中指定了目录，该目录中的每个文件都将被用作配置文件。

如果没有给出命令行参数，**logrotate**将打印版本和版权信息，以及一个简短的使用摘要。 如果在旋转日志时发生任何错误，**logrotate**将退出非零状态。

## 选项

##### -v,  — —verbose

> 打开详细模式。

##### -d,  — — debug

> 打开调试模式，并表示**-v**。 在调试模式下，不会对日志或**logrotate**状态文件进行任何更改。

##### -f,  — —force

> 告诉**logrotate**强制旋转，即使它不认为这是必要的。 有时，在将新条目添加到**logrotate**之后，或者如果旧的日志文件已被手动删除，则新文件将被创建，并且日志记录将继续正常。

##### -m, ——mail <command>

> 告诉**logrotate**在发送日志时使用哪个命令。 该命令应接受两个参数：
> 
1. 消息主题，
2. 收件人。 
> 然后，该命令必须在标准输入上读取消息并将其邮寄给收件人。 默认邮件命令为**/bin/mail -s**。

##### -s, ——state <statefile>

> 告诉**logrotate**使用备用状态文件。 如果以不同的日志文件集合的不同用户运行logrotate，这将非常有用。 默认状态文件为/var/lib/logrotate/status。

##### ——usage

> 打印一个短的使用信息。

## 配置文件

**logrotate**从命令行中指定的一系列配置文件读取应该处理的日志文件的所有内容。 每个配置文件都可以设置全局选项（本地定义覆盖全局选项，后面的定义会覆盖较早的定义），并指定要旋转的日志文件。 一个简单的配置文件如下所示：

```
# sample logrotate configuration file
       compress

       /var/log/messages {
           rotate 5
           weekly
           postrotate
                                     /sbin/killall -HUP syslogd
           endscript
       }

       "/var/log/httpd/access.log" /var/log/httpd/error.log {
           rotate 5
           mail www@my.org
           size=100k
           sharedscripts
           postrotate
                                     /sbin/killall -HUP httpd
           endscript
       }

       /var/log/news/news.crit {
           monthly
           rotate 2
           olddir /var/log/news/old
           missingok
           postrotate
                                     kill -HUP ‘cat /var/run/inn.pid‘
           endscript
           nocompress
       }
```

前几行设置全局选项; 在该示例中，日志在旋转后被压缩。 请注意，任一行上的第一个非空白字符为＃则为注释，可能会显示在配置文件的任何位置。
配置文件的下一部分定义了如何处理日志文件`/var/log/messages`。 日志将被删除之前经过五周轮换。 日志文件旋转后（但在旧版本的日志已压缩之前），命令/ sbin / killall -HUP syslogd将被执行。
下一节将定义/var/log/httpd/access.log和/var/log/httpd/error.log的参数。 当它们的长度超过100k时，它们被旋转，并且旧的日志文件在经过5次旋转之后被邮寄（未压缩）到www@my.org，而不是被移除。 **sharedscripts**意味着**postrotate**脚本只能运行一次（在旧日志被压缩之后），而不是一次旋转的每个日志。 请注意，本节开头的第一个文件名的双引号允许logrotate在名称中旋转具有空格的日志。 普通shell引用规则适用，支持'，'和\字符。
最后一节定义/ var / log / news中所有文件的参数。 每个文件每月轮换一次。 这被认为是一个单循环指令，如果多于一个文件发生错误，则日志文件不会被压缩。
请谨慎使用通配符。 如果指定`*`，则**logrotate**将旋转所有文件，包括以前旋转的文件。 这样做的方法是使用**olddir**指令或更精确的通配符（例如* .log）。
以下是有关可以包含在logrotate配置文件中的指令的更多信息：

##### compress

> 日志文件的旧版本默认使用**gzip**进行压缩。另请参见**nocompress**。

##### compresscmd

> 指定用于压缩日志文件的命令。 默认是**gzip**。 另见**compress**。

##### uncompresscmd

> 指定用于解压缩日志文件的命令。 默认是**gunzip**。

##### compressext

> 指定在压缩日志文件中使用的扩展名，如果启用压缩。 默认值为配置的压缩命令。

##### compressoptions

> 如果有一个正在使用，命令行选项可能会被传递到压缩程序。 **gzip**的默认值为“-9”（最大压缩）。

##### copy

> 制作日志文件的副本，但不要更改原始文件。 例如，可以使用此选项来创建当前日志文件的快照，或者某些其他实用程序需要截断或修剪文件时使用此选项。 使用此选项时，**create**选项将不起作用，因为旧日志文件保持原样。

##### copytruncate

> 在创建副本后，将原始日志文件截断，而不是移动旧的日志文件，并可选择创建一个新的日志文件。当某些程序无法关闭其日志文件时，可以使用它，因此可能会永远写入（附加）到 以前的日志文件。 请注意，在复制文件和截断文件之间存在非常小的时间片段，因此某些对数据可能会丢失。 使用此选项时，**create**选项将不起作用，因为旧日志文件保持原样。

##### create

> mode owner group

> 在旋转之后（运行**postrotate**脚本之前），立即创建日志文件（与刚刚旋转的日志文件的名称相同）。 mode指定八进制日志文件的模式（与**chmod**相同），owner指定拥有日志文件的用户名，group指定日志文件所属的组。 可以省略任何日志文件属性，在这种情况下，新文件的这些属性将使用与原始日志文件相同的值作为省略的属性。 可以使用**nocreate**选项禁用此选项。

##### daily

> 日志文件每天都会旋转。

##### dateext

> 存档旧版本的日志文件，添加像YYYYMMDD这样的日常扩展，而不是简单地添加一个数字。可以使用**dateformat**选项配置扩展。

##### dateformat

> format_string

> 使用类似于strftime函数的符号指定**dateext**的扩展名。只允许％Y％m％d和％s说明符。默认值为 - ％Y％m％d。请注意，扩展名中分隔日志名称的字符也是dateformat字符串的一部分。系统时钟必须设置为2001年9月9日，以便％s正常工作。请注意，以此格式生成的日期戳记必须是可排序的（即，首先是年份，然后是当天的月份），例如，2001/12/01可以，但01/12/2001不是，因为01/11 / 2002年将排序较低，会在最后）。这是因为当使用rotate选项时，logrotate会对所有旋转的文件名进行排序，以查找哪些日志文件较旧，应该删除。

##### delaycompress

> 将上一个日志文件的压缩推迟到下一个旋转周期。 这仅在与**compress**组合使用时才起作用。 当某些程序无法关闭其日志文件时，可以使用它，因此可能会持续写入以前的日志文件一段时间。

##### extension

> ext

> 日志文件在旋转后被赋予最终扩展名ext。 如果使用压缩，则分机后会出现压缩扩展（通常为**.gz**）。

##### ifempty

> 旋转日志文件即使它是空的，覆盖**notifempty**选项（ifempty是默认的）。

##### include

> 文件或目录

> 读取作为参数给出的文件，就好像它被包含在**include**指令出现的inline中一样。 如果给出了一个目录，则该目录中的大多数文件将按照字母顺序读取，然后继续处理包含文件。 唯一被忽略的文件是不是常规文件（例如目录和命名管道）的文件以及其名称以**tabooext**指令指定的禁用扩展名之一的文件。 **include**指令可能不会出现在日志文件定义内。

##### mail

> address

> 当一个日志被旋转出来，它被邮寄到地址。 如果特定日志不要生成邮件，则可以使用**nomail**指令。

##### mailfirst

> 使用**mail**命令时，邮寄刚刚转动的文件，而不是即将到期的文件。

##### maillast

> 使用**mail**命令时，邮寄即将到期的文件，而不是刚刚转动的文件（这是默认值）。

##### maxage

> count

> 删除长于<count>天的旋转日志。仅当日志文件要旋转时，才会检查该时间。如果配置了**maillast**和**mail**，这些文件将邮寄到配置的地址。

##### minsize

> size

> 当日志文件长度大于字节大小时，日志文件将被旋转，但不会在额外指定的时间间隔（**daily**, **weekly**, **monthly**, or **yearly**）之间旋转。相关的**size**选项是相似的，除了它与时间间隔选项相互排斥，并且它导致日志文件被旋转而不考虑最后的旋转时间。当使用**minsize**时，会考虑日志文件的大小和时间戳。

##### missingok

> 如果日志文件丢失，请转到下一个文件，不会发出错误消息。 另见**nomissingok**。

##### monthly

> 日志文件在一个月内首次运行**logrotate**（这通常在月的第一天）旋转。

##### nocompress

> 旧版本的日志文件不会用**gzip**压缩。 另见**compress**。

##### nocopy

> 不要复制原始日志文件并将其保留。 （这覆盖**copy**选项）。

##### nocopytruncate

> 创建副本后，请勿将原始日志文件截断（这将覆盖**copytruncate**选项）。

##### nocreate

> 不会创建新的日志文件（这将覆盖**create**选项）。

##### nodelaycompress

> 不要将上一个日志文件的压缩推迟到下一个循环周期（这覆盖了**delaycompress**选项）。

##### nodateext

> 不将旧版本的日志文件添加日期扩展名（这将覆盖**dateext**选项）。

##### nomail

> 不要将旧的日志文件邮寄到任何地址。

##### nomissingok

> 如果日志文件不存在，则发出错误。 这是默认的。

##### noolddir

> 日志在日志通常所在的同一目录中旋转（这覆盖了**olddir**选项）。

##### nosharedscripts

> 为每个旋转的脚本运行**prerotate**和**postrotate**脚本（这是默认值，并覆盖**sharedscripts**选项）。

##### notifempty

> 如果日志为空，则不要旋转日志（这将覆盖**ifempty**选项）。

##### olddir

> directory

> 日志被移动到目录中进行旋转。 该目录必须与正在旋转的日志文件位于相同的物理设备上，并且假定为相对于保存日志文件的目录，除非指定了绝对路径名。 当使用此选项时，所有旧版本的日志最终在目录中。 该选项可能被**noolddir**选项覆盖。

##### postrotate/endscript

> 在旋转日志文件之后，执行**postrotate**和**endscript**（两者都必须出现在行上）之间的命令。 这些指令只能出现在日志文件定义的内部。 另见**prerotate**。

##### prerotate/endscript

> **prerotate**和**endscript**之间的命令（两者都必须出现在行上）在日志文件旋转之前执行，只有日志将被实际旋转。 这些指令只能出现在日志文件定义的内部。 另见**postrotate**。

##### firstaction/endscript

> 在执行预旋转脚本运行之前，只有至少有一个日志将被实际旋转，才能执行**firstaction**和**endscript**之间的命令（两者必须出现在行上），所有与通配符匹配的模式匹配的日志文件都将被旋转 。 这些指令只能出现在日志文件定义的内部。 另见**lastaction**。

##### lastaction/endscript

> 在运行postrotate脚本后，只有至少有一个日志被旋转后，**lastaction**和**endscript**（两者必须出现在行上）之间的命令将被执行一次，所有符合通配符的模式的日志文件都会被旋转。 这些指令只能出现在日志文件定义的内部。 另见**firstaction**。

##### rotate

> count

> 日志文件在删除或邮寄到**mail**指令中指定的地址之前旋转<count>次。 如果count为0，则删除旧版本，然后旋转。

##### size

> size

> 当日志文件长度大于字节大小时会旋转。 如果大小后跟M，则大小假设为兆字节。 如果使用k，则大小为千字节。 所以尺寸100，尺寸100k，尺寸100M都是有效的。

##### sharedscripts

> 通常，对于旋转的每个日志，**prescript**和**postscript**，这意味着单个脚本可能会针对匹配多个文件（例如/ var / log / news / *示例）的日志文件条目运行多次。 如果指定了**sharedscript**，则脚本只能运行一次，无论有多少个日志与通配符一致。 但是，如果模式中的任何日志都不需要旋转，则脚本将不会运行。 此选项将覆盖nosharedscripts选项，并且意味着**create**选项。

##### shred

> 使用**shred -u**而不是unlink()删除日志文件。这应该确保日志在计划删除后不可读取;这是默认关闭。另见**noshred**。

##### shredcycles

> count

> 要求GNU shred在删除之前覆盖日志文件**count**。没有此选项，将使用**shred**的默认值。

##### start

> count

> 这是用作旋转基数的数字。 例如，如果指定0，那么将在原始日志文件中旋转日志时使用.0扩展名创建。 如果指定9，将创建日志文件.9，跳过0-8。 文件仍将旋转指定的**count**指令的次数。

##### tabooext

> [+] list

> 当前的禁忌扩展列表已更改（有关禁忌扩展的信息，请参阅**include**指令）。 如果一个+提供了扩展名列表，则当前禁用扩展名列表将被扩充，否则被替换。 在启动时，禁用扩展列表包含.rpmorig，.rpmsave，，v，.swp，.rpmnew和〜。

##### weekly

> 如果当前工作日小于上次轮换的工作日，或者自上次旋转以来已经过了一周以上，则会旋转日志文件。 这通常与一周中的第一天的旋转日志相同，但如果每天晚上不运行登录，它的工作效果会更好。

##### yearly

> 如果当前年份与最后一次旋转不同，则会旋转日志文件。