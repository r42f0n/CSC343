java c
CSC343, Fall 2024
Assignment 3
Due:  Wednesday, November 27 by 4:00pm
IMPORTANT: A summary of key clarifications for this assignment will be provided in an FAQ on Piazza if needed. Check it regularly for updates. Both the FAQ and any Quercus announcements are required reading.
Learning Goals
By the end of this assignment you should be able to:
•  identify tradeoffs that must be made when designing a schema in the relational model, and make reasonable choices,
•  express a schema in SQL’s Data Definition Language,
•  identify limitations to the constraints that can be expressed using the DDL,
•  identify and document constraints that would require assertions or triggers to implement (but you will NOT write any assertions or triggers in this assignment),
•  appreciate scenarios where the rigidity of the relational model may force an awkward design,
•  generate example data to populate the schema you design,
•  formally reason about functional dependencies, and
•  apply functional dependency theory to database design.
This assignment is more open-ended that the other two assignments in the course.  This is on purpose.  We will be looking for you to follow the guiding principles below, but there is no one “right” answer we are looking for.
Part 1: Informal Relational DesignIn class, we are in the middle of learning about functional dependencies and how they are used to design relational schemas in a principled fashion.  After that, we will learn how to use Entity-Relationship diagrams to model a domain and come up with a draft schema which can be normalized according to those principles.  By the end of term you will be ready to put all of this together, but in the meantime, it is instructive to go through the process of designing a schema informally. We expect that what you are learning in class helps inform. your decisions in this assignment, but we do not expect that your schema is normalized in the ways we discuss in lecture. Instead, you should aim to make good design decisions, informed by what we are learning in class, and you should document the choices you are making and why.
The domainYou are working on a proof-of-concept version of a database to track information about airports and the flights and passengers that travel through them.  Below is the information that this database needs to be able to store.  There is much more to be added later, but this is not your responsibility.
•  Airports have names and locations.
•  Airlines operate flights that fly on routes between two airports.  Each flight must be assigned a plane and a certain number of flight crew.
•  Planes cannot be on two flights at the sametime, and require at least an hour between flights.  A plane cannot have a flight that departs from a different airport than where it last arrived (unless it is its first flight).
•  Flight crew cannot work more than 14 hours of each 24 hour period.  Flight crew must be certified on the type of plane for each flight they are assigned to.
•  Passengers travel on flights,  and must have a ticket issued to them for each flight they take.   A group of passengers can travel together on the same booking, but they must all have their own ticket.  Passengers can have an assigned seat or travel standby, where they have a ticket for a route but not a particular flight. Seats can only be assigned to a maximum of one passenger per flight.  Passengers under the age of 12 must travel with at least one other passenger age 18 years or older on that booking.
• If a flight route is between two different countries, passengers must have a valid visa that allows them to enter that country, unless they are a citizen of that country.  Passengers can purchase a ticket without showing proof of a valid visa, but they cannot board a flight until the airline has confirmed proof of valid visa.
•  Passengers can travel with checked luggage, with a maximum of two pieces of luggage with a combined weight of 50kg per flight.  “Elite” status passengers can have a maximum of four pieces of luggage with a combined weight of 100kg per flight.
•  Passengers board and deplane from flights at gates.  Each gate must be staffed by a ground crew employee of the airline for departures, but this is optional for arrivals.  Only one plane can be at a gate at a time.  Planes arrive at a gate at least 30 minutes before a departure and remain there for at least 30 minutes after an arrival. If the same plane is carrying two flights that end/begin within 2 hours of each other, then it should arrive at/depart from the same gate for those flights.
•  Passengers can board flights by zone, based which is recorded as part of their ticket.  Passengers with “elite” sta- tus board first, passengers travelling on a booking with children under 6 board second, and standby passengers board last. Everyone else boards between the second group and the standby group.Some features above are not realistic, but they simplify your assignment.  (For example, we assume all flights happen as scheduled and on-time.) If we have not constrained something, assume it is unconstrained. For example, if we said houses have windows but didn’t constrain it further, you should be prepared that a house may have no windows, 1 window, or many windows.
Define a schema
Your first task is to construct a relational schema for our domain, expressed in DDL. Write your schema in a file called schema.ddl.
As you know, there are many possible schemas that satisfy the description above.  There is no single right answer we are looking for. Instead, we are looking to see how the schema you choose deals with the principles below.
We aren’t following a formal design process for Part 1, so instead follow as many of these general principles as you can when choosing among options:
1.  Avoid redundancy.
2.  Avoid designing your schema in such a way that there are attributes that can be NULL.
3. If a constraint given above in the domain description can be expressed without using assertions or triggers, then it should be enforced by your schema, unless you can articulate a good reason not to do so.
4.  There may be additional constraints that make sense but were not specified in the domain description.  You get to decide on whether to enforce any of these in your DDL.
You may find there is tension between some of these principles.  Where that occurs, prioritize in the order shown above. Use your judgment to make any other decisions. Additional requirements:
•  Define appropriate constraints, i.e.,


