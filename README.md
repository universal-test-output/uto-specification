## Universal Test Output


### Description

UTO aims to be an unambiguous output format for test runners.


#### `v1.0.0`

The first non-space character of each line is the control character. What follows a control character varies, but is not important to the meaning of the line (except `%`, pragma). Lines may be indented by space characters (<code> </code>); leading spaces are ignored. Empty lines are allowed but are also ignored. Lines have no max width and end with a line feed (`\n`). Consuming programs should read line by line until end of transmission (`^D`).

* `%` *pragma*: communicate metadata to a consumer
  * `uto vX.Y`: specify the specification and major and minor version. this line must be first and is required
  * `count N`: optional hint to specify how many tests and/or groups should follow at the current level. does not specify test count in nested groups
    `N` must be a non-negative integer and must be honored
* `"` *comment*: apply to the line above, not below
* `.` *success*: passing test. use the remainder of the line to specify which test
* `?` *pending*: skip test
* `!` *error*: failing test
* `(` *open*: the remainder of the line may be taken to be the group label
  * groups may be nested
* `)` *close*


### Sample Output

```
% uto v1.0

% count 5

. nice. a passing test

! woops! this one failed!
  " comments are attached to whatever line preceded
  " and can span multiple lines

" comments don't have to be indented (nothing does actually). this comment still applies to the failure above

? pending. this lets you know the test was skipped on purpose

. another passing test

( start a group. this is the label
  % count 3
  . assert 1 + 1 == 2

  ( nested group
    " groups can be nested, but please don't get carried away
  )

  ! assert 0 == 1
) end group
```
