
Iterate over array to search for something in files
```
array=( 73644 69818 73528 ); for i in "${array[@]}"; do grep "$i cre" *; done
```
