# Version Update Manager

## Description
<br>
    This library is a light weight version management library. That only does two things.<br>
    <br>
    1: It gives you a class where you can implement where you want to store your version number.<br>
    2: It provides a VersionUpdate class with a update procedure where you can put the changes you want in your system.<br>
    <br>
    NOTE: Currently you will find a TableUpdate package to manage DataFlex databases with int-files and fd-files. This should be moved to it's own library, but lives here for now until we see what the tech stack has to offer. Perhaps we wount need that library at all in the future. So, why start building.<br>
    <br>
    IMPORTANT: This is not intended for database updates only. The idea is that you can do what ever needs to be done in the workspace in the production server. Like updating config files and so on.<br>
    Also importent to note is that there for the same reason is no built in rollback functionallity. If you want to roll something back, you add a new version that reverse the changes you want to roll back.<br>
    <br>
    Sample files exist for storing the verison number in a file or a database. But you could store it wherever. Perhaps en a RegEdit register if that is your coup of tea.
    <br>
<b>## Usage</b><br>
<br>
Use VersionUpdateManager\VersionUpdateConfigurationBase.pkg<br>
<br>
Class cVersionUpdateConfigurationLocal is a cVersionUpdateConfigurationBase<br>
    Procedure Construct_Object<br>
        Forward Send Construct_Object<br>
        Property Integer piVersionNumber -1<br>
        <br>
    End_Procedure<br>
    <br>
    Function setVersion Integer iVersion Returns Boolean<br>
        Integer iVersionNumber<br>
        <br>
        // WRITE CODE THAT STORES THE iVersion<br>
        <br>
        Get getVersion to iVersionNumber<br>
        If (iVersion = iVersionNumber) Function_Return True<br>
        Function_Return False<br>
    End_Function<br>
    <br>
    Function getVersion Returns Integer<br>
        Integer iVersionNumber<br>
        <br>
        // IMPLEMENT CODE THAT READS THE CURRENT VERSION NUMBER TO iVersionNumber<br>
        <br>
        Function_Return iVersionNumber<br>
    End_Function<br>
    <br>
    Function ValidateSetup Returns Boolean<br>
        // PUT ANY VALIDATION THAT TEST IF THE VERSION SYSTEM IS INSTALLED ON THE SERVER HER, OR JUST RETURN TRUE IF YOU FEEL CONFIDENT<br>
        Function_Return True<br>
    End_Function<br>
End_Class<br>
<br>
Object oLocalConfiguration is a cVersionUpdateConfigurationLocal<br>
End_Object<br>
<br>
Use VersionUpdateManager\VersionUpdateManagerBase.pkg<br>
<br>
Class cVersionUpdateManagerLocal is a cVersionUpdateManagerBase<br>
End_Class<br>
Object oVersionUpdateManagerLocal is a cVersionUpdateManagerLocal<br>
    Set phVersionUpdateConfiguration to oLocalConfiguration<br>
End_Object<br>
<br>
// Use VersionUpdater\IncludeVersionFiles.pkg<br>
Use AppSrc\VersionUpdater\VersionFiles\UpdateVersion_1.pkg<br>
Send AppendVersion of oVersionUpdateManagerLocal (RefClass(cUpdateVersion_1))<br>
<br>
Use AppSrc\VersionUpdater\VersionFiles\UpdateVersion_2.pkg<br>
Send AppendVersion of oVersionUpdateManagerLocal (RefClass(cUpdateVersion_2))<br>
<br>
Use AppSrc\VersionUpdater\VersionFiles\UpdateVersion_3.pkg<br>
Send AppendVersion of oVersionUpdateManagerLocal (RefClass(cUpdateVersion_3))<br>
<br>
<br>
// The VersionFiles look like this (IncludeVersionFiles.pkg is to have cleaner code in the implementation and keen all the includes of versions in one place)<br>
Use UpdateVersionBase.pkg<br>
Class cUpdateVersion_1 is a cUpdateVersionBase<br>
    <br>
    Procedure update<br>
        <br>
        // CODE THAT DOES THE CHANGES YOU WANT DONE FOR THIS VERSION<br>
        // If there are many things that change, I recommend breaking it out into functions.<br>
        // That makes it easier to comment out the completed functions when adding new ones for new changes, to the code<br>
        <br>
    End_Procedure<br>
End_Class<br>
<br>
<b>## Project structure</b>
<br>
In order to keep the implementation version as clean as possible the Development version with Unit Tests are in a separate SWS-file.<br>
<b>CronBackend.sws</b><br>
- Core library workspace. No external dependencies. (End-Users / Implementers)<br>
<b>CronBackendUnitTest.sws</b><br>
- Development workspace. Includes the Unit Test library. (Developers / Maintainers)<br>
<br>
<b>## TODO</b><br>
<br>
    [ ] Currently there are many different versions on top of eachother since this has been a library used for learning the most apropriate structure of the code.<br>
        This means that as we update our systems where this is used, that old code will be removed.<br>
    [ ] Currently this library includes a TableUpdate.pkg file with special code for database management. This is not intended to be a part of this library. But due to DataAccess plan to phace out some of the features that create the need for them, we keep them here instead of creating a spearate library, until we can mark them as obsolete and eventually remove them.<br>
    [ ] There is a cVersionUpdateConfigurationBase and a cVersionUpdateConfigurationInterface mixin in the library. This is because I'm still experimenting with the different approaches to find the most omptimal implemnetation for this library. But it is leaning towards the Interface implementation.