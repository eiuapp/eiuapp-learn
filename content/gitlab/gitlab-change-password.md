---
title: "gitlab修改密码"
date: 2018-04-17T17:15:47+08:00
draft: false
tags: ["gitlab"]
categories: ["gitlab"]
subtitle: ""
descriptions: ""
bigimg:
gitment: true
---

gitlab修改密码

## env

- IP: 192.168.31.169
- OS: ubuntu-16.04
- gitlab: 10.6.4

## step

```
root@gitlab:~# sudo gitlab-rails console production
Warning: fuzzy message was ignored.
  : msgid '<strong>Removes</strong> source branch'
Warning: fuzzy message was ignored.
  : msgid 'A new branch will be created in your fork and a new merge request will be started.'
Warning: fuzzy message was ignored.
  : msgid 'A user with write access to the source branch selected this option'
Warning: fuzzy message was ignored.
  : msgid 'All features are enabled for blank projects, from templates, or when importing, but you can disable them afterward in the project settings.'
Warning: fuzzy message was ignored.
  : msgid 'Allow edits from maintainers.'
Warning: fuzzy message was ignored.
  : msgid 'Branches|Active'
Warning: fuzzy message was ignored.
  : msgid 'Branches|Active branches'
Warning: fuzzy message was ignored.
  : msgid 'Branches|All'
Warning: fuzzy message was ignored.
  : msgid 'Branches|Overview'
Warning: fuzzy message was ignored.
  : msgid 'Branches|Protected branches can be managed in %{project_settings_link}.'
Warning: fuzzy message was ignored.
  : msgid 'Branches|Show active branches'
Warning: fuzzy message was ignored.
  : msgid 'Branches|Show all branches'
Warning: fuzzy message was ignored.
  : msgid 'Branches|Show more active branches'
Warning: fuzzy message was ignored.
  : msgid 'Branches|Show more stale branches'
Warning: fuzzy message was ignored.
  : msgid 'Branches|Show overview of the branches'
Warning: fuzzy message was ignored.
  : msgid 'Branches|Show stale branches'
Warning: fuzzy message was ignored.
  : msgid 'Branches|Stale'
Warning: fuzzy message was ignored.
  : msgid 'Branches|Stale branches'
Warning: fuzzy message was ignored.
  : msgid 'ClusterIntegration|Learn more about security configuration'
Warning: fuzzy message was ignored.
  : msgid 'ClusterIntegration|Security'
Warning: fuzzy message was ignored.
  : msgid 'Contribution'
Warning: fuzzy message was ignored.
  : msgid 'Done'
Warning: fuzzy message was ignored.
  : msgid 'Failed'
Warning: fuzzy message was ignored.
  : msgid 'Forking in progress'
Warning: fuzzy message was ignored.
  : msgid 'If your HTTP repository is not publicly accessible, add authentication information to the URL: <code>https://username:password@gitlab.company.com/group/project.git</code>.'
Warning: fuzzy message was ignored.
  : msgid 'Import all repositories'
Warning: fuzzy message was ignored.
  : msgid 'Label'
Warning: fuzzy message was ignored.
  : msgid 'Labels can be applied to %{features}. Group labels are available for any project within the group.'
Warning: fuzzy message was ignored.
  : msgid 'Labels|Promote Label'
Warning: fuzzy message was ignored.
  : msgid 'Labels|Promote label %{labelTitle} to Group Label?'
Warning: fuzzy message was ignored.
  : msgid 'Milestones|Promote %{milestoneTitle} to group milestone?'
Warning: fuzzy message was ignored.
  : msgid 'Not available for private projects'
Warning: fuzzy message was ignored.
  : msgid 'Not available for protected branches'
Warning: fuzzy message was ignored.
  : msgid 'Personal Access Token'
Warning: fuzzy message was ignored.
  : msgid 'Pipelines|CI Lint'
Warning: fuzzy message was ignored.
  : msgid 'Pipelines|Clear Runner Caches'
Warning: fuzzy message was ignored.
  : msgid 'Pipelines|Loading Pipelines'
Warning: fuzzy message was ignored.
  : msgid 'Pipelines|Project cache successfully reset.'
Warning: fuzzy message was ignored.
  : msgid 'Pipelines|Run Pipeline'
Warning: fuzzy message was ignored.
  : msgid 'Pipelines|Something went wrong while cleaning runners cache.'
Warning: fuzzy message was ignored.
  : msgid 'Pipelines|There are currently no %{scope} pipelines.'
Warning: fuzzy message was ignored.
  : msgid 'Pipelines|There are currently no pipelines.'
Warning: fuzzy message was ignored.
  : msgid 'Pipelines|This project is not currently set up to run pipelines.'
Warning: fuzzy message was ignored.
  : msgid 'PrometheusService|Common metrics'
Warning: fuzzy message was ignored.
  : msgid 'Promote'
Warning: fuzzy message was ignored.
  : msgid 'Scheduled'
Warning: fuzzy message was ignored.
  : msgid 'Search'
Warning: fuzzy message was ignored.
  : msgid 'Started'
Warning: fuzzy message was ignored.
  : msgid 'You are going to remove %{project_full_name}. Removed project CANNOT be restored! Are you ABSOLUTELY sure?'
Warning: fuzzy message was ignored.
  : msgid 'You are going to transfer %{project_full_name} to another owner. Are you ABSOLUTELY sure?'
Warning: fuzzy message was ignored.
  : msgid 'You are on a read-only GitLab instance.'
Warning: fuzzy message was ignored.
  : msgid 'Your changes can be committed to %{branch_name} because a merge request is open.'
Warning: fuzzy message was ignored.
  : msgid 'mrWidget|Allows edits from maintainers'










Loading production environment (Rails 4.2.10)
irb(main):001:0>
irb(main):012:0> user=User.where(name: "tom").first
=> nil
irb(main):013:0> user.password=12345678
NoMethodError: undefined method `password=' for nil:NilClass
	from (irb):13
	from /opt/gitlab/embedded/lib/ruby/gems/2.3.0/gems/railties-4.2.10/lib/rails/commands/console.rb:110:in `start'
	from /opt/gitlab/embedded/lib/ruby/gems/2.3.0/gems/railties-4.2.10/lib/rails/commands/console.rb:9:in `start'
	from /opt/gitlab/embedded/lib/ruby/gems/2.3.0/gems/railties-4.2.10/lib/rails/commands/commands_tasks.rb:68:in `console'
	from /opt/gitlab/embedded/lib/ruby/gems/2.3.0/gems/railties-4.2.10/lib/rails/commands/commands_tasks.rb:39:in `run_command!'
	from /opt/gitlab/embedded/lib/ruby/gems/2.3.0/gems/railties-4.2.10/lib/rails/commands.rb:17:in `<top (required)>'
	from bin/rails:9:in `require'
	from bin/rails:9:in `<main>'
irb(main):014:0> user=User.where(name: "root").first
=> nil
irb(main):015:0> user=User.where(name: "tom").first
=> nil
irb(main):016:0> user = User.where(email: 'zengyl@jiaolongchuhai.com').first
=> #<User id:2 @tom>
irb(main):017:0> user.password = 'yourpassword'
=> "yourpassword"
irb(main):018:0> user.save!
Enqueued ActionMailer::DeliveryJob (Job ID: d7bca2e5-0408-4509-9933-624e67c47358) to Sidekiq(mailers) with arguments: "DeviseMailer", "password_change", "deliver_now", gid://gitlab/User/2
=> true
irb(main):019:0> user.save!
=> true
irb(main):020:0> exit
root@gitlab:~#
```

发现了吧, 

- 通过邮件,可以找到用户,从而修改密码, 
- 而通过用户名, 却找不到用户,从而修改不了密码.

## ref

- [gitlab忘记密码如何重置](http://blog.chinaunix.net/uid-17291169-id-4454012.html)