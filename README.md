# INDIA_General_Elections_2024-SQL-PYTHON
***📌 Overview***:
Tools Used:
SQL (via Jupyter Notebook), Python, Pandas, MySQL,. A comprehensive data analysis project of the 2024 Indian General Elections, conducted using SQL within Jupyter Notebook alongside Python libraries.
The project focuses on uncovering insights from state-wise and constituency-wise results to analyze party and alliance performance.


![INDIA_General_Elections](https://github.com/user-attachments/assets/27a672c4-8596-4250-af33-588db565d109)


**Description** :
1. Executed this project using SQL within Jupyter Notebook, integrated with Python.

2. Performed SQL-based data analysis in Jupyter Notebook using Python and SQLAlchemy for seamless database interaction.

3. Implemented the entire project using SQL queries inside Jupyter Notebook with Python for analysis and visualization.

***🧰 Tools & Technologies***

1.Python (Jupyter Notebook)

2.MySQL

3.SQLAlchemy

4.Pandas


***📂 Database Structure & Tables***

1. Partywise_results
Stores all political parties and their details.

***Columns***: Party ID, Party, Total Seats, Total Votes, party_alliance (added later)

2. Constituencywise_results
Contains candidate-level results per constituency.

***Columns***: Candidate Name, Party ID, Parliament Constituency, EVM Votes, Postal Votes, Total Votes

3. State_results
Maps constituencies to their respective states.

***Columns***: Parliament Constituency, State ID, Result Status

4. States
List of all Indian states and their IDs.

***Columns***: State ID, State



***✅ Key Insights Extracted***

1. Seats won by each political alliance: I.N.D.I.A, NDA, and OTHER

2. Top candidates with the highest EVM Votes across constituencies

3. State-wise seat share comparison

4. Party performance across different regions

5. Use of SQL logic like JOINs, GROUP BY, CASE WHEN, and Subqueries


***CODING***

*** State-wise seat distribution by alliance***
```
SELECT s.State AS state_name,
  SUM(CASE WHEN pr.party_alliance = 'I.N.D.I.A' THEN 1 ELSE 0 END) AS INDIA_Seats_Won,
  SUM(CASE WHEN pr.party_alliance = 'NDA' THEN 1 ELSE 0 END) AS NDA_Seats_Won,
  SUM(CASE WHEN pr.party_alliance = 'OTHER' THEN 1 ELSE 0 END) AS OTHER_Seats_Won
FROM partywise_results pr
JOIN constituencywise_results cr ON cr.`Party ID` = pr.`Party ID`
JOIN state_results sr ON sr.`Parliament Constituency` = cr.`Parliament Constituency`
JOIN states s ON s.`State ID` = sr.`State ID`
GROUP BY s.State
ORDER BY s.State;
```



***Which candidate received the highest number of EVM votes in each constituency (Top 10)?***
```
SELECT cr.`Parliament Constituency`, cr.`Candidate Name`, cr.`EVM Votes`
FROM constituencywise_results cr
JOIN (
    SELECT `Parliament Constituency`, MAX(`EVM Votes`) AS max_votes
    FROM constituencywise_results
    GROUP BY `Parliament Constituency`
) AS top_votes
ON cr.`Parliament Constituency` = top_votes.`Parliament Constituency`
AND cr.`EVM Votes` = top_votes.max_votes
ORDER BY cr.`EVM Votes` DESC
LIMIT 10;
 ```


***For the state of Maharashtra, what are the total number of seats, total number of candidates, total number of parties, total votes (including EVM and postal), and the breakdown of EVM and postal votes?***
```
select count(distinct cr.`Constituency ID`),
 count(distinct cd.`Candidate`),
 count(distinct pr.`party`),
 sum(cd.`EVM Votes` + cd.`Postal Votes`),
 sum(cd.`EVM Votes`),
 sum(cd.`Postal Votes`)
 from constituencywise_results as cr
 join partywise_results as pr 
 on cr.`Party ID` = pr.`Party ID`
 join state_results as sr
 on cr.`Parliament Constituency`=sr.`Parliament Constituency`
 join states as s
 on s.`State ID` = sr.`State ID`
 join constituencywise_details as cd
 on cd.`Constituency ID` = cr.`Constituency ID` 
 where s.State='Maharashtra'; ``` 


***What is the total number of seats won by each party alliance (NDA, I.N.D.I.A, and OTHER) in each state for the India Elections 2024***

``` select s.State as state_name ,
sum(
case 
when pr.party_alliance="I.N.D.I.A" THEN 1 ELSE 0 END ) AS INDIA_Seats_Won,
Sum(
case 
when pr.party_alliance="NDA" THEN 1 ELSE 0 END ) AS NDA_Seats_Won,
sum(
case 
when pr.party_alliance="OTHER" THEN 1 ELSE 0 END ) AS OTHER_Seats_Won
From partywise_results as pr
join constituencywise_results aS cr
on cr.`Party ID`=pr.`Party ID`
join state_results as sr
on sr.`Parliament Constituency` = cr.`Parliament Constituency`
join states as s
on s.`State ID`=sr.`State ID`
GROUP BY 1
order by 1;
 ```



*** Which parties won the most seats in s State, and how many seats did each party win?***
```
select pr.`party`,
count(cr.`Constituency ID`)
from partywise_results as pr
join constituencywise_results aS cr
on cr.`Party ID`=pr.`Party ID`
join state_results as sr
on sr.`Parliament Constituency` = cr.`Parliament Constituency`
join states as s
on s.`State ID`=sr.`State ID`
where s.State="Andhra Pradesh"
group by 1
order by 2 desc;
 ```

***  What is the distribution of EVM votes versus postal votes for candidates in a specific constituency?*** 

``` 
SELECT 
    cd.`EVM Votes`,
    cd.`Postal Votes`,
    cd.`Candidate`,
    cd.`Total Votes`,
    cr.`Constituency Name`
FROM
    constituencywise_details AS cd
        JOIN
    constituencywise_results AS cr ON cr.`Constituency ID` = cd.`Constituency ID`
WHERE
    cr.`Constituency Name` = 'ETAH'
ORDER BY cd.`Total Votes` DESC;
 ```


***✅ Conclusion***


1. The India General Elections 2024 Result Analysis provides a detailed, data-driven understanding of the electoral outcomes using SQL and Python. By querying across multiple related tables (partywise_results, constituencywise_results, constituencywise_details, state_results, and states), this project successfully answers key political and statistical questions, including:

2. Alliance-wise performance across the nation — I.N.D.I.A, NDA, and OTHER — was clearly established by categorizing parties and aggregating seat counts by state.

3. Top 10 candidates by EVM votes were identified, giving insight into the most voter-supported individuals in the election.

4. EVM vs. postal vote distribution in individual constituencies (e.g., ETAH) was analyzed, revealing voting trends and turnout types.

5. State-specific insights (e.g., Maharashtra and Andhra Pradesh) showed total seats, candidate count, parties, and vote types — aiding regional performance analysis.

6. Alliance-level comparisons across states helped visualize the political landscape and how voter preferences shifted geographically.

7. This analysis demonstrates the power of structured relational databases and SQL querying within a Python Jupyter environment to derive valuable political insights from complex election datasets.


