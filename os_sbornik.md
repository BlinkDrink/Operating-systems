
https://drive.google.com/file/d/1W7nO7o6HdBlX7_W8NHI7whcoSU3bO4MU/view?usp=share_link

# 1.1. Поточна обработка (теми 1,2,3)

### 1. 2016-SE-01
```Bash
egrep '[02468]' philip-j-fry.txt | egrep -v '[a-w]' | wc -l
egrep '[.]'
```
### 2. 2017-IN-01
```Bash
find / -readable -printf "%u\n" 2> /dev/null | egrep $(id | sed -E "s/(\|)/ /g" | cut -d " " -f 2) | wc -l
find / -readable -printf "%U\n" 2> /dev/null | awk '$0 == PROCINFO["uid"]' | wc -l
find / -user $(id | sed -e "s/(\|)/ /g" | cut -d " " -f 2) | wc -l
find / -user $(whoami) 2> /dev/null | wc -l
```
### 3.  2017-IN-02
```Bash
find . -readable -type f -printf "%s %p\n" | sort -n -k 1,1 | awk '$1 == 0 {print $2}' | xargs rm
find $(awk -F ":" '$3 == PROCINFO["uid"] {print $6}' /etc/passwd) -type f -readable -printf "%s %p\n" | sort -n -k 1,1 | cut -d " " -f 2 | tail -n 5 | xargs rm
find ~ -type f -printf "%s %p\n"  |  sort -n -k 1,1 |  cut -d " " -f 2 |  tail -n 5 |  xargs  rm
```
### 4. 2017-IN-03
```Bash
awk -F ":" '$5 ~ /[A-Za-z]+ ([a-zA-Z]+a,)|([a-zA-Z]+a$)/ {print substr($1, 3 , 2)}' ~/fakepasswd  | sort -n | uniq -c | sort -n -k 1,1 | tail -n 1
awk -F ":" '$5 ~ /^[a-zA-Zа-яА-Я]+ [a-zA-Zа-яА-Я]+(a|а),*[A-ZА-Я]/' /etc/passwd | cut -c 3-4 | sort | uniq -c | sort -n -k 1,1 | tail -n 1
awk -F ":" '$6 ~ /Inf/' ~/fakepasswd | cut -d ":" -f 1,5 | cut -d "," -f 1 | egrep "(a|а)$" | cut -c 3-4 | uniq -c | sort -n -k 1,1 | tail -n 1
 ```
### 5. 2017-SE-01
```Bash
find . -readable -printf "%n %p\n" | sort -n -k 1,1 | tail -n 5 | cut -d " " -f 2
```
### 6. 2018-SE-01
```Bash
find . -type d -exec chmod 644 {} +
```
### 7. 2018-SE-02
```Bash
find $(awk -F ":" '$1 == "pesho" {print $6}' /etc/passwd) -exec stat -c "%i %h %Y" {} + | sort -n -k 2,2 | awk '$2 > 1' | sort -n -k 3,3 | tail -n 1 | cut -d " " -f 1
find ~pesho -printf "%n %T@ %i\n" | awk '$1 > 1' | sort -n -k 2,2 | tail -n 1 | cut -d " " -f 3,3
```
### 8. 2018-SE-03
```Bash
grep "^s[0-9]+:x:[0-9]+:$(sort -t ":" -n -k 3,3 /etc/passwd | head -n 201 | tail -n 1 | cut -d ":" -f 4):" /etc/passwd | cut -c 2- | sort -t ":" -n -k 1,1 | awk -F ":" '{print $2 ":" $1}'
sort -t ":" -k 3,3 ~/fakepasswd | tail -n +1 | awk -F ":" '{print $6":"$5}' | cut -d "," -f 1 | awk -F ":" '{print $2":"$1}'
```
### 9. 2019-SE-01
```Bash
grep "^[^,]+$(tail -n +2 planets.txt | sort -t ";" -n -k 3,3 | tail -n 1 | cut -d ";" -f 2);" planets.txt | sort -t ";" -n -k 3,3 | head -n 1 | awk -F ";" "{printf("%s\t%n",$1,$4)}"
awk -F ";" -v planetType=$(tail -n +2 planets.txt | sort -t ";" -k 3,3n | tail -n 1 | cut -d ";" -f 2) '$2 == planetType' planets.txt | sort -t ";" -k 3,3n | head -n 1 | cut -d ";" -f 1,4 | tr ';' '\t'
```
### 10. 2019-SE-02
```Bash
awk -F ":" '{print $1":"$6":"$5}' /etc/passwd | cut -d "," -f 1 | awk -F ":" '$2 ~ /\/home\/SI\// {print $2}' | xargs stat -c "%n %Z" | awk '$2 > 1669396735 && $2 < 1674484231' | cut -d " " -f 1 | xargs -I {} awk -F ":" '$6 == "{}" {print $1,$5}' /etc/passwd
```
### 11. 2019-SE-03
```Bash
find ~velin -inum $(find ~velin -type f -printf "%i %T@\n" | sort -n -k 2,2 | tail -n 1 | cut -d " " -f 1) | tr -dc "/\n" | sort | head -n 1 | tr -d "\n" | wc -c
another way instead of tr -d "\n" => grep -o "." | wc -l
find ~velin -printf "%T@ %d %i\n" | sort -k 1,1n -k 2,2n | tail -n 1 | cut -d " " -f 3 | xargs -I {} find / -inum {} -printf "%d\n" 2> /dev/null | head -n 1
```
### 12. 2020-SE-01
```Bash
find ~ -maxdepth 1 -type f -perm 644 | xargs chmod 664
find ~ \( -perm 755 -or -perm 644 \) | xargs chmod g=g+w
```
### 13. 2020-SE-02  
```Bash
egrep "^[0-9]+\|$(awk -F " | " '$3 == "Failure" {print $2}' spacex.txt | sort | uniq -c | sort -n -k 1,1 | tail -n 1 | awk -F " " '{print $2}')\|" spacex.txt | sort -n -t "|" -k 1,1 | tail -n 1 | awk -F "|" '{print $3 ":" $4}'
egrep $(cat spacex.txt | tail -n +2 | egrep "\|Failure\|" | cut -d "|" -f 2 | sort | uniq -c | sort -n -k 1,1 | tail -n 1 | awk '{print $2}') spacex.txt | sort -t "|" -k 1,1n | tail -n 1 | awk -F "|" '{print $3" "$4}'
```
### 14. 2022-CE-01
```Bash
find ~ -maxdepth 1 -type f -user $(whoami) | xargs chmod 664
```
# 1-2. Скриптове (теми 1,2,3,4,5)

### 15. 2016-SE-01
```bash
find . -type l -printf "%p %Y\n" | awk '$2 == "N" {print $1}'
find -L . -type l -print
```
### 16. 2016-SE-02
```Bash
#! /bin/bash
reg='^[0-9]+$'
[ $# -eq 1 ] || exit 1
[ $(id -u) -eq 0 ] || exit 2
[[ $1 =~ $reg ]] || exit 3
T=$(mktmp)
ps -e -o uid,pid,rss > $T
for i in $(awk '{print $1}' $T | sort | uniq ); do
	R=$(awk -v foo=$i '$1 == foo {s+=$3} END {print s}' $T)
	#echo "$i $R"
	if [ $R -le $1 ]; then
	continue
	fi
	P=$(grep "$i" $T | sort -rn -k 3,3 | head -n 1 | awk '{print $2}')
	kill -15 $P
	sleep 2
	kill -9 $P
done
rm -- $T
```
### 17. 2016-SE-03
```bash
#! /bin/bash
while read U H; do
	if [ -z $H ]; then
		echo "$U"
		continue
	fi
	if [ ! -d "$H" ]; then
		echo "$U"
		continue
	fi
	MyUsr=$(stat -c "%U" $H)
	if [ "$MyUsr" != "$U" ]; then
		echo "$U"
		continue
	fi
	M=$(stat -c "%A" $H | cut -c 3)
	if [ "$M" != "w" ]; then
		echo "$U"
		continue
	fi
done < <(cat /etc/passwd | awk -F ":" '{print $1 " " $6}')
```
### 19. 2016-SE-04
```bash
#! /bin/bash
[ $# -eq 2 ] || exit 1
targetFile=""
if [ $(grep "$1" $1 | wc -l) -gt $(grep "$2" $2 | wc -l) ]; then
        targetFile=$1
else
        targetFile=$2
fi
cut -d "-" -f 2- $targetFile | sort | cut -c 2- > ${targetFile}.songs
```
### 20. 2016-SE-04
```bash
#! /bin/bash
[ $# -eq 1 ] || exit 1
cat $1 | sed -E "s/ - / : /2" | awk -F " - " '{counter+=1; print counter ".  : " $2}' | sort -t ":" -k 2,2 | sed -E "s/ : / - /2" | sed -E "s/ : //1"
```

```bash
#! /bin/bash
cut -d "-" -f 2- $1 | awk '{s=s+1; print s"."$0}' | sort -k 2,
```
### 21. 2016-SE-04
```bash
#! /bin/bash
[ $# -eq 3 ] || exit 1
str1Val=$(grep "$2=" $1 | cut -d "=" -f 2)
str2Val=$(grep "$3=" $1 | cut -d "=" -f 2)
newStr2=""
for i in $str2Val; do
        if [ $(echo $str1Val | grep -c $i) -eq 0 ]; then
                newStr2="${newStr2} ${i}"
        fi
done
newStr2=$(echo $newStr2)
sed -E "s/($3=).*/\1$newStr2/" $1
```
Better
```Bash
#!/usr/bin/bash
[ $# -eq 3 ] || exit 1

File=$1
K1=$2
K2=$3
V1=$(grep "${K1}=" ${File} | awk -F "=" '{print $2}')
V2=$(grep "${K2}=" ${File} | awk -F "=" '{print $2}')

NV=$(comm -13 <(echo ${V1} | tr ' ' '\n' | sort) <(echo ${V2} | tr ' ' '\n' | sort) | xargs)
sed -i "s/${K2}=${V2}/${K2}=${NV}/" ${File}
```
### 22. 2016-SE-04
```bash
#! /bin/bash
[ $# -eq 1 ] || exit 1
[ $(id -u) -eq 0 ] || exit 2
T=$(mktmp)
fooProcesses=$(ps -u $1 | wc -l) # count of processes of target user or add -U $1 for both RUID or EUID
ps --no-headers -e -o user,pid,time --sort user > $T # copy current snapshot of all processes to $T
awk '{print $1}' $T | uniq -c | sort -k 1,1n | awk -v targetCnt=$fooProcesses '$1 > targetCnt {print $2}' # Print all users who have more processes than FOO user

cnt=0
initialTime=$(date +%s -d "00:00:00")
for t in $(awk '{print $3}' $T);do
	cnt=$(($cnt+1))
	currTime=$(echo "$(date +%s -d $t) - $initialTime" | bc)
	totalTime=$((totalTime + currTime))
done

averageTime=$((totalTime / cnt))
echo $averageTime
for i in $(egrep $1 $T); do
	seconds=$(cut -d " " -f 3 | awk -F ":" '{totalTime=$1 * 3600 + $2 * 60 + $3;print  totalTime}')
	if [ $seconds -gt $((averageTime * 2)) ];then
		kill -15 $i
		sleep 2
		kill -9 $i
	fi
done
rm $T
```
### 23. 2017-IN-03
```bash
#! /bin/bash
latestFile=""
while read U H; do
	currFile=$(find $H -type f -printf "%p %T@\n" | sort -k 2,2n | tail -n 1)
	if [ -z $latestFile ]; then
		latestFile=$currFile
		continue
	fi

	if [ $(echo $currFile | awk '{print $2}') -gt $(echo $latestFile | awk '{print $2}') ]; then
		latestFile=$currFile
	fi
done < <(awk -F ":" '{print $1 " " $6}' /etc/passwd)
echo $latestFile
```
One liner
```Bash
awk -F ":" '{print $6}' /etc/passwd | xargs -I {} find {} -type f -printf "%u %p %T@\n" 2> /dev/null | sort -k 3,3n | tail -n 1 | awk '{print $1" "$2}'
```
### 24. 2017-SE-01
```bash
#! /bin/bash
[ $# -ge 1 ] || exit 1
if ! [ -z $2 ]; then
	find $1 -printf "%p %n\n" | awk -v links=$2 '$2 >= links {print $1}'
	exit 0
fi
find $1 -type l -printf "%p %Y\n" | awk '$2 == "N" {print $1}'
```

### 25. 2017-SE-02
```bash
#! /bin/bash
src="$(dirname $1)/$(basename $1)"
dst="$(dirname $2)/$(basename $2)"
str="${3}"
for result in $(find $src -type f -name "*${str}*" 2>/dev/null); do
        NN=$(echo $result | sed -E "s|${src}|${dst}|1")
        mkdir -p $(dirname $NN)
        mv $result $NN
done
```
### 26. 2017-SE-03
```bash
#! /bin/bash
[ $(id -u) -eq 0 ] || exit 1
T=$(mktemp)
ps --no-headers -e -o user,pid,rss > $T
for currUsr in $(awk '{print $1}' $T | sort | uniq); do
        usrData=$(grep "$currUsr" $T | awk '{sum+=$3} END {print $1 " " NR " " sum}')
        avgRss=$(echo $usrData | awk '{print $3 / $2}')
        biggestProcess=$(egrep $currUsr $T | sort -n -k 3,3 | tail -n 1 | awk '{print $2" "$3}')
        biggestPid=$(echo $biggestProcess | awk '{print $1}')
        biggestRss=$(echo $biggestProcess | awk '{print $2}')
        if [ $(echo "${biggestRss} > (${avgRss} * 2)" | bc) -eq 1 ]; then
	            kill -15 $biggestPid
                sleep 2
                kill -9 $biggestPid
        fi
done
rm $T
```
### 27. 2017-SE-04
```bash
#! /bin/bash
[ $# -ge 1 ] || exit 1
dirName=$1
fileName=""
if ! [ -z $2 ];then
        fileName=$2
fi
# %l - obj %Y - if link is broken
if ! [ -z $fileName ]; then
        find $dirName -type l -printf "%p %Y %l\n" | awk '$2 == "N" {nonexistentCnt+=1}  $2 != "N" && $3 != "" {print $1,"->",$3} END {print "Broken symlinks:",nonexistentCnt}' > $fileName
else
        find $dirName -type l -printf "%p %Y %l\n" | awk '$2 == "N" {nonexistentCnt+=1} $2 != "N" && $3 != "" {print $1,"->",$3} END {print "Broken symlinks:",nonexistentCnt}'
fi
```
### 28. 2017 -SE-05
```bash
#! /bin/bash
[ $# -eq 2 ] || exit 1
D=$1
S=$2
reg="vmlinuz-[0-9]+\.[0-9]+\.[0-9]+-${2}"
find $1 -maxdepth 1 | egrep $reg | rev | cut -d "/" -f 1 | rev | sed -E "s/(vmlinuz)-([0-9]+)\.([0-9]+)\.([0-9]+)-([a-zA-Z0-9]+)/\2 \3 \4 \1-\2.\3.\4-\5/" | sort -k 1,1n -k 2,2n -k 3,3n | tail -n 1 | cut -d " " -f 4
```

