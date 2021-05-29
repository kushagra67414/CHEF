CHEF is an open-source configuration management tool. Earlier system administrators used to manage the machines/servers like updating, installation, security, etc.<br>
By configuration we mean- each minute detail of our machine. Due to which chances of error increase because it’s done manually by an administrator.<br>
CHEF, an automated tool comes into existence.<br>
DevOps CHEF tool comes into an operation section. Now instead of the system administrator, DevOps Engineer will handle the server configuration process using script.<br>
Infrastructure builds/configured by a script is known as IAC (Infrastructure as code).<br>
It’s the Pull based management tool. Pull configuration nodes check the server periodically and fetch the configuration from it. <br>
Nodes itself sends a pull request to modify/update the specific configurations.<br>

**Advantage:** when new nodes get linked with the server, the node automatically sends the pull request to validate the updated configurations.<br>
CHEF is written in Ruby and Erlang. CHEF is used by Facebook, AWS ops work, etc. CHEF is an administrator tool that system admins used to do manually, <br>
now we can automate that task using this CHEF tool. Turns code into infrastructure. Code/script are repeatable, versionable, and testable.<br>


**Advantage of CM Tool =>**<br>
Complete Automate<br>
Increase uptime<br>
Improve Performance<br>
Ensure Compliance<br>
Prevent errors<br>
Reduce cost<br>

**Ohai =>** In this complete current configuration of our node is stored.

**Idempotency =>** Tracking the state of system resources to ensure that the change in configurations that are not required(Because of the same version) should not reapply, reducing the time.


**Components of Chef =>** <br>
1. Workstation (Where we write code): workstations are a personal computer or Virtual Server where all configuration code is created, tested, or changed.
DevOps Engineer modify/write the code in this workstation. Here, the Code is also known as a recipe whereas the Collection of the recipe is known as a cookbook. Workstation Communicate with the chef–server using a knife. A knife is a command-line tool that uploads the cookbook to the server.

**2. Chef-Server =>**
The Chef server act as an intermediate between the workstation and the node. All types of the cookbook are stored here. Plus, the Server can be hosted Locally or Remotely.

**3. Node=>**
Nodes are host systems that have to be configured. Ohia fetches the current version of the software of the node. Node communicates with the chef-server using a chef-client using a knife. As different nodes can have a different state of configurations so chef-client is installed at each node.