–  Define a primary key for every table.
–  Use UNIQUE if appropriate to further constrain the data.
–  Define foreign keys as appropriate.
–  For each column, add a NOT  NULL constraint unless there is a good reason not to.
•  All constraints associated with a table must be defined either in the table definition itself, or immediately after it.
•  To facilitate repeated importing of the schema as you correct and revise it, begin your DDL file with our standard three lines:
DROP  SCHEMA  IF  EXISTS  A3Airport  CASCADE;
CREATE  SCHEMA  A3Airport;
SET  SEARCH_PATH  TO  A3Airport;
You may create IDs, or define additional columns if you feel this is appropriate. Use your best judgment.There may be things we didn’t specify that you would like to know.  In a real design scenario, you would ask your client or domain experts.  For this assignment, make reasonable assumptions.  Keep track of these in writing, as we will ask you to articulate them at the top of your DDL file.
Document your choices and assumptions
At the top of your DDL file, include a comment that answers these questions:
Could not: What constraints from the domain specification could not be enforced without assertions or triggers, if any?  (Again, you are not writing any assertions or triggers on this assignment.)
Did not: What constraints from the domain specification could have been enforced without assertions or triggers, but were not enforced, if any? Why not?
Extra constraints: What additional constraints that we didn’t mention did you enforce, if any?
Assumptions: What assumptions did you make?
Instance and queriesOnce you have defined your schema, create a file called data .sql that inserts data into your database with the schema you designed above.  This file should insert the data necessary to have the queries described below produce results as described.  You will also need to insert more data of your own choosing.  Yo代 写CSC343, Fall 2024 Assignment 3Python
代做程序编程语言u may find it instructive to consider this data as you are working on the design.Note: If you have not already developed some automated processes for generating data, we encourage you to do that now.  Writing a Python script. or using some other automated process to generate the data that matches your schema is a great idea.  A reminder that, as is stated in the syllabus, the only generative AI tool you should be sharing course materials with is UofT’s Copilot instance.  It is OK to use Copilot to help you generate the data here; you will still need to verify it though.
Then, write queries to do the following:
1.  For each airport, report the name of the airport and total number of passengers that travelled through that airport in 2023. The result should include at least three airports, with one having at least 50 passengers, one with 0 passengers, and one with somewhere between 0 and 50 (exclusive).  It is OK to have more airports in your result.
2.  Find the passenger(s) who travelled on the same route the most times in 2023.  Report the passenger name and the route. The result should contain at least 5 routes, at least one of which has a tie.


