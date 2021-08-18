Awk is easy

`[pattern] action`

`cat file | awk '$1 > 0 { print $0 }'`


Special cases:
`BEGIN` pattern: action happens before we process first line
- e.g. set up a variable, or a file separator (`{FS="\t"}`)
`END` pattern: action happens after we process last line
- e.g. print results from variables we've been using to record info

Special values:
`NF`: number of fields on line
`NR`: number of current line (on `END`, this will equal total number of lines)
`/something/`: regex matches something
`length($0)`: length in characters of line

Commands:
`print`: by itself, prints whole line (just like `print $0`)
`printf("%d:%d\n", varname1, varname2)`: does standard formatting stuff
`for (i = 1; i <= NF; i = i + 1) sum sum + $i`: for loop





EXAMPLE:
```
#! /usr/bin/awk -f

BEGIN { (thr == 0)? thr=200000 : thr=thr * 1000;
        rampup=(thr * 10); count=0; total=0
      }
NR > rampup { if (count == thr) {
                avg = (total / count);
                printf("%d\n", avg)
                count = 0; total = 0;
              } else {
                count = count + 1; total = total + $1
              }
            }
```



MY COMPARISON PROGRAM:
```
#! /bin/sh

# Compares average of values in CONTROL_FILE with average of values in EXP_FILE.
# If the averages are within a margin specified via MARGIN, then echo 0.
# Echo 1 if control is greater than exp, and -1 if exp is greater than control.

CONTROL_FILE=$1
EXP_FILE=$2
MARGIN=$(bc <<< "1 + $3")

control_avg=$(cat $CONTROL_FILE \
  | awk 'BEGIN { total=0; } { total = total + $1 } END { avg = total / NR; print avg}'
  )
#echo "CONTROL: $control_avg"
exp_avg=$(cat $EXP_FILE \
  | awk 'BEGIN { total=0; } { total = total + $1 } END { avg = total / NR; print avg}'
  )
#echo "EXP: $exp_avg"

# You need double quote to expand variables in bc
# 1 for true, 0 for false
gt=$(bc -l <<< "$control_avg > $exp_avg")
lt=$(bc -l <<< "$control_avg < $exp_avg")
if [ $gt -eq 1 ]; then
    d=$(bc -l <<< "$control_avg / $exp_avg")
    is_in_range=$(bc -l <<< "$d < $MARGIN")
    if [ $is_in_range == 1 ]; then
        echo 0
    else
        echo 1
    fi
elif [ $lt -eq 1 ]; then
    d=$(bc -l <<< "$exp_avg / $control_avg")
    is_in_range=$(bc -l <<< "$d < $MARGIN")
    if [ $is_in_range == 1 ]; then
        echo 0
    else
        echo -1
    fi
else
    echo "ELSE"
    echo 0
fi
```

