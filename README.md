# voterDatabaseApplicationREADME

## Utilizes 
### - SQL 
### - PHP 
### - HTML 
### - CSS 

A full-stack web application accessing an SQL database containing North Carolina voter registration data records. The database containing over 350,000 data points, each with 51 possible individual data attributes . The web application allows front-end mutation of data within the database, including updates, insertions, and deletions, while also providing tools to efficiently locate and view specific data points. Raw code is not available to the public due to this project being academic. 

## Demonstration of web application functions: 

## Front page 

![](voterDataImages/Screenshot(74).png)

## Searching a voter in a database 

![](voterDataImages/Screenshot(75).png)

## The following pictures demostrate inserting a new voter and their information into the database (some information can be left blank if chosen, however some information is essential and will not allow an insert without it) 

![](voterDataImages/Screenshot(76).png)
![](voterDataImages/Screenshot(77).png)
![](voterDataImages/Screenshot(78).png)
![](voterDataImages/Screenshot(79).png)
![](voterDataImages/Screenshot(80).png)
![](voterDataImages/Screenshot(81).png)
![](voterDataImages/Screenshot(82).png)

## Now we will search for the fictional voter we just inserted into the database (Luke Skywalker)

![](voterDataImages/Screenshot(83).png)

## Now we will demostrate deleting the voter we just inserted. 

![](voterDataImages/Screenshot(84).png)
![](voterDataImages/Screenshot(85).png)

## We can see that the voter is no longer in the database

![](voterDataImages/Screenshot(86).png)

## Later additions include the ability to do the following 
### - View districts of a voter 
### - See the time and date of the last update (insert, update, or delete)
### - Update a specific voter's address
### - View the recent history of changes to the database 
### - View new voters in the database (voters inserted in the last 30 days) 

![](voterDataImages/Screenshot(128).png)






Description of the Normaliztion process to acheive 3NF:   <br/> <br/> 
	The data as taken from the MECKNC source was consistent with having only one piece of information in each cell. The data as taken also did not have any repeating columns, in the sense that the columns displayed the exact same information or were titled the same. Therefore, as taken from the source, the data when loaded into the made tables was already in First Normal Form. It was divided into the unique attributes as listed : precinct_desc, party_cd, race_code, ethnic_code, sex_code, age, pct_portion, first_name, middle_name, last_name, name_suffix_lbl, full_name_mail, mail_addr1, mail_addr2, mail_city_state_zip, house_num, street_dir, street_name, street_type_cd, street_sufx_cd, unit_num, res_city_desc, state_cd, zip_code, registr_dt, voter_reg_num, status_cd, municipality_desc, ward_desc, cong_dist_desc, super_court_desc, nc_senate_desc, nc_house_desc, county_commiss_desc, school_dist_desc, E1, E1_date, E1_VotingMethod, E1_PartyCd, E2, E2_Date, E2_VotingMethod, E2_PartyCd, E3, E3_Date, E3_VotingMethod, E3_PartyCd, E4, E4_Date, E4_VotingMethod, E4_PartyCd, E5, E5_Date, E5_VotingMethod, and  E5_PartyCd.    <br/> <br/> 
	Firstly, getting the table into 1NF was the first priority. The problem was that the columns E1, E2, and so on were identical to each other, and 1NF does not allow repeating columns. A new table was created, with a single column for the election number labeled E, a single column labeled VothingMethod, and a single column labeled PartyCd. The primary key is the combination of voter_reg_num and E. This table is named election. Since the date of the election is dependent solely on E, another table was made. This contains E as the primary key, and has an additional attribute E_date. This table is named edate.<br/> <br/> 
