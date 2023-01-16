### 1. 03-a-0200
- Сортирайте /etc/passwd лексикографски по поле UserID.

		cat /etc/passwd | sort -t ":" -k 3,3

### 2. 03-a-0201
- Сортирайте /etc/passwd числово по поле UserID.
(Открийте разликите с лексикографската сортировка)

		cat /etc/passwd | sort -t ":" -n -k 3,3

### 3. 03-a-0210 
- Изведете само 1-ва и 5-та колона на файла /etc/passwd спрямо разделител ":".

		cat /etc/passwd | awk -F ":" '{print $1, $5}'

### 4. 03-a-0211 
- Изведете съдържанието на файла /etc/passwd от 2-ри до 6-ти символ.
	
		cat /etc/passwd | head -n 1 | cut -c 2-6

### 5. 03-a-0212 
- Отпечатайте потребителските имена и техните home директории от /etc/passwd.

		cat /etc/passwd | awk -F ":" '{print $5, $6}'

### 6. 03-a-0213 
- Отпечатайте втората колона на /etc/passwd, разделена спрямо символ '/'.

		cat /etc/passwd | awk -F "/" '{print $2}'

### 7. 03-a-1500
- Изведете броя на байтовете в 

		/etc/passwd. cat /etc/passwd | wc -c
- Изведете броя на символите в 

		/etc/passwd. cat /etc/passwd | wc -m
- Изведете броя на редовете  в 

		/etc/passwd. cat /etc/passwd | wc -lcat

### 8. 03-a-2000 - useful link https://www.baeldung.com/linux/display-first-n-characters-of-file#:~:text=Using%20the%20head%20Command,already%20installed%20on%20our%20machine.&text=Mind%20that%20newlines%2C%20tabs%2C%20and,are%20also%20counted%20as%20bytes.
С отделни команди, извадете от файл /etc/passwd:
- първите 12 реда 

		head /etc/passwd -n 12
- първите 26 символа 

		/etc/passwd | cut -z -c -26
- всички редове, освен последните 4 

		 head /etc/passwd -n -4
- последните 17 реда 

		tail /etc/passwd -n 17
- 151-я ред (или друг произволен, ако нямате достатъчно редове) 

		head /etc/passwd -n 151 | tail -n 1
- последните 4 символа от 13-ти ред (символът за нов ред не е част от реда) 

		one way => head /etc/passwd -n 13 | tail -n 1 | rev | cut -c -4 | rev
		other way => head /etc/passwd -n 13 | tail -n 1 | sed -e 's/.*\(.\{4\}\)$/\1/'

### 9. 03-a-3000 
Запаметете във файл в своята home директория резултатът от командата `df -P`.
Напишете команда, която извежда на екрана съдържанието на този файл, без първия ред (хедъра), сортирано по второ поле (numeric).

	df -P > dfout
	tail -n +1 dfout | sort -n -k 2,2

### 10. 03-a-3100
- Запазете само потребителските имена от /etc/passwd във файл users във вашата home директория.

		cat /etc/passwd | awk -F ":" '{print $5}' > ~/users

### 11. 03-a-3500
- Изпишете всички usernames от /etc/passwd с главни букви. 

		cat users | tr 'абвгдежзийклмнопрстуфхцчшщъюя' 'АБВГДЕЖЗИЙКЛМНОПРСТУФХЦЧШЩЪЮЯ'

### 12. 03-a-5000 
Изведете реда от /etc/passwd, на който има информация за вашия потребител. cat /etc/passwd | egrep 's12345'
- Изведедете този ред и двата реда преди него. 

		cat /etc/passwd | egrep 's45811' -B2
- Изведете този ред, двата преди него, и трите след него. 

		cat /etc/passwd | egrep 's45811' -B2 -A3
- Изведете *само* реда, който се намира 2 реда преди реда, съдържащ информация за вашия потребител. 

		cat /etc/passwd | egrep 's45811' -B2 | head -n 1

### 13. 03-a-5001
- Изведете колко потребители не изпозват /bin/bash за login shell според /etc/passwd
(hint: 'man 5 passwd' за информация какъв е форматът на /etc/passwd)

		cat /etc/passwd | egrep -v '/bin/bash' | wc -l

