---
title: "💯 Hong Kong School Internal Examination Seating Arrangement System"
layout: post
date: 2014-01-22 23:00
tag:
- Pascal
- exam
- system
projects: true
hidden: true # don't count this post in blog pagination
description: "School Internal Examination Seating Arrangement System specifically designed for secondary schools in Hong Kong"
category: project
author: edenau
---

***Source code is now available on <a href="https://github.com/edenau/HKSchool-Exam-Sitting-Arrangement" target="_blank">Github</a>. Check it out!***

# Introduction

This was my high school project that was specifically designed for secondary schools in Hong Kong to arrange internal examinations automatically.

## Table of Contents
- [Background](#background)
- [System Requirements and Benefits](#system)
- [Work Flow](#work-flow)
- [Preliminary Design](#pre-design)
- [Stage Design](#stage-design)
- [Remarks](#remarks)

<div class="breaker"></div> <a id="background"></a>

# Background

High school students studying in Hong Kong local schools are required to take part in 4 compulsory subjects, 1-3 elective subjects and an optional extended part of Mathematics module 1 or 2 (M1 or M2) under the New Senior Secondary (NSS) curriculum. There was a huge variety of subject combinations for them. There were about 600 students in Form 4 to Form 6 in my high school, and they all took different combinations of elective courses. Apart from the compulsory subjects, there were 11 elective subjects, and M1, M2 available for students to take.

All students were required to complete the papers in the hall at the same time, and therefore *(i)* seat allocation in hall centre was required. *(ii)* A general examination timetable and *(iii)* specific slips of admission form for each candidate were also necessary. The form should include information such as:
- Student name
- Class and class number
- Papers taken
- Date and time of each paper arranged in chronological order
- Location of examination for each paper

The specific slip of admission form should look like this:

<table>
  <tr><th colspan="5">Eden's High School 2012-2013 Final Examination</th></tr>
  <tr><th colspan="5">AU, Eden 5E (1)</th></tr>
  <tr><td>Date</td><td>Time</td><td>Paper</td><td>Venue</td><td>Seat No.</td></tr>
  <tr><td>10 June (Mon)</td><td>08:30-10:00</td><td>English 1</td><td>Hall</td><td>278</td></tr>
  <tr><td>10 June (Mon)</td><td>10:30-12:30</td><td>English 2</td><td>Hall</td><td>278</td></tr>
  <tr><td>11 June (Tue)</td><td>08:30-10:45</td><td>Mathematics</td><td>Hall</td><td>341</td></tr>
  <tr><td>13 June (Thur)</td><td>08:30-09:45</td><td>Chinese 1</td><td>Hall</td><td>271</td></tr>
  <tr><td>13 June (Thur)</td><td>10:45-12:15</td><td>Chinese 2</td><td>Hall</td><td>271</td></tr>
  <tr><td>14 June (Fri)</td><td>08:30-10:30</td><td>Liberal Studies</td><td>Hall</td><td>266</td></tr>
  <tr><td>18 June (Tue)</td><td>08:30-10:30</td><td>Chemistry</td><td>Hall</td><td>273</td></tr>
</table>

<div class="breaker"></div> <a id="system"></a>

# System Requirements and Benefits
The Examination Seating Arrangement System (ESAS) aims to provide a convenient way to lay out pre-exam preparation such as timetable arrangement, seat allocation etc.. The program has the following requirements:
- Clear interface for users – manager and assistant of examination. Clear instructions should be displayed to guide them.
- Data of students can be imported for executing different tasks.
- Subject crash comparison (students doing the same combinations of courses) can be calculated and the best solution of timetable arrangement with the least crash can be found.
- Seat numbers should be allocated in order of class and class number for management convenience.
- General timetable and specific slips of admission form for all students can be
generated automatically referring to the data imported before and the timetable arranged, which retrenches human power.

It can potentially bring the benefits, for instance
- Reduce time and workload for calculating subject crash comparison
- Avoid human errors
- Perform tasks effectively and analyze data automatically using imported data
- Allocate seats for students in good order

<div class="breaker"></div> <a id="work-flow"></a>

# Work Flow

ESAS requires students’ data and examination information. The former mainly refer to personal profile (e.g. name, class, class number) and subjects each student takes, and it is stored in a spreadsheet file; whereas the latter includes number of papers of each subject, duration of each paper and dates available for the examination period. These inputs are used to calculate subject crashes by exhaustive search in order to set an optimal timetable. Hence, we can then generate the timetable and admission forms for every student. The following flowchart (from top to bottom) illustrates how ESAS can be done by breaking it into different sub-problems.

<table>
  <tr><th colspan="3">ESAS login authentication</th></tr>
  <tr><th colspan="3">Import student data and exam information</th></tr>
  <tr><th colspan="3">Calculate subject crash (exhaustive search)</th></tr>
  <tr><th colspan="3">Arrange exam setting</th></tr>
  <tr><th>Generate timetable</th><th>Generate seat allocation</th><th>Generate admission forms</th></tr>
</table>

<div class="breaker"></div> <a id="pre-design"></a>

# Preliminary Design
## Interface

I adopted command line interface but it could potentially be upgraded to a graphical one. The interface contains three parts. Part A shows some reminders to the user. Using the following figure as an example, the system is now reminding the user to import a file called ‘all_sub.csv’. Part B shows the menu containing all procedures which the user can choose to do next. Part C provides a space for the user to input their option among those given procedures. They are expected to input the number representation of the corresponding procedures.

![fig1](https://github.com/edenau/HKSchool-Exam-Sitting-Arrangement/raw/master/figures/fig1.png)
<sup> Yup this was the old name of my system, didn't bother to fix it </sup>

## Top Level Data Flow
I used Data Flow Diagrams (DFDs) to illustrate how ESAS works. This is a very useful graphical modeling tool and it shows all the main requirements of the system such as processes, data flowing into, out of and within the system, and the way information is stored in the system.

### Context (Level 0 DFD)
It describes the essence of ESAS. The major flows between the system and the stakeholder (i.e. assistant) is shown below.

![dfd0](https://github.com/edenau/HKSchool-Exam-Sitting-Arrangement/raw/master/figures/dfd0.png)
*Input:*
- Information of each student
- Basic setting
- Information of each exam paper
Output:
- Arrangement result

### Decomposition (Level 1 DFD)
ESAS was divided into five stages:
- 1.0 Update basic setting
- 2.0 Analyze information of students
- 3.0 Update exam information
- 4.0 Arrange exam
- 5.0 Request arrangement result

There are five various data stored:
- D1 Basic setting
- D2 Subject taken for each student
- D3 Information of each paper
- D4 Timetable
- D5 Seat allocation for each exam

The following level 1 DFD constructs the main idea of the system after decomposing it into several processes.

![dfd1](https://github.com/edenau/HKSchool-Exam-Sitting-Arrangement/raw/master/figures/dfd1.png)

### Interface Logic Flow
The following diagram displays the logic flow on user interface. There should also be one of the options that provide a pathway for user to exit the sysetm if they want to.

![dfd_interface](https://github.com/edenau/HKSchool-Exam-Sitting-Arrangement/raw/master/figures/dfd_interface.png)

<div class="breaker"></div> <a id="stage-design"></a>

# Stage Design
## Stage 1.0 - Update basic setting
For arranging an exam, the first thing to do is to know all the subjects available. Note that no output is required for this stage. The input contains both short term (maximum 4 characters) and full name of each elective subject available, specified by the user. Since the input involved loads of data, user is recommended to insert this information using a csv file in advance.

*Input:*
- All available elective subjects’ name (both full name & short term not exceeding 4 characters) in csv file

For the format, the first column with all short terms (not exceeding 4 characters) of available elective subjects, with the second column all relevant full names of those elective subjects.

![fig2](https://github.com/edenau/HKSchool-Exam-Sitting-Arrangement/raw/master/figures/fig2.png)

Subfunctions are are illustrated below, just go through the code or run the system for more information.
<table>
  <tr><th colspan="4">Update basic setting</th></tr>
  <tr><th>Show current elective subjects available</th><th colspan="3">Modify subjects available</th></tr>
  <tr><th></th><th>Add an elective subject</th><th>Delete an elective subject</th><th>Delete all subjects</th></tr>
</table>

## Stage 2.0 - Analyze information of students
For analyzing information of students, loads of scattered student information (from Form 4-6) regarding elective subjects they took are inputted by the user. This stage tries to tidy up those data. The following input involves thousands of data, and user must insert this kind of information using a csv file.

*Input:*
- Ungrouped subjects taken by students (in any order, containing all Form 4-6 students) - in csv file

For the format, the first column contains the class which the student belongs to, carrying 2 characters – the form number at the front and the class (latin) letter at the back. The second column contains the class number of the student. The third column contains the full name of him/her. The fourth column contains a short term of an elective subject that the student is taking. Note that almost each student studies more than 1 subjects, a student will occupy more than 1 row of the data in this csv file.

![fig3](https://github.com/edenau/HKSchool-Exam-Sitting-Arrangement/raw/master/figures/fig3.png)

*Output:*
- 3 tidied student information files (one for each form) – in csv files, see the graph below
- 3 configuration files for use in other stages (one for each form) - in ini files

![fig4](https://github.com/edenau/HKSchool-Exam-Sitting-Arrangement/raw/master/figures/fig4.png)

The data rows are ordered in alphabetical order of class, followed by ascending order of class number, with each row consisting all information of a single student. The first column is a character showing the form (the form number should be the same inside the same file). The second column is a latin character showing the class of the student. The third column is the class number of the student and the fourth column is the full name of the student. From the fifth to eighth column, it stores some short term of elective subjects that the student is currently taking. Not all cells are filled since not all students take 4 subjects. The leftmost column is preferentially filled than the rightmost, as it is clearly demonstrated in the above example.

Regarding the configuration file, here is an example.
```
0110000001000
0101000001000
0010010000000
1100000000000
0010010000000
0010010000000
0110000000000
0100010000000
```
The data rows are ordered the same way as the previous csv file output. In an ini file, the number of rows equals to the number of students in that form. Each row represents the status of a single student. The number of column should be equivalent to the number of elective subjects available. Each column represents different subjects. The order of columns follows the order of subjects arranged inside “all_sub.csv”. That means a certain character is showing if a student takes a certain subject or not, with ‘0’ meaning they don't take it whereas ‘1’ means they do.

*File handling:*
<table>
  <tr><th>File Name</th><th>I/O</th><th>Description</th></tr>
  <tr><td>ele_list.csv</td><td>Input</td><td>Ungrouped information about who takes what subjects (in any order, containing all Form 4-6 students)</td></tr>
  <tr><td>stu_info_4.csv</td><td>Output</td><td>Tidied student information file for Form 4</td></tr>
  <tr><td>stu_info_5.csv</td><td>Output</td><td>Tidied student information file for Form 5</td></tr>
  <tr><td>stu_info_6.csv</td><td>Output</td><td>Tidied student information file for Form 6</td></tr>
  <tr><td>sub_rec_4.ini</td><td>Output</td><td>Configuration file for use in other stages for Form 4</td></tr>
  <tr><td>sub_rec_5.ini</td><td>Output</td><td>Configuration file for use in other stages for Form 5</td></tr>
  <tr><td>sub_rec_6.ini</td><td>Output</td><td>Configuration file for use in other stages for Form 6</td></tr>
</table>

## Stage 3.0 -	Update exam information
Simple stuff. No external files involved in input.
*Input:*
- Time (in minutes) needed for each paper

![fig5](https://github.com/edenau/HKSchool-Exam-Sitting-Arrangement/raw/master/figures/fig5.png)

*Output:*
- Time (in minutes) needed for each paper - in csv file

![fig6](https://github.com/edenau/HKSchool-Exam-Sitting-Arrangement/raw/master/figures/fig6.png)

They are all pretty self explanatory.

*File handling:*
<table>
  <tr><th>File Name</th><th>I/O</th><th>Description</th></tr>
  <tr><td>sub_time.csv</td><td>Output</td><td>Time (in minutes) needed for each paper</td></tr>
</table>

## Stage 4.0 - Arrange exam
*Input:*
- “sub_rec_4.ini”, “sub_rec_5.ini”, and “sub_rec_6.ini” – the configuration files made in the previous stage

The task here is to set a timetable with minimal subject crashes. To avoid having so many students need to have different subject exams in consecutive days, subject crash should be calculated separately for each form seprately in order to obtain an optimum solution for each specific form. The optimal solution should be able to let most of the students have enough time for revision during exam break.

### Quantitative Method
Given *n* subjects available, let the queue be the following:
<table>
  <tr><th>S<sub>1</sub></th><th>S<sub>2</sub></th><th>S<sub>3</sub></th><th>...</th><th>S<sub>n-2</sub></th><th>S<sub>n-1</sub></th><th>S<sub>n</sub></th></tr>
</table>
where S stands for subject. The maths comes from the following:

![fig7](https://github.com/edenau/HKSchool-Exam-Sitting-Arrangement/raw/master/figures/fig7.png)

The best solution is the *argmin* of the coefficient function.

*Output:*
- 3 timetable list (one for each form) – in txt files
For example, in our simulation, the output is
<table>
  <tr><td>PHY</td><td>BAFS</td><td>BIO</td><td>HIST</td><td>CHIS</td><td>M2</td><td>M1</td><td>VA</td><td>GEOG</td><td>CLIT</td><td>ICT</td><td>ECON</td><td>CHEM</td></tr>
</table>

*File handling:*
<table>
  <tr><th>File Name</th><th>I/O</th><th>Description</th></tr>
  <tr><td>sub_rec_4.ini</td><td>Input</td><td>Configuration file for subject crash optimization in Form 4</td></tr>
  <tr><td>sub_rec_5.ini</td><td>Input</td><td>Configuration file for subject crash optimization in Form 5</td></tr>
  <tr><td>sub_rec_6.ini</td><td>Input</td><td>Configuration file for subject crash optimization in Form 6</td></tr>
  <tr><td>timetable_4.txt</td><td>Output</td><td>Timetable for Form 4</td></tr>
  <tr><td>timetable_5.txt</td><td>Output</td><td>Timetable for Form 5</td></tr>
  <tr><td>timetable_6.txt</td><td>Output</td><td>Timetable for Form 6</td></tr>
</table>

## Stage 5.0 - Allocate seat and produce final arrangement
*Input:*
- “sub_rec_4.ini”, “sub_rec_5.ini”, “sub_rec_6.ini” - the configuration files made in previous stage
- “stu_info_4.csv”, “stu_info_5.csv”, “stu_info_6.csv” - the tidied student information files made in previous stage
- “timetable_4.txt”, “timetable_5.txt”, “timetable_6.txt” - the timetables made in previous stage

*Output:*
- Seat allocation forms for a specific subject in a specific form – in csv files
- Seat allocation forms for a specific student – in csv files

For the first output, each column (from left to right) contains seat number, the number representing their current form, class, class number, and their full name respectively.

![fig8](https://github.com/edenau/HKSchool-Exam-Sitting-Arrangement/raw/master/figures/fig8.png)

For the second output, each column (from left to right) contains the day number, the paper (in short term), the seat number on that paper, and the duration of that exam respectively.

![fig9](https://github.com/edenau/HKSchool-Exam-Sitting-Arrangement/raw/master/figures/fig9.png)

*File handling (input):*
<table>
  <tr><th>File Name</th><th>I/O</th><th>Description</th></tr>
  <tr><td>sub_rec_4.ini</td><td>Input</td><td>Configuration file for subject crash optimization in Form 4</td></tr>
  <tr><td>sub_rec_5.ini</td><td>Input</td><td>Configuration file for subject crash optimization in Form 5</td></tr>
  <tr><td>sub_rec_6.ini</td><td>Input</td><td>Configuration file for subject crash optimization in Form 6</td></tr>
  <tr><td>stu_info_4.csv</td><td>Input</td><td>Tidied student information file for Form 4</td></tr>
  <tr><td>stu_info_5.csv</td><td>Input</td><td>Tidied student information file for Form 5</td></tr>
  <tr><td>stu_info_6.csv</td><td>Input</td><td>Tidied student information file for Form 6</td></tr>
  <tr><td>timetable_4.txt</td><td>Output</td><td>Timetable for Form 4</td></tr>
  <tr><td>timetable_5.txt</td><td>Output</td><td>Timetable for Form 5</td></tr>
  <tr><td>timetable_6.txt</td><td>Output</td><td>Timetable for Form 6</td></tr>
</table>

*File handling (output):*
<table>
  <tr><th colspan="4">ESAS</th></tr>
  <tr><th colspan="4">seat_allocation</th></tr>
  <tr><th>4</th><th>5</th><th>6</th><th>personal</th></tr>
  <tr><td>[subject].csv</td><td>[subject].csv</td><td>[subject].csv</td><td>[identity].csv</td></tr>
</table>

Inside the folder of ESAS, there is a folder “seat_allocation”, with 4 different folders inside, as shown above. [subject] represents multiple files, with each one representing each subject. Similarly, [identity] represents multiple files for every student.

<div class="breaker"></div> <a id="remarks"></a>

# Remarks
Special thanks to my high school ICT teacher - Drizzt Wong for his insight and assistance.