The effort of normalization to 3NF began by splitting the information into logically appropriate tables, from which minimal key dependencies could be identified. Firstly, the most obviously related information was the geographical information. The mail_addr1 and mail_addr2 were identified as the determining factors for the rest of the information related to address, so they in combination would be labeled the primary key for a new table containing all attributes that are dependent on those two attributes. This table will be called address, as all the attributes are dependent on geography specified by the mail_addr1 and mail_addr2. These attributes include : mail_addr1, mail_addr2, street_dir, street_name, street_type_cd, and street_sufx_cd.<br/> <br/> 
  Mail_city_state_zip, res_city_desc, and state_cd are dependent on the attribute zip_code. That is why they cannot be included in the table address, they must be in their own table with zip_code the primary key of that table. One issue however is that all these attributes are dependent on zip_code or mail_city_state_zip, since both have a one to one relationship to all other attributes. This does not meet 3NF form since attributes are dependent on more than just primary key. 
Therefore we will need two tables for all of this information. The first table will be called zipcode and will contain zip_code as the primary key. The only other attribute will be mail_city_state_zip. Zip_code the attribute determines mail_city_state_zip for all but 28 anomalies when queried. The second table will be called city_state and will have mail_city_state as the primary key. The other two attributes in city_state will be res_city_desc and state_cd. Now, these tables are in 3NF.
House_num and unit_num seem like they would be associated with the addresses, however, a single mailing address could have varying house numbers and unit numbers. This is proven by an A->B dependency query grouping by addresses and returning tuples that do not have a distinct house or unit numbers. Also logically, with multiple unit homes it makes sense that some homes would share a single address with different home or unit numbers. There are 823 addresses (mail_addr1 and mail_addr2 in conjunction) that have more than one house_num associated. There are 823 addresses (mail_addr1 and mail_addr2 in conjunction) that have more than one house_num. There are 175 addresses (mail_addr1 and mail_addr2 in conjunction) that have more than one unit_num associated. Considering the house_num and unit_num are not dependent on the geography table primary key of mail_addr1 and mail_addr2, then they cannot go in that table. The only attribute they could be dependent on is the voter_reg_num. 
  <br/> <br/> The next new table will contain the attributes off of full_name_mail. Full_name_mail will be the primary key for this table, called name. These attributes are first_name, middle_name, last_name, and name_suffix_lbl. These three attributes are not dependent on each other or any other attributes except for the full_name_mail. The dependency between these attributes has been queried. It is an A->B dependency between the full_name_mail and each other the attributes. There are exceptions, however, they usually are the result of extreme coincidence and are very low. For example, there are only two full_name_mail instances that result in multiple first name associations, and they are the result of the user not entering in one of the other two names, which creates strange scenarios. We can group these anomalies into the miscellaneous table since they number so low. <br/> <br/> 
  The next table that will be made will start with having pct_portion as the primary key. The next address will be precinct_desc. The precinct_desc is always a string of ???PCT??? followed by a space and then the first three digits of the pct_portion. There is only one exception to this, and that exception will of course be moved to the miscellaneous table. Following this, every attribute describing a particular ???district??? will be added to this table. This includes, municipality_desc, ward_desc, cong_dist_desc, super_court_desc, nc_senate_desc, nc_house_desc, county_commiss_desc, and school_dist_desc. All of these attributes are dependent on the pct_portion. This can be tested with a query to see if any of the pct_portions have more than one of a certain district. Any exclusions are anomalies. The only problem with this is that all of the districts could be dependent on either the pct_portion or the precinct_desc as they are one to one, unique, and wholly synonymous. Therefore, this table will have to be split into two tables. The first table will contain pct_portion as the primary key and will have precinct_desc as the only dependent attribute. This table will be called pct. Then, the second table will have precinct_desc as the primary key and contain all the rest of the district attributes, municipality_desc, ward_desc, cong_dist_desc, super_court_desc, nc_senate_desc, nc_house_desc, county_commiss_desc, and school_dist_desc. This table will be called districts. This way 3NF standards will be met. <br/> <br/> 
  Lasty, we will need a table for all the attributes that depend on only the voter_reg_num attribute, which will be the primary key of this table. This table will also include all other primary keys that so far are not dependent on any other attributes. This table will include the attributes, pct_portion as a foreign key, status_cd, registr_dt, party_cd, race_code, ethnic_code, sex_code, age, full_name_mail as a foreign key, mail_addr1, mail_addr2 as a foreign key when paired with mail_addr1, zip_code as a foreign key, house_num, and unit_num. This table will be called voter.

 
