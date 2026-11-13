=========================================
LAB 3: PROCESSES, JOBS, AND SCRIPTS
STUDENT: Malak Louabdia
=========================================

--- PART 0 & 1: SETUP & PROCESS LIFECYCLE ---
ps
ps aux
ps -ef

--- PART 2: PROCESS MONITORING (TOP & HTOP) ---
top
htop

--- PART 3: JOB CONTROL ---
sleep 300 &
sleep 400 &
jobs
fg %1
# (Ctrl+Z used to suspend process)
jobs
ps -o pid,stat,cmd -C sleep
bg %1
jobs
yes > /dev/null &
# (top used to check CPU usage)
kill %3
kill %1 %2
jobs

--- PART 4: AUTOMATION & SCRIPTS ---
name="Sara"
echo "Hello $name"
now=$(date +%H:%M)
echo "It is $now"

# greet.sh content & execution:
cat << 'EOF' > greet.sh
#!/bin/bash
name=$1
echo "Hello $name, it is $(date +%H:%M)"
EOF
chmod +x greet.sh
./greet.sh Sara

# Exit Status checks:
ls /etc > /dev/null
echo $?
ls /nonexistent
echo $?
pgrep -x nosuchproc
echo $?

# monitor.sh content & execution:
cat << 'EOF' > monitor.sh
#!/bin/bash
if [ $# -eq 0 ]; then
    echo "Usage: $0 process_name"
    exit 1
fi
name=$1
if pgrep -x "$name" > /dev/null; then
    echo "$name is running"
else
    echo "$name is NOT running"
fi
EOF
chmod +x monitor.sh
./monitor.sh
sleep 200 &
./monitor.sh sleep
kill %1
./monitor.sh sleep

--- PART 4.5: DISK & ARCHIVES ---
mkdir data
echo one > data/one.txt
echo two > data/two.txt
du -sh data
tar -czf backup.tar.gz data
tar -tzf backup.tar.gz
rm -r data
tar -xzf backup.tar.gz
ls data
df -h /