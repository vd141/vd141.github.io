---
title: Database Case Study
date:
  created: 2026-08-04
tags:
  - OMSCS
---

# Databases
*For my first summer term in OMSCS, I completed the Database Systems Concepts and Design course! I was curious about backend data management after taking Machine Learning for Trading in the spring. This class definitely delivered on that front and provided me with a solid foundation in databases and database management systems. In this post, I'll describe how I designed an application database from scratch. I undertook this task in 3 phases: analysis, design, and implementation. Along the way, I learned how to think about building a consistent database application from first principles, how to work with teammates from different countries and time zones, built my first ever web application, and gained experience designing the full stack.*

## *The Customer* 
PowerShare is a nonprofit organization that wishes to accumulate data regarding households in the United States,
specifically around alternative power sources and other household properties. 

## *The Requirements*
The requirements document contained descriptions of data that PowerShare wanted to collect from users, as well as mockups of the web application. Household, appliance, and power generation data was collected from the user and displayed in reports. The data collected was “open” in the sense that any user of the application was able to submit their data. Conversely, any user could browse the selected set of reports available on the PowerShare website.

In industry, this document is generated with the help of various stakeholders: product managers, architects, developers, QA, UX/UI designers, and DevOps, among other roles. Luckily for us, the requirements document abstracted away countless meetings and change requests, freeing up valuable summertime hours. :)

## Analysis
I used several tools to capture the results of our analysis: an information flow diagram, extended entity relationship (EER) diagram, attribute tables, task decompositions, and abstract code. It's quite a mouthful, but the main point of these tools is to help organize the planning before any code has been written. The information flow diagram is the highest-level diagram and maps each application functionality with the type(s) of database interaction. The EER diagram captures all entities associated with the database and describes their attributes and relationships with each other. Attribute tables summarize the data types and business constraints for attributes associated with each entity. Finally, the task decomposition associates each spoke of the information flow diagram with a piece of abstract code. The abstract code is a language-agnostic representation of the application's functionality. It captures the core logic of teh application code, so its intent can be implemented in any language or framework. 

The analyis is useful for distilling the functionality of the web application from a database perspective. It is also easier to further refine requirements with stakeholders by using a common language, such as these tools. The implementation phase adds a level of complexity that is best tackled after analysis has been conducted. Most importantly about the three-phase process is that it is quite iterative. I found myself amending previous diagrams/tables as we went deeper towards the implementation. 

Collected data could be ingested in several formats such as decimals, integers, and strings. Some data had additional constraints, such as zip codes being limited to five digits or latitude/longitude values being limited to their respective measurements in degrees. Validating the input data before it was added to the database was important, as incompatible data could seriously affect the quality and accuracy of the reports. 
This analysis process is rigorous, and is best suited towards applications that depend on database management systems. These types of applications tend to manage large volumes of data, provide access to multiple users concurrently, enforce constraints

## Design
With most of the high-level thinking completed in the analysis phase, I refined these ideas in the design phase. The abstract code was updated with syntactical SQL queries. I replaced the attribute tables with entity relationship maps, which is just a fancy way of describing a diagram of schemas connected by their foreign keys. 

The most significant deliverable in the design phase was a .sql script containing definitions of schemas and constraints in the DBMS. Each table had its own CREATE statement. It also contained DROP statements preceding the CREATE statements, so it could be used to completely wipe an existing database. For our project, I chose PostgreSQL due to its popularity, which would invariably enable us to use the ample documentation, forum posts, and Youtube tutorials to our advantage. Going forward, I think this is a great way to go about building a personal project from scratch. I'd recommend using more niche/bespoke solutions when the problem calls for it and when there is quality developer support. This could come in the form of thorough documentation or having access to the developers of the solution.

## Implementation
Now, my favorite part. This was where the rubber hit the road. I had never built a web application before, so I was curious to see how the process would unfold. The teaching staff had suggested some popular stacks, such as LAMP or WAMP. We decided that a Linux-Flask-Python-PostgreSQL (LFPP) would allow us to build out the core functionality while reducing uneeded complexity. The app would be demo'ed by one of us at the final presentation, so a beefier framework was not necessary.

Some important context that I considered at the onset was that each developer would be contributing to the application from their own development environment. Between the four of us, we used Linux, Mac, and Windows operating systems. So configuration management was important. I also considered the idea of extending functionality in the future. It's certainly easier to do that when less time is spent fiddling around with software packages. To this end, I used a Poetry package manager, although a simple pip package manager could have been used as well. To bundle up the entire application (including Postgres), I could have used a Docker container, but this also would have added complexity that wasn't necessary. The application used basic Postgres functionality, so pretty much any Postgres version that was available for download as of July 2026 would have been fine. 

For version control, we used git and a shared github repo. The tried and true combination.

With configuration out of the way, I 