```Bash
#! /bin/bash
D=$1
S=$2
RE="vmlinz-[0-9]+\.[0-9]+\.[0-9]+-${S}"
find $D -type f -regextype egrep -regex ".+/${RE}" -printf "%f\n" | sort -V
```
### 29. 2017-SE-06
```bash
#! /bin/bash
#[ $(id -u) -eq 0 ] || exit 1
T=$(mktemp)
ps --no-headers -o user,pid,rss > $T
totalRootRSS=$(awk '$1 == "root" {sum+=$3} END {print sum}' $T)
while read U P R; do
        iDir=$(egrep $U /etc/passwd | cut -d ":" -f 6)
        totaliRss=$(awk -v targetUsr=$U '$1 == targetUsr {sum+=$3} END {print sum}' $T)
        if [ -z $iDir ]; then
                echo "$U this user has undefined home diretory"
        elif ! [ -d $iDir ]; then
                if [ $totalRootRSS -lt $totaliRss ]; then
                        echo "$U should stop all their processes"
                        #loop over all processes for iUser in $T and kill them
                fi
        elif [ "$(stat -c "%U" $iDir)" != $U ]; then
                echo "$U isnt the owner of the directory"
                #loop over all processes for iUser in $T and kill them
        elif [ "$(stat -c "%A" $iDir | cut -c 3)" != "w" ]; then
                echo "$U cannot write in their own directory"
                #repeat same as row 15
        fi
done <(cat $T)
rm $T
```
### 30. 2018-SE-01
```Bash
#!/usr/bin/bash
touch friends.txt
touch friendsList.txt
touch result.txt
while read i ; do
while read j ; do
while read p ; do
while read f ; do

echo $(basename ${p}) $(wc -l ${f}|awk '{print $1}') >> friends.txt
echo $(basename ${p}) >> friendsList.txt
done< <(find $p -type f)
done< <(find $j -type d -maxdepth 1 -mindepth 1)
done< <(find $i -type d -maxdepth 1 -mindepth 1)
done< <(find $1 -type d -maxdepth 1 -mindepth 1)

#while read i; do
# sz=0
# while read p; do
# if [ $i==$(echo ${p}|awk '{print $1}') ]; then
# sz=$((sz+$(echo ${p}|awk '{print $2}')))
# fi
# done< <(sort friends.txt)
# echo ${i} ${sz} >> result.txt
# sz=0
#done< <(sort friendsList.txt|uniq)

while read i ; do
awk '{ if(${i}==${1}) {sum+=${2}} END {print ${i} sum}}' friends.txt >> result.txt
done < <(sort friendsList.txt | uniq)
sort -k 1,1 result.txt | head
```
### 31. 2018-SE-02
```Bash
#!/usr/bin/bash
[ $# -eq 2 ] || exit 1
txtFile=$2
dir=$1
#[ $(ls -A $dir) ] || exit 2
touch $dir/dict.txt
touch helper.txt
number=0

while read i ; do
	echo "$i;$number" >> $dir/dict.txt
	touch $dir/$number.txt
	number=$(( number + 1))
done < <(awk -F ':' '{print $1}' $txtFile | awk '{print $1 " " $2}' | sort | uniq)

index=0
while read i || [ $index -lt $number ]; do
	grep "$i" $txtFile | awk -F ':' '{print $2}' >> $dir/$index.txt
	index=$((index + 1))
done < <(awk -F ';' '{print $1}' $dir/dict.txt)
```
### 32. 2018-SE-03
```bash
#! /bin/bash
src=$1
dst=$2
sort -t ',' -k 2 -k 1,1n $src | sed -E "s/,/\t/" | uniq -f 1 | tr '\t' ',' > $dst
```
### 33. 2019-SE-01
```bash
#a)
#! /bin/bash
reg='^(-|)[0-9]+$'
myMax=0
T=$(mktemp)
while read row; do
        echo $row
        if [[ $row =~ ^(-|)[0-9]+$ ]]; then
	        if [ $(echo $i | egrep -c "-") -eq 1 ];then	
		        if [ $myMax -lt $(echo $row | cut -c 2-) ]; then
                        myMax=$row
                fi
            else
	            if [ $myMax -lt $row ]; then
                        myMax=$row
                fi
		    fi
        fi
done > $T
egrep "$myMax" $T | sort | uniq
rm -r $T
```
```Bash
# b)
#/bin/bash
maxSum=0
maxSumNum=0

while read i ; do
	ifNegat=$(echo $i | sed "s/-//")
	sum=0
	while [ $ifNegat -gt 0 ] ; do
		sum=$(($ifNegat%10 + $sum))
		ifNegat=$((ifNegat/10))
	done
	#sum=$(echo $ifNegat | sed -E "s/(.)/\1\n/g" | awk '{sum+=$0} END {print sum}')
	if [ $maxSum -lt $sum ] ; then
		maxSum=$sum
		maxSumNum=$i
	fi
	if [ $maxSumNum -gt $i ] && [ $sum -eq $maxSum ] ; then
		maxSumNum=$i
	fi
done < <(egrep "^(-|)[0-9]+$" test.txt | sort | uniq)
echo $maxSumNum
```

### 34. 2019-SE
```bash
#! /bin/bash

[ $# -ge 1 ] || exit 1
N=10
if [ $1 == "-n" ]; then
		[ $# -ge 2 ] || exit 2
        N=$2
        shift 2 
fi

for F in $@; do
        IDF=$(basename -s .log $F)
        #tail -n $N $F | sed -E "s/ /@/2" | awk -F "@" -v idfTitle=$IDF '{print $1, idfTitle, $2}'
        tail -n $N $F | sed -E "s/^(.{19}) (.*)$/\1 $IDF \2/" # better way
done | sort -k 1,2
```
### 35. 2019-SE-03
```bash
#! /bin/bash

[ $# -eq 1 ] || exit 1
[ -d $1 ] || exit 2
if ! [ -d ./extracted ];then
        mkdir extracted
fi

DIR=$1

for archive in $(find $DIR -name "*.tgz"); do
        archiveName=$(basename -s .tgz $archive | cut -d "_" -f 1)
        archiveTimeStamp=$(basename -s .tgz $archive | rev | cut -d "-" -f 1 | rev)
        for target in $(tar -tf $archive | grep "meow.txt"); do
                tar -xf $archive -C ./extracted $target
                mv ./extracted/$target ./extracted/${archiveName}_${archiveTimeStamp}.txt
        done
done
```
### 36. 2020-SE-01
```bash
#! /bin/bash

[ $# -eq 2 ] || exit 1
[ -f $1 ] || exit 2
[ -d $2 ] || exit 3

file=$1
dir=$2

echo -e "hostname,phy,vlans,hosts,failover,VPN-3DES-AES,peers,VLAN Trunk Ports,license,SN,key\n" > $file
for logFile in $(find $dir -name "*.log"); do
        echo -n -e "$(basename -s .log $logFile)," >> $file
        echo -n $(grep "^[^\n]" $logFile | tail -n +2 | sed -E "s/ +/ /g" | sed -E "s/[^:]*: (.*)/\1/" | sed -E "s/.*This platform has a (.*) license\./\1/" | tr "\n" "," | sed -E "s/,//10") >> $file
done
```

```Bash
#!/bin/bash

[ $# -eq 2 ] || exit 1
[ -f $1 ] || exit 2
[ -d $2 ] || exit 3

echo hostname,phy,vlans,hosts,failover,VPN-3DES-AES,peers,VLAN Trunk Ports,license,SN,key >> $1

while read i ; do
	hostname=$(basename -s ".log" $i)
	phy=$(grep "Maximum Physical Interfaces" $i | awk -F ":" '{print $2}' | sed "s/ //")
	vlan=$(grep "VLANs" $i | awk -F ":" '{print $2}' | sed "s/ //")
	host=$(grep "Inside Hosts" $i | awk -F ":" '{print $2}' | sed "s/ //")
	failover=$(grep "Failover" $i | awk -F ":" '{print $2}' | sed "s/ //")
	VPN=$(grep "VPN-3DES-AES" $i | awk -F ":" '{print $2}' | sed "s/ //")
	peers=$(grep "*Total VPN Peers" $i | awk -F ":" '{print $2}' | sed "s/ //")
	VLAN=$(grep "VLAN Trunk Ports" $i | awk -F ":" '{print $2}' | sed "s/ //")
	license=$(grep "This platform has" $i | cut -d " " -f 5- | rev | cut -d " " -f 2- | rev)
	SN=$(grep "Serial Number" $i | awk -F ":" '{print $2}' | sed "s/ //")
	key=$(grep "Running Activation Key" $i | awk -F ":" '{print $2}' | sed "s/ //")
echo ${hostname}','${phy}','${vlan}','${host}','${failover}','${VPN}','${peers}','${VLAN}','${license}','${SN}','${key} >> $1

done < <(find $2 -type f)
```
### 37. 2020-SE-02
```bash
#! /bin/bash
[ $# -eq 1 ] || exit 1
[ -f $1 ] || exit 2
file=$1
for host in $(awk '{print $2}' $file | sort -k 1,1 | uniq -c | sort -k 1,1n | tail -n 3 | awk '{print $2}'); do
        echo "$host HTTP/2.0: $(egrep $host $file | egrep -c "HTTP/2.0") non-HTTP/2.0: $(grep $host $file | grep -c -v "HTTP/2.0")"
        egrep $host $file | awk '{print $1}' | sort | uniq -c | sort -n -k 1,1 | tail -n 5
done
#for ipAdress in $(awk '$9 > 302 {print $1}' $file | sort -k 1,1 | uniq -c | sort -k #1,1n | tail -n 5 | awk '{print $2}'); do
#        echo "$(grep $ipAdress $file | awk '$9 > 302 {print $1}' | wc -l) $ipAdress"
#done
```
### 38. 2020-SE-03
```bash
#!/bin/bash
[ $# -eq 2 ] || exit 1
repo=$1; package=$2; packageName=$(basename $package); packageVersion=$(cat $package/version)
tar -cf $packageName.tar $package/tree
xz $packageName.tar
sha=$(sha256sum $packageName.tar.xz | cut -d ' ' -f 1)
if [ $(egrep -c "$packageVersion" $repo/db) -ne 0 ]; then
    R=$(sed -E "s/.*$packageVersion.*//" $repo/db | tr -s '\n')
    rm $repo/packages/$(egrep "$packageVersion" $repo/db | cut -d ' ' -f 2).tar.xz
    echo -e "$R" > $repo/db
fi
mv $packageName.tar.xz $repo/packages/$sha.tar.xz
echo "$packageName-$packageVersion $sha" >> $repo/db
echo -e "$(sort $repo/db)" > $repo/db
```
### 39. 2020-SE-04
```bash
#! /bin/bash
[ $# -eq 2 ] || exit 1
[ -d $1 ] || exit 2
SRC=$1
DST=$2
mkdir -p $DST/images
for image in $(find $SRC -name "*.jpg" | sed -E "s/ /-/g"); do
    fileName=$(echo $image | sed -E "s/-/ /g")
    title=$(basename -s .jpg "$fileName" | sed -E "s/\([^()]*\)//g" | tr -s ' ' #| sed -E "s/ *$//g")
    album=""
    if [ $(echo "$fileName" | egrep -c "\(.*\)") -eq 0 ]; then
            album="misc"
    else
            album=$(echo -e $fileName | egrep -o "\([a-zA-Z ]*\)" | tail -n 1 | tr -d '()'
            #album=$(echo -e $fileName | awk -F "(" '{print $NF}' | cut -d ")" -f 1)

)
    fi
    lastModif=$(date -I -r $fileName)#$(date -d $(stat -c "%Y" "$fileName") +"%Y-%m-%d")
    sha16=$(sha256sum "$fileName" | cut -c -16)

    shaFileName="$DST/images/${sha16}.jpg"
    cp "$fileName" $shaFileName

    dateAlbumTitle="$DST/by-date/$lastModif/by-album/$album/by-title"
    dateTitle="$DST/by-date/$lastModif/by-title"
    albumDateTitle="$DST/by-album/$album/by-date/$lastModif/by-title"
    albumTitle="$DST/by-album/$album/by-title"
    titleDir="$DST/by-title"
    mkdir -p $dateAlbumTitle 2> /dev/null
    mkdir -p $dateTitle 2> /dev/null
    mkdir -p $albumDateTitle 2> /dev/null
    mkdir -p $albumTitle 2> /dev/null
    mkdir -p $titleDir 2> /dev/null

    cp -s $shaFileName "${dateAlbumTitle}/${title}.jpg"
    cp -s $shaFileName "${dateTitle}/${title}.jpg"
    cp -s $shaFileName "${albumDateTitle}/${title}.jpg"
    cp -s $shaFileName "${albumTitle}/${title}.jpg"
    cp -s $shaFileName "${titleDir}/${title}.jpg"
done
```
### 40. 2020-SE-05
```bash
#! /bin/bash

[ $# -eq 3 ] || exit 1
[ -d $3 ] || exit 2

pwdFile=$1
configFile=$2
dir=$3

for cfg in $(find $dir -name "*.cfg"); do
        hasErrors=$(awk '$0 !~ /(^# .*)|({ [a-zA-Z-]+;? };)|(^\s*$)/ {print "Line",NR ":" $0}' ${cfg})
        if [ -n "${hasErrors}" ]; then
                echo "Error in $(basename ${cfg}):"
                echo -e "${hasErrors}"
        else
                userName=$(basename -s .cfg $cfg)
                isContained=$(egrep -c "${userName}:" $pwdFile)
                if [ $isContained -eq 0 ]; then
                        pass=$(pwgen 16 1)
                        info="$(basename -s .cfg $cfg) $pass"
                        echo "$info" | sed -E "s/ /:/" >> $pwdFile
                        echo "$info"
                fi
                cat $cfg >> $configFile
        fi
done
```
### 41. 2020-SE-06
```bash
#! /bin/bash
[ $# -eq 3 ] || exit 1
src=$1
key=$2
val=$3
currUser=$(id -u)
currDate=$(date)
if [ $(egrep -c "${key} *=" $src) -eq 0 ]; then
        echo "$key = $val # added at $currDate by $currUser" >> $src
else
        sed -i -E "s/( *${key} *= *.+)/# \1 # edited at ${currDate} by ${currUser}\n${key} = ${val} # added at ${currDate} by ${currUser}/" $src
fi
```
### 43. 2021-SE-02
```bash
#! /bin/bash

[ $# -gt 0 ] || exit 1
for i in $@; do
        [ -f $i ] || exit 2
done

for i in $@; do
        [ $(head -n 1 $i | egrep -c "SOA") -eq 0 ] || (echo "File ${i}'s first row doesn't contain SOA" && continue)
        fields=11
        todaysDate=$(date +"%Y%m%d")
        newDateSOA="$(date +"%Y%m%d")00"
        if [ $(head -n 1 $i | egrep -c "(" ) -eq 1 ] ;then
                serialNum=$(echo $(egrep "serial" $i | awk -F ";" '{print $1}'))
                timeOnly=$(echo $serialNum | cut -c -8)
                changesOnly=$(echo $serialNum | cut -c 9-)
                if [[ "$todaysDate" > "$timeOnly" ]]; then
                        sed -i -E "s/$serialNum/$newDateSOA/g" $i
                elif [[ "$todaysDate" == "$timeOnly" ]]; then
                        newChanges=$(( $changesOnly + 1 ))
                        [ $newChanges -gt 99 ] && echo "Cannot exceed 99 changes per day!" && continue
                        if [ $newChanges -lt 10 ]; then
                                newDate=$(echo "${timeOnly}0${newChanges}")
                                sed -i -E "s/$serialNum/$newDate/g" $i
                        else
                                newDate=$(echo "${timeOnly}${newChanges}")
                                sed -i -E "s/$serialNum/$newDate/g" $i
                        fi
                fi
        else
                if [ $(head -n 1 $i | awk '{print NF}') -eq 10 ]; then
                        fields=10
                fi
                whereToPutDash=$(($fields - 7))
                divided=$(head -n 1 $i | sed -E "s/ /-/$whereToPutDash")
                timeStamp=$(echo $divided | awk -F "-" '{print $2}' | awk '{print $3}') # 2022101099
                timeOnly=$(echo $timeStamp | cut -c -8) # 20221010
                changesOnly=$(echo $timeStamp | cut -c 9-) # 99
                   if [[ "$todaysDate" > "$timeOnly" ]]; then #[ $(date +"%s") -gt $(date -d "$timeOnly" +"%s") ]; then
                       sed -i -E "s/$timeStamp/$newDateSOA/g" $i
                   elif [ "$todaysDate" = "$timeOnly" ]; then
                       newChanges=$(( $changesOnly + 1 ))
                       [ $newChanges -gt 99 ] && echo "Cannot exceed 99 changes per day!" && continue
                       if [ $newChanges -lt 10 ]; then
                               newDate=$(echo "${timeOnly}0${newChanges}")
                               sed -i -E "s/$timeStamp/$newDate/g" $i
                   else
                               newDate=$(echo "${timeOnly}${newChanges}")
                               sed -i -E "s/$timeStamp/$newDate/g" $i
                   fi
               fi
       fi
done
```
### 44. 2021-SE-03
```bash
#! /bin/bash
[ $# -eq 2 ] || (echo "insufficient args" && exit 1)
[ -f $INPUT ] || exit 2
INPUT=$1
OUTPUT=$2
numbers=$(hexdump $INPUT | cut -d " " -f 2- | head -n -1) #$(hexdump $INPUT | awk '{ s = ""; for (i = 2; i <= NF; i++) s = s $i " "; print s }')
outputTextArr="const uin16_t arr={"
count=0
for i in $numbers; do
        littleEnd=$(echo $i | sed -E "s/(..)(..)/\2\1/g" | tr '[a-z]' '[A-Z]')
        toDec=$(echo "obase=10; ibase=16; $littleEnd" | bc)
        outputTextArr="${outputTextArr} ${toDec},"
        count=$(($count + 1))
done
outputTextArr="$(echo $outputTextArr | rev | cut -c 2- | rev)};" # sed -E "s/,$/\};/"
outputSizeArr="uint32_t arrN=${count};"
echo -e "${outputTextArr}\n${outputSizeArr}" > $OUTPUT
```
### 46. 2022-CE-01
```bash
#! /bin/bash

baseCSV="base.csv"
prefixCSV="prefix.csv"

([ -f $baseCSV  ] && [ -f $prefixCSV ]) || (echo "base and prefix do not exist" && exit 1)
[ $# -eq 3 ] || (echo "insufficient args" && exit 1)
num=$1 # 2.1
metric=$2 # M
metricSymbol=$3 # s

metricDecimal=$(grep ",${metric}," $prefixCSV | awk -F "," '{print $3}') # e.g 1000000

metricRow=$(grep ",${metricSymbol}," $baseCSV) # second,s,time
metricUnitName=$(echo $metricRow | awk -F "," '{print $1}') # second
metricMeasure=$(echo $metricRow | awk -F "," '{print $3}') # time

metricResult=$(echo "$num * $metricDecimal" | bc)
echo "$metricResult $metricSymbol ($metricMeasure, $metricUnitName)"
```
### 47. 2022-CE-02
```bash
#! /bin/bash
[ $# -eq 1 ] || (echo "insufficient args" && exit 1)
DEVICE=$1
wakeUpDir="/home/students/s45811/scripts/wakeup"
sed -i -E "s/\*/-=-/g" $wakeUpDir # LID S4 *enabled ... -> LID S4 -=-enabled ...

currStatus=$(grep $DEVICE $wakeUpDir | awk '{print $3}' | cut -c 4-) # enabled/disabled
oldLine=$(grep $DEVICE $wakeUpDir)
newLine=$(echo $oldLine | sed -E "s/$currStatus/disabled/g")
sed -i -E "s/$oldLine/$newLine/g" $wakeUpDir
sed -i -E "s/-=-/\*/g" $wakeUpDir
```
### 48. 2022-IN-01
```bash
#! /bin/bash
[ $# -eq 2 ] || exit 1
([ -d $1 ] && [ -d $2 ]) || exit 2
dir1=$1
dir2=$2
for i in $(find $dir1 -type f \! -name ".*.swp"); do
        newPath=$(echo $i | sed -E "s|$dir1|$dir2|1")
        mkdir -p "$(dirname $newPath)"
        cp $i $newPath
done
```
This one is probably more accurate
```Bash
#! /bin/bash

[ $# -eq 2 ] || exit 1
([ -d $1 ] && [ -d $2 ]) || exit 2
dir1=$1
dir2=$2
for i in $(find $dir1 -type f); do
        baseNameFile=$(basename $i | sed -E "s/\.(.*)\.swp/\1/g")
        if [ $(echo $i | grep -c "\.*\.swp") -eq 0 ]; then
                if [ $(find $dir1 -name "$baseNameFile" | wc -l) -gt 0 ];then
                        continue
                fi
        fi
        newPath=$(echo $i | sed -E "s|$dir1|$dir2|1")
        mkdir -p "$(dirname $newPath)"
        cp $i $newPath
done
```

