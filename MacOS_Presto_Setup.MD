# PrestoDB MacOS Setup

#### Context: A MacOS makes life 10000x easier. I tried using the school computer, but was fortunate enough to get my hands on a Mac. Setup worked like first try for everything on the Mac and was much quicker. Windows develpoment is possible but may come with lots of wasted setup time.  


This guide walks through a Presto development environment setup on MacOS. This includes installation and config of Homebrew, Java, Maven, Python, and IntelliJ. This is good for a fresh macOS user environment and/or onboarding new developers. 

#### Four Basic Requirements
1. MacOS X / Linux
2. Java 8 Update 151 or higher 
3. Maven 3.6.3+
4. Python 2.4+  

####   Install HomeBrew 
(I assume most already have it installed)

` /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)" `

Ensure it's installed with no issues:
`brew doctor`


#### Java Installation
Before installing java you need SDK Man. 
Which can be installed at: ` https://sdkman.io/ `
Then run ` sdk install java 8.0.442-amzn `

If needed, install Java 17
`brew install openjdk@17` 

#### Maven Installation

`brew install maven`

Ensure its Installed with `mvn -version`

#### Python Installation
`brew install python`

I assume most of you guys would have python and homebrew if you are in to RCOS and choose Presto

***

# GitHub Setup/Tips


Now fork off the PrestoDB repo. And clone it into your local machine. 

##### Add the Main Repo to your upstream 
`git remote add upstream https://github.com/prestodb/presto.git`


##### Update your fork (main repo has new commits daily)
Fetch Main Repo Code
`git fetch upstream`

Checkout to your Fork Master
`git checkout master`

Merge the new upstream code with your fork code
`git merge upstream/master`

Push to your remote to match with your local
`git push origin master`

Then you can follow the **Building Presto** section of the Main Presto GitHub
<https://github.com/prestodb/presto?tab=readme-ov-file#building-presto>

***

# IntelliJ Setup

PrestoDB developers work using IntelliJ. IntelliJ Ultimate is free for students if you have never used it before. You can set it up by going to the jetbrains website: <https://www.jetbrains.com/community/education/#students>

After doing the mvn building (as indicated from the Building Presto section) you need to setup some IntelliJ configurations. You can continue following the GitHub instructions but here is a more "dummy" explanation.

1. Open Project.
2. Go to File > Project Structure
3. In Project. Ensure that the SDK is 1.8 and the Language Level is 8 

Now setup the Run Config for the **Presto Server**

1. In the Top Right, you can click Current File then Edit Configurations
2. Then click the + and Click Application.
3. You can name it Presto Server
4. Ensure the module is java 1.8
5. Make the -cp no module to be **-cp presto-main** 
6. Put `com.facebook.presto.server.PrestoServer` into the Main Class
7. Click Modify options > "Add VM Options"
8. In the VM options, paste the following: `-ea -XX:+UseG1GC -XX:G1HeapRegionSize=32M -XX:+UseGCOverheadLimit -XX:+ExplicitGCInvokesConcurrent -Xmx2G -Dconfig=etc/config.properties -Dlog.levels-file=etc/log.properties`
9. In Working Directory: put ``$MODULE_DIR$``
10. Hit Okay and Run the Presto Server. 







