---
title: Requesting GPUs At Job Submission With Torque
layout: post
categories:
  - General Tips
tags:
  - gpgpu
  - gpu
  - high performance computing
  - hpc
  - Linux
  - torque
---
{% img center http://andyregan.net/blog/wp-content/uploads/2011/05/gpu_torque.jpg 'A picture of a GPU card' 'A picture of a GPU card' %}

According to the [Torque Documentation][1], **"in TORQUE 2.5.4 and later, users can request GPUs on a node at job submission"**. This 
post describes how to configure Torque for this functionality, allowing GPU resources to be scheduled among multiple users.

In a future post, I will also describe one approach that could be used to manage mutually exclusive CUDA access to NVIDIA GPUs on a 
multi-GPU compute node.

The examples that follow involve a single workstation with 2 NVIDIA GPU devices.

## Informing Torque Where the GPUs Are

In order for Torque to know which nodes have GPUs, you must update the [nodes][2] file. In the following example, I have specified 
that **node1** has 12 CPU cores and 2 GPU devices.

{% codeblock lang:bash %}
root@service-node:~# cat /var/spool/torque/server_priv/nodes
node1 np=12 gpus=2
{% endcodeblock %}

After saving the changes, you must restart **pbs_server** for the change to take affect.

{% codeblock lang:bash %}
root@service-node:~# /etc/init.d/pbs_server restart
{% endcodeblock %}

Looking at the **pbsnodes** output, we can see that Torque is now aware that there should be 2 GPU devices on **node1**.

{% codeblock lang:bash %}
user@login-node:~# pbsnodes -a
node1
     np = 12
     gpus = 2
{% endcodeblock %}

## Requesting GPUs at job submission

When submitting a job, a user can request GPU devices similar to how they request CPU cores. In the following example, 
I'm requesting an [interactive job][3] with 1 node , 1 processor per node and 2 GPUs per node.

{% codeblock lang:bash %}
user@login-node:~$ qsub -I -l nodes=1:ppn=1:gpus=2  
qsub: waiting for job <job id> to start  
qsub: job <job id> ready
{% endcodeblock %}

When the job starts, Torque populates a file whose path is specified by the environment variable `$PBS_GPUFILE`. Each 
allocated GPU appears on a separate line within this file.

As you can see, the contents of `$PBS_GPUFILE` show that the job has been allocated GPU index 0 and 1 (the first and 
second GPU) on node1 to execute on.

{% codeblock lang:bash %}
user@login-node:~$ env | grep PBS_GPUFILE
PBS_GPUFILE=/var/spool/torque/aux/JOB_IDgpu
{% endcodeblock %}

{% codeblock lang:bash %}
user@login-node:~$ cat /var/spool/torque/aux/JOB_IDgpu
node1-gpu0
node1-gpu1
{% endcodeblock %}

## Restricting the GPUs available to CUDA to Those Allocated

As specified in Torque's documentation, **"it is left up to the job's owner to make sure that the job executes properly on the GPU"**.

In my next post, I will describe how you can take advantage of the $CUDA\_VISIBLE\_DEVICES environment variable (available since CUDA 3.1) to restrict the GPUs available to CUDA to those allocated by Torque.

 [1]: http://www.adaptivecomputing.com/resources/docs/torque/3.7schedulinggpus.php
 [2]: http://www.adaptivecomputing.com/resources/docs/torque/1.5nodeconfig.php
 [3]: http://www.clusterresources.com/torquedocs21/users/2.2files.shtml#interactive
