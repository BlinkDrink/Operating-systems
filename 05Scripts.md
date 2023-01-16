### 1. 05-a-2000
Сменете вашия prompt с нещо по желание. После върнете оригиналния обратно.

		Във файл ~/.bashrc сменяме PS1="\u >" тогава пред командите които пишем ще виждаме само нашият username и това е. За да го върнем просто си копираме файла .bashrc преди да правим промени по него.

### 2. 05-a-2100
Редактирайте вашия .bash_profile файл, за да ви поздравява (или да изпълнява някаква команда по ваш избор) всеки път, когато влезете в системата.

		Във файл ~/.profile накрая можем да напишем различни серии от команди които да се изпълнят в момента когато се логнем. Промените можем да ги зададем с vim ~/.profile 

### 3. 05-a-2200
Направете си ваш псевдоним (alias) на полезна команда.

		alias loggedUsers="who | wc -l"

### 4. 05-b-2000
Да се напише shell скрипт, който приканва потребителя да въведе низ (име) и изпечатва "Hello, низ".

	#! /usr/bin/bash

	read INPUT
	echo "Hello, $INPUT"

### 5. 05-b-2800
Да се напише shell скрипт, който приема точно един параметър и проверява дали подаденият му параметър се състои само от букви и цифри.

	#! /usr/bin/bash
  
	REGEX='^[0-9a-zA-Z]+$'
	if [[ $1 =~ $REGEX ]]
	  then
	    echo "True $1"
	  else
	     echo "False $1"
	fi
### 6. 05-b-3100
Да се напише shell скрипт, който приканва потребителя да въведе низ - потребителско име на потребител от системата - след което извежда на стандартния изход колко активни сесии има потребителят в момента.

	#! /usr/bin/bash
	read INPUT_USERNAME
	SEARCH_RES=$(ps -eo sid,stat,user | awk '$2 == "Ss" {print $0}' | grep 	"$INPUT_USERNAME" | wc -l)
	echo "Active sessions for $INPUT_USERNAME: $SEARCH_RES"
### 7. 05-b-3200
Да се напише shell скрипт, който приканва потребителя да въведе пълното име на директория и извежда на стандартния изход подходящо съобщение за броя на всички файлове и всички директории в нея.

	#! /usr/bin/bash

	read DIR_INPUT
	INPUT_REG_FILES=$(find $1 -maxdepth 1 -type f | wc -l)
	INPUT_DIRS=$(find $1 -maxdepth 1 -type d | wc -l)
	INPUT_TOTAL=$(find $1 -maxdepth 1 | wc -l)

	echo "Total number of files: $INPUT_TOTAL"
	echo "Total number of regular files: $INPUT_REG_FILES"
	echo "Total number of directories: $INPUT_DIRS"

### 8. 05-b-3300
Да се напише shell скрипт, който чете от стандартния вход имената на 3 файла, обединява редовете на първите два (man paste), подрежда ги по азбучен ред и резултата записва в третия файл.

	#! /usr/bin/bash
	read F1
	read F2
	read F3
	paste $F1 $F2 | sort > $F3
### 9. 05-b-3400
Да се напише shell скрипт, който чете от стандартния вход име на файл и символен низ, проверява дали низа се съдържа във файла и извежда на стандартния изход кода на завършване на командата с която сте проверили наличието на низа. NB! Символният низ може да съдържа интервал (' ') в себе си.

	#! /usr/bin/bash

	echo "Enter file name: "
	read FILE_NAME
	echo "Enter string to check if it exists in file: "
	read STR_TO_FIND

	grep "$STR_TO_FIND" $FILE_NAME 1>/dev/null
	exitStatus=$?

	echo "$exitStatus"
