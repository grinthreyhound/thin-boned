//Stata file for sense checking and setting up the data from H:\JOE\THIN\all15wide_old.dta
//should have the full 15 month data with all of them having correct dates. 
//Wide format - one line per event

//stata 11 set memory: set memory 1g

use "H:\JOE\THIN\all15wide_old.dta", clear

//create a new indexdate for the controls
gen caseindexdate=indexdate
format %td caseindexdate

replace indexdate = eventdate if indexevent==1 
replace indexdate=. if indexevent==. & case_control==0

egen max_index = max(indexdate), by(id) //this creates 35 missing values

codebook eventdate
//there are 35 missing eventdates

codebook id //there are 10,000 ids, none missing

codebook case_control