### 50. 2022-IN-04
```bash
#! /bin/bash
[ $# -ge 3 ] || exit 1
[ $(echo $@|grep -o "-jar" |wc -l) -eq 1 ] || exit 2
[ $1 == "java" ] || exit 3
jarLocation=$(echo $@|awk '{for(i=1;i<=NF;i++){ if($i=="-jar") print i} }')
allD=$(echo $@|awk '{output=""; for(i=1;i<=NF;i++){ if($i~ /^-D.+=.+/) output=output" "$i} print output}')
restDash=$(echo $@|awk '{ output=""; for(i=1;i<=NF;i++){ if($i!~/^-jar/ && $i!~/^-D.+/ && $i~/^-.+/) output=output" "$i} print output}')
path=$(echo $@|awk '{output=""; for(i=1;i<=NF;i++){if($i~/^\.jar$/) output=$i } print output}')
[ -f $path ] || exit 4
echo "java ${allD} -jar ${path} ${restDash}"
```

### 51. 2022-IN-04
```bash
#! /bin/bash
[ $# -eq 1 ] || exit 1
fugaDir=$1
validationScript="${fugaDir}/validate.sh"
fugaCfg="${fugaDir}/cfg"
authFile="${fugaDir}/foo.pwd" # user_name:hashed_password
configFile="${fugaDir}/foo.conf"
tmpFile=$(mktemp)
for i in $(find $fugaCfg -type f -name "*.cfg"); do
        userName=$(basename -s .cfg $i)
        validOutput=$($validationScript $i)
        lastStatus=$?
        if [ $lastStatus -eq 1 ];then
                echo -e "$validOutput" | awk -v currUsr=$i '{print currUsr ":" $0}' >&2
        elif [ $lastStatus -eq 0 ]
                if [ $(egrep -c $userName $authFile) -eq 0 ];then
                        newPass=$(pwgen 16 1)
                        hashedPass=$(mkpasswd $newPass)
                        echo "${userName}:${newPass}"
                        echo "${userName}:${hashedPass}" >> $authFile
                fi
                cat $i >> $tmpFile
        fi
done
cp $tmpFile ${fugaDir}/foo.conf
rm $tmpFile
```

### 52. 2022-SE-01
```bash
#! /bin/bash
[ $# -eq 1 ] || (echo "insufficient args" && exit 1)
fileLoc=$1
wakeUpDir="/home/students/s45811/scripts/wakeup"
sed -i -E "s/\*/-=-/g" $wakeUpDir # LID S4 *enabled ... -> LID S4 -=-enabled ...

while read line; do
        currLine=$(echo $line | sed -E "s/#.*//g")
        if [ -z "$currLine" ]; then
	        continue
		fi      
        currDevice=$(echo $currLine | awk '{print $1}')
        currStatus=$(echo $currLine | awk '{print $2}')
        
        if [ $(egrep -c $currDevice $wakeUpDir ) -eq 0 ];then
                echo "Device $currDevice doesn't exist in wakeup file." >&2
        else
                oldLine=$(grep $currDevice $wakeUpDir)
                oldStatus=$(echo $oldLine | awk '{print $3}' | cut -c 4-)
                newLine=$(echo $oldLine | sed -E "s/$oldStatus/$currStatus/g")
                sed -i -E "s/$oldLine/$newLine/g" $wakeUpDir
        fi  
done < <(cat $fileLoc)
sed -i -E "s/-=-/\*/g" $wakeUpDir
```

### 53. 2022-SE-02
```bash
#! /bin/bash

[ $# -eq 2 ] || exit 1
[ -d $1 ] || exit 2
[[ $2 =~ ^[0-9]{1,2} ]] || exit 3

DIR=$1
PERCENTAGE=$2

find -L ${DIR} -type l | xargs rm

for i in $(seq 0 3); do
	cnt=$(( $i + 1 ))
	for obj in $(find $DIR/$i -type f,l -name "*.tar.xz" | cut -d "." -f 1 | awk -F "-" '{print $1"-"$2}' | sort | uniq );do
		while [ $(find $DIR/$i -type f -name "${obj}*.tar.xz" | wc -l ) -gt $cnt ];do
			if [ $PERCENTAGE -ge $(df $DIR | tail -n 1 | awk '{print $5}' | tr -d '%') ]; then 
				exit 0;
			fi
			find $DIR/$i -type f -name "${obj}*.tar.xz" | sort -t "-" -k 3,3 | head -n 1 | xargs rm
		done
	done 
done

find -L ${DIR} -type l | xargs rm
```

# 2-1. Вход/Изход задачи - C (тема 6)

###  Template
```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
int main (const int argc,const char * argv[]) 
{
}
```
### 54. 2016-SE-01 Idea - Counting sort [0..255]
```C
#include <fcntl.h> 
#include <stdio.h>
#include <err.h>
#include <unistd.h>
#include <stdint.h>
int main(int argc, char* argv[])
{
        if (argc != 3)
        {
                errx(1, "Error: not enough args");
        }
        int fd1,fd2;
	    fd1 = open(argv[1], O_RDONLY);
        if (fd1 == -1)
        {
                err(2,"Couldnt open %s for reading",argv[1]);
        }

        ssize_t count_sort[UINT8_MAX+1]={0,};
        ssize_t read_size;
        uint8_t b;
        while((read_size=read(fd1,&b,1)) > 0)
        {
                count_sort[b]++;
        }

        if(read_size == -1)
        {
                err(5,"err");
        }

        fd2 = creat(argv[2],S_IRUSR | S_IWUSR);
        if (fd2 == -1)
        {
                err(3,"Couldnt open %s for reading/writing", argv[2]);
        }

        for(uint8_t i = 0; i < UINT8_MAX+1; i++)
        {
                for (ssize_t j = 0; j < count_sort[i];j++)
                {
                        if(write(fd2,&i,1) != 1)
                        {
                                err(4, "There was error reading %c", i);
                        }
                }
        }
        return 0;
}
```