### 14. 03-a-5002
- Изведете само имената на хората с второ име по-дълго от 6 (>6) символа според /etc/passwd

		cat /etc/passwd | awk -F ":" '{print $5}' | sed -e 's/\(.*\)\,\{3,4\}.*$/\1/' | sed -e 's/\(.*\),.*$/\1/' | cut -d " " -f 2 | awk 'length($1) > 6'

### 15. 03-a-5003 
- Изведете имената на хората с второ име по-късо от 8 (<=7) символа според /etc/passwd // !(>7) = ?

		cat /etc/passwd | awk -F ":" '{print $5}' | sed -e 's/\(.*\)\,\{3,4\}.*$/\1/' | sed -e 's/\(.*\),.*$/\1/' | cut -d " " -f 2 | awk 'length($1) <= 7'
		cat /etc/passwd | cut -d ":" -f 5 | egrep -o '[а-Яa-zA-Z]+ [а-Яa-zA-Z]{,7}'
		cat /etc/passwd | awk -F ":" '{print $5}' | awk -F "," '{print $1}' | awk 'length($2) > 6' | cut -f 1 -d ","

### 16. 03-a-5004
- Изведете целите редове от /etc/passwd за хората от 03-a-5003

		cat /etc/passwd | sed -e 's/:\([^,0-9]*\) \([^,]*\),\{3,4\}\([^a-zA-Z0-9]*\):/:\2:/' | awk -F ":" 'length($5) <= 7'   # works on all names
	egrep -w '[а-Я]+ [а-Я]{,7}' /etc/passwd             #works only on cyrilic names
	cat /etc/passwd | awk -F ":" '$5 ~ /[а-Я]+ [а-Я]{,7}/ {print $0}'
	cat /etc/passwd | awk -F ":" '$5 ~ /[а-яА-Я]+ [а-яА-Я]{,7}[^a-zA-Zа-яА-Я]+/ {print $0}'


### 17. 03-a-6000
Копирайте <РЕПО>/exercises/data/emp.data във вашата home директория.
Посредством awk, използвайки копирания файл за входнни данни, изведете:
- общия брой редове    

		cp /srv/fmi-os/exercises/data/emp.data ~/myData
		awk 'END {print NR}' myData  or awk '{rows= rows + 1 } END {print rows}' myData
- третия ред   

		awk 'NR == 3 {thirdRow = $0} END {print thirdRow}' myData or  awk 'NR == 3 {print $0}' myData
- последното поле от всеки ред      

		awk '{print $NF}' myData
- последното поле на последния ред    

		awk 'END {print $NF}' myData
- всеки ред, който има повече от 4 полета     

		awk 'NF > 4 {print $0}' myData
- всеки ред, чието последно поле е по-голямо от 4    

		awk '$NF > 4' myData
- общия брой полета във всички редове     

		awk '{AF = AF + NF} END {print AF}' myData
- броя редове, в които се среща низът Beth   

		awk '/.*Beth.*/ {occ = occ + 1} END {print occ}' myData
- най-голямото трето поле и редът, който го съдържа  

		 awk '$3 > max {max = $3; owner = $0} END {print max, owner}' myData
- всеки ред, който има поне едно поле  

		awk 'NF >= 1' myData
- всеки ред, който има повече от 17 знака  

		awk '/^.{18,}$/' myData
- броя на полетата във всеки ред и самият ред   

		awk '{print NF,$0}' myData
- първите две полета от всеки ред, с разменени места  

		awk '{print $2,$1}' myData
- всеки ред така, че първите две полета да са с разменени места  

		awk '{first=$1;second=$2;$1="";$2=""; print second, first, $0}' myData 
- всеки ред така, че на мястото на първото поле да има номер на реда 

		awk '{$1 = NR; print $0}' myData
- всеки ред без второто поле  

		awk '{$2=""; print $0}' myData 
- за всеки ред, сумата от второ и трето поле 

		awk '{sum= $2 + $3; print sum}' myData
- сумата на второ и трето поле от всеки ред 
 
		awk '{sum=sum+ $2 + $3; print sum}' myData
		cp /srv/exercises/data/emp.data ~/myData

### 18. 03-b-0300
- Намерете само Group ID-то си от файлa /etc/passwd.

		egrep 's12345' /etc/passwd | cut -d ":" -f 4  (one way)
	awk -F ":" '$1 == "s12345" {print $4}' /etc/passwd (another way)

