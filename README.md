#Dallas Couty 2020 Voter Registration

> All data downloads are at opensource.org at the link: http://openrecords.org/stories/downloads.html

#FOR FOREIGN WHITE HATS ONLY
http://openrecords.org/stories/WhiteHats2.html

>If you don't know what a "White Hat" is then pick another article. What follows is for hard core computer geeks outside the US who want to replicate our work.

##TARGET AUDIENCE AND MISSION
>We've been contacted by individuals from India, Ireland, China, and Serbia regarding election management. This document is a tutorial for you. We apologize that we are unable to provide instructions in all these beautiful languages, but we must default to English.

>We provide this document to you White Hats trying to clean up your country's elections. The tutorial includes the dataset for 1.4 million Dallas County, Texas voters and directions for finding the approximately 107,000 hacked votes.

>It is up to worldwide White Hats to learn from Dallas and educate your public and politicians on the risks and solutions to computer-based election fraud.

##THE SECRET BALLOT
>The secret ballot is a cornerstone of US elections. If your culture does not value the secret ballot this document may be of limited use. In the US regardless of political persuasion, most people prefer an honest election to one decided by who is the better election computer hacker.

##THE DALLAS COUNTY ELECTIONS HACK
>The Open Records Project downloaded 21 consecutive Daily Vote Roster snapshots for the third largest city in Texas and the ninth largest city in the US.

>As a result The ORP may have the only time-series longitudinal hacked election dataset in the US. (That means we have hacked voter names and addresses and can interview victims.)

>As required by state law, the Dallas County Elections Department published the Daily Vote Roster for all voters who cast ballots during Absentee and In-Person Early Voting. The Roster contained the VoterID, name, address, type of vote, and various dates associated with every Early-Voting vote cast.

>The County claims its source of roster data was the In-Person Electronic Poll Books, and the Absentee Ballot scanners. The County has claimed that entry into the Vote Roster can only be done by a registered Dallas County voter who either appeared In-Person or by Absentee Ballot.

>The computer that generated the roster was apparently hacked between October 7 and October 30. During that period tens of thousands of vote records were purged, added, or edited from the Vote Roster.

>From October 7 until October 30 The Open Records Project took snapshots and archived the Daily Vote Roster for Early Voters.

##WHY THE VOTE ROSTER MATTERS
>The Vote Roster is the list of all voters who have cast votes.

>Americans vote by secret ballot. At the instant before a voter casts a ballot there is a one-to-one relationship between the voter and their ballot as well as a one-to-one association between the voter and their votes.

>At the instant that ballot is cast, the one-to-one relationship between the voter and ballot still exist, but the relationship between the voter and their votes is gone. No one can know how they voted.

>The key security check on voting integrity is the absolute match between the number of voters in the Vote Roster and the number of ballots counted.

>If these numbers do not match, either physical ballots were added or removed from the Ballot Counter or "voters" were added or removed from the Vote Roster. In either case, the election has been compromised and the election is nothing more than a lottery.

>With tens of thousands of Vote Roster entries purged and other tens of thousand of entries apparently created out of thin air, Dallas County Elections Department is definitely in the lottery business.

###========== HOW TO SET UP THE DATABASE TABLE ==========

##DOWNLOAD THE DALLAS DAILY VOTE ROSTERS

Get the Daily Vote Roster raw data HERE.

>>Get SQL and other code beyond this document at github.

##DATA FORMAT AND FILE ORGANIZATION
>>DallasEarlyVotingFromCounty.zip provides you the Daily Vote Roster exactly as downloaded day-after-day from the Dallas County Elections Department.

    * The County published the following zipped CSV Vote Roster files, one for each group of precincts, 1000s, 2000s, 3000s, and 4000s.
       * Precincts 1000 - 1731.zip
       * Precincts 2000 - 2942.zip
       * Precincts 3000 - 3950.zip
       * Precincts 4000 - 4664.zip

>>The County apparently updated file contents continuously, so the downloaded contents were a snapshot of the Vote Roster at the instant of the download.

    * From October 7 until October 30 the ORP downloaded the Vote Roster files each day (except for when the County server was down).
>>- To distinguish the files we place each day's files in a directory named with the date. Ex: 10_07, 10_08, 10_09

    * NOTE: The County assigned filenames include TWO SPACES following the filename hyphen. The multiple spaces will confuse some CSV import tools including Mariadb LOAD LOCAL, so edit out the extra space.

##MARIADB/MYSQL FOR SQL ANALYSIS
>>Once you have unzipped the Vote RosterCSV files any SQL database can import and run the fraud analysis. However most of our developers run Ubuntu and use Mysql or its twin Mariadb. To install Mariadb under Ubuntu try How to Install Mariadb.

##To log in and use the Mariadb command line try Use the Mariadb Command Line

    * Select a database to hold your table.
>> - Create a SQL table to hold the Vote Roster by copying the SQL below into the Mariadb command line and executing.