### 10. 05-b-4200
Имате компилируем (a.k.a няма синтактични грешки) source file на езика C. Напишете shell script, който да покaзва колко е дълбоко най-дълбокото nest-ване (влагане).

	Примерен .c файл:
	#include <stdio.h>
	int main(int argc, char *argv[]) {
		if (argc == 1) {
			printf("There is only 1 argument");
		} else {
			printf("There are more than 1 arguments");
		}
		return 0;
	}

	Тук влагането е 2, понеже имаме main блок, а вътре в него if блок.
	Примерно извикване на скрипта:
	./count_nesting sum_c_code.c
	Изход:
	The deepest nesting is 2 levels
	
	#! /usr/bin/bash
	GET_ALL_LINES=$(egrep -o "\{|\}"  $1)
	counter=0
	maxNesting=0
	for i in $GET_ALL_LINES
	do
	        if [[ $i = "{" ]]
	        then
	                counter=$(($counter+1))
	        else
	                if [[ $counter -gt $maxNesting ]]
	                then
	                        maxNesting=$counter
	                fi
	                counter=$(($counter-1))
	        fi
	done
	echo "The deepest nesting is $maxNesting levels"

### 11. 05-b-4301
Напишете shell script, който автоматично да попълва файла указател от предната задача по подадени аргументи: име на файла указател, пълно име на човека (това, което очакваме да е в /etc/passwd) и избран за него nickname.
Файлът указател нека да е във формат:
<nickname, който лесно да запомните> <username в os-server>
// може да сложите и друг delimiter вместо интервал
Примерно извикване:
./pupulate_address_book myAddressBook "Ben Dover" uncleBen
Добавя към myAddressBook entry-то:
uncleBen <username на Ben Dover в os-server>
***Бонус: Ако има няколко съвпадения за въведеното име (напр. има 10 човека Ivan Petrov в /etc/passwd), всички те да се показват на потребителя, заедно с пореден номер >=1, след което той да може да въведе някой от номерата (или 0 ако не си хареса никого), и само избраният да бъде добавен към указателя.

	#! /usr/bin/bash

	findUsers=$(grep "$2" /etc/passwd)
	howMany=$(grep "$2" /etc/passwd | wc -l)
	echo "$howMany >>>>> $findUsers"

	if [[ $howMany -eq 1 ]]
	then
	        getUserName=$(echo "$findUsers" | cut -d ":" -f 1)
	        echo "$3 $getUserName" >> $1
	else
	        counter=1
	        grep "$2" /etc/passwd | nl -n ln
	        echo "Choose which one to add to the address book as $3"
	        read personNumber
	        chosenPerson=$(grep "$2" /etc/passwd | head -n $personNumber | tail -n 1)
	        chosenPersonUsername=$(echo "$chosenPerson" | cut -d ":" -f 1)
	        echo "$3 $chosenPersonUsername" >> $1
	fi

### 12. 05-b-4400
Напишете shell script, който да приема параметър име на директория, от която взимаме файлове, и опционално експлицитно име на директория, в която ще копираме файлове. Скриптът да копира файловете със съдържание, променено преди по-малко от 45 мин, от първата директория във втората директория. Ако втората директория не е подадена по име, нека да получи такова от днешната дата във формат, който ви е удобен. При желание новосъздадената директория да се архивира.

	#! /usr/bin/bash
	if [ -z "$2" ]
	then
	        currDate=$(date +"%d-%B-%Y-%H:%M:%S")
	        mkdir ~/$currDate
	        find $1 -maxdepth 1 -type f -mmin -45 | xargs cp -t ~/$currDate
	else
	        outputFolder=$2
	        outputFolder+="/"
	        find $1 -maxdepth 1 -type f -mmin -45 | xargs cp -t $outputFolder
	fi

### 13. 05-b-4500
Да се напише shell скрипт, който получава при стартиране като параметър в командния ред идентификатор на потребител. Скриптът периодично (sleep(1)) да проверява дали потребителят е log-нат, и ако да - да прекратява изпълнението си, извеждайки на стандартния изход подходящо съобщение.
NB! Можете да тествате по същият начин като в 05-b-4300.txt

	#! /usr/bin/bash

	isLoggedIn=1
	echo "Checking if $1 is logged in at $(date +"%H:%M:%S")..."
	while [ $isLoggedIn -gt 0 ]
	do
	        who | grep "$1" 1>/dev/null
	        isLoggedIn=$?
	        sleep 1
	done
	echo "User $1 logged in at $(date +"%H:%M:%S")"

