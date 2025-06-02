# IPL-2008-2024-data-analysis-with-SQL


select * from ipl..matches;

drop table if exists #ipls;


create table #ipls
(
match_id float,
inning float,
batting_team nvarchar(255),
bowling_team nvarchar(255),
overs float,
ball float,
batter nvarchar(255),
bowler nvarchar(255),
non_striker nvarchar(255),
batsman_runs float,
extra_runs float,
total_runs float,
extras_type nvarchar(255),
is_wicket float,
player_dismissed nvarchar(255),
dismissal_kind nvarchar(255),
fielder nvarchar(255)
)

insert into #ipls
select * from ipl.dbo.delivery1
union
select * from ipl.dbo.delivery2
union
select * from ipl.dbo.delivery3
union
select * from ipl.dbo.delivery4
union
select * from ipl.dbo.delivery5;

select count(*) from #ipls;

select count(*) from ipl.dbo.delivery1;
select count(*) from ipl.dbo.delivery2;
select count(*) from ipl.dbo.delivery3;
select count(*) from ipl.dbo.delivery4;
select count(*) from ipl.dbo.delivery5;

select * from #ipls;
select * from ipl.dbo.matches;


--number of matches played in each season
select yr ,count(distinct id) no_of_matches from
(select year(date) yr,id from ipl..matches)a
group by yr;

--most player of match winners  top 20 players list

select * from 
(select player_of_match,yr,no_of_times,rank() over(partition by yr order by no_of_times desc) rnk from
(select player_of_match,year(date) yr,count(player_of_match) no_of_times from ipl..matches group by player_of_match ,year(date) )a)b
where rnk=1;

--most wins by a team
select winner,count(winner) no_of_matches_won from ipl..matches group by winner order by no_of_matches_won desc;

select venue,count(venue) no_of_matches from ipl..matches group by venue order by no_of_matches desc;


--most no of runs by each batsman

select  batter,sum(batsman_runs) batter_runs from #ipls group by batter order by batter_runs;