```
CREATE TABLE `early_voting_roster_Dallas` (
  `Precinct` CHAR(6) COLLATE utf8_unicode_ci DEFAULT '' COMMENT 'Voter
precinct from county clerk',
  `PrecinctSub` CHAR(3) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Voter precinct subsidiary from county clerk',
  `StateIDNumber` CHAR(12) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Texas state voter id from county clerk',
  `VoterName` CHAR(40) COLLATE utf8_unicode_ci DEFAULT '' COMMENT 'Voter
name from county clerk',
  `BallotType` CHAR(6) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Ballot type P=InPerson, M=Mail from county clerk',
  `DateBallotRequestedChar` CHAR(10) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Date ballot requested in character format from county clerk',
  `DateBallotMailedChar` CHAR(10) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Date ballot mailed in character format from county clerk',
  `DateBallotReturnedChar` CHAR(10) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Date ballot returned in character format from county clerk',
  `ResidenceAddress` CHAR(40) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Voter residence address from county clerk',
  `ResidenceCity` CHAR(40) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Voter residence city from county clerk',
  `ResidenceZip` CHAR(5) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Voter 5 digit residence zip code from county clerk',
  `PartyCode` CHAR(3) COLLATE utf8_unicode_ci DEFAULT '' COMMENT 'Voter
PartyCode from county clerk',
  `ElectionCode` CHAR(8) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Election admistrator assigned code for an election',
  `VoterCityCode` CHAR(3) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Voter city code from county clerk',
  `VoterCityName` CHAR(40) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Voter city name from county clerk',
  `VoterISDCode` CHAR(3) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Voter ISD code from county clerk',
  `VoterISDName` CHAR(40) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Voter ISD name from county clerk',
  `InPersonVotingLocation` CHAR(40) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'In Person voting location from county clerk',
  `VoterUSCongressCode` CHAR(3) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Voter US Congress code from county clerk',
  `VoterUSCongressName` CHAR(40) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Voter US Congress name from county clerk',
  `VoterUSStSenateCode` CHAR(3) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Voter US State Senate code from county clerk',
  `VoterUSStSenateName` CHAR(40) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Voter US State Senate name from county clerk',
  `VoterStRepCode` CHAR(3) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Voter State Representative code from county clerk',
  `VoterStRepName` CHAR(40) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Voter US State Reprenestative name from county clerk',
  `VoterCommissionerCode` CHAR(3) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Voter Commissioner code from county clerk',
  `VoterCommissionerName` CHAR(40) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Voter Commissioner name from county clerk',
  `VoterCitySingleMemberCode` CHAR(3) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Voter Commissioner code from county clerk',
  `VoterCitySingleMemberName` CHAR(40) COLLATE utf8_unicode_ci DEFAULT
'' COMMENT 'Voter City Single Member name from county clerk',
  `VoterISDSingleMemberCode` CHAR(3) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Voter ISD Single Member code from county clerk',
  `VoterISDSingleMemberName` CHAR(40) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Voter ISD Single Member name from county clerk',
  `VoterWaterDistCode` CHAR(3) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Voter Water District code from county clerk',
  `VoterWaterDistName` CHAR(40) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Voter Water District name from county clerk',
  `VoterDCCCDCode` CHAR(3) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Voter DCCCD code from county clerk',
  `VoterDCCCDName` CHAR(40) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Voter DCCCD name from county clerk',
  `VoterFloodControlCode` CHAR(3) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Voter Flood Control code from county clerk',
  `VoterFloodControlName` CHAR(40) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Voter Flood Control name from county clerk',
  `VoterCountyJPCode` CHAR(3) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Voter County JP code from county clerk',
  `VoterCountyJPName` CHAR(40) COLLATE utf8_unicode_ci DEFAULT ''
COMMENT 'Voter County JP name from county clerk',
  `VoterSBOECode` CHAR(3) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Voter SBOE code from county clerk',
  `VoterSBOEName` CHAR(40) COLLATE utf8_unicode_ci DEFAULT '' COMMENT
'Voter SBOE name from county clerk'
) ENGINE=MYISAM DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;

```

    * Check to see if the schema was successfuly created.
Cut and past the following into the Mariadb command line:
desc early_voting_roster_Dallas

    * LOAD THE VOTE ROSTER CSV INTO MARIADB
Paste the following LOAD into the Mariadb command line.

