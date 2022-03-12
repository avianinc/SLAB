# OpenMBEE - MMS(4.0.7) w/Visual Editor and Cameo MDK Plugin

OK... Let me go through this. It was a tough learn and a bit if this setup is not working as advertized but its a great head start for all you regular engineers (like me...) to get going with a full fledged OpenMBEE installation with little overhead. 

Whats included:
- MMS 4.0.7
- VE 3.6.1 - must use branch feature/remove-rootscope (per https://openmbee.slack.com/archives/C3XR7DZ7C/p1643999205072539) its already here so dont worry.
- MDK 5.0.1 - Local build located in the `cameo_mdk_5.0.1` folder

To get this running you will need docker desktop installed on windows (tested) or docker and docker compose on linux (not tested).

To run open powershell and:
1. Clone the project: `git clone https://github.com/avianinc/OpenMBEE_mms_4.0.7.git`
2. `cd` into the cloned directory
3. `docker compose up`

Thats it... so here are some notes on the features and issues.

1. The docker compose file creates local volumes for persistent storage. The noted setup seems to work but may not be 100% correct yet.
2. To configure the OpenMBEE installation one must modify the config file `application-test.properties`. Not much needed for a local installation but worth a lookie-loo. Per the properties file the admin account:
    - Username: test
    - Password: test
3. The visual editor is located at: `http://localhost`
4. The example API is located at: `http://localhost:8080/v3/swagger-ui/index.html?configUrl=/v3/api-docs/swagger-config`<br>
Note: If you go to `http://localhost:8080/v3/swagger-ui/index.html` it takes you to some sort of pet store example???
5. For some reason the API authorize function only accepts a bearerToken... not sure why one cannot login with the standard un/pw and this issue was a surprise to the group. Need to fix. In the meantime one can obtain the token with a curl call from windows command prompt... doesn't seem to work with powershell nor anaconda shell:
    - `curl -X GET -u test "http://localhost:8080/authentication"`
    - Note: if you receive error: `curl: (1) Protocol "'https" not supported or disabled in libcurl` this is typically due to improper translation of copied quotation marks... retyping the quotations marks should fix this issue. 
    - Copy the token... its the long annoying text between the quotation marks, and enter it into the `bearerToken` area of the authorization section of the API.
6. When using the Visual Editor as the test user the upper right corner will look a little messy. This is due to the test user not having a first and last name. To correct just add a new user and provide a first and last name. You will see the users initials instead of the weird looking text once performed.
7. When running the initial docker compose up command there may be issues with the gradle and associated file downloads. This is generally due to a slow internet connection and can occur when on a wireless network. Moving to a high speed wired network will typically fix the issue.
8. Make sure you install the MDK plugin from the included `cameo_mdk_5.0.1` folder in your Cameo or MD installation. Follow the included help files from the Cameo help menu to start working with OpenMBEE :). Let me know if you are having issues... I dont mind working these things out.

ToDo:

- Test out the other MDKs in particular Matlab and JupyterLab
- Integrate the system with the model versioning tool Lemontree removing any need for TWC. There is a plugin here https://github.com/danielsiegl/LemonTreePublicDemo that I hear will work. I'm going to be working to make this happen. (https://www.lieberlieber.com/lemontree/en/)
- Work to install ve4 where (I believe) a local Confluence installation can act as a visual editor. (Still need some work on my part here to make sure I know what Im talking about)
- Look at alternates for Minio as the artifact repo. Boeing is using MarkLogic for performing RDF graph queries on SysML data (https://cdn1.marklogic.com/wp-content/uploads/2019/06/MLW19-Boeing-Querying-Model-Based-Systems-Definition-Data.pdf)
- Create and include an Ares PLM container to start developing a prepackaged installation.
- Integrate SysMLv2 at some level???
- JupyterHub
- Linux desktop with a few desktop applications accessible through a JupyterLab enabled websockets (scilab, octave, Rstudio, OpenFoam... what else???)
<hr>
The ultimate goal here is to create +80% solution for an entirely open source digital engineering environment which can be setup in minutes, preloaded with training and samples for super fast engineering startups.
<hr>

