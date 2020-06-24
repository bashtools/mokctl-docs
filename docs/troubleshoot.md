# Trouble-shooting Tips

#### `mokctl create ...` doesn't work

Some troubleshooting steps:

* Make sure Cgroups2 is disabled.
  
  Run:
  
  `sudo grubby --update-kernel=ALL --args="systemd.unified_cgroup_hierarchy=0"`
  
  Then reboot the machine and try again. This might also need to be done after kernel upgrades (`cat /proc/cmdline` should have the 'args=systemd...' setting shown).

* Try deleting the cluster and trying again!

* It's something else. Use `--tailf` to troubleshoot, for example:
  
  `mokctl create cluster myclust --masters 1 --tailf`
  
  Something shown might stand out.

* It still doesn't work.
  
  Try with `docker` and `podman` - one might work. If both are installed `mokctl` will choose `docker`.

* Try increasing the number of inotify watchers, for example:
  
  `echo "256"> /proc/sys/fs/inotify/max_user_instances`.
  
  Example:
  
  For a 1 master and 10 worker cluster only 6 started. Looking at worker-7 logs I saw:
  
  `crio[223]: time="2020-06-07 13:52:54.247657748Z" level=fatal msg="Too many open files"`
  
  Instead of increasing max_user_instances, or the system fs.file-max, I stopped OwnCloud and tried again.
  
  The next time I only got 3 worker nodes! Looking at `kubectl get pods -A` output I could see pods were 'Evicted'. There was less than 10% disk space free.
  
  I deleted a bunch of docker images and tried again. I got 6 worker nodes, and on the 7th crio said:
  `... could not create new watcher too many open files`
  
  So I did:
  
  ```bash
  cat /proc/sys/fs/inotify/max_user_instances
  # "128"
  sudo su
  echo "256">/proc/sys/fs/inotify/max_user_instances
  exit
  ```
  
  Then all 10 worker nodes worked without error.

* Disable the firewall temporarily to see if it fixes the problem.

* Disable selinux to see if it fixes the problem.

* Docker must be run as root, not in user mode as it requires root user privileges.

#### `mokctl build image` doesn't work

Try to:

* Run the build with `--get-prebuilt-image`.
  
  To download a prebuilt image.

* Run the build with `--tailf`.
  
  To watch the log as it's running.

* Errors are often due to network problems.
