CREATE TABLE employees (
employeeid int,
firstname CHAR(255),
lastname CHAR(255),
PRIMARY KEY(employeeid)
);
Insert into employees values(1, 'bonnie', 'zhang');
Insert into employees values(2, 'david', 'kong');
Insert into employees values(3, 'jessie', 'yu');


CREATE TABLE provinces (
provinceid int,
province char,
PRIMARY KEY(provinceid)
);
Insert into provinces values(1, 'BC');
Insert into provinces values(2, 'AB');


CREATE TABLE addresses (
addressid int,
employeeid int, 
address char,
city char,
provinceid int, 
postalcode char,
moveindate date,
PRIMARY KEY(addressid),
FOREIGN KEY (employeeid) REFERENCES employees(employeeid),
FOREIGN KEY (provinceid) REFERENCES provinces(provinceid)
);

Insert into addresses values(5, 1, 'kingsway', 'vancouver',1,'p1','2019-05-06');
Insert into addresses values(2, 2, 'nelson ave','burnaby',1,'p2','2019-10-01');
Insert into addresses values(3, 2, 'royal oak ave','burnaby',1,'p3','2019-12-02');

SELECT *
FROM employees e
LEFT JOIN (
SELECT employeeid, address, city, province, postalcode, moveindate
FROM addresses a
LEFT JOIN provinces p ON p.provinceid = a.provinceid
GROUP BY employeeid
HAVING moveindate = MAX(moveindate)
) AS temp ON e.employeeid = temp.employeeid
