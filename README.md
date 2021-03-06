ansible-sample-tdd
==================

Test Driven Development for Ansible  by ServerSpec. It's Sample

ServerSpec is test framework based on Ruby.

# Environment

```
$ ruby -v
ruby 1.9.3p194 (2012-04-20 revision 35410) [x86_64-darwin11.4.2]

$ gem list |grep serverspec
serverspec (0.13.5)

```

# Directory

```
.
├── README.md
├── hosts                        #use Ansible and Serverspec
├── site.yml                     #use Ansible and Serverspec
├── nginx.yml                    #(comment-out) incluted by site.yml
├── roles
│   ├── mariadb
│   │   ├── spec
│   │   │   └── mariadb_spec.rb
│   │   ├── tasks
│   │   │   └── main.yml
│   │   └── templates
│   │       └── mariadb.repo
│   └── nginx
│       ├── handlers
│       │   └── main.yml
│       ├── spec                 #use Serverspec
│       │   └── nginx_spec.rb
│       ├── tasks
│       │   └── main.yml
│       ├── templates
│       │   └── nginx.repo
│       └── vars
│           └── main.yml
├── Rakefile                     #use Serverspec
└── spec                         #use Serverspec 
    └── spec_helper.rb
```

# Run Playbook

**Please re-write Your target IP-Adress of Server -> hosts (default is 192.168.0.103 and 104)**

```
$ ansible-playbook site.yml -i hosts
```

#Test
## Serverspec with Ansible
Serverspec use this file.  (Rakefile understand Notation of Ansible.)  

* hosts  
hosts can use [group_name]  

```hosts
[server]
192.168.0.103
192.168.0.104
```

* site.yml  
site.yml can use ```include```  

```site.yml
- name: Ansible-Sample-TDD
  hosts: server
  user: root
  roles:
    - nginx
    - mariadb
```

## Run Test

```
$ rake -T
rake serverspec:Ansible-Sample-TDD  # Run serverspec for Ansible-Sample-TDD / Run serverspec for Ansible-Sample-TDD 

$ rake serverspec:Ansible-Sample-TDD
Run serverspec for Ansible-Sample-TDD to 192.168.0.103
/Users/Adr/.rvm/rubies/ruby-1.9.2-p320/bin/ruby -S rspec roles/mariadb/spec/mariadb_spec.rb roles/nginx/spec/nginx_spec.rb
...........

Finished in 0.40289 seconds
11 examples, 0 failures
Run serverspec for Ansible-Sample-TDD to 192.168.0.104
/Users/Adr/.rvm/rubies/ruby-1.9.2-p320/bin/ruby -S rspec roles/mariadb/spec/mariadb_spec.rb roles/nginx/spec/nginx_spec.rb
...........

Finished in 0.4004 seconds
11 examples, 0 failures
```

# TODO

* hard-coding some things in Rakefile (inventory-file, private-key) to Configfile??
* create gem =>  Gem is [here](http://rubygems.org/gems/ansible_spec)
