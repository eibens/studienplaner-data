# Studienplaner Data

This repository contains public data used by the [Studienplaner](http://studienplaner.com) 
app. Hosting this data on GitHub is not only convenient, I also hope that others 
will see it as an opportunity to contribute.

## Styleguide

When contributing, please try to match the style used in the examples and in the 
existing files as closely as possible. As long as I don't know if anyone is 
actually willing to contribute, I will not go into further detail here.

## Language

For now all text in data and examples will be written in german, unless the
official text is not german (for example english course titles).

## Ping

The `ping` file can be used to check if a client can access this repository.
Most of the content in this repository is likely to change over time, which may
lead to clients requesting a file that does not exist anymore. Since the `ping` 
file is guaranteed to stay available, it can be used to decide whether another 
resource is simply unavailable, or if the client cannot connect at all.

For example, a client tries to access the `programs/index.json` resource. The
request fails. The client sends a request to `ping`, which fails as well. An
error message can be displayed, that suggests to check the internet connection
and try again. On the other hand, if the ping request succeeds, the error 
message can say that the resource is currently not available.

## Programs

The `programs` directory contains university program. There are two types of 
files, index files and program files.

### Index files

The index files help navigating the directory structure. They list and label the 
contents of that directory, which are either sub-directories or program files.
There is at most one index file in each directory and it must be named 
`index.json`.

Take this example:

    {
      "type": "Land",
      "options": {
        "at/": "Österreich",
        "de/": "Deutschland",
        "comp-sci": "Computer Science"
      }
    }

`type` *(required)* describes which kind of options this directory contains in
a human readable form. In this case the first two options are countries, so
'Countries' would be a good description (ignoring the third option for now). 

`options` *(required)* contains key-value pairs, that represent the 
directory's contents. The keys represent the relative name of the resource in 
the file system and must consist of lowercase english letters, digits, and 
hyphens only (excluding a trailing slash, see below). The values are human
readable titles for the respective resources.

A forward slash after the key indicates that the option is a sub-directory. So 
this particular index file suggests that there are two sub-directories, `at` 
and `de`. In order to find out what these directories contain the client may 
request `at/index.json` and `de/index.json` respectively. Though, it is not 
guaranteed that a sub-directory contain an index file.

If there is no forward slash, the resource is a file. In this example there 
would be a single file in that directory called `comp-sci.json`. Notice, that
the file extension is *not* included in the key.

**Index files may lie! Expect missing resources! Use a [ping](#ping) to
double check!**

### Program files

A program file contains a representation of a university program. In particular
it lists the courses that are associated with that program. Here is an example
of a Bachelor program about apple picking:

    {
      "name": "BAC Apple Picking",
      "courses": [{
        "id": "bsya",
        "type": "VU",
        "title": "Basics of Stretching Your Arms",
        "credits": 40.0,
        "tags": ["Pflichtmodul", "STEOP"]
      }, {
        "id": "hpr",
        "type": "VO",
        "title": "Human Perception of Ripeness",
        "credits": 30.0,
        "tags": ["Pflichtmodul", "STEOP erforderlich"]
      }, {
        "id": "db",
        "type": "VU",
        "title": "Distributing Buckets",
        "credits": 75.0,
        "tags": ["Pflichtmodul", "STEOP erforderlich"]
      }, {
        "id": "ls",
        "type": "UE",
        "title": "Introduction to Ladder Safety",
        "credits": 35.0
        "tags": ["Wahlmodul"]
      }]
    }
    
`name` *(required)* is the title of the program. It should include the degree,
but use an abbreviation.

`courses` *(required)* is an array containing the courses that belong to this
program. The order of courses is not relevant, but pick the order that makes
the most sense.

`id` *(required)* is an identifier for the course, which is unique for all courses in the
program. It must consist of lowercase english letters, digits, and hyphens only.
It is encouraged to use a natural ID, for example an abbreviation of the title
that is known to the students attending the program. Generally, pick an ID that
is short and unlikely to change.

`type` *(required)* describes the mode of the course. It consists of up to two characters.
These types are often dictated by the university itself, for example the
University of Technology in Vienna uses *VO* for lectures (German: *Vorlesung*)
and *UE* for exercises (German: *Übung*).

`title` *(required)* is the official title of the course. It usually summarizes
the subject. Try to avoid abbreviations, unless they are part of the official 
title.

`credits` *(optional)* is the number of credits a student receives for completing
the course. Defaults to `0` if not specified.

`tags` *(optional)* is an array of strings, that are used to categorize courses.
For example, there might be compulsory courses, optional courses, courses that
are a requirement for another set of courses, etc. Use tags to group courses
in a way, that help students understand the structure of the program. Try to 
keep the number of different tags below ten, and the number of tags per course
below five. Also, make sure to put the most important tags first. Pick an order
and stick to it.
