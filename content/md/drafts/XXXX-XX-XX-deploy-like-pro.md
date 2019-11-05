In the upcoming series of posts, I will highlight the tools I have been working on and use extensively in my day to day job.
All of them were built out of the necessity to have something simple and functional in a way I imagined serving the project at hand. 
I always search first for the available products, but none of them delivered the features or simplicity I wanted.

Introduce VMX
VMX is a tool which allows managing set of remote machines over SSH.
Motivation
Although initially it has been built to embedded the "read-only" SSH key in it, so to allow to distribute the tool among not technical people in the company, so they can check various activities on the servers themselves, but it didn't work well, and the project has been transformed to what it is now.
Be able to reuse the ~/.ssh/config
Distinguish between commands which don't modify the state of the machine and which do, and for the later ask for the confirmation before proceeding with the command execution
Simple familiar language to define the commands
Ability to define and group hosts following Ansible syntax https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#inventory-basics-hosts-and-groups
Support of the profiles, similar to the AWS CLI tool
Easy to share and distribute the config within the team
Ease of installation
Examples
In all examples below the profile could be specified, i.e. 
`vmx --profile <profile name>`
where all configuration is then put at ~/.vmx/<profile name>/ directory.
	2. I will use some fictional host group names: prod, rest, cache, db, etc., just for the demonstration purpose, although these group names are pretty common, the actual hosts to connect are 
defined in the ~/.vmx/hosts or ~/.vmx/<profile name>/hosts.
I will write the command first, and underneath will write the actual command definition as it might look like in the ~/.vmx/commands or ~/.vmx/<profile name>/commands, and will comment on the commands syntax when the new concept is introduced
Display and follow all services logs
`vmx run prod logs`

commands:
[logs]                                                                                                                                                                                                               
workingdir=/opt/app
command=docker-compose logs -f                                                                                                                                                                                       
follow=true

Explanation:
workingdir: defines the working directory it should switch to before execute the command
command: actual command to execute
follow: tells vmx to not close the stdout and terminate the command, but rather continue wait for the output. The command itself should be responsible for following the output, "follow" is the instruction to the vmx to how handle the output and manages in a way the lifecycle of the executed command

Display and follow logs for the particular service

`vmx run rest app-logs rest.log`

or the last N logs from each service running on multiple AWS EC2 instances, for example:

`vmx run dev logs --tail=100`

where `dev` is the group name for all dev services, and so `staging`, `prod`, etc. could be used instead.

commands:
[app-logs]                                                                                                                                                                                                           
command=tail -f -n 10 logs/%s                                                                                                                                                                                        
follow=true

Explanation:
Here workingdir is omitted, so either the command will be executed from the logged in user home directory, or use the value from the ~/.vmx/defaults or ~./vmx/<profile name>/defaults, where it might look like:
[all]                                                                                                                                                                                                                
workingdir=/opt/app                                                                                                                                                                                          
                                                     
or                                                                                                                                                                 
[rest]                                                                                                                                                                                                           
workingdir=$REST_HOME

or 
[rest*]
workingdir=/opt/app

Command can be parametrized by defining the placeholders %s (can be anywhere in the command string), and they will be replaced by the values provided after the command as "rest.log" in the example above (technically it is implemented as Go `fmt.Sprintf()`).


