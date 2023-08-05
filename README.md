<h1>Creating custom Inspec Profile</h1>


<h2>Description</h2>
This lab focuses on creating custom InSpec profiles, allowing participants to define and implement specific compliance and configuration requirements tailored to their organization's needs. 
<br />


<h2>Languages and Utilities Used</h2>

- <b>Inspec</b>
- <b>JSON</b>
- <b>GitLab</b>

<h2>Environments Used </h2>

- <b>Windows 11</b>
- <b>Linux</b> 

<h2>Program walk-through:</h2>

Install the InSpec package via script: <br/>
```
curl https://omnitruck.chef.io/install.sh | sudo bash -s -- -P inspec
``` 
<p align="center">
<img src="https://i.imgur.com/qEqR86b.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

We have successfully installed Inspec tool, let’s explore the functionality it provides us: <br/>
```
inspec --help
``` 
<p align="center">
<img src="https://i.imgur.com/YcUuX6u.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Create a new folder and cd into that folder: <br/>
```
mkdir inspec-profile && cd inspec-profile
``` 
<p align="center">
<img src="https://i.imgur.com/1CiA3P7.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Create the Ubuntu profile: <br/>
```
inspec init profile ubuntu --chef-license accept
``` 
<p align="center">
<img src="https://i.imgur.com/cih1QId.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Run the following command to append the inspec task to the file at ubuntu/controls/example.rb. If you wish, you can edit the file using nano or any text editor: <br/>
 
<p align="center">
<img src="https://i.imgur.com/NR0FSU0.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Let’s validate the profile to make sure there are no syntax errors: <br/>
```
inspec check ubuntu
``` 
<p align="center">
<img src="https://i.imgur.com/ZrrY53Q.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Now run the profile on the local-machine before executing on the server: <br/>
```
inspec exec ubuntu
``` 
<p align="center">
<img src="https://i.imgur.com/BbL9kFQ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Let’s try to run the custom profile created by us against the server. Before executing the profile we need to execute the below command to avoid being prompted with Yes or No when connecting to a server via ssh: <br/>
```
echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config
``` 
<p align="center">
<img src="https://i.imgur.com/5Xxt6uI.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Let’s run inspec with the following options: <br/>
```
inspec exec ubuntu -t ssh://root@prod-rcgsg0ei -i ~/.ssh/id_rsa --chef-license accept
``` 
<p align="center">
<img src="https://i.imgur.com/BRYOsIy.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Use inspec init to create a new inspec profile with name challenge under /inspec-profile directory: <br/>
```
mkdir /inspec-profile 
cd /inspec-profile
inspec init profile challenge --chef-license accept
``` 
<p align="center">
<img src="https://i.imgur.com/DYeFnuF.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Edit our newly created Inspec skeleton to add four basic checks (system, password, ssh, and useradd) from PCI/DSS requirements: <br/>
 
<p align="center">
<img src="https://i.imgur.com/Ud6pZos.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Run the tests locally before setting it up in the CI pipeline: <br/>
```
inspec check challenge
inspec exec challenge 
``` 
<p align="center">
<img src="https://i.imgur.com/vT2GAY0.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
<img src="https://i.imgur.com/8HuHQqb.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Commit the inspec profile to the project’s repository using either the GitLab UI or Git commands: <br/>
```
git clone git@gitlab-ce-rcgsg0ei:root/django-nv.git
cd django-nv
cp -r /inspec-profile/challenge challenge<br />
git add challenge<br />
git config --global user.email "you@example.com"<br />
git config --global user.name "Your Name"<br />
git commit -m "Add custom inspec profile"<br />
git push origin main<br />
``` 
<p align="center">
<img src="https://i.imgur.com/gFP8DIZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Re-create gitlab-ci.yml file to check and execute profile using inspec only with compliance job in the test stage: <br/>
 
<p align="center">
<img src="https://i.imgur.com/C7n3XJa.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>
<br /> 
Results: <br />
<p align="center">
<img src="https://i.imgur.com/EQTixCg.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
