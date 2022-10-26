# ContactOut data-engineering challenge

## Part-1 ETL
* **Look at the [log_data](./data/log_data) and [song_data](./data/song_data) and design a schema for the database.**
    * [Schema Diagram can be viewed here](https://dbdiagram.io/d/63568eb3fa2755667d5d8c58)
    * Following conventions for db and tables are used: 
        * Database : contactout
            * Tables : logs, song

        * ![DB Schema](resources/README/dbschema.png)

        * Table : logs(contactout.logs)
            * artist    text
            * auth  varchar
            * firstName text
            * gender varchar
            * iteminSession int
            * lastName text
            * length float
            * level varchar
            * location text
            * method varchar
            * page varchar
            * registration varchar
            * sessionId int
            * song text
            * status int
            * ts float
            * userAgent text
            * userId int

        * Table : songs(contactout.songs)
            * num_songs int
            * artist_id text
            * artist_latitude float
            * artist_longitude float
            * artist_location text
            * artist_name text
            * song_id text
            * title text
            * duration float
            * year int
            
        
        * Relationships
            * contactout.logs(artist, song) has many to one relationship with contactout.songs(artist_name, title)


    * Reasoning and Explanations
        * Why is there no primary key and foreign key relationship between the tables?
            * contactout.logs and contactout.songs has following overlapping keys:

            * | contactout.logs | contactout.songs |
                |----|-----|
                |artist|artist_name|
                |length|duration|
                |song|title|
            
            * artist_name,title together can identify rows uniqely in contactout.songs(an artist can have multiple songs, multiple artists can have same song name, so only the pair (artist_name,title) happens to be unique)
            * However (artist,song) cannot uniquely identify a row because user can listen to single song multiple times.
            * That's why contactout.logs has many to one relationship with contactout.songs
            * Foreign key relationship is not possible because we will have songs table(parent table with (artist_name, title) as PRIMARY KEY) but there are some pair of (artist, song) in logs table which are not present in songs table and hence the validity constraint of Foreign key doesn't stand.

* **Load the data into the database.** 
    * Please refer to the [jupyter notebook](eda.ipynb)

* **Answer the questions:**
    * Please refer to the same [jupyter notebook](eda.ipynb)

* **E[xplanation Recording](https://drive.google.com/drive/folders/1rXP6txtH5Eiy4CjmlLH_jHwD1vBCXCvx?usp=sharing)**



## Part-2 SQL
* **[Explanation Recording](https://drive.google.com/drive/folders/1rXP6txtH5Eiy4CjmlLH_jHwD1vBCXCvx?usp=sharing)**

* Kindly run the following commands:
```sql
SELECT
    company_name, industries, title, salary_range, posted_date, job_functions
FROM linkedin_jobs_sample
WHERE salary_range is NOT NULL;

SELECT * FROM linkedin_industry_sample;

ALTER TABLE linkedin_jobs_sample ADD COLUMN industries_names TEXT;
ALTER TABLE linkedin_jobs_sample ADD COLUMN job_function_names TEXT;


SELECT company_name,
       industries_names as industries,
       title, salary_range, posted_date,
       job_function_names as job_functions
       FROM linkedin_jobs_sample
WHERE salary_range is NOT NULL;
```

* RIGHT After this let's run the following procedures

```sql
DROP PROCEDURE IF EXISTS getIndustryNames;
CREATE PROCEDURE getIndustryNames()
    BEGIN
        DECLARE finished INTEGER DEFAULT 0;
        DECLARE industry_names_array VARCHAR(200) DEFAULT '';

        DECLARE industries_array TEXT DEFAULT '';
        DECLARE const_industries_array TEXT DEFAULT '';
        DECLARE single_id TEXT DEFAULT '';
        DECLARE single_industry TEXT DEFAULT '';


        DECLARE cur_linkedin_jobs_sample
            CURSOR FOR
                SELECT industries FROM linkedin_jobs_sample;
        #WHERE id = 498714606;

        DECLARE CONTINUE HANDLER FOR NOT FOUND SET finished = 1;

        OPEN cur_linkedin_jobs_sample;
        #####1
        #SELECT '1';
        #####
        getIndustries: LOOP
            FETCH cur_linkedin_jobs_sample INTO industries_array;
            FETCH cur_linkedin_jobs_sample INTO const_industries_array;

            #####1-1-2
            #SELECT const_industries_array;
            #####

            IF finished = 1 THEN
                LEAVE getIndustries;
            END IF;

            SET industries_array = SUBSTRING(industries_array,2,length(industries_array)-2);
            #{4,5,6}
            #4,5,6 <= 2(4) to 6


            SET industry_names_array = '';
            #####1-1-3
            SELECT industries_array as above_loop;
            #####
            WHILE industries_array != '' DO
                SET single_id = SUBSTRING_INDEX(industries_array, ',', 1); #4
                ###1-1-4
                #SELECT single_id;
                ###

                SET single_industry = (SELECT name FROM linkedin_industry_sample WHERE id = single_id);

                ###1-1-5
                #SELECT single_industry;
                ###

                SET industry_names_array = CONCAT(single_industry, ',', industry_names_array);

                ###1-1-5
                #SELECT industry_names_array;


                IF LOCATE(',', industries_array) > 0 THEN
                    SET industries_array = SUBSTRING(industries_array, LOCATE(',', industries_array) + 1);
                ELSE
                    SET industries_array = '';
                END IF;

            END WHILE;

            UPDATE linkedin_jobs_sample
                SET industries_names = industry_names_array
            WHERE linkedin_jobs_sample.industries = const_industries_array;
#               WHERE linkedin_jobs_sample.industries = industries_array;



        end loop getIndustries;

END;
```

```sql
DROP PROCEDURE IF EXISTS getJobNames;
CREATE PROCEDURE getJobNames()
    BEGIN
        DECLARE finished INTEGER DEFAULT 0;
        DECLARE job_names_array VARCHAR(200) DEFAULT '';

        DECLARE jobs_array TEXT DEFAULT '';
        DECLARE const_jobs_array TEXT DEFAULT '';
        DECLARE single_id TEXT DEFAULT '';
        DECLARE single_job TEXT DEFAULT '';


        DECLARE cur_linkedin_jobs_sample
            CURSOR FOR
                SELECT job_functions FROM linkedin_jobs_sample;
        #WHERE id = 498714606;

        DECLARE CONTINUE HANDLER FOR NOT FOUND SET finished = 1;

        OPEN cur_linkedin_jobs_sample;
        #####1
        SELECT '1';
        #####
        getJobs: LOOP
            FETCH cur_linkedin_jobs_sample INTO jobs_array;
            FETCH cur_linkedin_jobs_sample INTO const_jobs_array;

            #####1-1-2
            SELECT const_jobs_array;
            #####

            IF finished = 1 THEN
                LEAVE getJobs;
            END IF;

            SET jobs_array = SUBSTRING(jobs_array,2,length(jobs_array)-2);
            SET job_names_array = '';
            #####1-1-3
            SELECT jobs_array as above_loop;
            #####
            WHILE jobs_array != '' DO
                SET single_id = SUBSTRING_INDEX(jobs_array, ',', 1);
                ###1-1-4
                SELECT single_id;
                ###

                SET single_job = (SELECT name FROM linkedin_industry_sample WHERE id = single_id);

                ###1-1-5
                SELECT single_job;
                ###

                SET job_names_array = CONCAT(single_job, ',', job_names_array);

                ###1-1-5
                SELECT job_names_array;


                IF LOCATE(',', jobs_array) > 0 THEN
                    SET jobs_array = SUBSTRING(jobs_array, LOCATE(',', jobs_array) + 1);
                ELSE
                    SET jobs_array = '';
                END IF;

            END WHILE;

            UPDATE linkedin_jobs_sample
                SET job_function_names = job_names_array
            WHERE linkedin_jobs_sample.job_functions = const_jobs_array;
#               WHERE linkedin_jobs_sample.industries = industries_array;



        end loop getJobs;

END;
```

Once those procedures are ran, let's call them

```sql
CALL getIndustryNames();
CALL getJobNames();
```

Run this command to see the final results
```

SELECT company_name,
       industries_names as industries,
       title, salary_range, posted_date,
       job_function_names as job_functions
       FROM linkedin_jobs_sample
WHERE salary_range is NOT NULL;
```

[Final results can also be found here](results/sql_query_result.csv)

