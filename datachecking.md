//Stata file for sense checking and setting up the data from H:\JOE\THIN\all15wide_old.dta
//should have the full 15 month data with all of them having correct dates. 
//Wide format - one line per event

//stata 11 set memory: set memory 1g

use "H:\JOE\THIN\all15wide_old.dta", clear

//Examine the data: 

codebook id 
//there are 10,000 ids, none missing

codebook case_control
//case =1, control =0 
// 77059 control events
// 23987 case events

codebook eventdate
// 35 missing - these are admin events - whilst waiting for the update, treat as indexdate

replace eventdate = indexdate if eventdate==.

//Table 1 stuff: descriptive statistics: need to collapse over id: sense checked 1st with duplicates report id
//and duplicated report age id 

preserve
collapse age case_con, by (id)
sum age if case_con==1, d
sum age if case_con==0, d
restore
//See the excel file for results

//now sex: male is 1 female is 2
preserve
collapse sex case_con, by(id)
tab sex case_con, col
restore

//Type of diabetes: 
preserve
replace typediab = 3 if typediab==. & case_con==1
collapse typediab case_con, by(id)
tab typediab case_con, col
restore
//unfortunately there are no missing diabetes classifications and there are way too may type 2s. 

//BMI: height/weight not in this dataset

//deprivation: not in this dataset

//look at the consultation codes:

//censoring: think about start, transfer and death dates:
// xferdate deathdate startdate enddate
gen xfertime=xferdate-indexdate
gen deathtime=deathdate-indexdate
gen starttime=startdate-indexdate
gen endtime=enddate-indexdate


//xfer - 14 controls transferred out before experiment ended, 6 cases as well.  
preserve
collapse xfertime case_control, by(id)
tab xfertime case_con
restore
//not clear how cases could transfer out before their indexdate


//deaths - all > 180 days post indexdate, 5 cases, 3 controls
preserve
collapse deathtime case_con, by (id)
tab death case_con
restore


//starttime
preserve
collapse starttime case_con, by(id)
hist starttime
restore

//endtime : mostly occuring post indexdate
preserve
collapse endtime case_con, by (id)
tab endtime case_con
restore





