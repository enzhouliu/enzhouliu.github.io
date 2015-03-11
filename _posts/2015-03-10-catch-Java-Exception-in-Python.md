---
published: true
---

My current project uses both Java and python, Java for the RESTful service side and the python used for ETL process. When we do test, we intend to do test in both side, so we decided to put all the test in python side, since python is easier for inter-language operation. And we in fact using jython for python part.

Recently I am developing the testcases. I found a issue today that python's "try except" can not catch Exception from Java. Since java.lang.Exception is different from Exception in python.

Ways to solve it:
1. use "try...except..." and then use sys.exc_info()[1], to reference exception instance.
2. use "from java.lang import Exception", then use "except Exception, err:" to catch Java Exception.
3. or directly use "except (Exception, java.lang.Exception), err:"