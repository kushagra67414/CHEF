# Roles and Its Importance in CHEF


Basic commands to List and delete cookbooks, roles, list of clients and nodes

1. To list cookbooks in chef-server <br>
Ø knife cookbook list <br>

2. To delete the cookbook from the chef-server <br>
Ø knife cookbook delete <cookbook_name> -y <br>

3. To list nodes in chef-server <br>
Ø knife node list <br>

4. To delete a node from chef-server <br>
Ø knife node delete <Node_name> -y <br>

5. To list Client in chef-server <br>
Ø knife client-list <br>

6. To delete clients from chef-server <br>
Ø knife client delete <client_name > -y <br>

7. To list roles in chef-server <br>
Ø knife role list <br>

8. To delete role form chef-server <br>
Ø Knife role delete <role_name> -y <br>

![image](https://user-images.githubusercontent.com/46487696/120618721-f7f3c600-c478-11eb-807e-c3f5ce50b43c.png)

It’s very important to learn bootstrapping which I had already published on the blog. You must go through it; without knowledge of it you can’t proceed.


**Let’s come to the implementation**

First, we will create a Role and will define a recipe that we want to run on the node. After that, role and cookbooks will update to chef-server and after creating an EC-2 instance node we will connect the node to Role run_list using bootstrap command and will automate the pull process such that the node can check after a minute if any update required or not from role’s we create.<br>

**Step-1:**

Right now this is my present ec-2 chef workstation which I had already created and downloaded the chef-workstation. <br>
First, create a cookbook and recipe inside it. <br>
**Cookbook name:** role_cookbook
**Recipe name:** role_recipe.rb

Go to chef-repo> cookbooks> create your cookbook and inside the cookbook create the recipe.

**Command:**

> chef generate cookbook role_cookbook

> chef generate recipe role_recipe

> vi recipes/role_recipe.rb

**Recipe code:**
```
package ‘httpd’ do
action :install
end
file ‘/var/www/html/index.html’ do
content ‘Automating the chef process using crontab scheduler’
action :create
end
service ‘httpd’ do
action [:enable, :start]
end
```

![image](https://user-images.githubusercontent.com/46487696/120619015-3b4e3480-c479-11eb-9048-94850111f734.png)

![image](https://user-images.githubusercontent.com/46487696/120619030-3e492500-c479-11eb-832f-95d71dfbaa83.png)

Now go to the chef-repo directory because it contains all the sever files and everything will be executed in the folder.

![image](https://user-images.githubusercontent.com/46487696/120619053-42754280-c479-11eb-9bdf-a11f3876eae4.png)


**Step-2:**

Now let’s create a role inside the Roles directory present in chef-server and upload it to the server.

Either you can check roles present in the server using CLI or through accessing the server(In Realtime we can’t access the server).

Command:

> knife role from file roles/devops2.rb

> Knife role list


Follow the below output

![image](https://user-images.githubusercontent.com/46487696/120619124-5325b880-c479-11eb-83d1-1b2f94bd1618.png)

![image](https://user-images.githubusercontent.com/46487696/120619136-55881280-c479-11eb-8bdc-e8a50f86071e.png)

![image](https://user-images.githubusercontent.com/46487696/120619148-58830300-c479-11eb-8dff-95e01c2ba5f6.png)


**Step-3:**

Now let’s create an Ec-2 instance node say “chef-node-6” and connect the node to the chef-server.

Remember: Availability Zone must be the same to prevent future errors. Here we have taken “ap-south-1b”

Add two protocol SSH and HTTP with source ANYWHERE

![image](https://user-images.githubusercontent.com/46487696/120619201-65075b80-c479-11eb-8c01-6c10683b4463.png)

![image](https://user-images.githubusercontent.com/46487696/120619219-6769b580-c479-11eb-9b6c-357353300c47.png)


At last, it will ask to choose key-pair if existing-key is present use it otherwise create new.

Never use the key of your workstation in which currently you are working.

And using WinSCP software move your node key from windows to the Linux server inside the chef-repo directory. (Follow the previous blog if don’t know how to do).

![image](https://user-images.githubusercontent.com/46487696/120619277-718bb400-c479-11eb-9f35-f243180a43f0.png)

Command to connect the node to chef-server:

> knife bootstrap <node_private_IP> — ssh-user ec2-user — sudo -i <instance_key> -N <Any_name_For_node> -y

> knife bootstrap 172.31.11.231 — ssh-user ec2-user — sudo -i Node-key.pem -N Node6 -y


To list node upload on server

> Knife node list

![image](https://user-images.githubusercontent.com/46487696/120620523-ad734900-c47a-11eb-8a78-740d09e1f8d2.png)

![image](https://user-images.githubusercontent.com/46487696/120620533-afd5a300-c47a-11eb-92ec-48c8147cf364.png)



**Step-4:**

Now, add a role to the runlist of the node and we will set a crontab scheduler in such a way that at every minute node will send a pull request to validate if any update is present at chef-server or not.

**Command to adding at runlist:**

> knife node run_list set Node6 “role[devops2]”

![image](https://user-images.githubusercontent.com/46487696/120619403-9122dc80-c479-11eb-979f-27cfc625e8dc.png)

> Knife node show Node6

![image](https://user-images.githubusercontent.com/46487696/120619440-997b1780-c479-11eb-9b42-3b3823511701.png)

Here in the above screenshot, Roles and Recipes values are still unknown.


**Step-5:**

Now, Upload the cookbook to the chef-server

![image](https://user-images.githubusercontent.com/46487696/120619512-aac42400-c479-11eb-90a0-f8fc02b562aa.png)

Command:
> Ø knife cookbook upload role_cookbook

![image](https://user-images.githubusercontent.com/46487696/120619530-aef04180-c479-11eb-9303-56dca52aefd1.png)


**Step-6:**

Open your node and schedule a crontab job to create a pull request to the chef-server client node.

![image](https://user-images.githubusercontent.com/46487696/120619578-ba436d00-c479-11eb-9597-422f32c7613d.png)

Now, Copy your public IP and run it through the browser.

![image](https://user-images.githubusercontent.com/46487696/120619605-bfa0b780-c479-11eb-85f9-62f199400eb6.png)

Roles and Recipes values that were unknown are updates now when nodes make a pull request.

![image](https://user-images.githubusercontent.com/46487696/120619621-c3ccd500-c479-11eb-994a-614ebffe86d6.png)



## Part-2

If, we want to implement more than 1 recipe present in our cookbook we can provide just the name of the cookbook in our role and upload it to the server.

**Original role:**

```
name “devops2”
description “web hosting”
run_list “recipe[role_cookbook::role_recipe]”
```
**New Role Code:**
```
name “devops2”
description “web hosting”
run_list “recipe[role_cookbook]”
```
Here we just only provide the cookbook name.

Now, Upload the updated role to the server by following the below command.

> Knife role from file roles/devops.rb


## Part-3:

Now, suppose instead of using a recipe(role_recipe), we want to work on a new recipe say role2.rb

Step-1: Create a new recipe say “role2”

Command:
> Go to role_cookbook directory

> Vi recipes/role2.rb

Code:
```
file ‘/basicinfo’ do
content “This is to get Attributes
HOSTNAME: #{node[‘hostname’]}
IPADDRESS: #{node[‘ipaddress’]}
CPU: #{node[‘cpu’][‘0’][‘mhz’]}
MEMORY: #{node[‘memory’][‘total’]}”
owner ‘root’
group ‘root’
action :create
end
```

Upload the updated cookbook to the server. Go to the chef-repo directory first.
Command:

> Knife cookbook upload role_cookbook

![image](https://user-images.githubusercontent.com/46487696/120619925-173f2300-c47a-11eb-91ac-b2fac12900f8.png)

Now, go to roles > devops2.rb and change the recipe name

**Code:**

```
name “devops2”
description “web hosting”
run_list “recipe[role_cookbook::role2]”
```

Upload the updated recipe to the server.

Command:
> Knife role from file roles/devops2.rb

![image](https://user-images.githubusercontent.com/46487696/120620107-42c20d80-c47a-11eb-8b25-2585c10cddf5.png)

Open the node i.e chef-node-6 and you will see the files have been created with the name “basicinfo” and if run an IP it will show an output too. That means both recipes worked well. <br>

![image](https://user-images.githubusercontent.com/46487696/120620162-4f466600-c47a-11eb-9458-e9cb2d137c03.png)

![image](https://user-images.githubusercontent.com/46487696/120620174-52d9ed00-c47a-11eb-8794-79e03a813bec.png)