### 19. 03-b-3400
- Колко коментара има във файла /etc/services ? Коментарите се маркират със символа #, след който всеки символ на реда се счита за коментар.

		egrep -o '#.+' /etc/services | cut -d '#' -f 2 | wc -m
		cat /etc/services | grep -o '#.*' | grep -o '[^#]*$' | wc -m


### 20. 03-b-3500
- Колко файлове в /bin са 'shell script'-oве? (Колко файлове в дадена директория са ASCII text?)

		find /bin -maxdepth 1 -type f ! -name "*.*" -exec grep -lvIP '[^[:ascii:]]' {} + | wc -l

### 21. 03-b-3600
- Направете списък с директориите на вашата файлова система, до които нямате достъп. Понеже файловата система може да е много голяма, търсете до 3 нива на дълбочина.

		find / -maxdepth 3 -type d 2> /dev/null ! -readable > noPermissions

### 22. 03-b-4000
- Създайте следната файлова йерархия в home директорията ви:

	dir5/file1
	dir5/file2
	dir5/file3

Посредством vi въведете следното съдържание:
- file1:
1
2
3

- file2:
s
a
d
f

- file3:
3
2
1
45
42
14
1
52

Изведете на екрана:
- статистика за броя редове, думи и символи за всеки един файл
- статистика за броя редове и символи за всички файлове
- общия брой редове на трите файла

		mkdir dir5
		cd dir5
		touch file1 file2 file3	
		wc file1 file2 file3

==Vim Commands==
- d - delete
motions 
    - w - delete current word up till the begining of the next word
    - e - delete only from begining of current word to end of current word
    - $ - delete from cursor to the end of line
- [number]motion 
    - 5w - moves cursor to begining of 6th word
    - 5e - moves cursor to end of 5th word
    - 5$ - moves cursor to the end of 5th line (counting from cursor)
- d[number]motion
    - d5w delets first 5 words from cursor
- dd -deletes whole line  
    - 2dd delets 2 consecutive lines
- u  - undo last command
- U  - return line to its original state
- Ctrl-R - redo 

- p - puts previously deleted text after the cursor
- r{char} - repaces the char over the cursor with {char}
	- hellk -> cursor at k press ro -> hello
- ce[word] - deletes the characters from cursor till end of word and replaces them with [word]
	- line tfree -> cursor on f press ce[hree] -> line three
	- c[number]motion
		- c1$[word] - deletes from cursor to end of line and replaces with word
		- c5w[word] - deletes from cursor 5 words ahead and replaces them with word
- CTRL-g - shows current line, col and percentage of file
	- gg - begining of file
	- G  - end of file
	- [number]G - go on line [number] of file
- /[pattern] - searches for [pattern] in file and with n you can navigate to next occurence or N for previous occurence. 
	- CTRL-o returns the cursor to the previous selection where we began 
