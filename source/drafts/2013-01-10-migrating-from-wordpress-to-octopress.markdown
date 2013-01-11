---
layout: post
title: "Migrating from Wordpress to Octopress"
date: 2013-01-10 22:18
comments: true
categories: 
---

#TODO

* Dates of posts
* Powered by Octopress, dressed by Slash

## Exporting old blog posts

## Testing on Engine Yard's orchestra.io 

* https://my.orchestra.io/apps

## Testing with Vagrant & Chef

###Environment

* Ubuntu 12.04LTS
* Nginx
* Git for deployment

## Hosting on EC2 micro instances

* Hosted Chef

`knife ec2 server create -I ami-960f03e2 -x ubuntu --region 'eu-west-1' -Z 'eu-west-1a' -G webserver -N blog01 -f t1.micro -i ~/.ssh/aregan-aws.pem`
