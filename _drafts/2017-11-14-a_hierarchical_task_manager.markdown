---
layout: post
title: "A hierarchical task manager"
category: dev
---

## Motivation ##

I used to use Taskwarrior to track my todos, but I found that it was
lacking in one particular area: arbitrary nesting of tasks and task
dependencies. Don't get me wrong, Taskwarrior is a wonderful tool and
was quite useful for a while; it just doesn't accurately represent how
I think about the things I need to get done.

So I decided to make my own tool, called FlexTime, built iteratively
around functionality I found myself wishing I had while working on
tasks.

## The prototype ##

The initial MVP (Minimum Viable Product) of FlexTime incorporated two
main ideas:

1. Arbitrary nesting of subtasks
2. Custom ordering of the task nodes

### The task tree ###

By default, the tasks are stored in `tasks.yml` in the directory in
which flextime is run. I chose the YAML format because, with the
PyYAML processor in Python, keys can be of any sensible type you would
encounter while fleshing out subtasks of a project. Take the following
example, from my initial sketches of what I'd like the tasks file to
look like:

```
Class:
  CSCI442:
    HW3:
	  _d: 10/10
	  3.1:
	    _t: 15
	  3.2:
	    _t: 25
	  ...
```

I wanted it to be trivial to break down a bigger project into parts,
and I wanted to be able to give a time estimate and due date for
sorting purposes. Each key represents a category, project, assignment,
problem on an assignment, or whatever you decide it should represent,
`_d` denotes a due date, and `_t` a time estimate in minutes. Why the
underscores? Since the task tree is arbitrarily nested, I needed some
way to denote that my program has found an actionable subtask, or
"TaskLeaf", as I called it; a leaf is any point in the tree that has
zero keys that are not preceded by underscores.