### 55. 2016-SE-02 
```C
#include<stdint.h>
#include <unistd.h>
#include <fcntl.h>
#include <err.h>
#include <stdio.h>

struct interval_m
{
        uint32_t begin;
        uint32_t length;
};

int main (const int argc,const char* argv[])
{
        if (argc != 3)
        {
                errx(2,"args not enough");
        }
        int fd1= open(argv[1], O_RDONLY);
        if (fd1 == -1)
        {
                err(1,"err opening fd1");
        }

        int fd2 = open(argv[2], O_RDONLY);
        if (fd2 == -1)
        {
                err(4,"couldnt open file2 for reading");
        }

        int fd3 = creat("result",S_IRUSR | S_IWUSR);

        uint32_t buf;
        ssize_t read_size,read_size2;
        struct interval_m interval;
        while ((read_size=read(fd1, &interval,sizeof(interval)))== sizeof(interval))
        {
                if(lseek(fd2,interval.begin, SEEK_SET) == -1)
                {
                        err(3,"There was error setting pointer in fd2");
                }

                for(size_t i=0;i<interval.length;i++)
                {
                        read_size2=read(fd2,&buf,sizeof(buf));

                        if(read_size2 == -1)
                        {
                                err(5,"Couldnt read from file2");
                        }
                        else if (read_size2 != sizeof(buf))
                        {
                                err(6,"there was difference in the read bytes and the expected read bytes");
                        }

                        if(write(fd3, &buf, sizeof(buf)) != sizeof(buf))
                        {
                                err(7, "couldnt write to result file");
                        }
                }
        }

        if(read_size == -1) { err (8, "reading from file1 has failed");}
        else if (read_size > 0) { err(9,"file1 reading error");}

        return 0;
}
```
### 56. 2016-SE-03 Idea - Get  the first 50 000 000 binary numbers -> sort them -> write them to a tmp_1 file. Then get the other 50 000 000 -> sort them -> do merge sort on the tmp_1 file and the currently sorted array
```c
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <err.h>
#include <sys/stat.h>
#include <stdint.h>
#include <string.h>
int compar(const void* a, const void* b);
int createTemp(void);
int compar(const void* a, const void* b) {
    const uint32_t* ca = (const uint32_t*) a;
    const uint32_t* cb = (const  uint32_t*) b;
    return *ca - *cb;
}
int createTemp(void) {
    int fd;
    char t[12];
    strncpy(t, "peshoXXXXXX", 12);
    if ((fd = mkstemp(t)) == -1) {
        err(8, "Temp error");
    }
    return fd;
}
int main(const int argc, char* argv[])
{
    int fd;
    if (argc != 2){
        errx(1, "Wrong number of arguments");
    }
    if ((fd = open(argv[1], O_RDONLY)) == -1) {
        err(2, "Couldn't open %s\n", argv[1]);
    }
    struct stat s;
    if (fstat(fd, &s) == -1) {
        err(3, "Stat error");
    }
    if (s.st_size % 4 != 0) {
        errx(4, "Not correct size of file %s\n", argv[1]);
    }
    int size = s.st_size / 4;
    if (size > 100000000){
        errx(5, "Too large file %s\n", argv[1]);
    }
    int half = size/2;
    int rest = size - half;
    uint32_t* arr = malloc(half*sizeof(uint32_t));
    if (arr == NULL) {
    	err(6, "Not enough memory");
    }
    for (int i=0; i<half; i++) {
        if (read(fd, &arr[i], sizeof(uint32_t)) < 0) {
            err(7, "Read error");
        }
    }
    qsort(arr, half, sizeof(uint32_t), compar);
    int fdt1 = createTemp();
    for (int i=0; i<half; i++){
        if (write(fdt1, &arr[i], sizeof(uint32_t)) < 0){
            err(9, "Write error");
        }
    }
    lseek(fdt1, 0, SEEK_SET);
    free(arr);
    arr = malloc(rest*sizeof(uint32_t));
    if(arr == NULL) {
        err(10, "Not enough memory");
    }
    for (int i = 0; i < rest; i++){
        if (read(fd, &arr[i], sizeof(uint32_t)) < 0){
            err(11, "Read error");
        }
    }
    int fdt2 = createTemp();
    qsort(arr, rest, sizeof(uint32_t), compar);
    for(int i=0; i<rest; i++){
        if (write(fdt2, &arr[i], sizeof(uint32_t)) < 0) {
            err(12, "Write error");
        }
    }
    free(arr);
    lseek(fdt2, 0, SEEK_SET);
    int out;
    if((out = open("out", O_WRONLY | O_TRUNC | O_CREAT, S_IRUSR | S_IWUSR)) == -1) {
        err(13, "Open error");
    }
    uint32_t left;
    uint32_t right;
    int h = 0;
    int r = 0;
    if(read(fdt1, &left, sizeof(uint32_t)) == sizeof(uint32_t)){
        h++;
    }
    if (read(fdt2, &right, sizeof(uint32_t)) == sizeof(uint32_t)){
         r++;
    }
    while (h <= half && r <= rest){
        if (left < right){
            write(out, &left, sizeof(uint32_t));
            read(fdt1, &left, sizeof(uint32_t));
            h++;
        }
        else{
            write(out, &right, sizeof(uint32_t));
            read(fdt2, &right, sizeof(uint32_t));
            r++;
        }
    }
    while (h <= half){
        write(out, &left, sizeof(left));
        read(fdt1, &left, sizeof(left));
        h++;
    }
    while (r <= rest) {
        write(out, &right, sizeof(right));
        read(fdt2, &right, sizeof(right));
        r++;
    }
    close(fd);
    close(fdt1);
    close(fdt2);
    close(out);
    exit(0);
}
```
### 57. 2017-IN-01 
Идея: четем от .idx файла който са ни дали, като четем sizeof(DAT) на брой байтове. За всяка такава структура бъркаме във първият файл на offset DAT.offset и в динамично заделен масив (buffer) с големина DAT.length четем толкова на брой байтове. Проверяваме дали buffer[0] е главна и ако е главна записваме съответната дума във файл с име <трети подаден аргумент>, а DAT структурата я записваме в файл с име <четвърти подаден аргумент>.
```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>

struct DAT
{
        uint16_t offset;
        uint8_t  length;
        uint8_t  saved;
};

int main (const int argc,const char * argv[])
{
    if(argc != 5)
    {
            errx(1,"not enough args");
    }
    int fd1,fd2,fd3,fd4;
    fd1=open(argv[1], O_RDONLY);
    fd2=open(argv[2], O_RDONLY);
    fd3=creat(argv[3], S_IRUSR | S_IWUSR);
    fd4=creat(argv[4], S_IRUSR | S_IWUSR);
    if(fd1 == -1 || fd2 == -1 || fd3==-1 || fd4==-1)
    {
            err(2,"error opening/creating one of the files");
    }
	struct stat s,f1;
	if(fstat(fd1,&f1)==-1)
    {
	    err(12,"Stat error on fd2");
    }

    if(fstat(fd2,&s)==-1)
    {
	    err(12,"Stat error on fd2");
    }
	if(s.st_size % sizeof(struct DAT) != 0)
	{
		err(3,"Inconsistent files - f2");
	}
    int readsize,readsize2;
    struct DAT dat_str;
    struct DAT currDat;
    currDat.offset = 0;
    currDat.length = 0;
    currDat.saved  = 0;

    while((readsize=read(fd2,&dat_str, sizeof(dat_str))) > 0)
    {
		if(dat_str.offset > f1.st_size)
		{
			err(2,"error lseeking in fd1, offset exceeds fd1 size");
		}	   
        lseek(fd1,dat_str.offset,SEEK_SET);

        uint8_t c;
        uint8_t currLen=dat_str.length;
        if((readsize2 = read(fd1,&c, sizeof(uint8_t)))< 0)
        {
                err(3,"error reading first char from word");
        }
        currLen--;


        if(c < 0x41 || c > 0x5A)
        {
			continue;
	    }
     
        if(write(fd3,&c,sizeof(uint8_t)) != sizeof(uint8_t))
        {
                err(4,"error writing the upper case letter");
        }

        while(currLen > 0 && (readsize2=read(fd1,&c,sizeof(uint8_t))) > 0 )
        {
                if(write(fd3, &c, sizeof(uint8_t)) != sizeof(uint8_t))
                {
                        err(5,"Error writing to fd3 of char %u", c);
                }

                currLen--;
        }

        currDat.length = dat_str.length;
        currDat.saved  = dat_str.saved;
        if(write(fd4, &currDat, sizeof(currDat)) != sizeof(currDat))
        {
                err(6, "error writing to fd4 of Dat structure: %d %d %d", dat_str.offset, dat_str.length,dat_str.saved);
        }

        currDat.offset+=dat_str.length;      
    }

    return 0;
}
```
### 58. 2017-SE-01
```c
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>

struct PATCH_STR
{
    uint16_t offset;
    uint8_t originalByte;
    uint8_t newByte;
};

int main (const int argc,const char * argv[])
{
    if(argc != 4)
    {
         errx(1,"not enough args");
    }

    int fd1,fd2,fdpatch;
    fd1=open(argv[1], O_RDONLY);
    fd2=open(argv[2], O_RDONLY);
    fdpatch=creat(argv[3], S_IRUSR | S_IWUSR);
    if (fd1 == -1 || fd2 == -1 || fdpatch == -1)
    {
            err(2,"Error opening/creating one of the files");
    }
	struct stat s,s1;
	if(fstat(fd1,&s)==-1)
	{
		err(2,"stat err on fd1");
	}
	if(fstat(fd2,&s1)==-1)
	{
		err(2,"stat err on fd2");
	}
	if(s.st_size != s1.st_size)
	{
		err(3,"inconsistent files");
	}
	lseek(fd1,0,SEEK_SET);
	lseek(fd2,0,SEEK_SET);
    uint8_t left,right;
    int readsize,readsize2,counter=0;
    while((readsize=read(fd1,&left,sizeof(left)) >0 && (readsize2=read(fd2,&right,sizeof(right)))> 0))
    {
            if(left != right)
            {
                    struct PATCH_STR strToWrite;
                    strToWrite.offset = counter;
                    strToWrite.originalByte=left;
                    strToWrite.newByte=right;
                    if(write(fdpatch, &strToWrite, sizeof(strToWrite)) != sizeof(strToWrite))
                    {
                            err(3,"error writing the struct to %s", argv[3]);
                    }
            }

            counter++;
    }

	if(readsize2 < 0 || readsize < 0)
	{
		err(4,"Error reading from file1 or file2");
	}
    return 0;
}
```
### 59. 2017-SE-02
```C
#include <stdlib.h>
#include <err.h>
#include <unistd.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdint.h>
#include <string.h>
#include <stdio.h>

enum exit_code
{
        ARG_ERR = 1,

        READ_ERR,
        WRITE_ERR,
        OPEN_ERR,
        OTHER_ERR,
        CLOSE_ERR
};

void rdwr(const int fdrd, const int fdwr, uint8_t enumerate);

void rdwr(const int fdrd, const int fdwr, uint8_t enumerate)
{
       int n = 1;
       char buf;
       char nbuf[256];
       ssize_t rdsz;
       if (enumerate == 1)
       {
               sprintf(nbuf, "%d ", n);
               fputs(nbuf, stdout);
               fflush(stdout);
       }

       while ((rdsz = read(fdrd, &buf, 1)) > 0)
       {
               if (write(fdwr, &buf, 1) != 1)
               {
                       err(WRITE_ERR, "writing err");
               }

               // ???
               if (buf == '\n' && enumerate == 1)
               {
                       n++;
                       sprintf(nbuf, "%d ", n);
                       setbuf(stdout, nbuf);
                       fputs(nbuf, stdout);
                       fflush(stdout);
               }
       }

       if (rdsz == -1)
       {
               err(READ_ERR, "reading err");
       }
}

int main(const int argc, const char* argv[])
{
       uint8_t enumerate = 0;

       if (argc == 1)
       {
               rdwr(0, 1, enumerate);
               exit(0);
       }

       if (strcmp(argv[1], "-n") == 0)
       {
               enumerate = 1;
       }

       if (argc == 2 && enumerate == 1)
       {
               rdwr(0, 1, enumerate);
               exit(0);
       }

       int fd;
       int i = (enumerate == 1) ? 2 : 1;
       while (i < argc)
       {
               if (strcmp(argv[i], "-") == 0)
               {
                       fd = 0;
               }
               else if ((fd = open(argv[i], O_RDONLY)) == -1)
               {
                       err(OPEN_ERR, "open err");
               }

               rdwr(fd, 1, enumerate);

               if (fd != 0 && close(fd) == -1)
               {
                       err(CLOSE_ERR, "close err");
               }

               i++;
       }

       exit(0);
}
```
Much better
```c
#include <unistd.h>
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>

#define BUFSIZE 4096

void cat_stdin(int n) {
  char buf[BUFSIZE];
  int line = n, len;
  while ((len = read(0, buf, BUFSIZE)) > 0) {
    for (int i = 0; i < len; i++) {
      if (buf[i] == '\n') {
        if (n) {
          char num[12];
          len = snprintf(num, 12, "%d ", line++);
          write(1, num, len);
        }
      }
      write(1, &buf[i], 1);
    }
  }
}

void cat_file(const char *filename, int n) {
  int fd = strcmp(filename, "-") == 0 ? STDIN_FILENO : open(filename, O_RDONLY);
  if (fd < 0) {
    perror(filename);
    return;
  }

  char buf[BUFSIZE];
  int line = n, len;
  while ((len = read(fd, buf, BUFSIZE)) > 0) {
    for (int i = 0; i < len; i++) {
      if (buf[i] == '\n') {
        if (n) {
          char num[12];
          len = snprintf(num, 12, "%d ", line++);
          write(1, num, len);
        }
      }
      write(1, &buf[i], 1);
    }
  }

  close(fd);
}

int main(int argc, char *argv[]) {
  int n = 0;

  if (argc > 1 && strcmp(argv[1], "-n") == 0) {
    n = 1;
    argc--;
    argv++;
  }

  if (argc == 1) {
    cat_stdin(n);
  } else {
    for (int i = 1; i < argc; i++) {
      cat_file(argv[i], n);
    }
  }

  return 0;
}

```
fixed up solution
```c
#include <unistd.h>
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define BUFSIZE 4096

int line=1;

void cat_stdin(int n);
void cat_stdin(int n) {
  char buf[BUFSIZE];
  int len;
  //setbuf(stdout,buf);
  while ((len = read(0, &buf, BUFSIZE)) > 0) {
    for (int i = 0; i < len; i++) {
      if(line==1)
      {
            if(n)
            {
	            char num[12];
		        int newLen = snprintf(num, 12, "%d ", line++);
		        write(1, &num, newLen);
            }
      }
      write(1, &buf[i], 1);
      if (buf[i] == '\n') {
        if (n) {
          char num[12];
          int newLen = snprintf(num, 12, "%d ", line++);
          write(1, &num, newLen);
        }
      }
    }
  }
  //fflush(stdout);
}

void cat_file(const char *filename, int n);
void cat_file(const char *filename, int n) {
  int fd = strcmp(filename, "-") == 0 ? 0 : open(filename, O_RDONLY);
  if (fd < 0) {
    perror(filename);
    return;
  }

  char buf[BUFSIZE];
  //setbuf(stdout,buf);
  int len;
  while ((len = read(fd, &buf, BUFSIZE)) > 0) {
    for (int i = 0; i < len; i++) {
          if(line==1)
          {
                if(n)
                {
                  char num[12];
          int newLen = snprintf(num, 12, "%d ", line++);
          write(1, &num, newLen);
                }
          }
      write(1, &buf[i], 1);
      if (buf[i] == '\n') {
        if (n) {
          char num[12];
          int newLen = snprintf(num, 12, "%d ", line++);
          write(1, &num, newLen);
        }
      }
    }
  }
  //fflush(stdout);
  close(fd);
}

int main(int argc, char *argv[]) {
  int n = 0;

  if (argc > 1 && strcmp(argv[1], "-n") == 0) {
    n = 1;
    argc--;
    argv++;
  }

  if (argc == 1) {
    cat_stdin(n);
  } else {
    for (int i = 1; i < argc; i++) {
      cat_file(argv[i], n);
    }
  }

  return 0;
}

```
### 60. 2017-SE-03
```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>

struct PATCH_STR
{
    uint16_t offset;
    uint8_t originalByte;
    uint8_t newByte;
};

int main (const int argc,const char * argv[])
{
    if(argc != 4)
    {
         errx(1,"not enough args");
    }

    int fd1,fd2,fdpatch;
    fd1=open(argv[2], O_RDONLY);
    fd2=creat(argv[3], S_IRUSR | S_IWUSR);
    fdpatch=open(argv[1], O_RDONLY);
    if (fd1 == -1 || fd2 == -1 || fdpatch == -1)
    {
            err(2,"Error opening/creating one of the files");
    }
    struct stat s;
    if(fstat(fdpatch, &s)==-1)
    {
            err(1,"Stat err on patch file");
    }
    if(s.st_size % sizeof(struct PATCH_STR) != 0)
    {
            err(2,"Incosistent file %s", argv[1]);
    }
    uint8_t f1Byte;
    int readsize,readsize2;
    uint16_t counter=0;
    struct PATCH_STR patchBin;
    while((readsize=read(fdpatch, &patchBin, sizeof(patchBin))) > sizeof(patchBin))
    {
        while((readsize2=read(fd1,&f1Byte,sizeof(f1Byte))) > 0)
        {
                if(counter == patchBin.offset)
                {
                        if(write(fd2,&patchBin.newByte, sizeof(patchBin.newByte)) != sizeof(patchBin.newByte))
                        {
                                err(3,"Error writing new byte to f2.bin");
                        }
                        counter++;
                        break;
                }
                else
                {
                        if(write(fd2,&f1Byte,sizeof(f1Byte)) != sizeof(f1Byte))
                        {
                                err(4,"Error writing old byte to f2.bin");
                        }
                        counter++;
                }
        }
    }

    if(readsize < 0 || readsize2 < 0)
    {
            err(5,"error with readsize or readsize2");
    }

    if(readsize2 > 0)
    {
            while((readsize2 = read(fd1,&f1Byte,sizeof(f1Byte))) == sizeof(f1Byte))
            {
                    if(write(fd2,&f1Byte,sizeof(f1Byte)) != sizeof(f1Byte))
                    {
                            err(4,"error writing old byte to f2.bin");
                    }
            }
    }

    return 0;
}

```
### 61 : same as 59 but without "-n"

