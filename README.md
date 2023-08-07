# Analyzed-the-data-of-movie-rental-store-for-growth-in-business.-Tool---SQL-
# Use sakila database in RDBMS and apply querying, data mining, transformation and basic statistical methods and techniques along with data # manipulation languages (DML), associations, and subqueries. 
-- -------------------------------- CAPStone Project --1 (SQL)-------------------------------- --

use sakila;

-- TASK-1 ---- Displaying full names of all the actors in the database.------
select concat (first_name , " " , last_name) 
as Full_name_of_actors 
from actor;

-- Task-2(i) --- Displaying the number of times each first name appears.------ 
select first_name, count(first_name) 
from actor 
group by first_name;

-- Task-2(ii) ---- Displaying the names and the count of actors who have unique first name-----
select first_name, count(first_name) 
from actor 
group by first_name 
having count(first_name)= 1;

 -- TASK-3(i) ---- Displaying the number of times each last name appears -----
select last_name, count(last_name) 
from actor 
group by last_name;

-- TASK-3(ii)----- Displaying all the unique last names ----
select last_name
from actor
group by last_name 
having count(last_name)= 1;

-- TASK-4(i) ------ Displaying all  te records of the movies with the rating R------
select film_id, title, rating
 from film 
 where rating ='R';
 
 -- TASK -4(ii) ----- Displaying all the records of the movies where rating is not 'R' ------
select *
from film 
where rating !='R';

-- TASK-4(iii) --- Displaying all the records of the movies for age group 13 and below (G= for all age groups, PG= Parental Guidence needed, PG-13= Parental Guidence needed for children below 13) -------
select *
from film 
where rating = 'G' or rating ='PG' or rating = 'PG-13'; 

-- TASK-5(i) ---- Displaying the records of the movies who have replacement cost less than $11 -------
select film_id, title, replacement_cost
from film 
where replacement_cost <= 11; 

-- TASK-5(ii) ------- Displaying the records of the movies who have replacement cost between $11 and $20----
select film_id, title, replacement_cost
from film 
where replacement_cost 
between 11 and 20 ; -- (here $11.00 and $20.00 are excluded)

-- TASK-5(iii) ------ Displaying all the movies records with decreasing order of their replacement cost-----
select film_id, title, replacement_cost
from film 
order by  replacement_cost desc;

-- TASK-6--- Displaying the names of top 3 movies  with the greatest number of actors----
select count(actor_id) , film_actor.film_id,film.title 
from film_actor join film 
on film.film_id=film_actor.film_id 
group by film_actor.film_id 
order by count(actor_id) desc 
limit 3;

 -- TASK-7 ---- Displaying the names of the movies where names start with 'K' and 'Q---------
 select film_id, title 
 from film 
 where title 
 like 'K%' or title 
 like 'Q%';

-- TASK-8 --- Displaying the names of the actors who played in movie Agent Truman-----
select film_id, title from film where title ='Agent Truman'; -- we have first found the film_id of the movie which is 6---
select actor_id, first_name, last_name 
from actor 
where actor_id 
in (select actor_id 
from film_actor 
where film_id=6);

-- TASK-9 ----- Displaying all the movcies categorized as family movies------------
select category_id, name from category where name='Family';
select film_id from film_category where category_id=8;
select film_id, title 
from film 
where film_id 
in (
select film_id 
from film_category 
where category_id=8);

-- TASK-10(i) ---- Displaying max, min and average of rental rates based on ratings-------
select rating, max(rental_rate),min(rental_rate),avg(rental_rate) 
from film 
group by rating 
order by avg(rental_rate) desc;
 
-- TASK-10(ii) ----- Displaying the movies in decending order based on there rental frequencies.----------
select film_id,title, rental_duration 
 from film 
 order by rental_duration desc;

-- TASK-11------ Displaying the film where the difference between the average film replacement cost and average rental rate is greater than $15 
select category_id, count(category_id) as 'No. of movie category',name,(
select avg(replacement_cost) 
from film 
join film_category 
on film.film_id=film_category.film_id) 
as 'Replacement_cost',(
select avg(Rental_rate) 
from film 
join film_category 
on film.film_id=film_category.film_id) as 'Rental_Rate'
from category
group by category_id
having (Replacement_cost-Rental_rate)>15;

-- TASK-12 ----Displaying the film categories where the number of movies is greater than 70------
select category.name, film_category.category_id,count(film_id) as 'No. of movies'
from category
join film_category
on film_category.category_id=category.category_id
group by film_category.category_id 
having count(film_id)>70 
order by count(film_id);
