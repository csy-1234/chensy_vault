---
date: "20250827"
docx:
  - shell脚本循环与case语句.docx
---
# shell脚本循环与case语句
## 笔记
==循环语句==

>for循环
	for 变量 in 值1 值2 值3 ...或\`eq 1 3\`
	do
	    命令
	done
	
==while循环语句==

>	while \[ 条件 ]
	do
	    命令
	done
>	
contiune进入下次循环
break跳出最近一层循环

==case分支语句==

>	case 变量 in
	    值1)
        命令
        ;;
	    值2)
	        命令
	    ;;
	    \*)
	        默认命令
	    ;;
	esac

