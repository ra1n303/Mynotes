getopts



格式

while getopts vx:y:: ARGS

do

​	case $ARGS in

​	x)

​	y)

​	esac

done







vx:y: 表示支持的选项

表示参数



v后面没冒号

表示不需要参数



x后面有一个冒号

表示必须加一个值



y后面两个冒号

容错性高





ARGS变量名，推荐用ARGS，保存当前处理的选项字符

$OPTARG是getopts内置变量，用于存储当前选项的参数值





```
#!/bin/bash
while getopts vx:y: ARGS  # 定义选项：v 不带参数，x 和 y 需要参数（后面跟 :）
do
        case $ARGS in # 根据选项进行处理
        v)  # 处理 -v 选项
                        echo "version:1.1";;
        x)  # 处理 -x 选项
                echo "x"
                sec=$OPTARG  # 获取 -x 后面的参数值
                echo "sec=$sec" ;;  # 输出参数值
        y)
                echo "y"
                thr=$OPTARG  # 获取 -y 后面的参数值
                echo "thr=$thr";;  # 输出参数值
        esac
done
```

