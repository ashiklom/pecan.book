Overview
=========

We use GitHub to track development. 

To learn about GitHub, it is worth taking some time to read through the [FAQ](https://help.github.com/). When in doubt, the first step is to click the "Help" button at the top of the page.

* **To address specific people**, use a github feature called "@mentions" e.g. write @dlebauer, @robkooper, @mdietze, or @serbinsh ... in the issue to alert the user as described in the [GitHub documentation on notifications](https://help.github.com/articles/notifications)


Bugs, Issues, Features, etc.
============================

Reporting a bug
---------------

1. (For developers) work through debugging (see [[debugging]] wiki). 
2. Once you have identified a problem, that you can not resolve, you can write a bug report
3. Write a bug report
4. submit the bug report
5. If you do find the answer, explain the resolution (in the issue) and close the issue

### Required content

Note: 

* **a bug is only a bug if it is reproducible**
* **clear bug reports save time**

1.  Clear, specific title
2.  Description - 
 * What you did
 * What you expected to happen
 * What actually happened
 * What does work, under what conditions does it fail?
 * Reproduction steps - minimum steps required to reproduce the bug
3. additional materials that could help identify the cause:
 * screen shots
 * stack traces, logs, scripts, output
 * specific code and data / settings / configuration files required to reproduce the bug
 * environment (operating system, browser, hardware)

Requesting a feature
--------------------

(from The Pragmatic Programmer, available as
[ebook](http://proquestcombo.safaribooksonline.com/0-201-61622-X/223)
through UI libraries, hardcopy on David’s bookshelf)\

* focus on “user stories”, e.g. specific use cases
* Be as specific as possible, 

* Here is an example:

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

When to submit an issue?
-----------------------

### Ideally, non-trivial code changes will be linked to an issue and a commit.

This requires creating issues for each task, making small commits, and referencing the issue within your commit message. Issues can be created [on GitHub](https://github.com/PecanProject/pecan/issues/new). These issues can be linked to commits by adding text such as `fixes gh-5` (see [[Using Git During Development|Using-Git#during-development]]).

Rationale: This workflow is a small upfront investment that reduces error and time spent re-creating and debugging errors. Associating issues and commits, makes it easier to identify why a change was made, and potential bugs that could arise when the code is changed. In addition, knowing which issue you are working on clarifies the scope and objectives of your current task. 

Label Categories
----------------

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg .tg-asmn{font-weight:bold;font-size:18px;text-align:center}
.tg .tg-0lao{font-weight:bold;font-style:italic;text-align:center}
.tg .tg-uitq{font-weight:bold;font-style:italic}
.tg .tg-mkqr{background-color:#fef2c0;text-align:center}
.tg .tg-97cz{background-color:#bfe5bf;text-align:center}
.tg .tg-0gsl{background-color:#f7c6c7;}
.tg .tg-yjqy{background-color:#d7e102;text-align:center}
.tg .tg-og1l{background-color:#5319e7;text-align:center}
.tg .tg-earr{background-color:#bfe5bf;text-align:center}
.tg .tg-38vt{background-color:#f7c6c7;text-align:center}
.tg .tg-6t6n{background-color:#e11d21;text-align:center}
.tg .tg-303t{background-color:#bfe5bf}
</style>
<table class="tg">
  <tr>
    <th class="tg-asmn" colspan="3">Label</th>
  </tr>
  <tr>
    <td class="tg-0lao">Type<br></td>
    <td class="tg-uitq">Status</td>
    <td class="tg-uitq">Priority</td>
  </tr>
  <tr>
    <td class="tg-mkqr">BETY, BioCro, BrownDog, CLM, DALEC, ED, LINKAGES, SIPNET</td>
    <td class="tg-97cz">0-Backlog</td>
    <td class="tg-0gsl">pri:Critical</td>
  </tr>
  <tr>
    <td class="tg-yjqy">VM, Web, HPC</td>
    <td class="tg-97cz">1-Ready</td>
    <td class="tg-0gsl">pri:Normal</td>
  </tr>
  <tr>
    <td class="tg-og1l">Documentation, Question, Discussion, Enhancement, Feature</td>
    <td class="tg-earr">2-Working</td>
    <td class="tg-38vt">pri:Low</td>
  </tr>
  <tr>
    <td class="tg-6t6n">Bug</td>
    <td class="tg-303t">3-Done</td>
    <td class="tg-031e"></td>
  </tr>
</table>
