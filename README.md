# Docker SoapUI

> Docker container that houses a configurable version of SoapUI and runs test suites / cases

How to Use
==========

With this container running, you can now fire off SoapUI tests by sending the SoapUI project file
forwards the `3000` port to the container.

```sh
$ curl --form "project=@/path/to/soapui-project.xml" \
       --form "suite=TestSuite" \
       http://localhost:3000
```

Develop/Test
============

I use Vagrant to mimic the environment of Docker.

```bash
$ vagrant up
$ vagrant ssh
# sudo -i
# cd /vagrant
# ./server.py
```

Continuous Integration / Response Codes
=======================================

After the tests are ran, you will receive the stdout and stderr from the SoapUI test runner,
as well as an HTTP status code to determine the result of the test run.

*Response Codes*

| Code | Message | Description |
| -----|---------|------------ |
| **200**  | OK      | All SoapUI Tests ran successfully and passed |
| **550**  | Test Failure(s) | You have failures in the SoapUI Test suite / cases. You can check the content of the request to determine what failed |
| **551**  | No Suite | You did not specify the `suite` POST parameter with the name of the suite you wanted to run |
| **552**  | No SoapUI Project | You did not specify the `project` POST parameter with the proper SoapUI XML data.  *Remember*: This needs to be the actual file itself sent as multipart/form-data. E.g: `curl -F "data=@the-soapui-project.xml"` |
| **500**  | Internal Server Error | An exception occured while running the SoapUI Tests |
