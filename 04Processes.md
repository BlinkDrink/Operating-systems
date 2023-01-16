### 1. 04-a-5000
Намерете командите на 10-те най-стари процеси в системата.

		ps -eo "%t %c" --sort etime | tail -n 10 | cut -d " " -f 2
		ps -eo cmd --sort etime | tail -n 10 -easier
### 2. 04-a-6000
Намерете PID и командата на процеса, който заема най-много виртуална памет в системата.

		ps -eo cmd,pid --sort vsz | tail -n 1
### 3. 04-a-6300
Изведете командата на най-стария процес

		ps -eo cmd --sort etime | tail -n 1
### 4. 04-b-5000
Намерете колко физическа памет заемат всички процеси на потребителската група root.

		 ps -G root -g root -o size | awk '{mySum += $1} END {print mySum}'
### 5. 04-b-6100
Изведете имената на потребителите, които имат поне 2 процеса, чиято команда е vim (независимо какви са аргументите й)

		ps -C vim -o user --sort user | uniq -c | sort -n -k 1,1 | awk '$1 >= 2 {print $2}'
### 6. 04-b-6200
Изведете имената на потребителите, които не са логнати в момента, но имат живи процеси

		ps -U $(echo "$(who | awk '{print $1}' | tr '\n' ',' | head -c -1)") -u $(echo "$(who | awk '{print $1}' | tr '\n' ',' | head -c -1)")  -N -o user | sort | uniq
### 7. 04-b-7000
Намерете колко физическа памет заемат осреднено всички процеси на потребителската група root. Внимавайте, когато групата няма нито един процес.

		ps -G root -g root -o size | tail -n +2 | awk '{sum+=$1} END { if (NR != 0) print sum/NR}'
### 8. 04-b-7000
Намерете всички PID и техните команди (без аргументите), които нямат tty, което ги управлява. Изведете списък само с командите без повторения.

		ps -t - -o comm | sort | uniq
### 9. 04-b-9000
Да се отпечатат PID на всички процеси, които имат повече деца от родителския си процес.

		ps --no-headers -o pid --ppid=$(echo "$(ps --no-headers -o ppid --pid=1 | tr -d '( )|(\n)|(\t)')") - gets the number of children of the parent of process with pid=1
		ps --no-headers -o ppid --pid=1 | tr -d '( )|(\n)|(\t)')" - gets the parent of process with pid=1
	
