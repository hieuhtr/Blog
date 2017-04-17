# Why we need patching linux kernel?
- Kernel patches are released for a number of reasons, but fixing security holes is the most frequent reason. This is why it's important to install the patch as soon as possible.
- Unlike other operating systems, Linux is able to update many different parts of the system without a reboot, but the kernel is different. Every running process integrates with the kernel intimately, so switching out parts of the kernel while it is running is quite risky. So, When the kernel is updated via a patch, the system needs to `reboot`

# "No Reboot" Kernel patching
- On the other hand, rebooting the computer is irksome, and in some cases, where uptime is important, it can be a real issue. This is why "no reboot" kernel patching has been a priority for many administrators.
- Some servers and critical real-time applications must not be taken down without advanced scheduling, even for a few minutes. This can be a pain when administrators need to keep the system secure and a patch is released to repair a newly discovered security hole. In this case, no-reboot patching becomes a real boon
- Recognizing this need, **Red Hat** has been working on `kpatch`, and `SUSE` has been working on `kGraft`. Both of these programs are designed to accomplish the same task, but they take a different approach and have different strengths.
- But this doesn't mean that system reboots are gone forever. Even on a system with the Linux 4.0 kernel, there will be security updates that still require a reboot, because there are other non-kernel components that can require patching, and some of these require a reboot as part of the process.

# How Kpatch works?
- **kpatch** is a Linux dynamic kernel patching infrastructure which allows you to patch a running kernel without rebooting or restarting any processes.
- **Kpatch** freezes every process and then reroutes system calls from the old kernel functions to the new, patched functions, before removing the old code. Because it handles every running process in one sweeping move, it runs quite fast - one to forty milliseconds and it's done. However, during this time the processes are frozen, `which means there is some downtime` - a mere fraction of a second, but in certain situations, that may be unacceptable.

Most important: kpatch ensures that hot patches are applied atomically and safely by `stopping all running processes` while the hot patch is applied, and by ensuring that none of the stopped processes is running inside the functions that are to be patched

![With live patching in place, calls to patched kernel functions invoke their replacement counterparts](2017/image/Linux_kernel_live_patching_kpatch.png)

# How about kGraft, how it work?
- **kGraft**, on the other hand, handles each thread one by one, as they make system calls (without forcing them to freeze first) until all of the threads are running the patched code. At this point, the patch is fully installed and the old code is replaced. This process takes longer to complete the patch, but it does it without any downtime


Reference: 
1. https://en.wikipedia.org/wiki/Kpatch
2. http://www.linuxjournal.com/content/no-reboot-kernel-patching-and-why-you-should-care
3. https://github.com/dynup/kpatch

--
***Hieu Huynh*** April 17, 2017.