### 62. 2018-SE-01
```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
enum OPTIONS
{
        NO_OP=0,
        S_OP=1,
        D_OP=2,
};
int isInArr(const char* word, uint8_t ch);
int isInArr(const char* word, uint8_t ch)
{
     int wordLen = strlen(word);
     for(int i = 0; i < wordLen; i++)
     {
             if(word[i] == ch)
             {
                     return 1;
             }
     }

    return 0;
}
int main (const int argc,const char * argv[])
{
    enum OPTIONS opt = NO_OP;
    if(argc < 2)
    {
            errx(1,"not enough args");
    }
	
    if(!strcmp(argv[1], "-d") && argc == 3)
    {
            opt = D_OP;
    }
    else if(!strcmp(argv[1], "-s") && argc == 3)
    {
            opt = S_OP;
    }
    else if(strcmp(argv[1],"-s") != 0 && strcmp(argv[1], "-d") != 0 && argc == 3)
    {
            opt = NO_OP;
    }
    else
    {
            err(2,"Arguments mismatch");
    }

    if(opt==NO_OP)
    {
            if(strlen(argv[1]) != strlen(argv[2]))
            {
                    errx(3,"Lengths of arrays mismatch");
            }

            int rdsz;
            uint8_t b;
            size_t arr1Size = strlen(argv[1]);
            while((rdsz=read(0,&b,sizeof(b))) > 0)
            {
                    uint8_t flag=0;
                    for(ssize_t i = arr1Size-1; i >= 0; i--)
                    {
                            if(argv[1][i] == b)
                            {
                                    if(write(1, &argv[2][i], sizeof(uint8_t)) != sizeof(uint8_t))
                                    {
                                            err(4,"Error writing to stdout of char %u", argv[2][i]);
                                    }
                                    flag = 1;
                                    break;
                            }
                    }

                    if(!flag)
                    {
                            if(write(1,&b,sizeof(b)) != sizeof(b))
                            {
                                    err(5,"Error writing to stdout of same char %u", b);
                            }
                    }
            }

            if(rdsz < 0)
            {
                    err(4,"Error reading with NO_OPT");
            }
    }
    else if(opt == S_OP)
    {
            uint8_t prev,curr;
            int rdsz,rdsz2;
            if((rdsz=read(0,&prev,sizeof(prev))) < 0)
            {
                    err(6,"Error reading prev the first time");
            }

            if(write(1,&prev,sizeof(prev)) != sizeof(prev))
            {
                    err(7,"error writing the first prev");
            }

            while((rdsz2 = read(0,&curr,sizeof(curr))) > 0)
            {
                    if(curr == prev && isInArr(argv[2],curr))
                    {
                            continue;
                    }
                    else
                    {
                            prev = curr;
                            if(write(1,&curr,sizeof(curr)) != sizeof(curr))
                            {
                                    err(8,"error writing inner curr to the stdout");
                            }
                    }
            }
    }
    else if(opt == D_OP)
    {
            uint8_t curr;
            int rdsz;
            while((rdsz=read(0,&curr,sizeof(curr))) > 0)
            {
                    if(isInArr(argv[2],curr))
                    {
                            continue;
                    }

                    if(write(1,&curr,sizeof(curr)) != sizeof(curr))
                    {
                            err(9,"Error writing curr to stdout in -d mode");
                    }
            }
    }
    else
    {
            errx(10,"Unrecocgnized command!");
    }

    return 0;
}
```
### 63. 2018-SE-02
```c
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>

static int compar(const void* left, const void* right);
static int compar(const void* left, const void* right)
{
        return *(const uint32_t *)left - *(const uint32_t *)right;
}
int createTemp(void);
int createTemp(void)
{
        char t[12];
        strncpy(t,"tmp63XXXXXX",12);
        int fd=mkstemp(t);
        if(fd==-1)
                err(2,"Error creating temp %s", t);
        return fd;
}
int main (const int argc,const char * argv[])
{
        if(argc != 3)
        {
                errx(1,"Arguments mismatch");
        }
        int fd0,fd1;
        fd0=open(argv[1], O_RDONLY);
        fd1=open(argv[2], O_WRONLY | O_TRUNC | O_CREAT, S_IRUSR | S_IWUSR);
        if(fd0 == -1 || fd1 == -1)
        {
                err(2,"Error opening/creating %s or %s", argv[1],argv[2]);
        }
        struct stat s;
        if(fstat(fd0, &s) == -1)
        {
                err(3,"Error statting %s", argv[1]);
        }
        if(s.st_size % sizeof(uint32_t) != 0)
        {
                err(4,"Inconsistent file %s", argv[1]);
        }
        lseek(fd0, 0, SEEK_SET);
        int size = s.st_size/sizeof(uint32_t);
        int half = size/2;
        int rest = size - half;
        ssize_t rdsz,rdsz2;
        uint32_t * buffer = malloc(half*sizeof(uint32_t));
        if(!buffer)
                err(4,"error allocating memory for first half");

        // Read, sort then write first half
        for(int i = 0; i < half; i++)
                if((rdsz=read(fd0,&buffer[i],sizeof(buffer[i]))) < 0)
                        err(5,"error reading in buffer[%d]", i);

        qsort(buffer, half, sizeof(uint32_t), compar);
        int fdtmp=createTemp();

        for(int i = 0; i < half; i++)
                if(write(fdtmp,&buffer[i],sizeof(buffer[i]))!=sizeof(buffer[i]))
                        err(6,"error writing buffer[%d]", i);

        free(buffer);
        buffer=malloc(rest*sizeof(uint32_t));
        if(!buffer)
                err(7,"error allocating memory for second half");
        for(int i = 0; i < rest; i++)
                if((rdsz=read(fd0,&buffer[i],sizeof(uint32_t)))< 0)
                        err(8,"error reading in buffer[%d]", i);

        qsort(buffer, rest, sizeof(uint32_t), compar);
        int fdtmp2=createTemp();
        for(int i = 0; i < rest; i++)
                if(write(fdtmp2,&buffer[i],sizeof(buffer[i])) != sizeof(buffer[i]))
                        err(9,"error writing buffer[%d]", i);
        free(buffer);
        lseek(fdtmp,0,SEEK_SET);
        lseek(fdtmp2,0,SEEK_SET);

        uint32_t left,right;
        int h=0,r=0;
        if((rdsz=read(fdtmp,&left,sizeof(left))) < 0 || (rdsz2=read(fdtmp2,&right,sizeof(right))) < 0)
        {
                err(10,"err reading first number from fdtmp or fdtmp2");
        }
        h++;
        r++;

        while(h < half && r < rest)
        {
                if(left < right)
                {
                        if(write(fd1,&left, sizeof(left))!=sizeof(left))
                                err(11,"err wriitng %d which is less than %d", left,right);
                        h++;
                        if(h< half && (rdsz=read(fdtmp,&left,sizeof(left)))< 0)
                                err(12,"err reading from fdtmp of left");
                }
                else
                {
                        if(write(fd1,&right, sizeof(right))!=sizeof(right))
                                err(11,"err wriitng %d which is less than %d",right,left);
                        r++;
                        if(r < rest && (rdsz2=read(fdtmp2,&right,sizeof(right)))<0)
                                err(12,"err reading from fdtmp2 of right");
                }
        }

        while(h < half)
        {
                if(write(fd1,&left, sizeof(left))!=sizeof(left))
                        err(11,"err wriitng %d", left);
                h++;
                if(h<half && (rdsz=read(fdtmp,&left,sizeof(left)))<0)
                        err(12,"err reading next left with half=%d",h);
        }
        while(r < rest)
        {
                if(write(fd1,&right, sizeof(right))!=sizeof(right))
                        err(11,"err wriitng %d", right);
                r++;
                if(r < rest && (rdsz2=read(fdtmp2,&right,sizeof(right)))<0)
                        err(12,"err reading next right with rest=%d",r);
        }

        return 0;
}
```
no for cycles
```c
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>

static int compar(const void* left, const void* right);
static int compar(const void* left, const void* right)
{
        return *(const uint32_t *)left - *(const uint32_t *)right;
}

int createTemp(void);
int createTemp(void)
{
        char t[12];
        strncpy(t,"tmp63XXXXXX",12);
        int fd=mkstemp(t);
        if(fd==-1)
                err(2,"Error creating temp %s", t);
        return fd;
}
int main (const int argc,const char * argv[])
{
        if(argc != 3)
        {
                errx(1,"Arguments mismatch");
        }
        int fd0,fd1;
        fd0=open(argv[1], O_RDONLY);
        fd1=open(argv[2], O_WRONLY | O_TRUNC | O_CREAT, S_IRUSR | S_IWUSR);
        if(fd0 == -1 || fd1 == -1)
        {
                err(2,"Error opening/creating %s or %s", argv[1],argv[2]);
        }
        struct stat s;
        if(fstat(fd0, &s) == -1)
        {
                err(3,"Error statting %s", argv[1]);
        }
        if(s.st_size % sizeof(uint32_t) != 0)
        {
                err(4,"Inconsistent file %s", argv[1]);
        }
        lseek(fd0, 0, SEEK_SET);
        int size = s.st_size/sizeof(uint32_t);
        int half = size/2;
        int rest = size - half;
        ssize_t rdsz,rdsz2;
        uint32_t * buffer = malloc(half*sizeof(uint32_t));
        if(!buffer)
                err(4,"error allocating memory for first half");

        // Read, sort then write first half
        if((rdsz=read(fd0,buffer,half * sizeof(uint32_t))) < 0)
                err(5,"error reading in buffer[]");

        qsort(buffer, half, sizeof(uint32_t), compar);
        int fdtmp=createTemp();

        if(write(fdtmp,buffer,half*sizeof(uint32_t)) < 0)
                err(6,"error writing buffer[]");

        free(buffer);

        buffer=malloc(rest*sizeof(uint32_t));
        if(!buffer)
                err(7,"error allocating memory for second half");
        if((rdsz=read(fd0,buffer,rest*sizeof(uint32_t)))< 0)
                err(8,"error reading in buffer[]");

        qsort(buffer, rest, sizeof(uint32_t), compar);
        int fdtmp2=createTemp();
        if(write(fdtmp2,buffer,rest*sizeof(uint32_t)) < 0)
                err(9,"error writing buffer[]");
        free(buffer);
        lseek(fdtmp,0,SEEK_SET);
        lseek(fdtmp2,0,SEEK_SET);

        uint32_t left,right;
        int h=0,r=0;
        if((rdsz=read(fdtmp,&left,sizeof(left))) < 0 || (rdsz2=read(fdtmp2,&right,sizeof(right))) < 0)
        {
                err(10,"err reading first number from fdtmp or fdtmp2");
        }
        h++;
        r++;

        while(h < half && r < rest)
        {
                if(left < right)
                {
                        if(write(fd1,&left, sizeof(left))!=sizeof(left))
                                err(11,"err wriitng %d which is less than %d", left,right);
                        h++;
                        if(h< half && (rdsz=read(fdtmp,&left,sizeof(left)))< 0)
                                err(12,"err reading from fdtmp of left");
                }
                else
                {
                        if(write(fd1,&right, sizeof(right))!=sizeof(right))
                                err(11,"err wriitng %d which is less than %d",right,left);
                        r++;
                        if(r < rest && (rdsz2=read(fdtmp2,&right,sizeof(right)))<0)
                                err(12,"err reading from fdtmp2 of right");
                }
        }

        while(h < half)
        {
                if(write(fd1,&left, sizeof(left))!=sizeof(left))
                        err(11,"err wriitng %d", left);
                h++;
                if(h<half && (rdsz=read(fdtmp,&left,sizeof(left)))<0)
                        err(12,"err reading next left with half=%d",h);
        }
        while(r < rest)
        {
                if(write(fd1,&right, sizeof(right))!=sizeof(right))
                        err(11,"err wriitng %d", right);
                r++;
                if(r < rest && (rdsz2=read(fdtmp2,&right,sizeof(right)))<0)
                        err(12,"err reading next right with rest=%d",r);
        }

        return 0;
}

```
### 64. 2018-SE-03
```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
enum OPTIONS
{
        NO_OP=0,
        C_OP_range=1,
        C_OP_only_one=2,
        D_OP_range=3,
        D_OP_only_one=4,
};
int main (const int argc,const char * argv[])
{
        enum OPTIONS opt = NO_OP;
        if(argc < 3)
        {
                errx(1,"Not enough args for cut");
        }
        // -c
        else if(argc == 3 && !strcmp(argv[1],"-c"))
        {
                // -c 5
                if(strlen(argv[2])== 1)
                {
                        if(argv[2][0] < 0x30 || argv[2][0] > 0x39)
                        {
                                errx(4,"argument given to -c isnt a number");
                        }
                        opt = C_OP_only_one;
                }
                // -c 3-5
                else if(strlen(argv[2]) == 3)
                {
                        if(argv[2][0] > 0x30 && argv[2][0] < 0x39 && argv[2][1] == 0x2D && argv[2][2] > 0x30 && argv[2][2] < 0x39)
                        {
                                opt = C_OP_range;
                        }
                        else
                        {
                                errx(2,"Check if your -c arg is appropriate. (e.g 3-5)");
                        }
                }
                else
                {
                        errx(3,"Check your -c arg input (number of args)");
                }
        }
        // -d
        else if(argc == 5 && !strcmp(argv[1],"-d"))
        {
                if(strcmp(argv[3],"-f"))
                {
                        errx(4,"Incorrect cut args. (-f)");
                }

               // -d ".." -f 1
               if(strlen(argv[4])== 1)
               {
		               if(argv[4][0]  <  0x30  || argv[4][0]  >  0x39)  {  errx(4,"argument given to -c isnt a number");  }
                       opt = D_OP_only_one;
               }
               // -d ".." -f 3-5
               else if(strlen(argv[4]) == 3)
               {
                       if(argv[4][0] > 0x30 && argv[4][0] < 0x39 && argv[4][1] == 0x2D && argv[4][2] > 0x30 && argv[4][2] < 0x39)
                       {
                               if(argv[4][0] == argv[4][2])
                               {
                                       opt=D_OP_only_one;
                               }
                               else
                               {
                                       opt = D_OP_range;
                               }
                       }
                       else
                       {
                               errx(2,"Check if your field arg is appropriate. (e.g 3-5)");
                       }
               }
               else
               {
                       errx(3,"Check your field input (number of args)");
               }
       }
       uint8_t c;
       int currLen,rdsz,left,right;
       int currDel=1;
       switch(opt)
       {
             case C_OP_only_one: // -c 5
                     //while((rdsz=read(0,&c,sizeof(c))) > 0)
                     //{
                     currLen=atoi(argv[2]);
                     while((rdsz=read(0,&c,sizeof(c))) > 0)
                     {
                             if(currLen == 1)
                             {
                                     if(write(1,&c,sizeof(c)) != sizeof(c))
                                     {
                                             err(5,"error writing only 1 byte in C_OP_only_one (%u)", c);
                                     }
                                     break;
                             }
                             currLen--;
                     }
                     break;
             case C_OP_range:
                     left=atoi(&argv[2][0]);
                     right=atoi(&argv[2][2]);
                     currLen=1;
                     while((rdsz=read(0,&c,sizeof(c))) > 0)
                     {
                             if(currLen >= left && currLen <= right)
                             {
                                     if(write(1,&c,sizeof(c)) != sizeof(c))
                                     {
                                             err(6,"error writing char %u of C_OP_range", c);
                                     }
                             }
                             currLen++;
                     }
                     break;
             case D_OP_only_one:
                     currLen=atoi(&argv[4][0]);
                     while((rdsz=read(0,&c,sizeof(c))) > 0 && currDel <= currLen)
                     {
                             if(c == argv[2][0])
                             {
                                     currDel++;
                                     continue;
                             }
                             if(currDel == currLen)
                             {
                                     if(write(1,&c,sizeof(c)) != sizeof(c))
                                     {
                                             err(7,"ERror writing single char to stdout in C_OP_only_one");
                                     }
                             }
                     }
                     break;
             case D_OP_range:
                     left=atoi(&argv[4][0]);
                     right=atoi(&argv[4][2]);
                     currLen=1;
                     while((rdsz=read(0,&c,sizeof(c))) > 0)
                     {
                             if(c==argv[2][0])
                             {
                                     currDel++;
                                     if(currDel == left)
                                     {
                                             continue;
                                     }
                             }
                             if(currDel >= left && currDel <=right)
                             {
                                     if(write(1,&c,sizeof(c)) != sizeof(c))
                                     {
                                             err(8,"Error writing char for D_OP_range");
                                     }
                             }
                     }
                     break;
             default:
                     errx(10,"No options");
     }
     printf("\n");
     return 0;
}
```
### 65. 2018-SE-04
```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
int mycmp(const void *left,const void* right);
int mycmp(const void *left, const void* right)
{
        return (*(const uint16_t *)left - *(const uint16_t *)right);
}
int main (const int argc,const char * argv[])
{
        if(argc != 3)
        {
                errx(1,"not enough args");
        }
        int fd1,fd2;
        fd1=open(argv[1], O_RDONLY);
        fd2=creat(argv[2],S_IRUSR | S_IWUSR);
        if(fd1 == -1 || fd2== -1)
        {
                err(2, "error opening one of the files");
        }
        ssize_t rdsz;
        ssize_t fileSize;
        if((fileSize=lseek(fd1,0,SEEK_END)) < 0)
        {
                err(5,"Error seeking the end of the file");
        }
        if(lseek(fd1,0,SEEK_SET) < 0)
        {
                err(6,"Error seeking back to the beginning");
        }
        uint16_t *buffer=malloc(fileSize);
        for (int index = 0; index < fileSize; ++index)
        {
		        if((rdsz=read(fd1, &buffer[index], 1)) < 0)
                {
                        err(7,"error reading char by char");
                }
        }
        qsort(buffer,fileSize,sizeof(uint16_t),mycmp);
        for (int index = 0; index < fileSize; ++index)
        {
                if(write(fd2, &buffer[index], 1) != 1)
                {
                        err(8,"error writing char by char at %d", index);
                }
        }
        return 0;
}
```
### 66. 2019-SE-01
```c
#include <math.h>
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
struct user_interval
{
        uint32_t uid;
        uint16_t useless1;
        uint16_t useless2;
        uint32_t start;
        uint32_t finish;
};
struct interval
{
        uint32_t uid;
        uint32_t length;
};
int member(uint32_t usrs[],size_t size, uint32_t elem);
int member(uint32_t usrs[],size_t size, uint32_t elem)
{
        for(size_t i = 0; i < size; i++)
        {
                if(usrs[i]==elem)
                {
                        return 1;
                }
        }
        return 0;
}
size_t findMax(struct interval processes[],size_t size, uint32_t usr, size_t *ind);
size_t findMax(struct interval processes[],size_t size, uint32_t usr, size_t *ind)
{
        size_t maxInterval=0;
        for(size_t i =0; i < size; i++)
        {
                if(processes[i].uid == usr && processes[i].length > maxInterval)
                {
                        maxInterval = processes[i].length;
                        *ind=i;
                }
        }
        return maxInterval;
}
int main (const int argc,const char * argv[])
{
        // Gather all intervals and calculate mean
        // for each interval calculate the sum of (x_i - mean)^2 and divide it by their number
        // for each interval check if it exceeds disperion and print it along with the user name

        if(argc != 2)
        {
                errx(1,"Not enough args");
        }
        int fd1=open(argv[1],O_RDONLY);
        if(fd1 == -1)
        {
                err(2,"error opening %s", argv[1]);
        }
        struct user_interval process;
        struct interval processes[16384];
        ssize_t rdsz;
        size_t size=0;
        double mean=0;
        uint32_t userIds[2048];
	    size_t usrSize=0;
        while((rdsz=read(fd1,&process, sizeof(process))) > 0)
        {
            struct interval currProc={process.uid,process.finish-process.start};
            processes[size++]=currProc;
            mean+=currProc.length;
            if(!member(userIds, usrSize, process.uid))
            {
                    userIds[usrSize++]=process.uid;
            }
            else
            {
                    size_t ind;
                    size_t maxInUsers=findMax(processes,size,currProc.uid,&ind);
                    if(maxInUsers<currProc.length)
                    {
                            processes[ind].length=currProc.length;
                    }
            }
        }
        mean/=size;
        double dispersion=0;
        for(size_t i = 0; i < size; i++)
        {
                double tmp = (processes[i].length-mean)*(processes[i].length-mean);
                dispersion+=tmp;
        }
        dispersion/=size;
        for(size_t i = 0; i < usrSize; i++)
        {
                size_t ind;
                size_t maxInterval = findMax(processes, size, userIds[i],&ind);
                if(maxInterval > dispersion)
                {
                        printf("%u %lu", userIds[i], maxInterval);
                }
        }
        return 0;
}

```
### 67. 2020-IN-01
```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
struct PATCH
{
        uint32_t magic;
        uint8_t headerVersion;
        uint8_t dataVersion;
        uint16_t count;
        uint32_t reserved1;
        uint32_t reserved2;
};
struct PATCH_00
{
        uint16_t offset;
        uint8_t originalByte;
        uint8_t newByte;
};
struct PATCH_01
{
        uint32_t offset;
        uint16_t originalWord;
        uint16_t newWord;
};
int main (const int argc,const char * argv[])
{

    if(argc != 4)
    {
		errx(1,"insufficient args");
    }
    int fd1,fdpatch,fd2;
    fd1=open(argv[2], O_RDONLY);
    fdpatch= open(argv[1], O_RDONLY);
    fd2=creat(argv[3], S_IRUSR | S_IWUSR);
    if(fd1 == -1 || fdpatch == -1 || fd2 == -1)
    {
            err(2,"Error opening one of the files");
    }

    ssize_t rdsz;
    struct PATCH currPatch;
    if((rdsz=read(fdpatch,&currPatch,sizeof(currPatch))) < 0)
    {
            err(3,"error reading patch header");
    }

    if(currPatch.dataVersion == 0x00)
    {
            // struct PATCH_00
            // same as 60 but with different structures depending on dataVersion
    }
    else
    {
            // struct PATCH_01
            // same as 60 but with different structures depending on dataVersion
    }

    return 0;
}
```