- % - this cmd takes a bracket and goes to the respective closing/opening bracket of the marked one. Example: hey there ( i am hiden and also [ I am hiden! ] ). If cursor is on first ( and press % it will send the cursor at its closing counterpart.
- :s/old/new/g - substitutes every occurence of old with new on the selected line
	-  2,5s/old/new/g - substitute old with new from line 2 to line 5
	- %s/old/new/g - substitute in whole file
	- %s/old/new/gc - prompt on each find whether to substitute or not
- :![linux cmd] - can execute command from vim.
	- :w [name]- writes current vim editing under [name] to your current working directory.
	- :!rm [name] - to delete it
- v - pressing v goes into marking mode. Moving around the cursor while in v - mode you can highlight text and do things with it. For example we can highlight text and use :w [name] to save only this text. Or click d to delete it
- :r [FILENAME] - on the place where the cursor is, paste the contents of [FILENAME]
	- :r !ls - this pastes the output from the ls command
- o/O - o puts us in insert mode under the line of the cursor. O does the same but above the line.
- a - goes into insert mode AFTER the cursor
- R - on the cursor, begin replacing characters
- y - yank(copy) the highlighted text. Paste it with p.
	- yy - copy whole line
	- yw - copy word
- :set ic - ignores case when searching with /[pattern]
	- :set hlsearch/hls - highlights searches
	- :set noic/nohlsearch removes the highlight or case insensitiveness
- F1 - open help menu. 
	- :help [some command] - shows the help window with information about given command
	- CTRL-w CTRL-w - moves between the different opened windows (either side by side or stacked)
	- :set nocp - enter no compatible mode
		- CTRL-D during normal mode with commands (example :e{press ctrl-d} will show a list of commands that start with e. With tab we can iterate over different ones.
==End Vim Commands==


### 23. 03-b-4001 VIM TUTORIAL
- Във file2 (inplace) подменете всички малки букви с главни.
either use vi to change them or cat file2 | tr 'a-z' 'A-Z'

### 25. 03-b-4003
- Изведете статистика за най-често срещаните символи в трите файла.

		cat file1 file2 file3 | sed -e 's/\(.\)/\1\n/g' | sort | uniq -c

### 26. 03-b-4004
- Направете нов файл с име по ваш избор, чието съдържание е конкатенирани
съдържанията на file{1,2,3}.

		 cat file{1,2,3} > cat123.txt
### 27. 03-b-4005
- Прочетете текстов файл file1 и направете всички главни букви малки като
запишете резултата във file2.

		cat file1 | tr 'a-z' 'A-Z' > file2

### 28. 03-b-5200
- Намерете броя на символите, различни от буквата 'а' във файла /etc/passwd

		cat /etc/passwd | tr -d 'a' | wc -m  - counting new lines too 
		cat file2 | tr -d 'A' | sed -e ':a;N;$!ba;s/\n//g' | wc -m    - without new lines


### 29. 03-b-5300
- Намерете броя на уникалните символи, използвани в имената на потребителите от
	
		awk -F ":" '{print $5}' /etc/passwd | awk -F "," '{print $1}' | sed -e 's/\(.\)/\1\n/g' | sort | uniq -c | grep -o "1 .$" | wc -l
		awk -F ":" '{print $5}' /etc/passwd | awk -F "," '{print $1}' | sed -e 's/\(.\)/\1\n/g' | sort | uniq | wc -l

### 30. 03-b-5400
- Отпечатайте всички редове на файла /etc/passwd, които не съдържат символния низ 'ов'.

		cat /etc/passwd | egrep -v '.*ов.*'
### 31. 03-b-6100
- Отпечатайте последната цифра на UID на всички редове между 28-ми и 46-ред в /etc/passwd.

		cat /etc/passwd | head -n 46 | tail -n 19 | awk -F ":" '{print $3}' | grep -o '[0-9]$'

### 32. 03-b-6700
- Отпечатайте правата (permissions) и имената на всички файлове, до които имате
read достъп, намиращи се в директорията /tmp. (hint: 'man find', вижте -readable)

		find /tmp/ -readable | xargs stat -c "%a %n"
### 33. 03-b-6900
- Намерете имената на 10-те файла във вашата home директория, чието съдържание е редактирано най-скоро. На първо място трябва да бъде най-скоро редактираният файл. Намерете 10-те най-скоро достъпени файлове. (hint: Unix time)

		find . -type f | xargs stat -c "%Y %y %n" | sort -n -k 1,1 | tail -n 10 - last modification
		find . -type f | xargs stat -c "%X %x %n" | sort -n -k 1,1 | tail -n 10 - last access

###  34. 03-b-7000
- Да приемем, че файловете, които съдържат C код, завършват на `.c` или `.h`. Колко на брой са те в директорията `/usr/include`? Колко реда C код има в тези файлове?

		find /usr/include -type f \( -name \*.h -o -name \*.c \)  |  wc -l  - number of C files in /usr/include
		-   `-type f`  - only search for files (not directories)
		-   `\(`  &  `\)`  - are needed for the  `-type f`  to apply to all arguments
		-   `-o`  - logical OR operator
		-   `-iname`  - like  `-name`, but the match is case insensitive
		find /usr/include -type f ( -name *.h -o -name *.c ) | xargs cat | wc -l - all lines of code in these files
		find /usr/include -type f \( -name \*.h -o -name \*.c \) -exec cat {} + | wc -l  ??? questionable

### 35. 03-b-7500
- Даден ви е ASCII текстов файл - /etc/services. Отпечатайте хистограма на 10-те най-често срещани думи. Дума наричаме непразна последователност от букви. Не правим разлика между главни и малки букви. Хистограма наричаме поредица от редове, всеки от които има вида:  
<брой срещания> <какво се среща толкова пъти>
		
		egrep -o "[a-zA-Z0-9_]+" /etc/services | tr 'A-Z' 'a-z' | sort | uniq -c | sort -nk 1,1 | tail
		egrep -o "[a-zA-Z0-9_]+" /etc/services | sort | uniq -ci | sort -nk 1,1 | tail  - this way we escape tr and use the built in -i in uniq

### 36. 03-b-8000
- Вземете факултетните номера на студентите (описани във файла <РЕПО>/exercises/data/mypasswd.txt) от СИ и ги запишете във файл si.txt сортирани. Студент е част от СИ, ако home директорията на този потребител (според <РЕПО>/exercises/data/mypasswd.txt) се намира в /home/SI директорията.

		 grep "/home/SI" /srv/fmi-os/exercises/data/mypasswd.txt | cut -d ":" -f 1 | wc -l
### 37.  03-b-8500
- За всяка група от /etc/group изпишете "Hello, <група>", като ако това е вашата група, напишете "Hello, <група> - I am here!".

		PROCINFO["gid"]
		cat /etc/group | awk -F ":" '$1 == "students" {print "Hello,",$1,"- I am here!"} {print "Hello,",$1}'   
		cat /etc/group | awk -F ":" '{if ($1 == PROCINFO["gid"]) print "Hello,",$1,"- I am here!"; else print "Hello,",$1;}'  - with if else statement
		cat /etc/group | awk -F ":" '$3 == PROCINFO["gid"] {print "Hello,",$1,"- I am here!"} $3 != PROCINFO["gid"]{print "Hello,",$1}'
 - built in PROCINFO[...] keeps track of users groupid, userid and so on. Read man awk

### 38. 03-b-8600
- Shell Script-овете са файлове, които по конвенция имат разширение .sh. Всеки такъв файл започва с "#!<interpreter>" , където <interpreter> указва на операционната система какъв интерпретатор да пусне (пр: "#!/bin/bash", "#!/usr/bin/python3 -u"). Намерете всички .sh файлове в директорията `/usr` и нейните поддиректории, и проверете кой е най-често използваният интерпретатор.

		find /usr -type f -readable -name "*.sh" -exec grep -h -m 1 "^#!" {} + | cut -d "!" -f 2 | cut -d " " -f 2 | sort | uniq -c | sort -n -k 1,1 | tail -n 1

### 39. 03-b-8700
1. Изведете GID-овете на 5-те най-големи групи спрямо броя потребители, за които съответната група е основна (primary). 

		cat /etc/passwd | cut -d ":" -f 4 | sort -n | uniq -c | sort -k 1,1 | tail -n 5
2. (*) Изведете имената на съответните групи. Hint: /etc/passwd

		cat /etc/passwd | cut -d ":" -f 4 | sort -n | uniq -c | sort -k 1,1 | tail -n 5 | cut -f 2| awk '{my_cmd="getent group " $2 " | cut -d \":\" -f 1 2>/dev/null >&1"; cmd_out=system(my_cmd) ; print cmd_out,$0}'


### 40. 03-b-9000
- Направете файл eternity. Намерете всички файлове, намиращи се във вашата home директория и нейните поддиректории, които са били модифицирани в последните 15мин (по възможност изключете .). Запишете във eternity името (път) на файла и времето (unix time) на последната промяна.

		find ~/ -mmin -15 | xargs stat -c "%n %Y" | sort -n -k 2,2 | tail -n 1  > ~/eternity

### 41. 03-b-9050
- Копирайте файл <РЕПО>/exercises/data/population.csv във вашата home директория.
		
		cp /srv/fmi-os/exercises/data/population.csv ~/

### 42. 03-b-9051
- Използвайки файл population.csv, намерете колко е общото население на света през 2008 година. А през 2016?

		cat population.csv | sed -e 's/"\(.*\),\(.*\)",\(.*\),\(.*\),\(.*\)/"\1-\2",\3,\4,\5/' | awk -F "," '$3 == 2008 {sum += $4} END {printf "Total population: %20.0f\n",sum}'
		cat population.csv | sed -e 's/"\(.*\),\(.*\)",\(.*\),\(.*\),\(.*\)/"\1-\2",\3,\4,\5/' | awk -F "," '$3 == 2016 {sum += $4} END {printf "Total population: %20.0f\n",sum}'
		cat population.csv | rev | cut -d "," -f 1,2 | rev | awk -F "," '$1 == 2008 {sum+=$2} END {print "Total:",sum}'

		
### 43.  03-b-9052
- Използвайки файл population.csv, намерете през коя година в България има най-много население.

		cat population.csv | sed -e 's/"\(.*\),\(.*\)",\(.*\),\(.*\),\(.*\)/"\1-\2",\3,\4,\5/' | awk -F "," '$1 == "Bulgaria" && $4 > max {max=$4;year=$3} END {print "Max population is",max,"reached during",year }'
		cat population.csv | awk -F "," '$1 == "Bulgaria" && $4 > max {max=$4;year=$3} END {print "Max population is",max,"reached during",year }'     - this solution doesnt format the file for the countries that have ',' in their names
		cat population.csv | egrep 'Bulgaria' | awk -F "," '$4 > max {max=$4;year=$3} END {print "Max population is",max,"reached during",year }'

### 44. 03-b-9053
- Използвайки файл population.csv, намерете коя държава има най-много население през 2016. А коя е с най-малко население? (Hint: Погледнете имената на държавите)

		cat population.csv | sed -e 's/"\(.*\),\(.*\)",\(.*\),\(.*\),\(.*\)/"\1-\2",\3,\4,\5/' | sort -n -t "," -k 4,4 | awk -F "," '$3 == 2016 {print "Most population in 2016 is",$4,"in",$1}' | tail -n 1   - max population in 2016
		cat population.csv | sed -e 's/"\(.*\),\(.*\)",\(.*\),\(.*\),\(.*\)/"\1-\2",\3,\4,\5/' | sort -n -t "," -k 4,4 | awk -F "," '$3 == 2016 {print "Most population in 2016 is",$4,"in",$1}' | head -n 1 - min population 2016
		
### 45. 03-b-9054
- Използвайки файл population.csv, намерете коя държава е на 42-ро място по население през 1969. Колко е населението й през тази година?

		cat population.csv | sed -e 's/"\(.*\),\(.*\)",\(.*\),\(.*\),\(.*\)/"\1-\2",\3,\4,\5/' | awk -F "," '$3 == 1969' | sort -n -t "," -k 4,4 | sed -n "42p"     ---> -n suppresses standart output and prints the 42nd line of the input
		cat population.csv | sed -e 's/"\(.*\),\(.*\)",\(.*\),\(.*\),\(.*\)/"\1-\2",\3,\4,\5/' | awk -F "," '$3 == 1969' | sort -n -t "," -k 4,4 | awk -F "," 'NR == 42 {print $1,$4}'   - other way

### 46. 03-b-9100
- В home директорията си изпълнете командата curl -o songs.tar.gz "http://fangorn.uni-sofia.bg/misc/songs.tar.gz"
	
		curl -o songs.tar.gz "http://fangorn.uni-sofia.bg/misc/songs.tar.gz"

### 47. 03-b-9101
- Да се разархивира архивът songs.tar.gz в директория songs във вашата home директория.

		tar -xf songs.tar.gz   - x stands for extract, f - for file name
### 48.  03-b-9102
- Да се изведат само имената на песните.

		find . -type f -name "*.ogg" | cut -d "/" -f 2 | awk -F ".ogg" '{print $1}' | cut -d "-" -f 2 | cut -d " " -f 2-

### 49. 03-b-9103
- Имената на песните да се направят с малки букви, да се заменят спейсовете с долни черти и да се сортират.

		find . -type f -name "*.ogg" | cut -d "/" -f 2 | awk -F ".ogg" '{print $1}' | cut -d "-" -f 2 | cut -d " " -f 2- | tr 'A-Z' 'a-z' | tr ' ' '_' | sort

### 50. 03-b-9104
- Да се изведат всички албуми, сортирани по година.

		find . -type f -name "*.ogg" | cut -d "/" -f 2 | awk -F ".ogg" '{print $1}' | cut -d "-" -f 2 | cut -d " " -f 2- | cut -d "(" -f 2 | sed -e 's/)//g'| sort -n -t "," -k 2,2
### 51. 03-b-9105
- Да се преброят/изведат само песните на Beatles и Pink.

		find . -type f -name "*.ogg" | cut -d "/" -f 2 | awk -F ".ogg" '$1 ~ /^Pink .*/ || $1 ~ /^Beatles.*/ {print $1}' 
### 52. 03-b-9106
- Да се направят директории с имената на уникалните групи. За улеснение, имената от две думи да се напишат слято: Beatles, PinkFloyd, Madness

		find ~/tarTest -type f -name "*.ogg"| rev | cut -d "/" -f 1 | rev | awk -F ".ogg" '{print $1}' | awk -F " - " '{print $1}' | tr -d ' ' | sort | uniq | xargs mkdir

### 53. 03-b-9200
- Напишете серия от команди, които извеждат детайли за файловете и директориите в текущата директория, които имат същите права за достъп както най-големият файл в /etc директорията.

		find . -readable -perm $(find /etc -type f -readable -printf "%s %m\n" 2>/dev/null | sort -n -k 1,1 | tail -n 1 | cut -d " " -f 2)

### 54. 03-b-9300
- Дадени са ви 2 списъка с email адреси - първият има 12 валидни адреса, а вторията има само невалидни. Филтрирайте всички адреси, така че да останат само валидните. Колко кратък регулярен израз можете да направите за целта? Валидни email адреси (12 на брой):
	- email@example.com
	- firstname.lastname@example.com
	-	email@subdomain.example.com
	-	email@123.123.123.123
	-	1234567890@example.com
	-	email@example-one.com
	-	_______@example.com
	-	email@example.name
	-	email@example.museum
	-	email@example.co.jp
	-	firstname-lastname@example.com
	-	unusually.long.long.name@example.com

- Невалидни email адреси:
	- #@%^%#\$@#\$@#.com
	- @example.com
	- myemail
	- Joe Smith <email@example.com>
	- email.example.com
	- email@example@example.com
	- .email@example.com
	- email.@example.com
	- email..email@example.com
	- email@-example.com
	- email@example..com
	- Abc..123@example.com
	- (),:;<>[\]@example.com
	- just"not"right@example.com
	- this\ is"really"not\allowed@example.com


			egrep '^((_+)|([a-zA-Z0-9]+(\.[a-zA-Z0-9]|-[a-zA-Z0-9])*)+)@([a-zA-Z0-9]+(\.|-)?[a-zA-Z0-9]+)+$' emails.txt

### 55. 03-b-9400
- Посредством awk, използвайки emp.data (от 03-a-6000.txt) за входнни данни, изведете: 
	- всеки ред, като полетата са в обратен ред (Разгледайте for цикли в awk)

			awk '{for(i=NF;i>1;i--) printf "%s ",$i;printf "%s",$1;print ""}' myData
			awk '{for (i=NF;i>0;i--){ res=res $i "\t"} print res;res="" }'  myData


### 56. 03-b-9500
Копирайте <РЕПО>/exercises/data/ssa-input.txt във вашата home директория.
Общият вид на файла е:


	- заглавна част:

	Smart Array P440ar in Slot 0 (Embedded)

	- една или повече секции за масиви:

	Array A

	Array B

	...

	като буквата (A, B, ...) е името на масива

	- във всяка таква секция има една или повече подсекции за дискове:

	physicaldrive 2I:0:5

	physicaldrive 2I:0:6

	...

	като 2I:0:5 е името на диска

	- във всяка подсекция за диск има множество параметри във вида:

	key name: value

	като за нас са интересни само:

	Current Temperature (C): 35

	Maximum Temperature (C): 36

	Напишете поредица от команди която обработва файл в този формат, и генерира

	следният изход:

	A-2I:0:5 35 36

	A-2I:0:6 34 35

	B-1I:1:1 35 50

	B-1I:1:2 35 49

	x-yyyyyy zz ww

	където:

	- x е името на масива

	- yyyyyy е името на диска

	- zz е current temperature

	- ww е max temperature

	answer: egrep '(Array [a-zA-B0-9])|(physicaldrive)|(Current Temperature)|(Maximum Temperature)' myInput.txt | awk '$1 == "Array" {currArr=$2} $1== "physicaldrive" {res="";res=currArr "-" $2 " "} $1 == "Current" {res = res $4 " "} $1 == "Maximum" {res = res $4;print res}'