3.  Find routes between two different countries where the average number of passengers requiring visas is less than 25% of the average number of passengers requiring visas over all routes between two different countries.  Report where these routes depart from and arrive at, and their average number of passengers requiring a visa.  The result should contain at least two routes where the average is not 0.
4.  For each airline, find crew that worked with the same plane at least 10 times in 2023, either as flight crew on a flight or as ground crew at an arrival or departure gate.  Report the airline, crew member, plane, and the number of times that crew member worked with that plane.We will not be autotesting your queries, so you have latitude regarding details like attribute types and output format.  Make good choices to ensure that your output is easy to read and understand.  You do not need to worry about how your queries handle corner cases.Write your queries in files called q1 .sql through q4 .sql.  Download file runner.txt, which has commands to import each query one at a time.  Once all your queries are working, start postgreSQL, import runner.txt, and cut and paste your entire interaction with the PostgreSQL shell into a plain text file called demo .txt. We will assess the correctness of your queries based on reading demo.txt, so it must show the results of the queries.  There is no need to insert into tables (since we are not autotesting). We will also be looking at your queries to make sure they access the appropriate tables you defined in your schema.
There will be lots of notices, like:  Eg.  INSERT 0 1, psql:q2.sql:16:  NOTICE: view  “blah” This is normal, and we are expecting to see it.
What to hand in for Part 1
Hand in plain text files schema.ddl, data.sql, and q1 .sql through q4.sql, and demo .txt.  These must be plain text files.
IMPORTANT: You must include the demo file, and it must show the output of your queries, or you will get zero for this part of the assignment.
How Part 1 will be markedPart 1 will be graded for design quality, including: whether it can represent the data described above, appropriate enforcement of the constraints described, avoiding redundancy, avoiding unnecessary NULLs, following the priorities given for any tradeoffs that had to be made, and good use of DDL (choice of types, NOT NULL specified wherever appropriate, etc.) Your queries will be assessed for correctness, as described above.
Your code will also be assessed for these qualities:
•  Names:  Is every name well chosen so that it will assist the reader in understanding the code quickly?  This includes table, view, and column names.
•  Comments:Does every table or view have a comment above it specifying clearly exactly what a row means?  Together, the comments and the names should tell the reader everything they need to know in order to use a table or view. For views in particular, comments should describe the data (e.g., “The student number of every student who has passed at least 4 courses.”) not how to find it (e.g., “Find the student numbers by self-joining ...”).
•  Formatting according to these rules:
–  An 80-character line limit is used.  This is important so your submission is readable in MarkUs for the graders
–  Keywords are capitalized consistently, either always in uppercase or always in lowercase.
–  Table names begin with a capital letter and if multi-word names, use CamelCase.
–  attribute names are not capitalized.
–  Line breaks and indentation are used to assist the reader in parsing the code.
Part 2: Functional Dependencies, Decompositions, and Normal FormsIn your answers for this part, please list all attributes in final relations and FDs for each part in alphabetical order. This will make it easier for the graders to read your answers. Within each individual FD, this means stating an FD as XY → ABC, not as Y X → BCA. Also, list the FDs in alphabetical order ascending according to the left-hand side, then by the right-hand side. This means, WX → A comes before WXZ → A which comes before WXZ → B. You could also combine FDs with the same LHS in your final answer.
1.  Consider a relation R1  with attributes DEFGHIJK with functional dependencies S1 :
S1  = { D → FG,   E → HK,   F → EIJ,   F → K }
(a)  State which of the given FDs violate BCNF.
(b)  Use the BCNF decomposition algorithm to obtain a lossless and redundancy-preventing decomposition of relation R1  into a collection of relations that are in BCNF. Make sure it is clear to the reader which relations are in the final decomposition, and don’t forget to project the dependencies onto each relation in that final decomposition.  Because there are choice points in the algorithm, there may be more than one correct answer. List the final relations in alphabetical order.
(c)  Does your schema preserve dependencies? Explain how you know that it does or does not.
(d)  Use the Chase Test to show that your schema is a lossless-join decomposition.  (This is guaranteed by the BCNF algorithm, but it’s a good exercise.)
2.  Consider a relation R2  with attributes JKLMNOPQ and functional dependencies S2 .
S2  = { JLM → N,    K → LM,    KN → JLO,    M → JKO,    N → JL }
(a)  Compute a minimal basis for S2 . In your final answer, put the FDs into alphabetical order. (b)  Using your minimal basis from the last subquestion, compute all keys for R2 .
(c)  Employ the 3NF synthesis algorithm to obtain a lossless and dependency-preserving decomposition of relation R2  into a collection of relations that are in 3NF. Do not  “over normalize” . This means that you should combine all FDs with the same left-hand side to create a single relation.  If your schema includes one relation that is a subset of another, remove the smaller one.
(d)  Does your schema allow redundancy? Explain how you know that it does or does not.
Show all of your steps so that we can give part marks where appropriate.  There are no marks for simply a correct answer. You must justify every shortcut that you take.
What to hand in for Part 2
Type your answers up using a tool like LaTeX or Word.  Hand in your typed answers, in a single pdf file called a3.pdf. Handwritten submissions will not be accepted and will receive a grade of 0.
Final Thoughts
Submission: Check that you have submitted the correct version of your files by downloading it from MarkUs; new files will not be accepted after the due date.Forming groups: Make sure you have created your group in MarkUs as soon as you decide to work with a partner. Make sure you both see the group as expected. The deadline for getting assistance from us in creating your groups is Monday, November 25. We strongly recommend you create your group before then.
Marking: The marking scheme will be approximately this: Part 1 70%, and Part 2 30%.Some parting advice: It will be tempting to divide the assignment up with your partner.  Remember that both of you probably want to answer all the questions on the final exam.  A reminder from the syllabus:  “If you choose to work with a partner, we expect that you are working together on all parts of the assignment. If you choose to work with a partner in another way, you are responsible for all issues that arise from splitting up the work.”



         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
