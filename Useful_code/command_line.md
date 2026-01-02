# List of useful scripts
This is a list of useful commands to data/text manipulation using the command line

#### while read
```while read -r line; do echo $line;rm -rf $line*; done < ../low_samples_names.txt```

#### pipe
```cut -f1 | xargs -i {} basename {} .fastq```

#### Summary table with awk
```
awk /^===/ {current_file = $3;next}
!/^#/ && $5 < 1e-6 {counts[current_files][$3]++ }
END{
print "File_name", "HMM_Name","Count"
for (files in counts){
for (hmm in counts[files]){
print file,hmm,counts[file][hmm]
}
}
}
```

#### Find missing output
This for loop is to find the missing output from a large number of files
```
for f in *fastq.gz
do
if [ ! -f "${f%.fastq.gz}.AGS.out" ]; then
echo "Missing output for: $f"
fi
done
```

#### Another example of finding missing output
```
for dir in */
do
base=$(basename "$dir")
if [ ! -f "${base}.AGS.out" ]; then
echo "Missing output for: $dir"
fi
done
```


#### Another example of finding missing output using while
```
while read -r line
do echo $line
if [ ! -f "Cao.et.al/${line}.AGS.out" ]; then
echo "Missing file for: $line"
fi
done < cao.sample.list.txt
```

#### The translate command to replace every "|" to "\n"
`tr "|" "\n"`
