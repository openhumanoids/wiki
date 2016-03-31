[via Pat Marion]

1. checkout mitdrc/drc-testing-data and place it next to the drc directory (so the same directory contains drc and drc-testing-data)
1. the cmake variable BUILD_TESTING is enabled by default (in ddapp/distro/pods/drc/pod-build/CMakeCache.txt), but just in case you have a very old build, do a make clean or set BUILD_TESTING to ON
1. run ctest:

```Shell
cddrc
cd ddapp/distro/pods/drc/pod-build
ctest

ctest -N # list tests but dont run them
ctest -V # print verbose info like test command lines and output
ctest -R MyTest # run all tests with labels matching "MyTest" 
```

##System testing
* Checkout mitdrc/drc-testing-data and place it next to the drc directory (so the same directory contains drc and drc-testing-data)
* Compile system tests:
```Shell
cddrc
cd ../tests
make all
```
* Run tests:
```Shell
cddrc
cd ../tests
make # Builds and runs all tests
```
or
```Shell
cddrc
cd ../tests/systemtests
make test # Builds and runs system tests
```
or
```Shell
cddrc
cd ../tests/systemtests/podbuild
ctest
```
**Note:** When SCS process is killed, the its ROS topics are not always cleared from the ROS core. To avoid mixed states while testing, restart the ROS core with every test (the test will attempt to run its own core, so it's enough to stop cores that are already running).

## Creating a system test that runs SCS and bot-procman-sheriff

You can create a system test that runs SCS, waits for it to start, then starts bot-procman-sheriff, waits for it to finish, and evaluates if any python (or other) code that sheriff executed without failure.
To do this you will need:
 1. An entry in the test/systemtest/CMakeLists.txt file.
 2. A *.pmd file for bot-procman-sheriff to run.
 3. A python script or other program that runs the actual test.

### CMakeLists.txt
In your CMakeLists.txt add the following entry:
```CMake
add_test(NAME test_testMyTest COMMAND ${CMAKE_SOURCE_DIR}/src/generic_test.bash MyTest config.pmd 100)  
```
This entry creates a unit test which launches using the generic_test.bash script.
3 arguments must be provided:
 1. **Name of the test** - this will be used to name the temporary file used to store the results of python  script (see test/systemtest/src/exoticaLWR/test.py).
 2. **Sherif config file** - a full path to the *.pmd file (see test/systemtest/src/exoticaLWR/kika_lwr_exotica.pmd).
 3. **Maximum allowed time** in seconds

### Sheriff config file (*.pmd)
The *.pmd file has to define a script called start and this script has to block the execution using a wait command on the Drake Designer entry:
```
script "start" {
    start group "0.params_and_model_pub";
    start cmd "exotica_json AICO" wait "running";
    wait ms 1000;
    start cmd "drake-designer" wait "stopped";
}
```
To start the drake designer and launch a custom python script, use the --startup command line argument, e.g.:
```
cmd "drake-designer" {
    exec = "drake-designer -lwr -exo -c $DRC_BASE/software/config/kuka_lwr.cfg --startup $DRC_BASE/../tests/systemtests/src/exoticaLWR/test.py";
    host = "localhost";
}
```

### Python test script
For the python script to pass the test, the script has to end with:
```Python
with open(os.environ.get('SYSTEMTEST_RESULT_FILE'),'w+') as f:
  f.write('1\n')
exit()
```
When the script fails, the python script can just call exit():
```Python
if not success:
  print "Test has failed because unicorns were unhappy!"
  exit()
```

The generic_test.bash script creates a temporary file, stores its name in the environmental variable SYSTEMTEST_RESULT_FILE, and writes a "0" into it. After the test has finished (the bot-procman-sheriff has stopped), the file is checked. If it contains "1", the test passes.