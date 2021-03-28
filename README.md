# Урок 11. Управление процессами.
## Написать свою реализацию ps ax используя анализ /proc:

```
echo -e "PID\tTTY\tSTAT\tTIME\t\tCOMMAND"

for pspro in $(ls /proc/ | grep "^[0-9]" | sort -n)
do
        if [[ -f /proc/$pspro/status ]]
        then
        PID=$pspro

        tty=`ls -l /proc/$pspro/fd | grep -m1  dev | sed 's/\/dev\///' | awk '{print $NF}'`
        if [[ -z "$tty" || "$tty" = "null" ]]
        then
                tty=`echo ?`
        fi

        state=`awk '{print $3}'  /proc/$pspro/stat`

        time=`awk -v  hz="$(getconf CLK_TCK)" '{print strftime ("%M:%S", ($14+$15)/hz)}' /proc/$pspro/stat`

        command=`cat /proc/$pspro/comm`

        echo -e "$PID\t$tty\t$state\t$time\t\t$command"
        fi
done
```
