## Things to Do Before Pushing a PR

Presto has a lot of Build Checks and one of the things they verify is style of coding. This is things like amount of whitespace in code, if the brackets are on a new line, if there is a space after the if word in an if statement like "if ()" or do you have "if()"" with no space. 

So you have two options to ensure you code has coding style that matches Presto's guidelines. 

#### Manually Checking

When you are done implementing your task and think you are ready to PR, run a mvn build according to the connector you were working in. 

For me, I worked on Iceberg Metrics so I worked inside the presto-iceberg directory so I ran the validate command below. 

`./mvnw validate -pl presto-iceberg`

When this builds, and most likely fails, you will get a build error telling you how many checkstyle violations you have. 

You then have to find the checkstyle-results.xml file to see which errors are occuring. 

Each connector has src folder and a target folder. Depending on which one you are working in, it'll always be located in the target folder. 

So if you work in presto-iceberg it'll be:

`presto-iceberg/target/checkstyle-result.xml `

Each connector has a target folder and it will be named the same "checkstyle-result.xml" so ensure you are in the correct folder. 

Then you just have to find the files you were working with and view the errors. These may be a little hard to read but you can extract all the information you need from it. It may say "leading whitespace" or "Parameter ... should be final".

Here are some examples of what the XML file may look like. 

` <error line="51" column="54" severity="error" message="Parameter metricName should be final." source="com.puppycrawl.tools.checkstyle.checks.FinalParametersCheck"/> `


`<error line="55" column="5" severity="error" message="Method length is 191 lines (max allowed is 150)." source="com.puppycrawl.tools.checkstyle.checks.sizes.MethodLengthCheck"/>`


`<error line="56" column="24" severity="error" message="Parameter report should be final." source="com.puppycrawl.tools.checkstyle.checks.FinalParametersCheck"/>`


`<error line="58" column="9" severity="error" message="&apos;if&apos; is not followed by whitespace." source="com.puppycrawl.tools.checkstyle.checks.whitespace.WhitespaceAroundCheck"/>`


`<error line="63" column="44" severity="error" message="&apos;typecast&apos; is not followed by whitespace." source="com.puppycrawl.tools.checkstyle.checks.whitespace.WhitespaceAfterCheck"/>`

So from these lines you can see which line the error occurs at, and the message. Its usually simple things so ensure that before you PR you have **NO CHECKSTYLE ERRORS**





#### Airlift File

This approach is the automated approach of what happens above. 

1. Navigate to <https://github.com/prestodb/codestyle> 

2. Navigate into the **IntelliJIdea14** folder

3. Click Airlift.xml

4. Click **Raw** in the top right of the file

5. Save all that as a file into your Downloads / Desktop or whever you want to save it.


Then Navigate over to your IntelliJ IDE. 
(This guide is assuming you are on a MacOS but this is applicable to Windows. This will reference macOS shortcuts)

Once in IntelliJ follow these steps to apply the Airlift.xml

1. Navigate to Settings (IntelliJ IDea > Settings)

2. Click the **Code Style** Drop Down then Click **Java**

3. Then click the Settings "gear" Icon

4. Click **Import Scheme** then **IntelliJ Idea code style XML**

5. Then choose the **Airlift.xml** file you just downloaded in the previous section.

6. Click Apply and Okay. 


If the xml file was uploaded successfully, go to the file you were working on then you are able to do **Command + Option + L**

This SHOULD auto fix a lot of the issues and align it to the checkstyle format. Ensure by running the validate again and making sure that you have no more checkstyle errors. 



Now just do run a build and ensure you create Unit Tests for the feature you implemented.

`./mvnm install -pl presto-iceberg -DskipTests`

That command will run just the presto-iceberg connector and skip running all their basic tests which will save a lot of time validating your code. 

If you pass all these checks and builds then put your code up for PR!


***

#### CLA Compliance
When you first do a PR, you will get an automated **linux-foundation-easycla** response. 

All you have to do is sign up as a **personal contributor** and agree to their compliance documents. 

Ensure you fill out their draft PR request properly to maintain their style of work. 

Request Review and once thats merged you are offically a Presto Contributor! Congrats. 




#### Git Tips

Your Presto  Mentor will go over this, but when you pass code review, you will most likely be told to squash all your commits for a cleaner commit history. 

Learn how to interact with git on the level of rebasing and squashing. 

Here is a step by step guide on how to do that. 

1. Start Interactive Rebase 
    Decide how many commits you want to modify and run the command
    `git rebase -i HEAD~<N>`

    In this case `<N>` will be how many commits you want to squash.

    Example: `git rebase -i HEAD~5` 

    This will open a vim editor (**learn basic vim commands if you don't know then**)


2. Choose Commits to Squash
    You will be shown at the top of the editor your latest 5 commits (if you put 5 for `<N>`)

    Example case:

    `pick 1234abcd Add metric logging`
    `pick 2345bcde Fix typo in logging`
    `pick 3456cdef Refactor logging format`


    You can delete the word **pick** and replace it with **squash** or **s**

    This will effectively squash the commit into the one above it. 

3. Example of Squashing all commits into one.

    Taking the previous example if we want to squash the 3 commits into one, the editor will look like:

    `pick 1234abcd Add metric logging`
    `squash 2345bcde Fix typo in logging`
    `squash 3456cdef Refactor logging format`

    Then **:wq** to save the changes done (**LEARN VIM**)


Now you can verify if you cleaned everything up by running 

`git log --oneline`

This will show you all your commits as one line to make it easier to read. 

Ensure that the squashed commits don't appear and its only that one commit that the other commits got "squashed" into. 

Ask a Mentor for help if you are lost so you don't accidentally lose your work. 