```
EDIT the '$filename' to be the CSV file you are loading.
For example: Precincts 1000 -  1731.zip
Don't forget to edit out the extra space after the hyphen.
Precincts 1000 - 1731.zip

LOAD DATA LOCAL INFILE '$filename'  INTO TABLE 
`early_voting_roster_Dallas` FIELDS ESCAPED BY '\\\\' TERMINATED BY ','
OPTIONALLY ENCLOSED BY '\"' LINES TERMINATED BY '\\r\\n' IGNORE 1 LINES (
                    `Precinct`,
                    `PrecinctSub`,
                    `StateIDNumber`,
                    `VoterName`,
                    `BallotType`,
                    `DateBallotRequestedChar`,
                    `DateBallotMailedChar`,
                    `DateBallotReturnedChar`,
                    `ResidenceAddress`,
                    `ResidenceCity`,
                    `ResidenceZip`,
                    `PartyCode`,
                    `ElectionCode`,
                    `VoterCityCode`,
                    `VoterCityName`,
                    `VoterISDCode`,
                    `VoterISDName`,
                    `InPersonVotingLocation`,
                    `VoterUSCongressCode`,
                    `VoterUSCongressName`,
                    `VoterUSStSenateCode`,
                    `VoterUSStSenateName`,
                    `VoterStRepCode`,
                    `VoterStRepName`,
                    `VoterCommissionerCode`,
                    `VoterCommissionerName`,
                    `VoterCitySingleMemberCode`,
                    `VoterCitySingleMemberName`,
                    `VoterISDSingleMemberCode`,
                    `VoterISDSingleMemberName`,
                    `VoterWaterDistCode`,
                    `VoterWaterDistName`,
                    `VoterDCCCDCode`,
                    `VoterDCCCDName`,
                    `VoterFloodControlCode`,
                    `VoterFloodControlName`,
                    `VoterCountyJPCode`,
                    `VoterCountyJPName`,
                    `VoterSBOECode`,
                    `VoterSBOEName`;
                    
```

5. Test to confirm your import worked.
From the command line enter:
select StateIDNumber, VoterName, ResidenceAddress from early_voting_roster_Dallas limit 20;

If your import was successfull you will see 20 voter records displayed.

DALLAS VOTER REGISTRATION DATA
We have voter registration data for Dallas County as of October 13, 2020. Any registrant with an effective date prior to November 3, 2020 was eligible to vote in the November 3, 2020 election.

UNIQUENESS
County datasets do not comply with the database First Normal Form. They do not contain a unique field suitable for use as a primary key.

We assumed the StateIDNumber would uniquely identify a voter. Not so. The County reported voters who voted with multiple StateIDNumbers and multiple voters who voted with the same StateIDNumber. The reports show voters voting more than once.

Therefore, when you import the CSV files create a UNIQUE INDEX for each record. Do not depend on an AUTOINCREMENT generated index because it will not allow rapid location of duplicate records in subsequent days.

We calculated an MD5 hash across the entire vote record. With the MD5 you can compare the records from one day to the next to quickly find duplicates as well as purged voters. Without the MD5 or something similar you have to compare each and every field in the record to find differences. Without the date fields even that is impossible.

========== HOW TO DO SOME FRAUD ANALYTICS ==========

VANISHING AND REAPPEARING VOTERS
The primary purpose of the unique index is to catch votes already cast that are included in the following day's cumulative vote roster.

However by comparing the MD5s of one day to the next you can find voters who were purged from one day to the next.

By building a cumulative table of purged voters you can compare purged voters to subsequent voter rosters to see if a purged StateIDNumber reappears days or even weeks later with a new vote. (Thousands do.)

MULTIPLE VOTES BY A SINGLE VOTER
Dallas has reported tens of thousands of voters who "voted" multiple times. After numberous voter interviews we do not believe that most of these people double voted. Come up with your own theory for the source of the extra reported "vote".

ANOMALIES TO SEARCH
Volunteers have done most of the searches below. However many new eyes may find some nuances we missed as well as dream up new searches.

- Voters who voted without any StateIDNumber
- Voters who voted with a bogus StateIDNumber
- Voters who voted multiple times with the same
StateIDNumber on different days
- Voters who voted multiple times from different addresses
- Voters who voted Absentee and then voted again In-Person
- Multiple voters who voted with a single StateIDNumber
- Voters over 110 years old
- Votes from jail or prison (Some are legal in Texas.)
- Votes from voters registered in foreign countries.
(Some are legal in Texas.)
- Multiple voters who voted from homeless shelters and retirement homes on the same day (Possible vote harvesting)
- Absentee voters who "voted" Absentee before their Absentee Ballot was requested
- Absentee voters whose "ballot" was requested, mailed, and received on the same day
- Counts of purged voters by zip or precincts
- Counts of purged voters by age
- Geolocation heat maps of purged voters
SUMMARY
Dallas is a particularly interesting case study of how not to run an election. We believe we've detected at least three independent hacks, and there may be more.

About the only thing they did right was comply with the law requiring publication of the Daily Vote Roster. Dallas also contracted out all vote management to a company that appears to be based overseas. Getting answers is therefore difficult if not impossible.

If you are involved with election security in your country and you want to run honest elections, based on the Dallas experience we make the following suggestions:

1. Run your own elections with local government employees. Do not contract them out. At least if things go wrong you can put your hands on the miscreants.

2. Copy the Texas Statute requiring Internet publication of cumulative Daily Vote Rosters. Publishing vote rosters will not eliminate election hacking but they can make it more labor intensive.

3. Avoid Absentee Ballots that make establishing identity difficult and ballot delivery out and delivery back insecure and questionable.

4. Use paper ballots. They are human readable and easy to understand. They can be recounted. They can be hacked but with significant costs in labor and material.

5. Avoid complicated centralized computers to count the votes. Computers obscure election hacking but make it as low-cost as sending spam.
As always if you find something interesting ping us at WHISTLEBLOWER@openrecords.org.


Copyright The Open Records Project 2020
