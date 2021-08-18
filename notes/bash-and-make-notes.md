


`make -s` <-- silent

Set variable to result of bash script
`TEST=$(echo hi | awk '{ print $1 }')`

Watching netstat results for only sockets with queueing
`sudo watch -n 1 $'netstat -antlp | awk \'$2 > 0 || $3 > 0 {print $0}\''`
