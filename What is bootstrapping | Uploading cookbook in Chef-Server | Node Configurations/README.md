# What is bootstrapping | Uploading cookbook in Chef-Server | Node Configurations

Prerequisite: https://medium.com/nerd-for-tech/attributes-in-chef-a56073de90ea<br>
https://medium.com/nerd-for-tech/runlist-multiple-recipes-linux-group-users-in-chef-170afcb51fba<br>

Step-1: Access your Amazon EC-2 machine and follow the below command:<br>
Ø sudo su<br>
Ø cd cookbooks <br>

![image](https://user-images.githubusercontent.com/46487696/120357792-73436380-c323-11eb-8685-6c2e757a4e4e.png)

Step-2:<br>
Create an account of chef-server. Go to https://manage.chef.io/login, create an organization for your chef-server. After some time we will see “starter-kit” download it using this we can connect our chef-workstation with different nodes.<br>
Start-kit contains all the server files which are used to connect over the server. Follow the below output sequentially.<br>
First, Go to https://manage.chef.io/login> sign up, and verify your account<br>

![image](https://user-images.githubusercontent.com/46487696/120357813-776f8100-c323-11eb-83c0-50d10638fc7f.png)

Second, create an organization, a pop-up dialogue will appear click on proceed.<br>

![image](https://user-images.githubusercontent.com/46487696/120357830-7b9b9e80-c323-11eb-84ff-3b80e8417376.png)

![image](https://user-images.githubusercontent.com/46487696/120357845-7f2f2580-c323-11eb-801e-6eff38da3d65.png)

The final chef-manage webpage will look like this<br>
![image](https://user-images.githubusercontent.com/46487696/120357854-822a1600-c323-11eb-9866-4d20e40bb7a5.png)

Third, Download starter-kit(server files) > unzip and using WinSCP software transfer it to a Linux EC-2 machine.<br>
a. host name = value of public IPv4 DNS<br>
b. User name = Linux instance username i.e. “ec2-user”<br>
![image](https://user-images.githubusercontent.com/46487696/120357867-85bd9d00-c323-11eb-90bb-7a5695c62f2d.png)

After that, click on the advance option > authentication and provide the key of the EC-2 Linux instance.<br>

![image](https://user-images.githubusercontent.com/46487696/120357881-8a825100-c323-11eb-9ea1-bd137bcf61ac.png)

![image](https://user-images.githubusercontent.com/46487696/120357899-8d7d4180-c323-11eb-8b86-dece10466e4a.png)

Click on OK > login and we will redirect to the WinSCP application. Let’s go to the directory where we have to unzip the starter kit. Drag and drop the “chef-repo” directory left to right means local machine to Linux server.<br>
![image](https://user-images.githubusercontent.com/46487696/120357910-9110c880-c323-11eb-9421-11e556529026.png)
![image](https://user-images.githubusercontent.com/46487696/120357922-940bb900-c323-11eb-8924-8ca5ea70b393.png)

Fourth, Let’s Verify the file we shared in our Linux server.
![image](https://user-images.githubusercontent.com/46487696/120357940-98d06d00-c323-11eb-8bff-73a58d4432c1.png)
![image](https://user-images.githubusercontent.com/46487696/120357955-9d952100-c323-11eb-879a-8c38a66eb916.png)


To verify that our Linux workstation has been connected to the chef-server use the following command.<br>
Ø Knife SSL check<br>

![image](https://user-images.githubusercontent.com/46487696/120357974-a2f26b80-c323-11eb-893b-3d95eaa8f793.png)

**Step-3:**<br>
Bootstrap a Node =><br>
Attaching a node to chef-server is called bootstrapping while the workstation and nodes are in the same availability zone.<br>
Now, to connect chef-server to the node first redirect to the chef-repo directory where all server files are present.<br>
First, Create an Ec-2 instance (chef-node-1)<br>

![image](https://user-images.githubusercontent.com/46487696/120358018-b0a7f100-c323-11eb-80eb-0759fb17aaa9.png)

Second, At the time of creating an instance, we downloaded a key-pair of the instance with a .pem extension in our windows local machine.<br>
Use WinSCP machine to transfer it to our Linux workstation<br>

![image](https://user-images.githubusercontent.com/46487696/120358040-b56ca500-c323-11eb-91ae-d6cac0789e88.png)

Let's verify the key-pair in our Linux instance(Workstation)<br>
Ø sudo su<br>
Ø cd chef-repo<br>
Ø ls<br>

![image](https://user-images.githubusercontent.com/46487696/120358052-b8679580-c323-11eb-821d-100c7044addb.png)

Third, Link the node with the workstation<br>
Command:<br>
Ø knife bootstrap 172.31.3.71 — connection-user ec2-user — sudo -i Node-key.pem -N Node1

![image](https://user-images.githubusercontent.com/46487696/120358009-ad146a00-c323-11eb-907e-ea682ac241a0.png)
![image](https://user-images.githubusercontent.com/46487696/120358158-d503cd80-c323-11eb-840b-de2984fc3ee7.png)
![image](https://user-images.githubusercontent.com/46487696/120358173-d8975480-c323-11eb-8cdd-5c43eeecf192.png)


**Step-4:**<br>
Now, There are two cookbooks in our workstation and we require only one cookbooks directory which currently exists in the “chef-repo” folder.<br>
To move the existing cookbook to the chef-repo/cookbooks/ and delete the present cookbook i.e. cookbooks.<br>

Command:<br>
Ø ls<br>
Ø ls cookbooks<br><br>
Ø ls chef-repo/cookbooks<br>
Ø mv cookbooks/test-cookbook chef-repo/cookbooks<br>
Ø mv cookbooks/apache-cookbook chef-repo/cookbooks<br>
Ø rm –rf cookbooks/<br>

![image](https://user-images.githubusercontent.com/46487696/120358230-e64cda00-c323-11eb-847c-f3231d487543.png)

Step-5:<br>
Let’s upload one of our cookbooks say “apache-cookbook” into chef-server<br>
Command:<br>
Ø knife cookbook upload apache-cookbook<br>
Ø knife cookbook list<br>
![image](https://user-images.githubusercontent.com/46487696/120358243-e9e06100-c323-11eb-930d-359380b81303.png)

![image](https://user-images.githubusercontent.com/46487696/120358255-ec42bb00-c323-11eb-8948-f54fc5d172cd.png)

Second, How to add recipe to run_list of a particular node<br>
Ø knife node run_list set Node1 “recipe[apache-cookbook::apache-recipe]”<br>

![image](https://user-images.githubusercontent.com/46487696/120358271-ef3dab80-c323-11eb-99f4-5233eb123b89.png)

To check existing recipes added to run_list<br>
Ø knife node show <node_name><br>
Ø knife node show Node1<br>

![image](https://user-images.githubusercontent.com/46487696/120358281-f2d13280-c323-11eb-8257-f1d190355561.png)

Third, To check this run_list graphically go to chef-server>Nodes> Node1> edit runlist.<br>
![image](https://user-images.githubusercontent.com/46487696/120358311-fb296d80-c323-11eb-847f-1ffe48271774.png)

Step-6:<br>
In the above step, we have updated/uploaded the cookbook (apache-cookbook) to chef-server. Since here we are not using automation we will manually go to node (Node1) and run command chef-client. It will help to run the script which is added as a runlist.<br>

Go to Node1 and follow the below command.<br>
Ø Sudo su // root<br>
Ø chef-client // run the recipe from parent chef-server<br>

![image](https://user-images.githubusercontent.com/46487696/120358335-011f4e80-c324-11eb-9fcb-f294720f1fa5.png)

![image](https://user-images.githubusercontent.com/46487696/120358352-054b6c00-c324-11eb-8aa0-c24fba950020.png)

Now, let’s automate the process from workstation to chef-server to node/nodes<br>

Let’s do some changes to our recipe (apache-recipe.rb) on our workstation.<br>
Ø cd chef-repo<br>
Ø vi cookbooks/apache-cookbook/recipes/apache-recipe.rb<br>

![image](https://user-images.githubusercontent.com/46487696/120358370-09778980-c324-11eb-8d31-c85d41ebed24.png)

Go to the node and try running the update runlist recipe. It will show output as up-to-date.<br>
![image](https://user-images.githubusercontent.com/46487696/120358383-0c727a00-c324-11eb-8d00-8134cdfe268b.png)

This means after modifying the script we have to upload it to the chef-server again.<br>
If we upload the recipe to the chef-server and then try running it manually on the node, we can see the changes.<br>

Workstation:<br>

![image](https://user-images.githubusercontent.com/46487696/120358403-11372e00-c324-11eb-8560-1540dcec9a5a.png)

Node:<br>
![image](https://user-images.githubusercontent.com/46487696/120358415-14321e80-c324-11eb-834a-21e563bfd771.png)

Step-7:<br>
Now let’s automate the process. If we change in a script node will automatically call the chef-server and update it accordingly. Here we will use the crontab scheduler to automate the process.<br>
Go to node1 and set the scheduler.<br>
Ø Sudo su<br>
Ø vi /etc/crontab<br>

```
Code:
* * * * * root chef-client
// It means this command with root permissions will run every minute.
```
![image](https://user-images.githubusercontent.com/46487696/120358438-1ac09600-c324-11eb-898b-3f87b218c3d2.png)

Come back to the workstation and do some modification to the recipe(apache-recipe.rb)
![image](https://user-images.githubusercontent.com/46487696/120358450-1dbb8680-c324-11eb-8ef0-5fa708bba9c2.png)
![image](https://user-images.githubusercontent.com/46487696/120358465-214f0d80-c324-11eb-9552-824ee6aeb3ab.png)

Also, the Modified recipe must be uploaded to the chef-server, and after that refresh it for a minute and we can see the node has called the chef server.<br>
Ø knife cookbook upload apache-cookbook<br>
Output:<br>
```
[root@ip-172–31–13–9 cookbooks]# knife cookbook upload apache-cookbook
Uploading apache-cookbook [0.1.0]
Uploaded 1 cookbook.
```
![image](https://user-images.githubusercontent.com/46487696/120357754-6888ce80-c323-11eb-8d10-018827eceaae.png)
