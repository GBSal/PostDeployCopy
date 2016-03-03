# PostDeployCopy
A demo ssdt project to show how to use an msbuild copy task to get post-deploy scripts from referenced projects

This works like this:

MainProject, refernced by the unit test project. The main project has the post deploy script but it is the unit tests that are deployed in the ci environment and the post deploy is only run from the dacpac being deployed, not referenced dacpacs.

There is an msbuild exec task:

  <Target Name="BeforeBuild">
    <Message Text="Copying Post Deploy Script" Importance="high"/>
    <Copy SourceFiles="$(SolutionDir)\DeployScripts\Deploy\PostDeploy.sql" DestinationFiles="$(SolutionDir)\DeployScripts.UnitTests\Deploy\PostDeploy.sql" OverwriteReadOnlyFiles="true"/>
  </Target>

This just overwrites the post deploy file in the unit test project.

This does means you can't have a custom deploy script for your unit tests.

Alternatives are:

 - Before deploying the dacpac, iterate through all the dacpacs which will be deployed and using the .net PackagingApi pull out all the PostDeploy.sql and PreDeploy.sql files, order them as you want and then write them to the main dacpac - then when you deploy you will get all the the scripts.
 - Write a deployment contributor to basically do the same thing


I personally find it easier just to copy the scripts I want into each project and just know that if I edit the script in the unit test project it will be overwritten (people only normally do it once or twice)!



