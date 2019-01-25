# Getting started with eICU

https://eicu-crd.mit.edu/about/

## eICU data 

https://physionet.org/works/eICUCollaborativeResearchDatabase/files/

```bash
wget --user chi.zhang@medisin.uio.no --ask-password -A csv.gz -m -p -E -k -K -np -nd https://physionet.org/works/eICUCollaborativeResearchDatabase/files/
```



Or work with demo data first 

```bash
wget --user chi.zhang@medisin.uio.no --ask-password -A csv.gz -m -p -E -k -K -np -nd https://physionet.org/works/eICUCollaborativeResearchDatabaseDemo/files/

gzip -d *.gz # to unzip all .gz
```



#### use database `eicudemo` 

```bash
andrea$ psql postgres -U chizhang
CREATE DATABASE eicudemo;
postgres=> GRANT ALL PRIVILEGES ON DATABASE eicudemo TO chizhang;

```

#### create table 

execute `postgres_create_tables.sql` in Postico. 

#### Load data (using command line)

```bash
postgres=# \connect eicudemo
eicudemo=# \cd Desktop/DataDemo-eicu
eicudemo=# \copy ADMISSIONDRUG FROM 'admissionDrug.csv' DELIMITER ',' CSV HEADER NULL ''
# and so on 
```





## what's in there

temporal info: no date. time is indicated by **offset**. 

### Patient 

demographics, admission, discharge. Key: `patientUnitStayID`

distinguish hospital and ICU discharge: within one hospital admission, there can be multiple ICU stays (linked by the same `patientHealthSystemStayID`)

offset unit is minute `hospitalAdmitOffset` 



### admissiondrug

medication taken before ICU



### admissiondx, diagnosis

Primary and later diagnosis 



### intakeoutput

as the name suggests 



### medication



### nurse assessment, nurse care, nurse charting

charting is free text 



## Patient 130804 (one ICU stay)

`patientunitstayid = 143599`. This is used to link to other tables 

```sql
SELECT * FROM patient WHERE patienthealthsystemstayid = 130804; 

SELECT * FROM admissiondx WHERE patientunitstayid = 143599;
```







996745

