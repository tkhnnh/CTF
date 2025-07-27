Note that the file is `disko-1.dd.gz`, so I unzip it with gunzip
```
gunzip disko-1.dd.gz
```
I tried to read the content of the disko-1.dd file but it display non-readable characters
Then I tried `strings` in order to read the file and filtered out the flag
```
strings disko-1.dd | grep "pico"
```
