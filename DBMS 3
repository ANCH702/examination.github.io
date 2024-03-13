CREATE TABLE ACTOR(
    Act_id INT PRIMARY KEY,
    Act_name VARCHAR(20) NOT NULL,
    Act_gender CHAR
);

INSERT INTO actor VALUES(101,'James Stewart','M'),
(102,'Deborah Keir','F'),(103,'Peter Otoole','M'),
(104,'Robert De Niro','M'),(105,'F.Mirray Abraham','M'),
(106,'Harrison Ford','M'),(107,'Nicole Kidman','F'),
(108,'Stephen Baldwin','M'),(109,'Jack Nicholsen','M'),
(110,'Mark Wahlberg','M');
SELECT * FROM actor;

CREATE TABLE DIRECTOR(
    Dir_id INT PRIMARY KEY,
    Dir_name VARCHAR(20) NOT NULL,
    Dir_phone INT(12)
);

INSERT INTO director VALUES (201,'Alfard Hitchcock',987654001),
(202,'Jack Claython',987654002),(203,'David Lean',987654003),
(204,'Michael Cimino',987654004),(205,'Milos Forman',987654005),
(206,'Ridley Scott',987654006),(207,'Stanley Kubrick',987654007),
(208,'Bryan Singer',987654008);

SELECT * FROM director;

CREATE TABLE MOVIES(
    Mov_id INT PRIMARY KEY,
    Mov_title VARCHAR(30),
    Mov_year INT,
    Mov_lang VARCHAR(10),
    Dir_id INT,
    CONSTRAINT dir_dirid_fk FOREIGN KEY(Dir_id)REFERENCES DIRECTOR(Dir_id)
);

INSERT INTO movies VALUES(901,'Vertigo',2000,'English',205),
(902,'The Innocents',1961,'English',204),(903,'Lawrence of Arabia',1998,'English',203),
(904,'The Deer Hunter',2001,'English',202),(905,'Amadeus',2002,'English',201),
(908,"The Usual Suspects",2010,"English",208),(909,"Chinatown",2010,"English",207),
(910,"Boogie Nights",2015,"English",206),
(911,"Annie hall",2016,"English",205),
(912,"Princess Mononoke",2017,"Japanese",204);

SELECT * FROM movies;

CREATE TABLE MOVIE_CAST(
    Act_id INT,
    Mov_id INT,
    Role VARCHAR(20),
    CONSTRAINT mc_actid_movid_pk PRIMARY KEY(Act_id, Mov_id),
    CONSTRAINT mc_actid_fk FOREIGN KEY(Act_id) REFERENCES ACTOR(Act_ID),
    CONSTRAINT mc_movid_fk FOREIGN KEY(Mov_ID) REFERENCES MOVIES(Mov_id)
);

INSERT INTO movie_cast VALUES 
(101,901,"John Scottle Ferguse"),
(101,902,"Ellie Adams"),
(101,910,"Eddie Adams"),
(102,901,"Eddie Adams"),
(102,902,"Miss Giddens"),
(102,904,"Jack Paul"),
(103,903,"T.E.Lawrence"),
(104,904,"Michael"),
(105,905,"Antonio Salieri"),
(105,910,"Smith"),
(108,908,"McManus"),
(109,911,"James Bond"),
(110,912,"Peter Park");

SELECT * FROM movie_cast;

CREATE TABLE RATING (
    Mov_id INT,
    Rev_stars INT,
    CONSTRAINT r_movid_revstars_pk PRIMARY KEY(Mov_id,Rev_stars),
    CONSTRAINT r_movid_fk FOREIGN KEY (Mov_id)REFERENCES MOVIES (Mov_id)
);

INSERT INTO rating VALUES(901,1),(901,2),(901,3),(901,4),(901,5),(902,3),(903,2),(904,0),(905,5),(908,1),(909,2),(910,3);

SELECT * FROM rating;


--Query 1

SELECT  Mov_title 
FROM    MOVIES 
WHERE   Dir_id IN ( SELECT Dir_id
                    FROM DIRECTOR
                    WHERE Dir_name LIKE '%Hitchcock');


--Query 2

SELECT  DISTINCT Mov_title 
FROM    MOVIES 
WHERE   Mov_id IN ( SELECT Mov_id 
                    FROM MOVIE_CAST 
                    WHERE Act_id IN (   SELECT Act_id
                                        FROM MOVIE_CAST 
                                        GROUP BY Act_id
                                        HAVING COUNT(Mov_id )>1));


--Query 3

SELECT A.Act_name
FROM ACTOR A
JOIN MOVIE_CAST C ON A.Act_id = C.Act_id
JOIN MOVIES M ON C.Mov_id = M.Mov_id
WHERE M.Mov_year < 2000 OR M.Mov_year > 2015
GROUP BY A.Act_name
HAVING COUNT(DISTINCT CASE WHEN M.Mov_year < 2000 THEN M.Mov_id END) > 0
AND COUNT(DISTINCT CASE WHEN M.Mov_year > 2015 THEN M.Mov_id END) > 0;


--Query 4

SELECT      MOVIES.Mov_title,RATING.Mov_id,SUM(RATING.Rev_stars) 
            AS Num_stars,MAX(Rev_stars) AS Max_star
FROM        MOVIES RIGHT OUTER JOIN RATING 
            ON MOVIES.Mov_id=RATING.Mov_id
GROUP BY    RATING.Mov_id ;


--Query 5

UPDATE  RATING 
SET Rev_stars = 5
WHERE   Mov_id IN(  SELECT Mov_id 
                    FROM MOVIES 
                    WHERE Dir_id IN(    SELECT Dir_id 
                                        FROM DIRECTOR 
                                        WHERE Dir_name='Steven Spielberg'));

SELECT * FROM rating;

