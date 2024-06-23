Containers in Detail
While we all talk about Linux containers as a single entity, they are implemented
through several separate mechanisms that all work together: Control Groups
(cgroups)cgroups provide for resource limits,

Control Groups (cgroups)

Operations teams have often aimed for one server per intensive task. So, for example,
you don’t run your applications on the database server because they have competing
resource demands and their resource usage could grow unbounded and come to
dominate the server, starving neighbors of performance.

On real hardware systems, this could be quite expensive and so solutions like virtual
servers are very appealing, in part because you can share expensive hardware between
competing applications, and the virtualization layer would handle your resource par‐
titioning.
While it saves money, this is a reasonably expensive way to go about it if
you don’t need all the other separation provided by virtualization, because running
multiple kernels introduces a reasonable overhead on the applications.

Maintaining virtual machines is also not the cheapest solution.
All the same, cloud computing has
shown that it’s immensely powerful and with the right tooling, incredibly effective.
But if the only kind of isolation you needed was resource partitioning, wouldn’t it be
great if you could do that on the same kernel?

Control Groups, or cgroups for short, allow you to set limits on resources for pro‐
cesses and their children. This is the mechanism that Docker uses to control limits on
memory, swap, and CPU resources.
They are built in to the Linux kernel and origi nally shipped back in 2007 in Linux 2.6.24. The official kernel documentation defines
them as “a mechanism for aggregating/partitioning sets of tasks, and all their future
children, into hierarchical groups with specialized behaviour.” It’s important to note
that this setting applies to a process and all of the children that descend from it. That’s
exactly how containers are structured.

Every Docker container is assigned a cgroup that is unique to that container. All of
the processes in the container will be in the same group. This means that it’s easy to
control resources for each container as a whole without worrying about what might
be running. If a container is redeployed with new processes added, you can have
Docker assign the same policy and it will apply to all of them.

We talked previously about the cgroups hooks exposed by Docker via the Remote
API. This allows you to control memory, swap, and disk usage. But there are lots of
other things you can limit with cgroups, including the number of I/O operations per
second (iops) a container can have, for example. You might find that in your environ‐
ment you need to use some of these levers to keep your containers under control, and
there are a few ways you can go about doing that. cgroups by their nature need to do
a lot of accounting of resources used by each group. That means that when you’re
using them, the kernel has a lot of interesting statistics about how much CPU, RAM,
disk I/O, and so on. that your processes are using. So Docker uses cgroups not just to
limit resources but also to report on them. These are many of the metrics you see, for
example, in the output of docker stats.