### 14. 05-b-4600
Да се напише shell скрипт, който валидира дали дадено цяло число попада в целочислен интервал.
Скриптът приема 3 аргумента: числото, което трябва да се провери; лява граница на интервала; дясна граница на интервала.
Скриптът да връща exit status:
- 3, когато поне един от трите аргумента не е цяло число
- 2, когато границите на интервала са обърнати
- 1, когато числото не попада в интервала
- 0, когато числото попада в интервала

		Примери:
		$ ./validint.sh -42 0 102; echo $?
		1
		$ ./validint.sh 88 94 280; echo $?
		1
		$ ./validint.sh 32 42 0; echo $?
		2
		$ ./validint.sh asdf - 280; echo $?
		3
		
		#! /usr/bin/bash
		REGEX='[^0-9]+'
		if [[ $1 =~ $REGEX ]] || [[ $2 =~ $REGEX ]] || [[ $3 =~ $REGEX ]]
		then
		        exit 3
		else
		        if [ $2 -gt $3 ]
		        then
		                exit 2
		        fi

		        if [ $1 -lt $2 ] || [ $1 -gt $3 ]
		        then
		                exit 1
		        else
		                exit 0
		        fi
		fi

### 15. 05-b-4700
Да се напише shell скрипт, който форматира големи числа, за да са по-лесни за четене.
Като пръв аргумент на скрипта се подава цяло число.
Като втори незадължителен аргумент се подава разделител. По подразбиране цифрите се разделят с празен интервал.

	Примери:
	$ ./nicenumber.sh 1889734853
	1 889 734 853
	$ ./nicenumber.sh 7632223 ,
	7,632,223

	#! /usr/bin/bash
	if [ -z $2 ]
	then
	        myDivider=" "
	else
	        myDivider=$2
	fi
	newNum=""
	counter=1
	inputCpy=$1
	while [ $inputCpy -gt 0 ]
	do
	        newNum=$(expr "$(expr $inputCpy % 10)""$newNum")
	        if [ $(($counter % 3)) -eq 0 ]
	        then
	                if [ $(expr length $1) -ne $counter ]
	                then
	                newNum="${myDivider}${newNum}"
	                fi
	        fi
	        inputCpy=$(expr $inputCpy / 10)
	        counter=$(expr $counter + 1)
	done
	echo "$newNum"
### 16. 05-b-4800
Да се напише shell скрипт, който приема файл и директория. Скриптът проверява в подадената директория и нейните под-директории дали съществува копие на подадения файл и отпечатва имената на намерените копия, ако съществуват такива.
NB! Под 'копие' разбираме файл със същото съдържание.

	#! /usr/bin/bash

	filesFound=$(find $2 -type f | xargs)
	for i in $filesFound
	do
	        if [ $(diff $1 $i | wc -l) -eq 0 ]
	        then
	                echo "$i"
	        fi
	done

