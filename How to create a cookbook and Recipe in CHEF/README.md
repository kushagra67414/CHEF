What’s Inside Cookbook? <br>
A. Chefbook => Like .gitignore <br>
B. Kitchen.yml => For testing Cookbook <br>
C. Metadata.rb => name, version author name etc info of the cookbook. <br>
D. Readme.md => information about usage of cookbook <br>
E. Recipe => where we write code <br>
F. Spec => It used for unit test <br>
G. Test => It is used for integration test <br>


Implementation =>

Step-1:
Create ac EC2 instance of Linux, update the instance and download the chef. <br>
Follow the below command <br>
Ø Yum update –y
Ø Wget https://packages.chef.io/files/stable/chef-workstation/21.4.365/el/7/chef-workstation-21.4.365-1.el7.x86_64.rpm

 ![image](https://user-images.githubusercontent.com/46487696/120080723-de005f00-c0d7-11eb-8a3a-4746905bf0eb.png)


Step-2: <br>
.rpm file of chef-workstation will get the download. Install the fill. <br>
Follow the below command => <br>
Ø yum install chef-workstation-21.4.365–1.el7.x86_64.rpm

![image](https://user-images.githubusercontent.com/46487696/120080746-f2445c00-c0d7-11eb-8db7-87e0f0403c5b.png)


Step-3: <br>
To verify the installation must check the version of the chef. <br>
Command: <br>
Ø which chef
Ø chef –v

![image](https://user-images.githubusercontent.com/46487696/120080749-f7091000-c0d7-11eb-92a3-605b57aced77.png)


Step-4: <br>
Create a cookbook directory where all type of cookbook data will be store. <br>

Command: <br>
Ø mkdir cookbooks <br>
Ø cd cookbooks <br>

Let us create a specific cookbook here: <br>
Ø chef generate cookbook <Cookbook_Name> <br>
Ø chef generate cookbook test-cookbook <br>

![image](https://user-images.githubusercontent.com/46487696/120080756-fff9e180-c0d7-11eb-8cce-db2d126bb8d5.png)


Step-5: <br>
Let see a tree structure of our system. <br>
The tree is used to recursively list or display the content of a directory in a tree-like format. It outputs the directory paths and files in each sub-directory and a summary of a total number of sub-directories and files. <br>
Command: <br>
Ø yum install tree –y <br>
Ø tree <br>

![image](https://user-images.githubusercontent.com/46487696/120080763-04be9580-c0d8-11eb-8403-aa2bfa85f14e.png)

![image](https://user-images.githubusercontent.com/46487696/120080766-07b98600-c0d8-11eb-9ebc-281f997c081e.png)

Step-6: <br>
Creating a Recipe for our cookbook. <br>
 <br>
Command:
Go to the cookbook directory i.e. test-cookbook and follow the below command. <br>
Command: <br>
Ø cd test-cookbook/ <br>
Ø chef generate recipe <recipe_Name> <br>
Ø chef generate recipe test-recipe <br>

![image](https://user-images.githubusercontent.com/46487696/120080782-0ee09400-c0d8-11eb-9745-75bfa28b8313.png)


Step-7: <br>
Let’s modify a recipe. <br>
Either go to directory i.e cookbooks>test-cookbook>recipes and use the following command <br>
Ø vi test-recipe.rb <br>
OR <br>
Ø vi cookbooks/test-cookbook/recipes/test-recipe.rb <br>
.rb extension is mandatory without it recipe will not be created. <br>
Write the following code in the text editor and exit using the “ESC-:wq” command <br>
Code: <br>
```
file ‘/myfile’ do
content ‘Welcome to Technical Guftgu’
action :create
end
```

![image](https://user-images.githubusercontent.com/46487696/120080792-14d67500-c0d8-11eb-918b-8641bdb3856b.png)


Step-8: <br>
Let’s execute our recipe. It first calls ohai if there’s any update in the configurations file it will update the ohai otherwise it will show an output “up to date” <br>
Command: <br>
Ø chef-client –zr “recipe[<Cookbook_name>::<Recipe_Name>]” <br>
Ø chef-client –zr “recipe[test-cookbook::test-recipe]” <br>

![image](https://user-images.githubusercontent.com/46487696/120080812-23249100-c0d8-11eb-8685-803ccb8df003.png)

If we again execute the recipe it will show an output “up to date” because it does not overwrite any configurations. <br>

![image](https://user-images.githubusercontent.com/46487696/120080815-26b81800-c0d8-11eb-8ef3-57361936a8e3.png)


Step-9: <br>
Let's update our recipe. <br>
Go to the test-cookbook directory <br>
Ø cd cookbooks > test-cookbook > recipes <br>
Ø vi test-recipe.rb <br>
OR, <br>
Go to the home directory <br>
Ø vi cookbooks/test-cookbook/recipes/recipe2.rb <br>
Write the following code <br>

![image](https://user-images.githubusercontent.com/46487696/120080821-2cadf900-c0d8-11eb-9ee8-101edee26904.png)


.rb extension is mandatory <br>
Execute the second updated recipe i.e. test-recipe <br>
Ø chef-client –zr “recipe[test-cookbook::test-recipe]” <br>

![image](https://user-images.githubusercontent.com/46487696/120080827-320b4380-c0d8-11eb-8c67-d4407019b810.png)


## Hosting a Website using Chef

Step-1: <br>
Create a directory where all types of cookbook directories will be store. <br>
Ø Mkdir cookbooks <br>
Ø Cd cookbooks <br>
Lets create a cookbook <br>
Ø chef generate cookbook <Cookbook_Name> <br>
Ø chef generate cookbook apache-cookbook <br>

![image](https://user-images.githubusercontent.com/46487696/120080885-67179600-c0d8-11eb-9ade-7c4dc13f5055.png)


Let’s create a recipe <br>
Ø chef generate recipe <Recipe_name> <br>
Ø chef generate recipe apache-recipe <br>

![image](https://user-images.githubusercontent.com/46487696/120080892-6aab1d00-c0d8-11eb-9afe-9feda7476c1d.png)


Write the following code in the recipe <br>
Ø vi recipes/apache-recipe.rb <br>
Code: <br>
```
package ‘httpd’ do
action :install
end
file ‘/var/www/html/index.html’ do
content ‘Hosting a Website using Chef’
action :create
end
service ‘httpd’ do
action [:enable, :start]
end
```

Step-2: <br>
Execute the recipe to host a website <br>
Ø chef-client -zr “recipe[apache-cookbook::apache-recipe]” <br>

![image](https://user-images.githubusercontent.com/46487696/120080904-75fe4880-c0d8-11eb-8e7b-fc280b670756.png)


Output: <br>
![image](https://user-images.githubusercontent.com/46487696/120080910-78f93900-c0d8-11eb-9af7-a6ca3bc6e171.png)
