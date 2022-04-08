https://sqlzoo.net/wiki/More_JOIN_operations

2) Give year of 'Citizen Kane'. 
```
Select yr
from movie
where movie.title = 'Citizen Kane'
```
3) List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.  
```
SELECT id, title, yr
FROM movie
WHERE title LIKE '%Star Trek%'
order by yr
```
4) What id number does the actor 'Glenn Close' have?  
```
select id
from actor 
where name = 'Glenn Close'
```
5) What is the id of the film 'Casablanca' 
```
select id
from movie 
where title = 'Casablanca'
```
6) Obtain the cast list for 'Casablanca'.what is a cast list?The cast list is the names of the actors who were in the movie.
```
select name
from actor
where id in 
 (
  select actorid
  from casting 
  where movieid in
   (
    select id
    from movie
    where movie.title = 'Casablanca'
   )
 )
```
8) List the films in which 'Harrison Ford' has appeared 
```
select title
from movie
where id in
  (
    select movieid
    from casting
    where actorid in
      (select id from actor where name = 'Harrison Ford')
  )
```
9) List the films where 'Harrison Ford' has appeared - but not in the starring role. [Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role] 
```
select title
from movie
where id in
  (
    select movieid
    from casting
    where actorid in
      (select id from actor where name = 'Harrison Ford')
    and ord != 1
  )
```
10) List the films together with the leading star for all 1962 films.  
```
select movie.title, actor.name
from casting 
inner join movie on movie.id = casting.movieid
inner join actor on actor.id = casting.actorid
where casting.ord = 1
and movie.yr = 1962
```
11) Which were the busiest years for 'Rock Hudson', show the year and the number of movies he made each year for any year in which he made more than 2 movies. 
```
select yr, count(yr) from movie where id in(select movieid from casting where actorid in (select id from actor where name= 'Rock Hudson')) group by yr having count(yr) > 2
```
12) List the film title and the leading actor for all of the films 'Julie Andrews' played in.
```
select mov.title, act.name
from casting as cast
inner join actor act on act.id = cast.actorid
inner join movie mov on mov.id = cast.movieid
where 
mov.id in (select movieid from casting where actorid in (select id from actor where name =  'Julie Andrews'))
and
cast.ord = 1
```
13) Obtain a list, in alphabetical order, of actors who've had at least 15 starring roles
```
select act.name
from casting as cast
inner join actor as act on act.id = cast.actorid
inner join movie as mov on mov.id = cast.movieid
where cast.ord = 1
group by act.name
having count(*) > 14
```
14) Obtain a list, in alphabetical order, of actors who've had at least 15 starring roles
```
select (select title from movie where id = cast.movieid ) as title , count(cast.movieid) 
from casting as cast
inner join actor as act on act.id = cast.actorid
inner join movie as mov on mov.id = cast.movieid
where mov.yr = 1978
group by cast.movieid
order by count(cast.movieid) desc, title
```
15) List all the people who have worked with 'Art Garfunkel'. 
```
select distinct name 
from actor
where id in (select actorid from casting where movieid in
(select movieid
from casting
where actorid in (select id from actor where name = 'Art Garfunkel')))
and 
name != 'Art Garfunkel'
```