### 17. 05-b-5500
Да се напише shell script, който генерира HTML таблица съдържаща описание на
потребителите във виртуалката ви. Таблицата трябва да има:
- заглавен ред с имената нa колоните
- колони за username, group, login shell, GECOS field (https://en.wikipedia.org/wiki/Gecos_field)
Пример:
$ ./passwd-to-html.sh > table.html
$ cat table.html


		<table>
			<tr>
				<th>Username</th>
				<th>group</th>
				<th>login shell</th>
				<th>GECOS</th>
			</tr>
			<tr>
				<td>root</td>
				<td>root</td>
				<td>/bin/bash</td>
				<td>GECOS here</td>
			</tr>
			<tr>
				<td>ubuntu</td>
				<td>ubuntu</td>
				<td>/bin/dash</td>
				<td>GECOS 2</td>
			</tr>
		</table>
		
		#! /usr/bin/bash

		awk -F ":" 'BEGIN {print "<table>\n\t<tr>\n\t\t<th>Username</th>\n\t\t<th>group</th>\n\t\t<th>login shell</th>\n\t\t<th>GECOS</th>\n\t</tr>"}
		{print "\t<tr>\n\t\t<td>" $1 "</td>\n\t\t<td>" $4 "</td>\n\t\t<td>" $7 "</td>\n\t\t<td>" $5 "</td>\n\t</tr>"}
		END {print "</table>"}' /etc/passwd > $1

### 18. 05-b-6600
Да се напише shell скрипт, който получава единствен аргумент директория и изтрива всички повтарящи се (по съдържание) файлове в дадената директория. Когато има няколко еднакви файла, да се остави само този, чието име е лексикографски преди имената на останалите дублирани файлове.

	Примери:
	$ ls .
	f1 f2 f3 asdf asdf2
	 # asdf и asdf2 са еднакви по съдържание, но f1, f2, f3 са уникални
	$ ./rmdup .
	$ ls .
	f1 f2 f3 asdf
	 # asdf2 е изтрит
	 
	#! /usr/bin/bash
	filesInDir=$(find $1 -maxdepth 1 -type f | xargs)
	allFilesToDel=""
	for i in $filesInDir
	do
	        filesToDelete=$(/home/students/s45811/scripts/16-05-b-4800.sh $i | sort | tail -n +2 | uniq | xargs)
	        allFilesToDel="${allFilesToDel}${filesToDelete} "
	done
	finalFilesToDel=$(echo $allFilesToDel | sed -E "s/ /\n/g" | sort | uniq | awk '$0 != "" {print $0}' | xargs)
	rm -i $finalFilesToDel

### 19. 05-b-6800
Да се напише shell скрипт, който получава единствен аргумент директория и отпечатва списък с всички файлове и директории в нея (без скритите).
До името на всеки файл да седи размера му в байтове, а до името на всяка директория да седи броят на елементите в нея (общ брой на файловете и директориите, без скритите).
a) Добавете параметър -a, който указва на скрипта да проверява и скритите файлове и директории.

	Пример:
	$ ./list.sh .
	asdf.txt (250 bytes)
	Documents (15 entries)
	empty (0 entries)
	junk (1 entry)
	karh-pishtov.txt (8995979 bytes)
	scripts (10 entries)

	#! /bin/bash
	[ $# -eq 1 ] || [ $# -eq 2 ] || exit 1

	if [ "$2" != "-a" ]
	then
	        allFiles=$(find . -maxdepth 1 -type f,d | sed -E "s/\.\///g" | grep -v "^\." | awk '{print "./" $0}')
	else
	        allFiles=$(find . -maxdepth 1 -type f,d)
	fi

	for i in $allFiles
	do
	        [ -d "$i" ] && echo "$i ($(find $i -maxdepth 1 | sed -E "s/\.\///g" | grep -v "^\." | wc -l) entries)"
	        [ -f "$i" ] && echo "$i ($(stat -c "%s" $i) bytes)"
	done
	
### 20. 05-b-7000
Да се напише shell скрипт, който приема произволен брой аргументи - имена на файлове. Скриптът да прочита от стандартния вход символен низ и за всеки от зададените файлове извежда по подходящ начин на стандартния изход броя на редовете, които съдържат низа.
NB! Низът може да съдържа интервал.
	
	#! /bin/bash

	read toSearch
	for i in $@
	do
	        echo "$i - $(grep "$toSearch" $i | wc -l) rows contain $toSearch"
	done
### 21. 05-b-7100
Да се напише shell скрипт, който приема два параметъра - име на директория и число. Скриптът да извежда на стандартния изход имената на всички обикновени файлове във директорията, които имат размер, по-голям от подаденото число.

	#! /bin/bash

	[ $# -eq 2 ] || exit 1
	find $1 -type f -size +$2c

### 22. 05-b-7200
Да се напише shell скрипт, който приема произволен брой аргументи - имена на файлове или директории. Скриптът да извежда за всеки аргумент подходящо съобщение:
- дали е файл, който може да прочетем
- ако е директория - имената на файловете в нея, които имат размер, по-малък от броя на файловете в директорията.
- 
		#! /bin/bash
		for i in $@
		do
		        if [ ! -d $i ] && [ -r $i ]
		        then
		                echo "$i - readable"
		                continue
		        fi

		        if [ -d $i ]
		        then
		                entriesInDir=$(find $i -type f| wc -l)
		                find $i -type f -printf "%p %s\n" | awk -v helper=$entriesInDir '$2 < helper {print $1}'
		        fi
			done

### 23. 05-b-7500
Напишете shell script guess, която си намисля число, което вие трябва да познаeте. В зависимост от вашия отговор, програмата трябва да ви казва "надолу" или "нагоре", докато не познаeте числото. Когато го познаете, програмата да ви казва с колко опита сте успели.

		./guess (програмата си намисля 5)
		Guess? 22
		...smaller!
		Guess? 1
		...bigger!
		Guess? 4
		...bigger!
		Guess? 6
		...smaller!
		Guess? 5
		RIGHT! Guessed 5 in 5 tries!
		
Hint: Един начин да направите рандъм число е с $(( (RANDOM % b) + a )), което ще генерира число в интервала [a, b]. Може да вземете a и b като параметри, но не забравяйте да направите проверката.

	#! /bin/bash
	re='^[0-9]+$'
	[ $# -ne 2 ] && exit 1
	if  ! [[ $1 =~ $re ]]
	then
	        exit 2
	fi
	if ! [[ $2 =~ $re ]]
	then
	        exit 3
	fi
	guessNum=$(( (RANDOM % $2) + $1 ))
	counter=1
	echo "Guess?"
	read guess
	while [[ $guess -ne $guessNum ]]
	do
	        if [[ $guess -gt $guessNum ]]
	        then
	                echo "...smaller!"
	        fi

	        if [[ $guess -lt $guessNum ]]
	        then
	                echo "...bigger!"
	        fi

	        echo "Guess?"
	        read guess
	        counter=$(($counter + 1))
	done

	echo "RIGHT! Guessed $guessNum in $counter tries!"

### 24. 05-b-7550
Да се напише shell скрипт, който приема параметър - име на потребител. Скриптът да прекратява изпълненито на всички текущо работещи процеси на дадения потребител, и да извежда колко са били те. NB! Може да тествате по същият начин като описаният в 05-b-4300

	#! /usr/bin/bash

	[ $# -ne 1 ] && exit 1
	processes=$(ps --no-header -u $1 -U $1 -o pid)
	counter=0
	for i in $processes
	do
	        echo "Killing process $i..."
	        counter=$(($counter + 1))
	        kill -9 $i
	done
	echo "Killed $counter processes"

### 25. 05-b-7700
Да се напише shell скрипт, който приема два параметъра - име на директория и число. Скриптът да извежда сумата от размерите на файловете в директорията, които имат размер, по-голям от подаденото число.

	#! /usr/bin/bash
	re='^[0-9]+$'
	[ $# -ne 2 ] && exit 1
	if ! [[ $2 =~ $re ]]
	then
	        exit 2
	fi
	find $1 -type f -size +$2c -printf "%s\n" | awk '{sum+=$1} END {print sum}'

### 26. 05-b-7800
Да се напише shell скрипт, който намира броя на изпълнимите файлове в PATH.
Hint: Предполага се, че няма спейсове в имената на директориите
Hint2: Ако все пак искаме да се справим с този случай, да се разгледа IFS променливата и констуркцията while read -d

	#!/bin/bash
	DIRS=$(echo ${PATH} | sed -e 's/:/ /g')
	COUNT=0
	for i in ${DIRS}; do
	    COUNT=$(( COUNT+$(find $i -type f -executable | wc -l) ))
	done
	echo ${COUNT}

### 27. 05-b-8000
Напишете shell script, който получава като единствен аргумент име на потребител и за всеки негов процес изписва съобщение за съотношението на RSS към VSZ. Съобщенията да са сортирани, като процесите с най-много заета виртуална памет са най-отгоре.
Hint:
Понеже в Bash няма аритметика с плаваща запетая, за смятането на съотношението използвайте командата bc. За да сметнем нампример 24/7, можем да: echo "scale=2; 24/7" | bc
Резултатът е 3.42 и има 2 знака след десетичната точка, защото scale=2.
Алтернативно, при липса на bc ползвайте awk.

	#! /bin/bash
	[ $# -ne 1 ] && exit 1
	ps --no-header -u $1 -U $1 -o rss,vsz | awk '$2 > 0 && $1 > 0 {ratio=$1 / $2; print ratio}' | sort -rn

### 28. 05-b-9100
Опишете поредица от команди или напишете shell скрипт, които/който при известни две директории SOURCE и DESTINATION:
- намира уникалните "разширения" на всички файлове, намиращи се някъде под SOURCE. (За простота приемаме, че в имената на файловете може да се среща символът точка '.' максимум веднъж.)
- за всяко "разширение" създава по една поддиректория на DESTINATION със същото име
- разпределя спрямо "разширението" всички файлове от SOURCE в съответните поддиректории в DESTINATION

		#! /bin/bash
		[ $# -ne 2 ] && exit 1
		allExtensions=$(find $1 -type f | rev | cut -d "/" -f 1 | rev | cut -d "." -f 2 | sort | uniq)
		for i in $allExtensions
		do
		        mkdir $2/$i
		        find $1 -type f | grep "$i$" | xargs cp -t $2/$i
		done

### 29. 05-b-9200
Да се напише shell скрипт, който получава произволен брой аргументи файлове, които изтрива.
Ако бъде подадена празна директория, тя бива изтрита. Ако подадения файл е директория с поне 1 файл, тя не се изтрива.
За всеки изтрит файл (директория) скриптът добавя ред във log файл с подходящо съобщение.
а) Името на log файла да се чете от shell environment променлива, която сте конфигурирали във вашия .bashrc.
б) Добавете параметър -r на скрипта, който позволява да се изтриват непразни директории рекурсивно.
в) Добавете timestamp на log съобщенията във формата: 2018-05-01 22:51:36

Примери:
$ export RMLOG_FILE=~/logs/remove.log
$ ./rmlog -r f1 f2 f3 mydir/ emptydir/
$ cat $RMLOG_FILE
[2018-04-01 13:12:00] Removed file f1
[2018-04-01 13:12:00] Removed file f2
[2018-04-01 13:12:00] Removed file f3
[2018-04-01 13:12:00] Removed directory recursively mydir/
[2018-04-01 13:12:00] Removed directory emptydir/

	vi ~/.bashrc
	touch remove.log
	export LOG=remove.log
	#!/bin/bash
	ARG=0
	if [ $1 == "-r" ]; then
	    ARG=1
	    shift 1
	fi
	for i in $@; do
	    if [ -f $i ]; then
	        rm $i
	        echo "Removed file $i" >> $LOG
	    elif [ -d $i ]; then
	        if [ $ARG -eq 1 ]; then
	            rm -r $i
	            echo "Removed directory recursively $i" >> $LOG
	        else
	            if [ $(ls -A $i | wc -l) -eq 0 ]; then
	                rmdir $i
	                echo "Removed directory $i" >> $LOG
	            else
	                echo "Can't remove non-empty directory $i" >> $LOG
	            fi
	        fi
	    fi
	done

### 30. 05-b-9500.txt
(Цветно принтиране) Напишете shell script color_print, който взима два параметъра.
Първият може да е измежду "-r", "-g" "-b", а вторият е произволен string.
На командата "echo" може да се подаде код на цвят, който ще оцвети текста в определения цвят.
В зависимост от първия аргумент, изпринтете втория аргумен в определения цвят:
"-r" е червено. Кодът на червеното е '\033[0;31m' (echo -e "\033[0;31m This is red")
"-g" е зелено. Кодът на зеленото е '\033[0;32m' (echo -e "\033[0;32m This is green")
"-b" е синьо. Кодът на синьото е '\033[0;34m' (echo -e "\033[0;34m This is blue")
Ако е подадена друга буква изпишете "Unknown colour", а ако изобщо не е подаден аргумент за цвят, просто изпишете текста.
Hint:
В края на скрипта си напишете:
echo '\033[0m' ,за да не се прецакат цветовете на терминала. Това е цветът на "няма цвят".

	#!/bin/bash
	if [ $1 == "-r" ]; then
	    echo -e "\033[0;31m $2"
	elif [ $1 == "-g" ]; then
	    echo -e "\033[0;32m $2"
	elif [ $1 == "-b" ]; then
	    echo -e "\033[0;34m $2"
	fi
	echo -e '\033[0m'

### 31. 05-b-9501.txt
Този път програмата ви ще приема само един параметър, който е измежду ("-r", "-b", "-g", "-x").
Напишете shell script, който приема редовете от stdin и ги изпринтва всеки ред с редуващ се цвят. Цветовете вървят RED-GREEN-BLUE и цветът на първия ред се определя от аргумента.
Ако е подаден аргумент "-x", то не трябва да променяте цветовете в терминала (т.е., все едно сте извикали командата cat).
Hint: Не забравяйте да връщате цветовете в терминала.

	#!/bin/bash
	if [ $# -ne 1 ]; then
	    exit 1
	fi
	ROWS=$(cat /dev/stdin)
	if [ $1 == "-x" ]; then
	     echo -e "${ROWS}"
	     exit 0
	fi
	START=-1
	if [ $1 == "-r" ]; then
	    START=1
	elif [ $1 == "-b" ]; then
	    START=2
	elif [ $1 == "-g" ]; then
	    START=3
	fi
	echo "${ROWS}" | awk -v start=$START 'start==1 {print "\033[0;31m"$1""} start==2 {print "\033[0;34m"$1""}
	                start==3 {print "\033[0;32m"$1""; start=0} {start+=1}'
	echo -e '\033[0m'

### 32. 05-b-9600
Да се напише shell скрипт, който получава произволен брой аргументи файлове, които изтрива.
Ако бъде подадена празна директория, тя бива изтрита. Ако подадения файл е директория с поне 1 файл, тя не се изтрива.
Да се дефинира променлива BACKUP_DIR (или друго име), в която:
- изтритите файлове се компресират и запазват
- изтритите директории се архивират, компрeсират и запазват
- имената на файловете е "filename_yyyy-mm-dd-HH-MM-SS.{gz,tgz}", където filename е оригиналното име на файла (директорията) преди да бъде изтрит  (%y-%m-%d-%H-%M-%S)
а) Добавете параметър -r на скрипта, който позволява да се изтриват непразни директории рекурсивно и съответно да се запазят в BACKUP_DIR

		Примери:
		$ export BACKUP_DIR=~/.backup/
		# full-dir/ има файлове и не може да бъде изтрита без параметър -r
		$ ./trash f1 f2 full-dir/ empty-dir/
		error: full-dir/ is not empty, will not detele
		$ ls $BACKUP_DIR
		f1_2018-05-07-18-04-36.gz
		f2_2018-05-07-18-04-36.gz
		empty-dir_2018-05-07-18-04-36.tgz
		$ ./trash -r full-dir/
		$ ls $BACKUP_DIR
		f1_2018-05-07-18-04-36.gz
		f2_2018-05-07-18-04-36.gz
		full-dir_2018-05-07-18-04-50.tgz
		empty-dir_2018-05-07-18-04-36.tgz
		# можем да имаме няколко изтрити файла, които се казват по един и същ начин
		$ ./trash somedir/f1
		$ ls $BACKUP_DIR
		f1_2018-05-07-18-04-36.gz
		f1_2018-05-07-18-06-01.gz
		f2_2018-05-07-18-04-36.gz
		full-dir_2018-05-07-18-04-50.tgz
		empty-dir_2018-05-07-18-04-36.tgz

		#! /bin/bash
		rec=0
		if [ $1 == "-r" ]; then
		        rec=1
		        shift 1
		fi
		for i in $@
		do
		        newName=$(echo "$i-$(date +"%y-%m-%d-%H-%M-%S")")
		        if [ -f $i ]; then
		                gzip -c $i > $newName.gz
		                mv $newName.gz $BACKUP_DIR/
		                rm -i $i
		        elif [ -d $i ]; then
		                if [ $(find $i -maxdepth 0 -empty -exec echo 1 \; | wc -l) -eq 1 ]; then
		                        tar -zcf ${newName}.tgz $i
		                        mv ${newName}.tgz $BACKUP_DIR/
		                        rmdir $i
		                elif [ $rec -eq 1 ]; then
		                        tar -zcf ${newName}.tgz $i
		                        mv ${newName}.tgz $BACKUP_DIR/
		                        rm -r -i $i
		                else
		                        echo "error: $i is not empty, will not delete"
		                fi
		        fi
		done


