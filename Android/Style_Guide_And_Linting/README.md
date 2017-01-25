# Style Guide

For all android projects we will be using the Google Java Style Guide. The painful details of this rigor may be found [here](https://google.github.io/styleguide/javaguide.html). Please make sure you are familiar with the basics at the least. 

# Linting

To make sure that all of our code follows the style guide, we will be using a linter. The linter will be run automatically on all commits by the CLI (TODO: write article and link it here), but you may run it locally on your machine as well. 

We are using [checkstyle](http://checkstyle.sourceforge.net/), which is a linting tool for java code. The linked site will give you all the details you may need, but if you're looking for a ready code sample, here it is. Please keep in mind that the following was tested on ubuntu 16.04 and some changes might be necessary for other systems.

```
# get the style linter
wget https://sourceforge.net/projects/checkstyle/files/checkstyle/7.4/checkstyle-7.4-all.jar
# run and pipe to a new file.
touch lint_results
# run the linter
java -jar checkstyle-7.4-all.jar -c /google_checks.xml app/src/* > lint_results
# check whether errors have occured and print them if so.
if [[ $(wc -l < lint_results) -ge 3 ]]; then cat lint_results; exit -1; else exit 0; fi
```

Note that the style guide linked to before is implemented as a set of rules within google_checks.xml. Feel free to check that out in your free time!