Kernel Namespaces, User Namespaces

Inside each container, you see a filesystem, network interfaces, disks, and other
resources that all appear to be unique to the container despite sharing the kernel with
all the other processes on the system. The network interface on the actual machine,
for example, is a single shared resource. But to your container it looks like it has the
run of an entire network interface to itself. This is a really useful abstraction: it’s what
makes your container feel like a machine all by itself. The way this is implemented in
the kernel is with namespaces. Namespaces take a single global resource and make it
appear as a single owned resource to the container.
Rather than just having a single namespace, however, containers have a namespace
on each of the six types of resources that are currently namespaced in the kernel:
mounts, UTS, IPC, PID, network, and user namespaces. We’ll explain all of those in a
minute. But essentially when you talk about a container, you’re talking about a num‐
ber of different namespaces that Docker sets up on your behalf. So what do they all
do?
Mount namespaces
Docker uses these primarily to make your container look like it has its entire own
filesystem namespace. If you’ve ever used a chroot jail, this is its tougher cousin.
It looks a lot like a chroot jail but goes all the way down to the kernel so that even
mount and unmount system calls are namespaced. If you use docker exec or
nsenter to get into a container, you’ll see a filesystem rooted on “/”. But we know
that this isn’t the actual root partition of the system. It’s the mount namespaces
that make that possible.
UTS namespaces
Named for the kernel structure they namespace, and ultimately from the “Unix
Timesharing System,” UTS namespaces give your container its own hostname
and domain name. This is also used by older systems like NIS to identify which
domain a host belongs to. When you enter a container and see a hostname that is
not the same as the machine on which it runs, it’s this namespace that makes that
happen.
IPC namespaces
These isolate your container’s System V IPC and POSIX message queue systems
from those of the host. Some IPC mechanisms use filesystem resources like
named pipes, and those are covered by the mount namespace. The IPC name‐
space covers things like shared memory and semaphores that aren’t filesystem
resources but which really should not cross the container wall.
PID namespaces
We already showed you that you can see all of the processes in containers in the
Linux ps output on the host Docker server. But inside the container, processes
Containers in Detail
|
161
have a totally different PID. This is the PID namespace in action. A process has a
unique PID in each namespace to which it belongs. If you look in /proc inside a
container, or run ps, you will only see the processes inside the container’s PID
namespace.
Network namespaces
This is what allows your container to have its own network devices, ports, etc.
When you run docker ps and see the bound ports for your container, you are
seeing ports from both namespaces. Inside the container your nginx might be
bound to port 80, but that’s on the namespaced network interface. This name‐
space makes it possible to have what seems to be a completely separate network
stack for your container.
User namespaces
These provide isolation between the user and group IDs inside a container and
those on the Docker host. Earlier when we looked at ps output outside the con‐
tainer and inside and saw different user IDs, this is how that happened. A new
user inside a container is not a new user on the Docker host’s main namespace,
and vice versa. There are some subtleties here, though. For example, root in a
user namespace is not necessarily root on the main system. Some of this work is
reasonably new to the Linux kernel and there are concerns about security leak‐
age, which we’ll talk about in a bit.
So namespaces provide the visual, and in many cases functional, isolation that makes
a container look like a virtual machine even though it’s on the same kernel.
