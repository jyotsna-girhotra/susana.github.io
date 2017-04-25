---
layout: post
title:  "Jenkins Notes"
date:   2017-01-11 22:22:03 -0500
categories: jekyll update
---

A few things I came across while working with Jenkins for the first time.

### Docker, Jenkins and Permissions

If you're planning to have jenkins execute docker commands, you may encounter permissions issues. It may not be entirely obvious as you'll most likely see a message such as `Cannot connect to the Docker daemon. Is the docker daemon running on this host?`, even if docker is running.

To resolve this issue, first add Jenkins to the docker group. This will allow Jenkins to execute docker commands:

{% highlight shell %}
sudo usermod -aG docker jenkins
{% endhighlight %}

At this point you may find that Jenkins is able to run the docker commands successfully. However, if the problem persists (as it did in my case), restart the Jenkins service.

### Jenkinsfiles

If you're using Jenkins, it's worth it to install and use the Jenkins Pipeline plugin suite. Rather than using the (incredibly) clunky Jenkins UI, you can express your Jenkins pipeline as code. [Check it out](pipeline-plugin).

[pipeline-plugin]: https://jenkins.io/solutions/pipeline/
