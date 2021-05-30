# Runlist, Multiple Recipes, Linux- Group & Users in CHEF
 

Prerequisite: https://bansalkushagra.medium.com/how-to-create-a-cookbook-and-recipe-in-chef-ae62e9ba8156

Step-1: Access your Amazon EC-2 machine and follow the below command: <br>
Ø sudo su <br>
Ø cd cookbooks <br>

![image](https://user-images.githubusercontent.com/46487696/120107711-bbc21c00-c17f-11eb-918d-134ccac94474.png)


Step-2: <br>
Let’s create a directory and a file in a Linux machine using the ruby script in chef and see the difference between the ruby script in the recipe and the Linux command run in the recipe. <br>
Directory & file name is kushagradirectory and kushagrafile <br>
First, create a recipe and enter a code <br>
Ø vi test-cookbook/recipes/recipe3.rb <br>

Code:

```
execute “run a script” do
command <<-EOH
mkdir /kushagradirectory
touch /kushagrafile
EOH
end
```
![image](https://user-images.githubusercontent.com/46487696/120107717-c1b7fd00-c17f-11eb-9c30-6fa7c6a89fb9.png)


Second, execute the recipe using below command <br>
Ø chef-client -zr “recipe[test-cookbook::recipe3]” <br>
Ø ls / <br>
In the output, we see the green color status which means the script is run successfully and directory & file has been created. <br>
This command will help you to see the entire root directory. <br>

![image](https://user-images.githubusercontent.com/46487696/120107807-0d6aa680-c180-11eb-9623-5de2fd0fefb5.png)

As we discussed earlier if we execute the same script again it first checks the current state stored in ohai and if the state of the node is the same it does not overwrite instead shows an output “up-to-date”. <br>
But, since here we are using the Linux command, here it executes the same script again and again. Follow the below output. <br>
![image](https://user-images.githubusercontent.com/46487696/120107815-178ca500-c180-11eb-81d2-6031eede60aa.png)


Step-3: <br>
Let’s create a user and group in Linux machine using chef <br>
First, open the recipe and continue adding the following code <br>

```
user “bansal” do
action :create
end
```

After running the script we see 2/2 resources updated which means earlier code used for creating directory and file as well as the present script for adding a user both has been executed.

![image](https://user-images.githubusercontent.com/46487696/120107820-1d828600-c180-11eb-8a35-ca99fbe33d25.png)

Second, now add a group by adding the code/script in continuation <br>

Code:

```
group “Kushagra_CHEF” do
action :create
members ‘bansal’
append true
end
```
![image](https://user-images.githubusercontent.com/46487696/120107823-23786700-c180-11eb-860c-5772aa33829c.png)

In the below output we see 2/3 resources update because the user we add was purely a ruby script and we know the ruby script does not overwrite. <br>

![image](https://user-images.githubusercontent.com/46487696/120107827-270bee00-c180-11eb-9064-a83fcc2ed9f9.png)

We can verify user and group name using the below command. <br>
Ø cat /etc/group <br>

Step-4: <br>
Running two recipe of two different cookbook <br>
Ø chef-client -zr “recipe[test-cookbook::recipe3],recipe[apache-cookbook::recipe3]” <br>

![image](https://user-images.githubusercontent.com/46487696/120107833-32f7b000-c180-11eb-9d23-0b3aa8ef244e.png)

Step-5: <br>
Running one or more recipes of a specific cookbook. Create multiple recipes inside the “test-cookbook”. Let say default.rb, recipe2.rb, recipe3.rb, recipe4.rb <br>
First, we can call one or more recipes inside any recipe we wanna run <br>
Ø vi /test-cookbook/recipes/default.rb <br>
here we are taking default.rb recipe <br>

![image](https://user-images.githubusercontent.com/46487696/120107838-3723cd80-c180-11eb-9109-ef1dce8fe51f.png)

Second, run the default recipe to execute multiple recipes from it. <br>
Ø chef-client -zr “recipe[test-cookbook::default]” <br>

![image](https://user-images.githubusercontent.com/46487696/120107847-3b4feb00-c180-11eb-80ba-06a34657f62b.png)

