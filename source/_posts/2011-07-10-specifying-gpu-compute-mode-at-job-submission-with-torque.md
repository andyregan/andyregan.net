---
title: Specifying GPU Compute Mode At Job Submission with Torque
description: >
  This post explains how to specify compute modes for nvidia GPUs at Torque job
  submission.
layout: post
categories:
  - General Tips
  - Linux
---
In Torque 2.5.6, 3.0.2 and later, you can specify GPU compute mode and reset GPU ECC error counts at job submission 
(provided that you have configured Torque with the **enable-nvidia-gpus** option).

For CUDA 4.0 (Nvidia 270.x drivers), there are four compute modes that can be set on the GPU.

1.  **Default** : Multiple threads can run on this GPU
2.  **Exclusive Thread** : Only one thread in one process can run on this GPU
3.  **Prohibited** : No threads are allowed to run on this GPU
4.  **Exclusive Process** : Many threads in one process will be able to run on this GPU

With Torque, you can now request **Default**, **Exclusive Thread** or **Exclusive Process** compute modes at job
submission time. The mode you specify applies to all the GPUs you request. If you do not specify a compute mode,
then Torque will set the GPUs to **Exclusive Thread**.

For example, to request 1 GPU and with compute mode set to **Exclusive Process** you could define the following resource:

{% codeblock lang:bash %}
$ qsub -l nodes=1:gpus=1:exclusive_process
{% endcodeblock %}

To request 1 GPU with compute mode set to **Exclusive Thread** and reset the ECC error counts on the GPU, you could define the following:

{% codeblock lang:bash %}
$ qsub -l nodes=1:gpus=1:reseterr:exclusive_thread
{% endcodeblock %}

The below table shows the various modes and their associated Torque keywords.

| Compute Mode      | Keyword           
| ------------      | -------           
| Default           | shared            
| Exclusive Thread  | exclusive_thread  
| Exclusive Process | exclusive_process 

More information is available in [Section 3.7 of the Torque Documentation][1].

Al Taufer from Adaptive Computing helpfully explained the reasoning behind the *shared* keyword in [reply to a question I submitted to the Torque Users mailing list][2].

 [1]: http://www.adaptivecomputing.com/resources/docs/torque/3.7schedulinggpus.php
 [2]: http://www.supercluster.org/pipermail/torqueusers/2011-June/013079.html
