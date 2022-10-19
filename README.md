# ContactOut data-engineering challenge
## Part 1: ETL

### Description
---
ContactOut has decided that they want to learn more about their staff members by understanding the types of songs and the artists of those songs they listen to via logs and files. Please create a database to store this data.

* This data will be useful in helping ContactOut reach some of its analytical goals, for example, finding the most popular songs or times of the day which many staff listen to music.

### Requirements
---
* Look at the logs and song_data and design a schema for the database.

* Load the data into the database

* Answer the questions:
  - What time is most popular for staff to listen to music
  - Rank of songs by popularity
  - Rank of artists by popularity
  - Most popular song & artist per user

## Part 2: SQL

### Description
---
At ContactOut we have a lot of data from job postings. Write a query to print the `company_name`, `industries`, `title`, `salary_range`, `posted_date`, `job_functions` for each job post. Exclude any job posts that do not have a salary range.

### Input formats
The schemas  for each of the tables can be found in [schemas](./schemas)

### Sample output
| company_name | industries                                                                             | title                              | salary_range                    | posted_date | job_functions                             |
|--------------|----------------------------------------------------------------------------------------|------------------------------------|---------------------------------|-------------|-------------------------------------------|
| Microsoft    | Computer Hardware,Computer Software,Information Technology and Services                | Senior Program Manager             | $116,000.00/yr - $173,000.00/yr | 2021-08-24  | Information Technology,Project Management |
| Microsoft    | Computer Hardware,Computer Software,Information Technology and Services,Consumer Goods | Principal Hardware Program Manager | $172,000.00/yr - $258,000.00/yr | 2021-09-12  | Engineering                               |
| Microsoft    | Computer Hardware,Computer Software,Information Technology and Services                | Senior Software Engineer           | $150,000.00/yr - $182,000.00/yr | 2021-09-12  | Engineering,Information Technology        |
| Microsoft    | Computer Hardware,Computer Software,Information Technology and Services                | Senior Program Manager             | $131,000.00/yr - $211,000.00/yr | 2021-09-11  | Information Technology,Project Management |
| Microsoft    | Computer Hardware,Computer Software,Information Technology and Services,Consumer Goods | Senior Program Manager             | $131,000.00/yr - $229,000.00/yr | 2021-10-26  | Information Technology,Project Management |
| Microsoft    | Computer Hardware,Computer Software,Information Technology and Services                | Senior Software Engineer           | $94,900.00/yr - $127,000.00/yr  | 2021-09-12  | Engineering,Information Technology        |
| Microsoft    | Computer Hardware,Computer Software,Information Technology and Services                | Software Engineering Manager       | $135,000.00/yr - $183,000.00/yr | 2021-09-12  | Engineering,Information Technology        |
| Microsoft    | Computer Hardware,Computer Software,Information Technology and Services                | Senior Software Engineer           | $94,600.00/yr - $129,000.00/yr  | 2021-10-11  | Engineering,Information Technology        |
| Microsoft    | Computer Hardware,Computer Software,Information Technology and Services                | Senior Software Engineer           | $117,000.00/yr - $196,000.00/yr | 2021-09-07  | Engineering,Information Technology        |
| Microsoft    | Computer Hardware,Computer Software,Information Technology and Services,Consumer Goods | Senior Program Manager             | $132,000.00/yr - $201,000.00/yr | 2021-10-18  | Information Technology,Project Management |