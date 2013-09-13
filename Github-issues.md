Overview
=========

We use Both GitHub and Redmine to track development. 

To learn about GitHub, it is worth taking some time to read through the [FAQ](https://help.github.com/). When in doubt, the first step is to click the "Help" button at the top of the page.

### Ideally, non-trivial code changes will be linked to an issue and a commit.

This requires creating issues for each task, making small commits, and referencing the issue within your commit message. Issues can be created [on GitHub](https://github.com/PecanProject/pecan/issues/new) or [Redmine](https://ebi-forecast.igb.illinois.edu/redmine/projects/pecan/issues/new). These issues can be linked to commits by adding text such as `fixes redmine # 200` or `fixes gh-5` (see [[Using Git During Development|Using-Git#during-development]]).

Rationale: This workflow is a small upfront investment that reduces error and time spent re-creating and debugging errors. Associating issues and commits, makes it easier to identify why a change was made, and potential bugs that could arise when the code is changed. In addition, knowing which issue you are working on clarifies the scope and objectives of your current task. 

Bugs, Issues, Features, etc.
============================

Reporting a bug
---------------

The first step is to work through debugging the issue yourself (see [[debugging]]). Once you have identified a problem, but do not know the solution, you can submit a request for support or bug report 

### Required content

1.  Clear Title - to allow efficient review of bug lists
2.  Bug description - should be concise but complete
3.  Reproduction steps - minimum steps required to reproduce the bug
 * specific code and data / settings required to reproduce the bug
 * _A bug that can’t be reproduced is not a debuggable bug, perhaps not a bug at all._
4. (Ideally) Write a (regression) test based on the reproducible bug; bug can be closed when test passes.
5.  Any additional materials that make digging into the problem easier
    (stack traces, logs, script files, specific data sets, screen shots.
    Evrionment considerations (OS, browser, hardware) will be less
    relevant for development and use on ebi-forecast and ebi-cluster.
    Once we start distributing PEcAn, this will be required.

### Additional information required for bugs in the BETYdb Web Interface

Use the following format ([Redmine issue 1386](https://ebi-forecast.igb.illinois.edu/redmine/issues/1386)):

* what I expect:
 * menu item -> sub-menu item -> link -> target page
*  What I get
 * menu item -> sub-menu item -> link -> Error

* Example:

    I expect:
    new site > new treatment > new trait >> launches traits/new
    I get:
    new site > new treatment > new trait >> undefined method `date_year' for #<Trait:0x7ffc8f110fb0>

Requesting a feature
--------------------

(from The Pragmatic Programmer, available as
[ebook](http://proquestcombo.safaribooksonline.com/0-201-61622-X/223)
through UI libraries, hardcopy on David’s bookshelf)\

* Request should focus on “user stories”, e.g. specific use cases
 * use cases, e.g. farm gate vs regional analysis, parameter tweaks, data access
* Be as specific as possible, 

* Here is an example (from [Redmine issue https://ebi-forecast.igb.illinois.edu/redmine/issues/62]):

 1.  Bob is at www.mysite.edu/maps
 2.  map of the the region (based on user location, e.g. US, Asia, etc)
 3.  option to “use current location” is provided, if clicked, map zooms in to, e.g. state or county level
    4.  for site run:
        1.  option to select existing site or specify point by lat/lon
        2.  option to specify a bounding box and grid resolution in
            either lat/lon or polar stereographic.

    5.  asked to specify start and end times in terms of year, month,
        day, hour, minute. Time is recorded in UTC not local time, this
        should be indicated

Closing an issue
----------------

1. Definition of “Done”
 * test
 * documentation
2.  when issue is resolved:
 * status is changed to “resolved”
 * assignee is changed to original author
3. if original author agrees that issue has been resolved
 * original author changes status to “closed”
4.  except for trivial issues, issues are only closed by the author 