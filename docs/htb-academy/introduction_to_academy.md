# Introduction to Academy

## Introduction

HTB Academy's goal is to provide a highly interactive and streamlined  learning process to allow users to have fun while learning. 

The structure of the Academy is as follows:

```shell-session
Path
  ├── Module_1
  │     └── Sections
  │
  ├── Module_2
  │     └── Sections
  │
  ├── Module_3
  │     └── Sections
  │
  └── Module_4
        └── Section
```

### Path

A path is a set of modules making up a broader topic. Some paths share modules, especially fundamental topics.

### Module

A module has the focus on a specific topic. A topic can be of wider range and specific. They can be standalone or part of a path. Different paths can have the same module. Some modules have a final skill assessment that has to be taken to finish the module.

Modules are split in 5 different tiers.

- Tier 0 - This modules are free
- Tier 1 - cost **50** cubes, upon finishing rewards **10** cubes back
- Tier 2 - cost **100** cubes, upon finishing rewards **20** cubes back
- Tier 3 - cost **500** cubes, upon finishing rewards **100** cubes back
- Tier 4 - cost **1000** cubes, upon finishing rewards **200** cubes back

> Tier 0,1,2 are included in the student subscription

A Module is marked complete once all the sections are marked as completed. This includes the Interactive sections of the module, meaning you need to submit the correct answers.

### Section

A module is broken down to sections. They are the smallest learning block. We have to finish all sections to finish the module and eventually finish the path. The sections names make up the Table of Contents on the right side of the web-page

#### Interactive Sections

Interactive sections have a small cube next to them. To finish such sections, you have to answer some questions at the end. The answers are in the section text or can be obtained solving task given to you. Solving interactive sections awards you with cubes. Those cubes can be used as a currency to get other modules. 

####  Interactive Sections with Targets

Interactive sections can have targets. This targets are used to get the answers and complete the section. There are different types of targets.

##### Docker Target

Docker targets are spawn pretty quick and are publicly accessible (no VPN needed)

##### VM Target

VM targets are full operating systems, like Linux or Windows, in a virtual machine. It takes longer to spawn them up and they are only accessible with VPN or HTB's own Pwnbox. 

![image-20200102140344890](/opt/Documentation/HTB Academy/structure2.png)