### 68. 2020-SE-01
```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
struct package
{
        uint16_t post_no;
        uint16_t post_count;
        uint16_t pre_no;
        uint16_t pre_count;
        uint16_t in_no;
        uint16_t in_count;
        uint16_t suf_no;
        uint16_t suf_count;
};
int main (const int argc,const char * argv[])
{
        if(argc!=7)
        {
                errx(1,"insufficient args");
        }

       int fdaff,fdpost,fdpre,fdin,fdsuf,fdcru;
       fdaff=open(argv[1],O_RDONLY);

       fdpost=open(argv[2],O_RDONLY);
       fdpre=open(argv[3],O_RDONLY);
       fdin=open(argv[4],O_RDONLY);
       fdsuf=open(argv[5],O_RDONLY);
       fdcru=creat(argv[6],S_IRUSR | S_IWUSR);
       if(fdaff == -1 || fdpost == -1 || fdpre == -1 || fdin == -1 || fdsuf == -1 || fdcru == -1)
       {
               err(2,"Error opening/creating one of the files");
       }

       ssize_t sz;
       uint32_t affCnt=0,postCnt=0,preCnt=0,inCnt=0,sufCnt=0;
       lseek(fdaff,5,SEEK_SET);
       lseek(fdpost,5,SEEK_SET);
       lseek(fdpre, 5, SEEK_SET);
       lseek(fdin,5,SEEK_SET);
       lseek(fdsuf,5,SEEK_SET);

       if((sz=read(fdaff,&affCnt,sizeof(affCnt))) < 0 || (sz=read(fdpost, &postCnt,sizeof(postCnt))) < 0 || (sz=read(fdin,&inCnt,sizeof(inCnt))) < 0 || (sz=read(fdsuf,&sufCnt,sizeof(sufCnt)))< 0)
       {
               err(3,"Error reading one of the counts");
       }

       struct package currPack;
       uint32_t postNum=0;
       uint8_t preNum=0;
       uint64_t sufNum=0;
       uint16_t inNum=0;
       while((sz=read(fdaff,&currPack, sizeof(currPack))) > 0)
       {
               if(currPack.post_no + currPack.post_count > postCnt || currPack.pre_no + currPack.pre_count > preCnt || currPack.in_no + currPack.in_count > inCnt || currPack.suf_no + currPack.suf_count > sufCnt )
               {
                       err(5,"Incosistent file.");
               }
               if(lseek(fdpost,currPack.post_no,SEEK_SET) < 0 || lseek(fdpre,currPack.pre_no, SEEK_SET) < 0 || lseek(fdin,currPack.in_no, SEEK_SET) < 0 || lseek(fdsuf, currPack.suf_no, SEEK_SET) < 0)
               {
                       err(4,"Error seeking one of the _no's");
               }

               for(size_t i = 0; i < currPack.post_count;i++)
               {
                       if((sz=read(fdpost,&postNum,sizeof(postNum)))< 0)
                       {
                               err(6,"Error reading for postNum");
                       }

                       if(write(fdcru,&postNum,sizeof(postNum)) != sizeof(postNum))
                       {
                               err(6,"Error writing postNum");
                       }
               }
               for(size_t i = 0; i < currPack.pre_count;i++)
               {
                       if((sz=read(fdpre,&preNum,sizeof(preNum)))< 0)
                       {
                               err(6,"Error reading for postNum");
                       }

                       if(write(fdcru,&preNum,sizeof(preNum)) != sizeof(preNum))
                       {
                               err(6,"Error writing postNum");
                       }
               }
               for(size_t i = 0; i < currPack.in_count;i++)
               {
                       if((sz=read(fdin,&inNum,sizeof(inNum)))< 0)
                       {
                               err(6,"Error reading for postNum");
                       }

                       if(write(fdcru,&inNum,sizeof(inNum)) != sizeof(inNum))
                       {
                               err(6,"Error writing postNum");
                       }
               }
               for(size_t i = 0; i < currPack.suf_count;i++)
               {
                       if((sz=read(fdsuf,&sufNum,sizeof(sufNum)))< 0)
                       {
                               err(6,"Error reading for postNum");
                       }

                       if(write(fdcru,&sufNum,sizeof(sufNum)) != sizeof(sufNum))
                       {
                               err(6,"Error writing postNum");
                       }
               }
       }

       return 0;
}
```
### 69. 2020-SE-02
Idea: Read uint8_t number. To see where this bit is located, divide number by 8 to get in which octet the bit is located. After that to get the position of the bit in the octet -> offset%8; Then seek to the byte that contais our bit. Afterwards shift the number bitwise offset%8 times to the left. Using mask 10000000 we can get that bit and check if it is 128 or 0(128 will equal 1 when we print it later on); If it is 128 then write offset to the resulting file.
```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
int main (const int argc,const char * argv[])
{
        if(argc !=3)
        {
                errx(1,"insufficient args");
        }
        uint8_t mask = 128; // 10000000
        uint8_t num,masked;
        uint16_t offset;
        int scl,sdl,res;
        scl=open(argv[1],O_RDONLY);
        sdl=open(argv[2],O_RDONLY);
        res=creat("result", S_IRUSR | S_IWUSR);
        if(scl==-1 || sdl == -1 || res == -1)
        {
                err(2,"Error opening one of the files");
        }

        ssize_t rdsz,rdsz2;
       while((rdsz=read(sdl,&offset,sizeof(offset))) > 0)
       {
               uint8_t currByte=offset/8;
               uint8_t inBytePosition = offset%8;
               if(lseek(scl,currByte,SEEK_SET) < 0)
               {
                       err(3,"Error seeking byte");
               }
               if((rdsz2=read(scl,&num,sizeof(num))) < 0)
               {
                       err(4,"Error reading num from scl");
               }
               num<<=inBytePosition;
               masked=num & mask;
               if(masked == 128)
               {
                       if(write(res,&offset,sizeof(offset)) != sizeof(offset))
                       {
                               err(5,"Error writing masked bit to result");
                       }
               }
       }
       return 0;
}
```
```c
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <err.h>
#include <sys/stat.h>
#include <stdint.h>
#include <string.h>
#include <stdbool.h>
int main(const int argc, char* argv[])
{
    if (argc != 3){
        errx(1, "input");
    }
    int fd1, fd2, output;
    if ((fd1 = open(argv[1], O_RDONLY)) == -1){
        err(2, "open");
    }
    if ((fd2 = open(argv[2], O_RDONLY)) == -1){
        err(3, "open");
    }
    int size1, size2;
    struct stat s;
    if (fstat(fd1, &s) == -1){
        err(4, "stat");
    }
    size1 = s.st_size;
    if (fstat(fd2, &s) == -1){
        err(5, "stat");
    }
    size2 = s.st_size;
    if (size2 % 2 != 0){
        errx(6, "wrong size");
    }
    if (size1 * 8 != size2 / 2){
        errx(7, "missmatching size");
    }
    if ((output = open("output", O_WRONLY | O_CREAT | O_TRUNC, S_IRUSR | S_IWUSR)) == -1){
        err(10, "open");
    }
    uint8_t currBits;
    uint16_t currNum;
    for (int i=0; i<size1; i++){
        uint8_t mask = 128;
        if (read(fd1, &currBits, sizeof(currBits)) != sizeof(currBits)){
            err(8, "read");
        }
        for (int j=0; j<8; j++){
            if (read(fd2, &currNum, sizeof(currNum)) != sizeof(currNum)){
                err(9, "read");
            }
            if ((currBits & mask) == mask){
                if (write(output, &currNum, sizeof(currNum)) != sizeof(currNum)){
                    err(11, "write");
                }
            }
            mask >>= 1;
        }
    }
    close(fd1);
    close(fd2);
    close(output);
    exit(0);
}
```
### 70. 2021-SE-01
Idea: Read uint8_t number from input.bin and for each of these numbers use a bit mask=00000001 to get the last bit of each uint8_t number. If the bit is 1 then to a variable resultNum add 0*2^(deg++) + 1*2^(deg++) where deg begins from 0. If bit is 0 then to resultNum add 1*2^(deg++) + 0*2^(deg++). Repeat this for all of the 8 bits in the number that we read from input.bin. After that just write resultNum to file.

```C
#include <stdio.h>
#include <err.h>
#include <math.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
uint16_t myPow(uint16_t num,uint16_t deg);
uint16_t myPow(uint16_t num,uint16_t deg)
{
    uint16_t var=num;
    if(deg==0)
    {
            return 1;
    }
    for(int i = 1; i < deg; i++)
    {
            num*=var;
    }
    return num;
}
int main (const int argc,const char * argv[])
{
    if(argc !=3)
    {
            errx(1,"insufficient args");
	}
    uint8_t mask = 1; // 00000001
    uint8_t num,masked;
    int fd1,res;
    fd1=open(argv[1],O_RDONLY);
    res=creat(argv[2], S_IRUSR | S_IWUSR);
    if(fd1==-1 || res == -1)
    {
            err(2,"Error opening one of the files");
    }
       ssize_t rdsz;
       while((rdsz=read(fd1,&num,sizeof(num))) > 0)
       {
               uint16_t resultNum=0;
               uint16_t deg=0,deg16=0;
               uint32_t magic = 0x699A;
               if(write(res,&magic,sizeof(magic)) != sizeof(magic))
               {
                       err(5,"Error writing resultNum to result file");
               }
               while(deg <= 7)
               {
                       masked=num&mask;
                       if(masked == 1)
                       {
                               deg16++;
                               resultNum+=myPow(2,deg16);
                               deg16++;
                               //printf("bit is 1:  %u %u\n",deg16,resultNum);
                       }
                       else
                       {
                               resultNum+=myPow(2,deg16);
                               deg16+=2;
                               //printf("bit is 0:  %u %u\n",deg16, resultNum);
                       }
                       num>>=1;
                       deg++;
               }
               if(write(res,&resultNum,sizeof(resultNum)) != sizeof(resultNum))
               {
                       err(5,"Error writing resultNum to result file");
               }
               //printf("my resultNum: %u\n",resultNum);
       }
       return 0;
}
```
much better
```c
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <err.h>
#include <sys/stat.h>
#include <stdint.h>
#include <string.h>
#include <stdbool.h>
int main(const int argc, char* argv[])
{
    if (argc != 3){
        errx(1, "input");
    }
    int fd1, fd2;
    if ((fd1 = open(argv[1], O_RDONLY)) == -1){
        err(2, "open");
    }
    if ((fd2 = open(argv[2], O_WRONLY | O_CREAT | O_TRUNC, S_IRUSR | S_IWUSR)) == -1){
        err(3, "open");
    }
    uint8_t currBits;
    int sz;
    while((sz = read(fd1, &currBits, sizeof(currBits))) == sizeof(currBits)){
        uint16_t res = 0;
        uint8_t mask = 128;
        uint16_t mask1 = 1;
        mask1 <<= 15;
        uint16_t mask2 = 1;
        mask2 <<= 14;
        for (int i=0; i<8; i++){
            if ((currBits & mask) == mask){
                res |= mask1;
            }
            else {
                res |= mask2;
            }
            mask >>=1;
            mask1 >>= 2;
            mask2 >>= 2;
        }
        if (write(fd2, &res, sizeof(res)) != sizeof(res)){
            err(4, "write");
        }
    }
    if (sz == -1){
        err(5, "read");
    }
    close(fd1);
    close(fd2);
    exit(0);
}

```
### 71. 2021-SE-02
```c
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
uint8_t myPow(uint8_t num,uint16_t deg);
uint8_t myPow(uint8_t num,uint16_t deg)
{
        uint8_t var=num;
        if(deg==0)
        {
                return 1;
        }
        for(int i = 1; i < deg; i++)
        {
                num*=var;
        }
        return num;
}
int main (const int argc,const char * argv[])
{
        if(argc != 3)
        {
                errx(1,"insufficient args");
        }
        uint16_t encoded,mask=1;
        int fd1,res;
        fd1=open(argv[1], O_RDONLY);
        res=creat(argv[2],S_IRUSR | S_IWUSR);
        if(fd1 == -1)
        {
                err(2,"Error opening encoded file.");
        }
       ssize_t rdsz;
       // 10011001 10011010
       while((rdsz=read(fd1,&encoded,sizeof(encoded)))>0)
       {
               uint16_t currBitPair=8,deg=0;
               uint8_t resultNum,left,right;
               while(currBitPair >0)
               {
                       right=encoded & mask;
                       encoded>>=1;
                       left=encoded & mask;
                       encoded>>=1;
                       if(left == 1 && right == 0)
                       {
                               resultNum+=myPow(2,deg);
                               deg++;
                       }
                       else // 0 1
                       {
                               deg++;
                       }
                       currBitPair--;
               }
               if(write(res,&resultNum,sizeof(resultNum)) != sizeof(resultNum))
               {
                       err(3,"Error writing the decoded num");
               }
       }
       return 0;
}
```
much better
```c
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <err.h>
#include <sys/stat.h>
#include <stdint.h>
#include <string.h>
#include <stdbool.h>
int main(const int argc, char* argv[])
{
    if (argc != 3){
        errx(1, "input");
    }
    int fd1, fd2;
    if ((fd1 = open(argv[1], O_RDONLY)) == -1){
        err(2, "open");
    }
    if ((fd2 = open(argv[2], O_WRONLY | O_CREAT | O_TRUNC, S_IRUSR | S_IWUSR)) == -1){
        err(3, "open");
    }
    uint16_t currBits;
    int sz;
    while((sz = read(fd1, &currBits, sizeof(currBits))) == sizeof(currBits)){
        uint8_t res = 0;
        uint8_t mask = 128;
        uint16_t mask1 = 1;
        mask1 <<= 15;
        for (int i=0; i<8; i++){
            if ((currBits & mask1) == mask1){
                res |= mask;
            }
            mask >>=1;
            mask1 >>= 2;
        }
        if (write(fd2, &res, sizeof(res)) != sizeof(res)){
            err(4, "write");
        }
    }
    if (sz == -1){
        err(5, "read");
    }
    close(fd1);
    close(fd2);
    exit(0);
}
```
### 72. 2022
```c
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
int main (const int argc,const char * argv[])
{
        if(argc != 3)
			err(1,"args");

        int fd1,fd2;
        if((fd1=open(argv[1],O_RDONLY)) == -1 || (fd2=creat(argv[2],S_IRUSR | S_IWUSR)) == -1)
        {
            err(2,"open/create");
        }
			
        struct stat s;
        if(fstat(fd1,&s)==-1)
			err(3,"staterror");
        if(s.st_size % sizeof(uint16_t) != 0)
            err(4,"incosistent file");

        ssize_t rdsz;
        int size=s.st_size /sizeof(uint16_t);
        uint16_t *buffer = malloc(s.st_size);
        if((rdsz=read(fd1,buffer,s.st_size))<0)
			err(5,"err reading in buffer");

        char output[50];
        int outputLen = snprintf(output,50,"const uint16_t arr={%u",buffer[0]);

        char sizeArr[50];
        int sizeArrLen=snprintf(sizeArr,50,"const uint32_t arrN=%d;\n",size);

        if(write(fd2,sizeArr,sizeArrLen) != sizeArrLen)
			err(6,"err sprintf on sizeArr");
        if(write(fd2,output,outputLen)!=outputLen)
            err(7,"err sprintf on output");

        char num[12];
        for(int i = 1; i < size; i++)
        {
            int newLen=snprintf(num,12,",%u",buffer[i]);
            if(write(fd2,num,newLen)!=newLen)
                    err(8,"err writing num %u to fd2", buffer[i]);
        }

        if(write(fd2,"};",2) != 2)
			err(9,"err writing };");

        free(buffer);
        return 0;
}

```

