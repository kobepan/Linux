# linux12shell流程的控制        

## 条件判断式语句
1.使用[-e /root ] && echo yes || echo no来判断/root是否存在如果-e改为-d则是判断/root是否为目录，如果为-f则判断是否文件。还可以通过-w、-r、-x来查看文件访问权限。还可以[ a.c -ef b.c ]来判断两个文件是否有一个iNode，或者-ef为-nt、-ot分别表示判断文件1是否比2新、旧。还可以[数字 -eq 数字]表示判断两个数字是否相等，或者为-ge、-le、-gt、-lt表示>=、<=、>、<。也可以在[]中使用-a、-o实现多重判断。

## 单分支if
1.脚本1判断当前用户是不是root：
------------------------
//这里的-f2表示切割=后第二个字符串
curUser=&( env | grep USER | cur -d "=" -f2  )

if [ "$curUser" == "root" ]
    then
        echo "curUser is $curUser"
fi

------------------------

2.脚本2查看当前根分区占用是否达到90%
-------------------------------------

rate=$( df -h | grep sda1 | awk '{print $5}' | cut -d "%" -f 1 )

if [ "$rate" -ge "90" ]
    then
        echo "/ is full"
fi

-------------------------------------

3.脚本3判断当前输入文件是否是目录
-------------------------------------

read -p "input a file" dir

if [ -d "$dir" ]
    then 
        echo "$dir is a dir"
    else
        echo "$dir is not a dir"
fi
    
-------------------------------------


4.脚本4判断atd是否存在进程
-------------------------------------

//这里的ps aux相当于查进程
test=$( ps aux | grep atd | awk '{print $11}' | cut -d "/" -f 4  | grep atd )

if [ -z "$test"  ]
        then
                echo "$test does not exit"
        else
                echo "$test does exit"
fi

-------------------------------------

5.