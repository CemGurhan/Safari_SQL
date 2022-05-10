# Task - Safari Park

We've now seen how to set up and query a relational database, including how to set up links between our tables and query many tables at once. Now we'll put it into practice by building a database to store data in a specific format.

Usually we'll have a back-end application handling user input and communicating with the database. It will receive that input in a format known as **JSON** (**J**ava**S**cript **O**bject **N**otation) and determine how to structure a query based on that. In this task we will provide you with sample data which you should use as a guide to set up the tables. 

## The Data

Our users will be able to enter details for the animals in the park, their enclosures and the staff working there. Each member of staff will be assinged to a different enclosure on a different day; each enclosure will have more than one person looking after it. The data entered by the user will look like this:

```json
// animal

{
	"id": 1,
	"name": "Tony",
	"type": "Tiger",
	"age": 59,
	"enclosure_id": 1
}

// enclosure

{
	"id": 1,
	"name": "big cat field",
	"capacity": 20,
	"closedForMaintenance": false
}

// staff

{
	"id": 1,
	"name": "Captain Rik",
	"employeeNumber": 12345,
}

// assignment

{
	"id": 1,
	"employeeId": 1,
	"enclosureId": 1,
	"day": "Tuesday"
}
```

## MVP

- Draw an entity relationship diagram to show the structure of the tables and the relationships between them. Each table should have enough columns to capture all the data shown in the JSON above.






- Set up the tables in a postgres database. You can set them up using the `psql` REPL, a GUI like Postico or PGAdmin or by writing an SQL file like the one in the previous task.
	
	CREATE TABLE staff (
	
	id SERIAL PRIMARY KEY,
	name VARCHAR(255),
	employeeNumber INT );

	
	CREATE TABLE animals (
	
	id SERIAL PRIMARY KEY,
	name VARCHAR(255),
	type VARCHAR(255),
	age INT,
	enclosure_id INT REFERENCES enclosure(id);

	CREATE TABLE ENCLOSURES (
	
	id SERIAL PRIMARY KEY,
	name VARCHAR(255),
	capacity INT,
	closedForMaintenance INT);

	CREATE TABLE assignment (
	
	id SERIAL PRIMARY KEY,
	employeeID INT REFERENCES staff(id),
	enclosureID INT REFERENCES enclosures(id),
	day VARCHAR(255));



- Populate the tables with some of your own data (you don't need to use more cereal mascots, unless you want to). Don't worry about the capacity restriction on enclosures for now, checking the would be handled by the back-end before the data gets sent to the database.

INSERT INTO enclosures (name,capacity,closedformaintenance) VALUES ('reptiles',10,0);
INSERT INTO enclosures (name,capacity,closedformaintenance) VALUES ('birds',10,0);

INSERT INTO animals (name,type ,age, enclosure_id) VALUES ('dragon','reptile',100,2);
INSERT INTO animals (name,type ,age, enclosure_id) VALUES ('eagle','bird',100,2);
INSERT INTO animals (name,type ,age, enclosure_id) VALUES ('tortoise','reptile',100,1);
INSERT INTO animals (name,type ,age, enclosure_id) VALUES ('turtle','reptile',100,1);
INSERT INTO animals (name,type,age,enclosure_id) VALUES ('snake','reptile',23,1);
INSERT INTO animals (name,type,age,enclosure_id) VALUES ('robin','bird',11,2);

INSERT INTO staff (name, employeenumber) VALUES (1, 12345);
INSERT INTO staff (name, employeenumber) VALUES ('Captain Rik', 12345);
INSERT INTO staff (name, employeenumber) VALUES ('Will turner', 56456);
INSERT INTO staff (name, employeenumber) VALUES ('Davy Jones', 45345);
INSERT INTO staff (name, employeenumber) VALUES ('Captain Barbossa', 43562);
INSERT INTO staff (name, employeenumber) VALUES ('Sao Feng', 58923);


INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 1, 2, 'Monday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 2, 2, 'Monday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 3, 2, 'Monday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 4, 1, 'Monday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 5, 1, 'Monday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 6, 1, 'Monday');

INSERT INTO assignment (employeeid,enclosureid,day) VALUES (1,1,'Tuesday');
INSERT INTO assignment (employeeid,enclosureid,day) VALUES (2,1,'Tuesday');
INSERT INTO assignment (employeeid,enclosureid,day) VALUES (3,1,'Tuesday');
INSERT INTO assignment (employeeid,enclosureid,day) VALUES (4,2,'Tuesday');
INSERT INTO assignment (employeeid,enclosureid,day) VALUES (5,2,'Tuesday');
INSERT INTO assignment (employeeid,enclosureid,day) VALUES (6,2,'Tuesday');

INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 1, 2, 'Wednesday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 2, 2, 'Wednesday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 3, 2, 'Wednesday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 4, 1, 'Wednesday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 5, 1, 'Wednesday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 6, 1, 'Wednesday');





INSERT INTO assignment (employeeID, enclosureid, day) VALUES ( 1, 1, 'Thursday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 2, 1, 'Thursday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 3, 1, 'Thursday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 4, 2, 'Thursday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 5, 2, 'Thursday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 6, 2, 'Thursday');






INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 1, 2, 'Friday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 2, 2, 'Friday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 3, 2, 'Friday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 4, 1, 'Friday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 5, 1, 'Friday');
INSERT INTO assignment (employeeID, enclosureID, day) VALUES ( 6, 1, 'Friday');



- Write queries to find:
	- The names of the animals in a given enclosure

	


	- The names of the staff working in a given enclosure

	
## Extensions

Write queries to find:

- The names of staff working in enclosures which are closed for maintenance

SELECT DISTINCT employeeid FROM assignment INNER JOIN enclosures ON  assignment.enclosureid = enclosures.id WHERE enclosures.closedformaintenance = 1; 




- The name of the enclosure where the oldest animal lives. If there are two animals who are the same age choose the first one alphabetically.

SELECT animals.name FROM enclosures INNER JOIN animals ON enclosures.id = animals.enclosure_id WHERE age = (SELECT MAX(age) FROM animals) ORDER BY animals.name;



- The number of different animal types a given keeper has been assigned to work with.

SELECT COUNT (DISTINCT type) FROM animals INNER JOIN assignment ON animals.enclosure_id=assignment.enclosureid WHERE assignment.employeeid = 1 ;


- The number of different keepers who have been assigned to work in a given enclosure

SELECT COUNT (DISTINCT employeeid) FROM assignment WHERE enclosureid = 1;

- The names of the other animals sharing an enclosure with a given animal (eg. find the names of all the animals sharing the big cat field with Tony)

SELECT name FROM animals WHERE enclosure_id = (SELECT enclosure_id FROM animals WHERE name = 'snake') AND NOT animals.name = 'snake';



## Hints

- Don't be tempted to perform too many joins, think about the data stored in each table.
- Remember that the result of a join is just another table. It can be filtered, ordered, summarised and joined again as much as you need.