### 73. 2022-IN-01
```c
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
struct HEADER_STR
{
        uint16_t magic;
        uint16_t fileType;
        uint32_t count;
};
struct LIST_STR
{
        uint8_t off_data;
        uint8_t off_out;
};
int myCmp(const void * left, const void * right);
int myCmp(const void * left, const void * right)
{
        const struct LIST_STR * leftP = (const struct LIST_STR *)left;
        const struct LIST_STR *rightP = (const struct LIST_STR *)right;
        return leftP->off_out - rightP->off_out;
}
int main (const int argc,const char * argv[])
{
        uint16_t outFileType=3;
        uint16_t outMagic=0x5A4D;
        if(argc != 4)
        {
                errx(1,"insufficient args");
        }
        int fdlist,fddata,fdout,fdtmp,fdtmp2;
        fdlist=open(argv[1], O_RDONLY);
        fddata=open(argv[2], O_RDONLY);
        fdout=creat(argv[3], S_IRUSR | S_IWUSR);
        fdtmp=creat("tmp", S_IRUSR | S_IWUSR);
        fdtmp2=creat("tmp2", S_IRUSR | S_IWUSR);
        if(fdlist == -1 || fddata == -1 || fdout == -1 || fdtmp == -1 || fdtmp2 == -1)
        {
                err(2, "Error opening/creating one of the files");
        }

       ssize_t rdsz,rdsz1;
       struct HEADER_STR listH,dataH,outH;
       if((rdsz=read(fdlist,&listH,sizeof(listH))) < 0 || (rdsz1=read(fddata,&dataH,sizeof(dataH))) < 0)
       {
               err(3,"Error reading header from data or list.");
       }

       outH.magic = outMagic;
       outH.fileType = outFileType;
       outH.count = listH.count;

       if(write(fdout,&outH,sizeof(outH)) != sizeof(outH))
       {
               err(10,"error writing header of out to out.bin");
       }

       size_t ind=0,ind2=0;
       struct LIST_STR listStr;
       struct LIST_STR * list = malloc(listH.count/2); // todo::
       while((rdsz=read(fdlist,&listStr,sizeof(listStr))) > 0 && ind < listH.count/2)
       {
               list[ind++]=listStr;
       }

       qsort(list, ind, sizeof(listStr),myCmp); // first half is sorted and written
       for(size_t i = 0; i < ind; i++)
       {
               if(write(fdtmp, &list[i], sizeof(listStr)) != sizeof(listStr))
               {
                       err(4, "Error writing list str to tmp file");
               }
       }

       while((rdsz=read(fdlist,&listStr,sizeof(listStr))) > 0)
       {
               list[ind2++]=listStr;
       }
       qsort(list, ind2, sizeof(listStr),myCmp);
       lseek(fdtmp, 0, SEEK_SET);

       size_t currInd=0;
       while((rdsz=read(fdtmp,&listStr,sizeof(listStr))) > 0 && currInd < ind2)
       {
               struct LIST_STR arrList = list[currInd];
               if(listStr.off_out < arrList.off_out)
               {
                       if(write(fdtmp2, &listStr, sizeof(listStr)) != sizeof(listStr))
                       {
                               err(5,"Error writing listStr from file to fdtmp2");
                       }
               }
               else
               {
                       if(write(fdtmp2,&arrList, sizeof(arrList)) != sizeof(arrList))
                       {
                               err(6,"Error writing arrList from array to fdtmp2");
                       }
                       currInd++;
                       lseek(fdtmp,-1,SEEK_CUR);
               }
       }

       while(rdsz > 0 && (rdsz=read(fdtmp,&listStr, sizeof(listStr))) > 0)
       {
               if(write(fdtmp2,&listStr, sizeof(listStr)) != sizeof(listStr))
               {
                       err(6,"Error writing rest of listStr from file to fdtmp2");
               }
       }

       while(currInd < ind2)
       {
               if(write(fdtmp2, &list[currInd],sizeof(list[currInd])) != sizeof(list[currInd]))
               {
                       err(7,"Error writing the rest of list to fdtmp2");
               }
       }

       while((rdsz=read(fdtmp2, &listStr, sizeof(listStr))) > 0)
       {
               lseek(fddata, listStr.off_data, SEEK_SET);
               uint32_t dataNum;
               ssize_t rdsz2;
               if((rdsz2=read(fddata,&dataNum,sizeof(dataNum))) < 0)
               {
                       err(8,"Error reading dataNum from data.bin");
               }
               uint64_t outNum=(uint64_t)dataNum;
               if(write(fdout,&outNum,sizeof(outNum)) != sizeof(outNum))
               {
                       err(9,"ERror writing outNum to out.bin");
               }
       }

       free(list);
       return 0;
}
```
another solution
```c
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <err.h>
#include <sys/stat.h>
#include <stdint.h>
#include <string.h>
#include <stdbool.h>
int main(const int argc, char* argv[])
{
    if (argc != 4){
        errx(1, "input");
    }
    int list, data, out;
    if ((list = open(argv[1], O_RDONLY)) == -1){
        err(2, "open");
    }
    if ((data = open(argv[2], O_RDONLY)) == -1){
        err(3, "open");
    }
    if ((out = open(argv[3], O_WRONLY | O_CREAT | O_TRUNC, S_IRUSR | S_IWUSR)) == -1){
        err(4, "open");
    }
    uint16_t magic;
    if (read(list, &magic, sizeof(magic)) == -1){
        err(5, "read");
    }
    if (magic != 0x5A4D){
        errx(6, "wrong magic");
    }
    if (read(data, &magic, sizeof(magic)) == -1){
        err(7, "read");
    }
    if (magic != 0x5A4D){
        errx(8, "wrong magic");
    }
    uint16_t filetype;
    if (read(list, &filetype, sizeof(filetype)) == -1){
        err(9, "read");
    }
    if (filetype != 1){
        errx(10, "wrong file type");
    }
    if (read(data, &filetype, sizeof(filetype)) == -1){
        err(11, "read");
    }
    if (filetype != 2){
        errx(12, "wrong filetype");
    }
    uint32_t count;
    if (read(list, &count, sizeof(count)) == -1){
        err(14, "read");
    }
    if (lseek(list, 8, SEEK_SET) == -1){
        err(13, "lseek");
    }
    uint32_t outCount = 0;
    uint8_t listPos, listVal;
    uint64_t toWrite;
    for (uint32_t i=0; i<count; i++){
        if (read(list, &listPos, sizeof(listPos)) == -1){
            err(15, "read");
        }
        if (read(list, &listVal, sizeof(listVal)) == -1){
            err(16, "read");
        }
        if (listVal > outCount){
            outCount = listVal + 1;
        }
        if (lseek(data, 8 + listPos*4, SEEK_SET) == -1){
            err(17, "lseek");
        }
        if (read(data, &toWrite, sizeof(uint32_t)) != sizeof(uint32_t)){
            err(18, "read");
        }
        if (toWrite == 0){
            continue;
        }
        if (lseek(out, 8 + listVal*8, SEEK_SET) == -1){
            err(19, "lseek");
        }
        if (write(out, &toWrite, sizeof(toWrite)) != sizeof(toWrite)){
            err(20, "write");
        }
    }
    if (lseek(out, 0, SEEK_SET) == -1){
        err(21, "lseek");
    }
    if (write(out, &magic, sizeof(magic)) != sizeof(magic)){
        err(22, "write");
    }
    filetype = 3;
    if (write(out, &filetype, sizeof(filetype)) != sizeof(filetype)){
        err(23, "write");
    }
    if (write(out, &outCount, sizeof(count)) != sizeof(count)){
        err(24, "write");
    }
    close(list);
    close(data);
    close(out);
    exit(0);
}
```
### 74. 2022-SE-01
```c
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
struct DataHeader
{
        uint32_t magic;
        uint32_t count;
};
struct ComparatorHeader
{
        uint32_t magic1;
	    uint16_t magic2;
        uint16_t reserved;
        uint64_t count;
};
struct ComparatorData
{
        uint16_t type;
        uint16_t reserved1,reserved2,reserved3;
        uint32_t offset1;
        uint32_t offset2;
};
int main (const int argc,const char * argv[])
{
    if(argc!=3)
    {
            errx(1,"insufficient args");
    }
	
    int fddata,fdcomp;
    fddata=open(argv[1],O_RDONLY);
    fdcomp=open(argv[2],O_RDONLY);
    if(fddata == -1 || fdcomp == -1)
    {
            err(2,"Error opening one of the files");
    }

    struct DataHeader dataH;
    struct ComparatorHeader compH;
    struct ComparatorData compData;
    ssize_t rdsz;

    if((rdsz=read(fddata,&dataH,sizeof(dataH))) <0 || (rdsz=read(fdcomp,&compH,sizeof(compH))) <0)
    {
            err(3,"Error reading dataHeader or compHeader");
    }

    if(dataH.magic != 0x21796F4A || compH.magic1 != 0xAFBC7A37 || compH.magic2 != 0x1C27)
    {
            errx(4,"Incosistent files");
    }

    uint64_t data;
    //size_t dataInd=0;
    uint64_t * dataArr=malloc(sizeof(uint64_t)*dataH.count);
    if((rdsz=read(fddata,dataArr, sizeof(uint64_t)*dataH.count)) < 0)
    {
            err(5,"ewrr reading fdata")
    }
    //dataInd=rdsz/sizeof(uint64_t);
    close(fddata);
    
    fddata=open(argv[1], O_WRONLY | O_TRUNC);
    if(fddata == -1)
    {
            err(5, "Failed opening/truncating data.bin");
    }

    if(write(fddata, &dataH, sizeof(dataH)) != sizeof(dataH))
    {
            err(6, "Error writing header of dataH to data.bin");
    }

    while((rdsz=read(fdcomp,&compData, sizeof(compData))) > 0)
    {
            if(compData.reserved1 != 0 || compData.reserved2 != 0 || compData.reserved3 != 0 || compData.type > 1)
            {
                    errx(7,"Incosistent files");
            }

            uint32_t off1 = compData.offset1-64,off2=compData.offset2-64;
            if(compData.type == 0)
            {
                    if(dataArr[off1] > dataArr[off2])
                    {
                            uint64_t tmp = dataArr[off1];
                            dataArr[off1]= dataArr[off2];
                            dataArr[off2]= tmp;
                    }
            }
            else
            {
                    if(dataArr[off1] < dataArr[off2])
                    {
                            uint64_t tmp = dataArr[off1];
                            dataArr[off1]= dataArr[off2];
                            dataArr[off2]= tmp;
                    }
            }
    }

    for(size_t i =0; i < dataH.count; i++)
    {
            if(write(fddata,&dataArr[i],sizeof(dataArr[i])) != sizeof(dataArr[i]))
            {
                    err(8,"Error writing the correct data to data.bin");
            }
    }
    free(dataArr);
    close(fddata);
    close(fdcomp);
    return 0;
}
```

# 2-2. Процеси, pipe-ове, вход и изход (теми 6,7,8)

### 75. 2016-SE-01
```C
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <err.h>
#include <stdint.h>
int main(const int argc, char* argv[])
{
    if (argc != 2){
        err(1, "argc");
    }
    int a[2];
    if (pipe(a) == -1){
        err(2, "pipe");
    }
    pid_t p1 = fork();
    if (p1 == -1){
        err(3, "fork");
    }
    if (p1 == 0){
        close(a[0]);
        if (dup2(a[1], 1) == -1){
            err(4, "dup2");
        }
        close(a[1]);
        if (execlp("cat", "cat", argv[1], (char*)NULL) == -1){
            err(5, "execlp");
        }
        exit(EXIT_SUCCESS);
    }
    else
    {
	    close(a[1]);
	    if (dup2(a[0], 0) == -1){
	        err(6, "dup2");
	    }
	    if (execlp("sort", "sort", (char*)NULL) == -1){
	        err(7, "execlp");
	    }
	    exit(EXIT_SUCCESS);
	}   
}
```
// дава permission denied
### 76. 2016-SE-02
```C
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <err.h>
#include <sys/stat.h>
#include <stdint.h>
#include <string.h>
#include <stdbool.h>
int main(const int argc, char* argv[])
{
    if (argc != 1){
        err(1, "argc");
    }
    char* arg = NULL;
    size_t len = 0;
    do {
        printf("prompt: ");
        if (getline(&arg, &len, stdin) == -1){
            err(2, "getline");
        }
		if (strcmp(arg, "exit\n") == 0){
			break;
		}
		char * whole = malloc(6+len);
        char arr[]= "/bin/";
        size_t currInd=0;
        while(currInd < 5+len)
        {
                if(currInd < 5)
                        whole[currInd]=arr[currInd];
                else
                        whole[currInd]=arg[currInd-5];
                currInd++;
        }
        whole[currInd]='\0';
        if (execl("/bin/", arg, (char*)NULL) == -1){ //execl("/bin/", arg, (char*)NULL
            err(3, "execl");
        }
    }
    while(true);
    exit(0);
}
```
a better one
```c
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <err.h>
#include <sys/stat.h>
#include <stdint.h>
#include <string.h>
#include <stdbool.h>
int main(const int argc, char* argv[])
{
    if (argc != 1){
        err(1, "argc");
    }
        ssize_t rdsz,i=0;
        char buff[32];
    do {
        write(1,"prompt: ",8);

                while((rdsz=read(0,&buff[i],sizeof(char)))>0)
                {
                        if(buff[i]== ' ' || buff[i]=='\n' || buff[i]=='\t')
                        {
                                buff[i]='\0';
                                break;
                        }
                        i++;
                }
                //write(1,buff,i);
                if (strcmp(buff, "exit") == 0){
                        break;
                }

                pid_t c1=fork();

                if(c1==-1)
                {
                        err(3,"err forking");
                }
                else if(c1==0)
                {
                        if (execlp(buff,buff,(char*)NULL) == -1)
                        {
                err(3, "execl");
                }
                        exit(EXIT_SUCCESS);
                }
                wait(NULL);
                i=0;
    }
    while(true);
    exit(0);
}

```

