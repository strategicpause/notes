~~~
sudo damo start \
  --damos_access_rate 0 0 \
  --damos_sz_region 4K max \
  --damos_age 5s max \
  --damos_action pageout \
  <pid>
~~~

* `damos_access_rate`
	* min/max access rate of damos target regions (percent)
* `damos_sz_region`
	* min/max size of damos target regions.
* `damos_age`
	* min/max age of damos target regions (microseconds)
* `damos_action`
	* Action to apply to the target regions

### Run speccpu from damo
~~~
sudo damo start \
	--damos_access_rate 0 0 \
	--damos_sz_region 4K max \
	--damos_age 5s max \
	--damos_action pageout \
	$(runcpu --copies=1 --iterations=2 --config=default-linux-x86 505.mcf_r)                                                                                  
~~~

### Damo Scheme Format
~~~
<>		 <>			<>		<>			<>	 <>  <>
min     max     0       10      1s     max pageout
~~~


~~~
function getcpid() {
    cpids=`pgrep -P $1|xargs`
    for cpid in $cpids;
    do
        echo "$cpid"
        getcpid $cpid
    done
}
~~~

~~~
#!/bin/bash

CGROUP_PATH="/sys/fs/cgroup/user.slice/user-0.slice/session-c31.scope"

# Check if the cgroup.procs file exists
if [ ! -f "$CGROUP_PATH/cgroup.procs" ]; then
    echo "Error: cgroup.procs file not found."
    exit 1
fi

# Initial list of PIDs
prev_pids=$(cat "$CGROUP_PATH/cgroup.procs")

# Function to check for new PIDs
check_for_new_pids() {
    current_pids=$(cat "$CGROUP_PATH/cgroup.procs")

    new_pids=$(comm -13 <(echo "$prev_pids" | tr ' ' '\n' | sort) <(echo "$current_pids" | tr ' ' '\n' | sort))

    if [ -n "$new_pids" ]; then
        echo "New PIDs found: $new_pids"
        # Add your action here to be performed when a new PID appears
        # Example: perform_action "$new_pids"
    fi

    prev_pids="$current_pids"
}

# Run the check periodically
while true; do
    check_for_new_pids
    sleep 5  # Adjust the interval as needed
done
~~~

~~~
#!/bin/bash

CGROUP_PATH="/sys/fs/cgroup/unified/my_cgroup"

# Check if the cgroup.procs file exists
if [ ! -f "$CGROUP_PATH/cgroup.procs" ]; then
    echo "Error: cgroup.procs file not found."
    exit 1
fi

# Initial list of PIDs
prev_pids=$(cat "$CGROUP_PATH/cgroup.procs")

# Function to check for new PIDs
check_for_new_pids() {
    current_pids=$(cat "$CGROUP_PATH/cgroup.procs")

    new_pids=$(comm -13 <(echo "$prev_pids" | tr ' ' '\n' | sort) <(echo "$current_pids" | tr ' ' '\n' | sort))

    if [ -n "$new_pids" ]; then
        echo "New PIDs found: $new_pids"
        # Add your action here to be performed when a new PID appears
        # Example: perform_action "$new_pids"
    fi

    prev_pids="$current_pids"
}

# Run the check periodically
while true; do
    check_for_new_pids
    sleep 5  # Adjust the interval as needed
done
~~~