# OpenMBEE - MMS(4.0.7) w/Visual Editor and Cameo MDK Plugin

OK... Let me go through this. It was a tough learn and a bit of this setup is not working as advertized but its a great head start for all you regular engineers (like me...) to get going with a full fledged OpenMBEE installation with little overhead. 

## Whats included:
- MMS 4.0.7
- VE 3.6.1 - must use branch feature/remove-rootscope (per https://openmbee.slack.com/archives/C3XR7DZ7C/p1643999205072539) its already here so dont worry.
- MDK 5.0.1 - Local build located in the `cameo_mdk_5.0.1` folder

## Notes:
This setup has only been tested using 19.0SP4 and TWC 19. Not sure how it will work with 20X yet. I'll test in the next couple of weeks and update as necessary. 

## Edits Required for a specfic setup:
- A review of the main commits will give you an idea of what to do but here they are in written form to help.
    1. In the ```ve-feature-remove-rootscope/app/config/``` folder create a configuration file specific to your needs. Here I created a file called ```config.avian.js``` (just copy the config.example.js and rename) where ```apiUrl``` is set to your IP. I have not tried this with a dns name. The baseUrl can be left blank. The rest can be per your needs. <br>
    <br>
    See commit - https://github.com/avianinc/openmbee-prebuild/commit/07a8160d90cee4623540d459df6bbfa1e58597c0<br>
    <br>
    2. Update the ```example/src/main/resources/application-test.properties``` to suit your needs. Update ```twc.instances[0].url``` to your twc instance ip. Update ```s3.endpoint=http``` to your openmbee_IP:9000 which points to the docker installed minio instance.
    3. Update ```ve-feature-remove-rootscope/Dockerfile``` to point to the environment file created in step one where ```ENV VE_ENV``` is set to the name of your cofig file using on the designator, for instance for a config file named config.avian.js set ```ENV VE_ENV config.avian.js``` <br>
    See commit - https://github.com/avianinc/openmbee-prebuild/commit/5ede19e59ba669587640f6e213fa942cf45b6593<br>
    <br>

That should do it for the set up. Submit an issue if you have any issues :). 
<hr>

To get this running you will need docker desktop installed on windows (tested) or docker and docker compose on linux (not tested).

To run open powershell and:
1. Clone the project: `git clone https://github.com/avianinc/OpenMBEE_mms_4.0.7.git`
2. `cd` into the cloned directory
3. `docker-compose up`

Thats it... so here are some notes on the features and issues.

1. The docker compose file creates local volumes for persistent storage. The noted setup seems to work but may not be 100% correct yet.
2. To configure the OpenMBEE installation one must modify the config file `example\bin\application-test.properties`. Not much needed for a local installation but worth a lookie-loo. Per the properties file the admin account:
    - Username: test
    - Password: test
3. The visual editor is located at: `http://localhost`
4. The example API is located at: `http://localhost:8080/v3/swagger-ui/index.html?configUrl=/v3/api-docs/swagger-config`<br>
Note: If you go to `http://localhost:8080/v3/swagger-ui/index.html` it takes you to some sort of pet store example???<br>
Also Note: In cameo set the url for the mms server to `http://localhost:8080` (follow all other directions...)
5. For some reason the API authorize function only accepts a bearerToken... not sure why one cannot login with the standard un/pw and this issue was a surprise to the group. Need to fix. In the meantime one can obtain the token with a curl call from windows command prompt... doesn't seem to work with powershell nor anaconda shell:
    - `curl -X GET -u test "http://localhost:8080/authentication"`
    - Note: if you receive error: `curl: (1) Protocol "'https" not supported or disabled in libcurl` this is typically due to improper translation of copied quotation marks... retyping the quotations marks should fix this issue. 
    - Copy the token... its the long annoying text between the quotation marks, and enter it into the `bearerToken` area of the authorization section of the API.
6. When using the Visual Editor as the test user the upper right corner will look a little messy. This is due to the test user not having a first and last name. To correct just add a new user and provide a first and last name and login as the new user. You will see the users initials instead of the weird looking text once performed.
7. When running the initial docker compose up command there may be issues with the gradle and associated file downloads. This is generally due to a slow internet connection and can occur when on a wireless network. Moving to a high speed wired network will typically fix the issue.
8. Make sure you install the MDK plugin from the included `cameo_mdk_5.0.1` folder in your Cameo or MD installation. Follow the included help files from the Cameo help menu to start working with OpenMBEE :). Let me know if you are having issues... I don't mind working these things out.

ToDo:

- Test out the other MDKs in particular Matlab and JupyterLab
- Integrate the system with the model versioning tool Lemontree removing any need for TWC. There is an example here https://github.com/danielsiegl/OpenMBEEwithGitDemo that I hear will work (not tested). I'm going to be working to make this happen. (https://www.lieberlieber.com/lemontree/en/)
- Work to install ve4 where (I believe) a local Confluence installation can act as a visual editor. (Still need some work on my part here to make sure I know what Im talking about)
- Look at alternates for Minio as the artifact repo. Boeing is using MarkLogic for performing RDF graph queries on SysML data (https://cdn1.marklogic.com/wp-content/uploads/2019/06/MLW19-Boeing-Querying-Model-Based-Systems-Definition-Data.pdf)
- Create and include an Ares PLM container to start developing a prepackaged installation.
- Integrate SysMLv2 at some level???
- JupyterHub
- Linux desktop with a few desktop applications accessible through a JupyterLab enabled websockets (scilab, octave, Rstudio, OpenFoam... what else???)
<hr>
The ultimate goal here is to create +80% solution for an entirely open source digital engineering environment which can be setup in minutes, preloaded with training and samples for super fast engineering startups.
<hr>

