# buildnumber-maven-test
Test Git functionality of buildnumber mojo.

## How to Test?
Simply check out the project and run maven. You can do this on different OS, with different git executable in the path and with different maven versions. You only need to run the validate phase and check the output of `Storing buildNumber`.

    buildnumber-maven-test> mvn validate
    2797 [INFO] Scanning for projects...
    3025 [INFO]
    3027 [INFO] ------------------------------------------------------------------------
    3029 [INFO] Building buildnumber-maven-test 0.0.1-SNAPSHOT
    3032 [INFO] ------------------------------------------------------------------------
    3196 [INFO]
    3198 [INFO] --- buildnumber-maven-plugin:1.3:create (default) @ buildnumber-maven-test ---
    4544 [INFO] Executing: cmd.exe /X /C "git rev-parse --show-toplevel"
    4546 [INFO] Working directory: C:\ws\github\buildnumber-maven-test
    4675 [INFO] Executing: cmd.exe /X /C "git status --porcelain ."
    4677 [INFO] Working directory: C:\ws\github\buildnumber-maven-test
    4791 [INFO] ShortRevision tag detected. The value is '8'.
    4798 [INFO] Executing: cmd.exe /X /C "git rev-parse --verify --short=8 HEAD"
    4799 [INFO] Working directory: C:\ws\github\buildnumber-maven-test
    4904 [INFO] Storing buildNumber: null at timestamp: 1444673946454
    5018 [INFO] Storing buildScmBranch: master
    5022 [INFO] ------------------------------------------------------------------------
    5023 [INFO] BUILD SUCCESS
    5024 [INFO] ------------------------------------------------------------------------
    5030 [INFO] Total time: 2.301 s
    5032 [INFO] Finished at: 2015-10-12T20:19:06+02:00
    5267 [INFO] Final Memory: 7M/155M
    5268 [INFO] ------------------------------------------------------------------------
    buildnumber-maven-test> buildnumber-maven-test>mvn --version
    Apache Maven 3.2.5 (12a6b3acb947671f09b81f49094c53f426d8cea1; 2014-12-14T18:29:23+01:00)
    Java version: 1.8.0_40, vendor: Oracle Corporation
    Default locale: de_DE, platform encoding: Cp1252
    OS name: "windows 7", version: "6.1", arch: "amd64", family: "dos"
    buildnumber-maven-test> git --version
    git version 1.9.5.github.0

Optionally you can dirty a tracked file:

    buildnumber-maven-test> echo test >> README.md
    buildnumber-maven-test> mvn validate
    ...
    4964 [ERROR] Failed to execute goal org.codehaus.mojo:buildnumber-maven-plugin:1.3:create (default)
         on project buildnumber-maven-test: Cannot create the build number because you have local modifications :
    4966 [ERROR] [README.md:modified]
    buildnumber-maven-test> git checkout -- README.md

(Expected result). 

Or try a untracked file:

    buildnumber-maven-test> echo test >> bla
    buildnumber-maven-test> mvn validate
    ...
    4749 [WARN] Ignoring unrecognized line: ?? bla
    4980 [INFO] BUILD SUCCESS

Which is not expected.