### 77. 2017-IN-01
```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
#include <stdbool.h>
int main (const int argc,const char * argv[])
{
        int a[2];
        if(pipe(a)==-1)
        {
                err(1,"err: pipe a");
        }
        pid_t c1=fork();
        if(c1 == -1)
        {
                err(1,"err: fork c1");
        }
        if(c1 == 0) // First child
        {
                close(a[0]);
                if(dup2(a[1],1)==-1)
                {
                        err(1,"err: dup2 for a[1]");
                }
                if(execlp("awk","awk","-F",":","{print $7}","/etc/passwd",(char*)NULL)==-1)
                {
                        err(1,"err: execlp awk -F : print $7");
                }
                exit(EXIT_SUCCESS);
        }
        close(a[1]);    // new
        int b[2];
        if(pipe(b)== -1)
        {
                err(2,"err: pipe b[2]");
        }
        pid_t c2=fork();
        if(c2 == -1)
        {
                err(2,"err: fork c2");
        }
        if(c2==0) // Second Child
        {
                close(b[0]);
                if(dup2(a[0],0) ==-1)
                {
                        err(2,"err: dup2 a[0] for c2");
                }
                if(dup2(b[1],1) == -1)
                {
                        err(2,"err: dup2 for b[1] for c2");
                }
                if(execlp("sort","sort",(char*)NULL)==-1)
                {
                        err(2,"err: execlp for sort in c2");
                }
                exit(EXIT_SUCCESS);
        }
        close(b[1]);
        int c[2];
        if(pipe(c) == -1)
        {
                err(3,"err: pipe for c[2]");
        }
        pid_t c3 = fork();
        if(c3==-1)
        {
                err(3,"err: fork c3");
        }
        if(c3==0) // Third child
        {
                close(c[0]); // new
                if(dup2(b[0],0)==-1)
                {
                        err(3,"err: dup2 b[0] for c3");
                }
                if(dup2(c[1],1)==-1)
                {
                        err(3,"err: dup2 c[1] for c3");
                }
                if(execlp("uniq","uniq","-c",(char*)NULL)==-1)
                {
                        err(3,"err: execlp for uniq -c");
                }
                exit(EXIT_SUCCESS);
        }

        close(c[1]);

        //Parent
        if(dup2(c[0],0)==-1)
        {
                err(4,"err: dup2 for d[0] for parent");
        }
        close(c[0]);
        if(execlp("sort","sort","-k","1,1n",(char*)NULL)==-1)
        {
                err(4,"err: execlp awk print $2");
        }

        exit(EXIT_SUCCESS);
}

```
// трябва друг начин различен от getline
### 78. 2017-IN-02
```C
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <err.h>
#include <sys/stat.h>
#include <sys/wait.h>
#include <stdint.h>
#include <string.h>
#include <stdbool.h>
int main(const int argc, char* argv[])
{
    if (argc > 2){
        err(1, "argc");
    }
    char command[5] = "echo";
    if (argc == 2){
        if (strlen(argv[1]) > 4){
            errx(2, "length");
        }
        strcpy(command, argv[1]);
    }
    char* str = NULL;
    size_t len = 0;
    while(getline(&str, &len, stdin) > 0){
        int size = strlen(str) - 1;
        char arg1[5], arg2[5];
        int ind1 = 0, ind2 = 0, spaces = 0;
        for (int i=0; i<size; i++){
            if (str[i] != 0x20 && str[i] != 0x0A){
                if (spaces == 0 && ind1 < 4){
                    arg1[ind1++] = str[i];
                    if (i == size - 1){
                        pid_t p1 = fork();
                        if (p1 == 0){
                            if (execlp(command, command, arg1, (char*)NULL) == -1){
                                err(3, "execl");
                            }
                        }
                        wait(&p1);
                    }
                }
                else if (spaces == 1 && ind2 < 4){
                    arg2[ind2++] = str[i];
                }
                else
                    errx(4, "input");
            }
            if (spaces == 0 && (str[i] == 0x20 || str[i] == 0x0A)){
                spaces++;
                continue;
            }
            else if (spaces == 1 && ((str[i] == 0x20 || str[i] == 0x0A) || i == size-1)) {
                spaces = 0;
                pid_t p1 = fork();
                if (p1 == 0){
                    if (execlp(command, command, arg1, (char*)NULL) == -1){
                        err(3, "execl");
                    }
                }
                wait(&p1);
                p1 = fork();
                if (p1 == 0){
                    if (execlp(command, command, arg2, (char*)NULL) == -1){
                        err(4, "execlp");
                    }
                }
                wait(&p1);
                ind1 = ind2 = 0;
            }
        }
    }
    free(str);
    exit(0);
}
```
Second way
```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
int main (const int argc,const char * argv[])
{
        char myCmd[5];
        if(argc > 3)
        {
                errx(1,"too many arguments");
        }
        else if(argc == 2)
        {
                if(strlen(argv[1]) > 4)
                {
                        errx(2,"Cannot pass argument longer than 4 symbols");
                }
                strcpy(myCmd,argv[1]);
        }
        else
        {
                strcpy(myCmd,"echo");
        }

        size_t currInd=0,currWord=0;
        ssize_t rdsz;
        char byte;
        char currArg[5];
        char firstArg[5],secondArg[5];

        while((rdsz=read(0,&byte,sizeof(byte))) > 0)
        {
                //printf("%c\n",byte);
                if(currInd >=5)
                        err(3,"Stdin word %s exceeds 4 chars!", currArg);

                if(byte == 0x20 || byte == 0x0A)
                {
                        currArg[currInd]='\0';

                        if(currWord==0)
                        {
                                for(size_t i = 0; i <= currInd; i++)
                                {
                                        firstArg[i]=currArg[i];
                                }
                        }
                        else
                        {
                                for(size_t i = 0; i <= currInd; i++)
                                {
                                        secondArg[i]=currArg[i];
                                }
                        }

                        currWord++;
                        currInd=0;
                        if(currWord == 2)
                        {
                                printf("%s %s\n", firstArg, secondArg);
                                currWord=0;
                                if(execlp(myCmd,myCmd,firstArg,secondArg,(char*)NULL) == -1)
                                {
                                        err(4,"err: execlp on myCmd %s with arg %s",myCmd,currArg);
                                }
                        }
                }
                else
                {
                        currArg[currInd++]=byte;
                }
        }

        return 0;
}

```
### 79. 2018-SE-01
```C
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <err.h>
#include <sys/stat.h>
#include <stdint.h>
#include <string.h>
#include <stdbool.h>
int main(const int argc, char* argv[])
{
    if (argc != 2){
        err(1, "argc");
    }
    int a[2];
    if (pipe(a) == -1){
        err(2, "pipe");
    }
    pid_t p1 = fork();
    if (p1 == -1){
        err(3, "fork");
    }
    if (p1 == 0){
        close(a[0]);
        if (dup2(a[1], 1) == -1){
            err(4, "dup2");
        }
        if (execlp("find", "find", argv[1], "-type", "f", "-printf", "%T@:%f\n", (char*)NULL) == -1){
            err(5, "execl");
        }
    }
    close(a[1]);
    int b[2];
    if (pipe(b) == -1){
        err(6, "pipe");
    }
    pid_t p2 = fork();
    if (p2 == -1){
        err(7, "fork");
    }
    if (p2 == 0){
        close(b[0]);
        if (dup2(a[0], 0) == -1){
            err(8, "dup2");
        }
        if (dup2(b[1], 1) == -1){
            err(9, "dup2");
        }
        if (execlp("sort", "sort", "-n", "-k1,1", (char*)NULL) == -1){
            err(10, "execl");
        }
    }
    close(b[1]);
    close(a[0]);
    int c[2];
    if (pipe(c) == -1){
        err(11, "pipe");
    }
    pid_t p3 = fork();
    if (p3 == -1){
        err(12, "fork");
    }
    if (p3 == 0){
        close(c[0]);
        if (dup2(b[0], 0) == -1){
            err(13, "dup2");
        }
        if (dup2(c[1], 1) == -1){
            err(14, "dup2");
        }
        if (execlp("tail", "tail", "-n1", (char*)NULL) == -1){
            err(15, "execlp");
        }
    }
    close(c[1]);
    close(b[0]);
    if (dup2(c[0], 0) == -1){
        err(16, "dup2");
    }
    if (execlp("cut", "cut", "-d:", "-f2", (char*)NULL) == -1){
        err(17, "execlp");
    }
    exit(0);
}
```
### 80. 2019-SE-01
```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
#include <time.h>
#include <sys/wait.h>
int main (const int argc,const char * argv[])
{
        if(argc < 3)
        {
                errx(1,"Insufficient args");
        }
        int fd=creat("run.log", S_IRUSR | S_IWUSR);
        if(fd == -1)
        {
                err(2,"error creating run.log");
        }
        close(fd);

        printf("todays time: %lu\n", time(NULL));
        int timeThreshold = atoi(argv[1]);

        size_t qArgsSize=argc-2;
        char ** qArgs;
        qArgs=malloc(qArgsSize+1);
        qArgs[qArgsSize]= (char*)NULL;
        for(size_t i=0;i<qArgsSize; i++)
        {
                size_t argLen=strlen(argv[i+2]);
                qArgs[i]=malloc(argLen+1);
                for(size_t j = 0; j < argLen;j++)
                {
                        qArgs[i][j]=argv[i+2][j];
                }
        }

        size_t successes=0;
        while(1)
        {
                if(successes==2)
                        exit(EXIT_SUCCESS);

                time_t startTime=time(NULL);
                pid_t c1 = fork();
                if(c1 == -1)
                {
                        err(129, "c forking");
                }
                if (c1 == 0)
                {
                        if(execvp(argv[2],qArgs) == -1)
                                err(129, "error: execv on Q with args");

                        exit(EXIT_SUCCESS);
                }

                int status;
                if(waitpid(c1,&status, 0)==-1)
                        err(129, "Error waiting for child pid");

                time_t endTime=time(NULL);
                fd=open("run.log", O_WRONLY);
                if(fd ==-1)
                {
                        err(129,"err opening run.log in parent for writing or reading");
                }

                if(write(fd,&startTime,sizeof(startTime))!=sizeof(startTime))
                        err(129, "err: writing startTime");
                if(write(fd,&endTime,sizeof(endTime))!=sizeof(endTime))
                        err(129, "err: writing endTime");
                if(write(fd,&status,sizeof(status))!=sizeof(status))
                        err(129, "err: writing status to run.log");

                close(fd);

                if(WEXITSTATUS(status) != 0 && endTime - startTime < timeThreshold)
                {
                        successes++;
                }

        }

        for(size_t i = 0; i < qArgsSize; i++)
        {
                free(qArgs[i]);
        }
        free(qArgs);
        return 0;
}


```

### 81. 2020-SE-01
program to write to named pipe
```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
int main (const int argc,const char * argv[])
{
    const char* myfifo = "/home/students/s45811/2-2/jivodar";
        if(argc != 2)
        {
                errx(3,"not enough args");
        }
        int fd;
        mkfifo(myfifo, 0666);

        fd=open(myfifo, O_WRONLY);
        if(fd == -1)
        {
                err(2,"err opening mkfifo file");
        }

        if(dup2(fd,1) == -1)
        {
                err(3,"err: fd dup2 fd and a[0]");
        }

        if(execlp("cat","cat",argv[1],(char*)NULL)==-1)
        {
                err(4,"err with cat in execlp");
        }
        close(fd);
        return 0;
}

```
program to read from named pipe and sort its content
```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
int main (const int argc,const char * argv[])
{
        const char * myfifo = "/home/students/s45811/2-2/jivodar";
        if(argc != 2)
        {
                errx(1,"args");
        }
        int fd;
        mkfifo(myfifo,0666);
        fd=open(myfifo, O_RDONLY);
        if(fd==-1)
        {
                err(2,"err opening myfifo-jivodar");
        }

        if(dup2(fd,0)==-1)
        {
                err(3,"err dup2 on fd");
        }

        if(execlp(argv[1],argv[1],(char*)NULL)==-1)
        {
                err(4,"err execlp on argv[1]:%s", argv[1]);
        }
        close(fd);
        return 0;
}

```

### 82. 2020-SE-02
```C
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <err.h>
#include <sys/stat.h>
#include <stdint.h>
#include <string.h>
#include <stdbool.h>
int main(const int argc, char* argv[])
{
    if (argc != 3){
        errx(1, "argc");
    }
    int output;
    if ((output = open(argv[2], O_WRONLY | O_CREAT | O_TRUNC, S_IRUSR | S_IWUSR)) == -1){
        err(2, "open");
    }
    int a[2];
    if (pipe(a) == -1){
        err(3, "pipe");
    }
    pid_t p = fork();
    if (p == -1){
        err(4, "fork");
    }
    if (p == 0){
        close(a[0]);
        if (dup2(a[1], 1) == -1){
            err(5, "dup2");
        }
        if (execlp("cat", "cat", argv[1], (char*)NULL) == -1){
            err(6, "execlp");
        }
    }
    close(a[1]);
    uint8_t c;
    int sz;
    while((sz = read(a[0], &c, 1)) == 1){
        if (c == 0x7D){
            if (read(a[0], &c, 1) != 1){
                err(7, "read");
            }
            c = c ^ 0x20;
            if (write(output, &c, 1) != 1){
                err(8, "write");
            }
        }
    }
    if (sz == -1){
        err(9, "read");
    }
    close(output);
    exit(0);
}
```
### 83. 2020-SE-03 
```C
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <err.h>
#include <sys/stat.h>
#include <stdint.h>
#include <string.h>
#include <stdbool.h>
struct triple{
    char name[8];
    uint32_t offset;
    uint32_t length;
};
int main(const int argc, char* argv[])
{
    if (argc != 2){
        errx(1, "argc");
    }
    int fd;
    if ((fd = open(argv[1], O_RDONLY)) == -1){
        err(2, "open");
    }
    struct stat s;
    if (fstat(fd, &s) == -1){
        err(3, "fstat");
    }
    int size = s.st_size / 16;
    struct triple arr[8];
    for (int i=0; i<size; i++){
        if (read(fd, &arr[i], sizeof(arr[i])) != sizeof(arr[i])){
            err(4, "read");
        }
    }
    int a[2];
    if (pipe(a) == -1){
        err(5, "pipe");
    }
    pid_t p = -1;
    int i;
    for (i=0; i<size; i++){
        p = fork();
        if (p == -1){
            err(6, "fork");
        }
        if (p == 0)
            break;
    }
    uint16_t num;
    if (p == 0){
        close(a[0]);
        int currFd;
        if ((currFd = open(arr[i].name, O_RDONLY)) == -1){
            err(7, "open");
        }
        if (lseek(currFd, arr[i].offset, SEEK_SET) == -1){
            err(8, "lseek");
        }
        uint16_t res=0;
        for(uint32_t j=0; j<arr[i].length; j++){
            if (read(currFd, &num, sizeof(num)) != sizeof(num)){
                err(9, "read");
            }
            res^=num;
        }
        if (write(a[1], &res, sizeof(res)) != sizeof(res)){
            err(10, "write");
        }
    }
    else{
        close(a[1]);
        uint16_t res = 0;
        int sz;
        while((sz = read(a[0], &num, sizeof(num))) == sizeof(num)){
            res^=num;
        }
        if (sz == -1){
            err(11, "read");
        }
        printf("result: %d\n", res);
        close(fd);
    }
    exit(0);
}
```

```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
struct tupleProcess
{
        char fileName[8];
        uint32_t offset;
        uint32_t length;
};
int main (const int argc,const char * argv[])
{
        if(argc !=2)
        {
                errx(1,"args");
        }
        int fd=open(argv[1], O_RDONLY);
        struct tupleProcess interval;
        ssize_t rdsz;
        uint16_t parentResult=0;
        while((rdsz=read(fd,&interval,sizeof(interval))) > 0)
        {
                int a[2];
                if(pipe(a)==-1)
                {
                        err(2,"err pipe for a[2] for %s", interval.fileName);
                }
                pid_t c1 = fork();
                if(c1 == -1)
                {
                        err(3,"err forking c1");
                }
                else if(c1 == 0)
                {
                        close(a[0]);
                        uint16_t childResult=0;
                        int fdc=open(interval.fileName, O_RDONLY);
                        if(fdc==-1)
                        {
                                err(4,"opening fdc went wrong");
                        }
                        lseek(fdc,interval.offset,SEEK_SET);
                        for(uint32_t i =0; i< interval.length;i++)
                        {
                                uint16_t num;
                                ssize_t currRdsz;
                                if((currRdsz=read(fdc,&num,sizeof(num))) < 0)
                                {
                                        err(5,"reading num from file %s", interval.fileName);
                                }
                                childResult= num ^ childResult;
                        }
                        if(write(a[1],&childResult,sizeof(childResult))!=sizeof(childResult))
                        {
                                err(6,"err: writing childResult for file %s", interval.fileName);
                        }
                        close(a[1]);
                }
                else
                {
                        close(a[1]);
                        ssize_t currRdsz;
                        uint16_t childRes;
                        if((currRdsz=read(a[0],&childRes,sizeof(childRes)))< 0)
                        {
                                err(7,"err reading child result from read end of pipe in parent");
                        }
                        parentResult= childRes ^ parentResult;
                }
        }

        printf("result: %uB", parentResult);
        return 0;
}

```
### 84. 2021-SE-01

### 85. 2022-IN-01
```C
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <err.h>
#include <sys/stat.h>
#include <stdint.h>
#include <string.h>
int main(const int argc, char* argv[])
{
    if (argc != 3){
        errx(1, "argc");
    }
    int N = atoi(argv[1]);
    int D = atoi(argv[2]);
    const char* ding = "DING";
    const char* dong = "DONG";
    int cw[2];
    int pw[2];
    pipe(cw);
    pipe(pw);
    pid_t a = fork();
    if (a == -1){
        err(2, "fork");
    }
    if (a == 0){
        close(cw[1]);
        close(pw[0]);
    }
    else{
        close(cw[0]);
        close(pw[1]);
    }
    int dongLen = strlen(dong);
    int dingLen = strlen(ding);
    char c;
    for (int i=0; i<N; i++){
        if (a == 0){
            if(read(cw[0], &c, 1) != 1){
                err(3, "read");
            }
            if(write(1, dong, dongLen) != dongLen){
                err(4, "write");
            }
            printf("\n");
            if(write(pw[1], &c, 1) != 1){
                err(5, "write");
            }
        }
        else {
            if(write(1, ding, dingLen) != dingLen){
                err(6, "write");
            }
            if(write(cw[1], &c, 1) != 1){
                err(7, "write");
            }
            if(read(pw[0], &c, 1) != 1){
                err(8, "read");
            }
            sleep(D);
        }
    }
    exit(0);
}
```

```C
#include <stdio.h>
#include <err.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <stdint.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
int main (const int argc,const char * argv[])
{
        if(argc != 3)
        {
                errx(1,"err args");
        }

        unsigned int N = atoi(argv[1]), D = atoi(argv[2]);
        int arr[2*N][2];

        for(unsigned int i = 0; i < 2*N; i+=2)
        {
                if(pipe(arr[i])==-1 || pipe(arr[i+1])==-1)
                {
                        err(2,"error opening one of the pipes");
                }

        }

        pid_t c1 = fork();
        if(c1==-1)
        {
                err(3,"forking c1");
        }
        for (unsigned int i = 0; i < 2*N; i+=2)
        {
                if(c1 == 0) // child
                {
                        ssize_t rdsz;
                        char byte;

                        close(arr[i][1]);
                        if((rdsz=read(arr[i][0], &byte, sizeof(byte)))<0)
                        {
                                err(5,"err reading byte in child");
                        }
                        close(arr[i][0]);

                        printf("DONG %d\n", i);

                        close(arr[i+1][0]);
                        if(write(arr[i+1][1], &byte, sizeof(byte)) != sizeof(byte))
                        {
                                err(6,"err writing byte to parent");
                        }
                        close(arr[i+1][1]);
                }
                else // parent
                {
                        ssize_t rdsz;
                        char byte;
                        printf("DING %d\n", i);

                        close(arr[i][0]);
                        if(write(arr[i][1],&byte,sizeof(byte))!=sizeof(byte))
                        {
                                err(6,"err writing byte to child from parent");
                        }
                        close(arr[i][1]);

                        close(arr[i+1][1]);
                        if((rdsz=read(arr[i+1][0], &byte, sizeof(byte)))<0)
                        {
                                err(5,"err reading byte in parent in i+1");
                        }
                        close(arr[i+1][0]);

                        sleep(D);
                }
        }
        exit(EXIT_SUCCESS);
}

```
