---
layout: post
title: "Automatically Prefix Your Commit Messages With A Jira Issue ID "
date: 2018-01-10 06:00:26 +0200
category: [Git, Command-Line]
featured-img: git
---
In my day-to-day work I'm using the command-line and therefore Git from the command-line a lot. Over the years I have customized my own software development environment to better fit my needs and to make me more productive. When working with Jira it's a habit to include the Jira issue id and the issue title in the branch name. This means a branch name typically is something like _feature/foo/ISSUE-123-issue-tile_. When working with Bamboo as well it`s nice to have the
Jira issue id as a prefix of all commit messages because this will allow Bamboo to collect all commits for specific build. However, adding the issue id in every commit message manually is a pain that's why I automated the process. 

My solution is an shell function because normaly I would use an alias or a function to commit to Git anyway. My function is called gcmj(short for **G**it **C**ommit **M**essage **J**ira), you can put it in your .bash_profile or whatever you put your aliases. This is how it looks:

{% highlight bash %}
function gcmj() { 
    id=$(gjira); 
    if [[ $? == 0 ]]; then 
        git commit -m $id": "$1; 
    else 
        echo "Error: Failed to retieve Jira Ticket Id from branch name!"; 
        return 1; 
    fi; 
}
{% endhighlight %}

Basically it calls a function which tries to parse a Jira issue id from the current branch name and prepends it to the Git commit message. If there is no issue id in the branch name the function will report an error and commit nothing. 
You can call it like this:
```
gcmj "This is my commit message"
```
And the commit message will be:
```
ISSUE-123: This is my commit message
```
The function to parse the issue id looks this way:
{% highlight bash %}
function gjira() { 
    git symbolic-ref --short -q HEAD  | grep -w -Eo "[A-Z0-9]{1,10}-?[A-Z0-9]+-\d+"; 
    if [[ $? != 0 ]]; 
        then echo "Error: Failed to retrieve Jira Ticket Id from branch name!"; 
        return 1;
    fi; 
}
{% endhighlight %}
I have also a function to prepend NO-TICKET to my commit messages. This function is called gcmn(short for **G**it **C**ommit **M**essage **N**o-Ticket):
{% highlight bash %}
function gcmn() { git commit -m "NO-TICKET: "$1; }
{% endhighlight %}


Reference: [Integrating with custom JIRA issue key](https://confluence.atlassian.com/display/STASHKB/Integrating+with+custom+JIRA+issue+key)
