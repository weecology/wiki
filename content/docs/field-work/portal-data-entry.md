---
title: Portal Data Entry and QA
summary: " "
---  
  
  **Open data** is *good* but **clean data** is *better*. 
  
  Below are guidelines/instructions for entering and QAing each type of data we collect that previous Portal RAs have developed to systematically update and ensure the quality of Portal data. 
  
## Rodent Data 
  
  Ideally, rodent data are entered and QA'ed before the next rodent census trip. This ensures no back logs in tasks, and that [portalcasting](https://github.com/weecology/portalcasting) is able to generate new forecasts based on the latest data point available.  
  
###   Data entry 
  
* Scan data sheets and deposit on Portal Dropbox folder: Portal -> PORTAL_primary_data -> Rodent -> Raw_data -> datasheet_scans -> year. Make sure to rename file to appropriate census period.
* Enter data and have field technician do the second entry.
* Open folder with .xls files of the rodent data: Portal -> PORTAL_primary_data -> Rodent -> Raw_data -> New_data. Copy a previous one, and rename file with the appropriate census date (Ex. newdatXX.xlsx).
* Enter rodent data on DE1 tab (field technician enters data on DE2 tab).
    - When filling in the year, encode full year value (Ex. Encode 2024 instead of 24).
    - When dragging down repetitive values (e.g., month, day, year, plot), make sure that the values are copied over in the target cells (i.e., make sure that when you drag down, the values are not increasing).
    - Use all caps in all columns.
    - Under the tag column, the usual values are 6-characters long. The characters range from numbers 0-9 and letters A-E.
    - In rows where individuals got *new tags*, put an asterisk (*) under note2.
    - Notes of individuals being dead (D), removed (R), or escaped (E) should be put under the column note5.  	
* Download PIT tags (Note: You need a Windows machine with BioTerm installed. The weather station laptop works.)
    - Connect the scanner using the USB adapter.
    - Open BioTerm. Make sure the BAUD rate is 9600. Select Open.
    - Using the scanner, press READ, then MENU until it says "Stored ID tags". Press the up arrow, then READ. It will ask if you want to send all tags. Press READ. The tags *should* appear in the BioTerm text box. You may need to squeeze the adapter into the scanner so you get a really secure connection (the screw is really difficult to turn all the way). In theory you can also type "G" into the BioTerm text box and the tags will appear.
    - Open the Notepad application. Copy the tags from the BioTerm text box into an empty text file in Notepad.
    - Save the file with the new tags with the census number (tags###.txt) in the Portal Dropbox folder: Portal -> PORTAL_primary_data -> Rodent -> Raw_data -> New_data -> tag scans.
* Delete old PIT tag scans 
    - *NOTE: This should only occur after you have completed data QA!*
    - Once you have completed all data QA, you will want to delete the previous PIT tag scans from the scanner so they do not get read into next month's tag scans file.
    - Using the scanner, press READ, then MENU unitl it says "Stored ID tags." Press the up arrow twice; at this point, the scanner should read "Delete tags?" with the default set to NO. Press READ to select YES, then press the up arrow.
    - When you check the stored tags, the number should now read 000.

###   Data QA  

* In GitHub (remote):
  - Refer to .githooks README [here](.githooks/README.md) and this only needs to be done once, and the readme explains how to use the version tags. Version tags will be needed when a new commit is done.
 - Make sure you have write access to the PortalData repo (ask Glenda about this).  
  - clone the repo by following the instructions [here](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository). Note: You only need to do this once.
  - create new branch and rename it to: rodents_censusperiod (Make sure you are in the correct repo: PortalData). Follow instructions [here](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository). 

* In RStudio (local):
  - Click File -> New Project -> Existing Directory and browse to where PortalData is saved in your computer and open it
  - In the terminal tab in RStudio, type:
    * git pull (assuming you are in the main branch, do this to make sure that you have the latest version of the files)
    * git checkout rodents_censusperiod (make sure you are working on the new branch you just created. The top part of your RStudio should reflect this)
  Note: If updates in the main branch were merged while you are working in your branch, do:
    * git checkout main
    * git pull
    * git checkout rodents_censusperiod
  - In the Files tab in RStudio, go to PortalData -> DataCleaningScripts, and click on new_rodent_data.r.
  - Run code from lines 1 to 11, and update line 18 to the correct census period number.
  Note: If you are using a Windows Machine:
    * line 20 (filepath= "") should have 2 back slashes after each location (Ex.: filepath= D:\\Dropbox\\Portal\\PORTAL_primary_data\\Rodent\\Raw_data\\New_data)
    * line 22 (newfile=""), there should be 2 back slashesbefore the word newdat (Ex.: paste(filepath, '\\newdat', newperiod, '.xlsx', sep = ''))
    * line 23 (scannerfile=""), there should be 2 back slashes before the phrase tag scans and tags (Ex.: paste(filepath, '\\tag scans\\tags', newperiod, '.txt', sep = ''))

 
  - Run line 29. Corect the .xlsx worksheets until they match.
  - Once the sheets are identical, run lines 36 to 38. If errors pop up, update Excel file and note changes in the red notebook.
  - Run subsequent lines and make necessary changes. Note them in the red notebok. This is so we can track the decisions on the species/sex/etc. IDs that were made. scan previous months' corrections to make sure you're not 'correcting' the same individuals back and forth.
  - Save R file.
  - Push to GitHub repo by clicking on the Git tab in RStudio, and selecting the files that were updated (Usually, this includes 2 files: new_rodent_data.r and Portal_rodent.csv).
  - make sure to write an intuitive commit message. Ex.: add rodent census XXX data [minor]). This [minor] message is a githook, which is needed to commit new messages. 

* In Github:
  - Go to the Weecology PortalData repo and click the message that says a new commit has been pushed by you.
  - Tag Glenda as a reviewer and create a pull request by clicking the create pull request button at the bottom of that page. 
  - If it passes all checks/tests,you're good to go! If not, go to the PR (pull request) page and read the details where the error occurred. Fix issues accordingly.

## Plant Data 
  
  Ideally, plant data are entered and QA'ed before the next plant census trip. This ensures no back logs in tasks.  
  
###   Data entry   

**A.  Quadrat data**  

* Scan data sheets and deposit on Portal Dropbox folder: Portal -> PORTAL_primary_data -> Plant -> Quadrats -> Dataraw -> Newdata. Make sure  to rename file accordingly. (Ex.:Summer20XX.pdf)
* Make a copy of a previous filled in .xlsx datasheet, clear it, and rename appropriately (Ex.:Summer20XX.xlsx). This ensures that the appropriate data validation settings are in place. 
* Enter data on abundance1 tab. Field technician enters data on abundance2 tab.
  - When filling in the year, encode full year value (Ex. Encode 2024 instead of 24).
  - If needed, check the Species List tab for the correct spelling of species ID codes.  
  - Refer to the [test](https://github.com/weecology/PortalData/blob/main/testthat/test-plant_quadrats.R) for appropriate values for each column.
  
**B.  Transect data** 

* Scan data sheets and deposit on Portal Dropbox folder: Portal -> PORTAL_primary_data -> Plant -> TRANSECTS -> DataRaw -> Datasheet_scans. Make sure to rename file accordingly. (Ex.:Summer20XX_transects.pdf) 
* Go to: Portal -> PORTAL_primary_data -> Plant -> TRANSECTS -> DataRaw -> RawData and make a copy of a previous filled in .xlsx datasheet, clear it, and rename appropriately (Ex.:ShrubTransect_SummerXX.xlsx).  
* Enter data on DE1 tab. Field technician enters data on DE2 tab.
  - When filling in the year, encode full year value (Ex. Encode 2024 instead of 24).
  - If needed, check the Species List tab for the correct spelling of species ID codes.  
  - Refer to the [test](https://github.com/weecology/PortalData/blob/main/testthat/test-shrub_transects.R) for appropriate values for each column.  
  
###   Data QA  

* In GitHub (remote):
  - create new branch and rename it to: plants_season-year (Ex.: plants-Summer2024). Make sure you are in the correct repo: PortalData).

**A.  Quadrat data**  

* In RStudio (local):
  - Click File -> New Project -> Existing Directory and browse to where PortalData is saved in your computer and open it
  - In the terminal tab in RStudio, type:
    * git pull (to make sure that you have the latest version of the files)
    * git checkout plants-season-year (make sure you are working on the new branch you just created)  
  - In the Files tab in R, go to PortalData -> DataCleaningScripts, and click on clean_plant_quadrat_data.R.
  - Run all lines of code and don't forget to update line 29 and 30 to the correct season and year.
  Note: If you are using a Windows Machine:
    * line 32 (filepath= "") should have 2 back slashes after each location (Ex.: filepath= D:\\Dropbox\\Portal\\PORTAL_primary_data\\Plant\\Quadrats\\Dataraw\\Newdata\\) 
  - Address all issues until the datasheets are matched. 
  - Push the changes and write an intuitive commit message. 

**B.  Transect data**  

* In RStudio (local):
  - Access the repo in your local.  
  - In the Files tab in R, go to PortalData -> DataCleaningScripts, and click on clean_shrub_transect_data.R.
  - Run all lines of code and don't forget to update line 28 and 29 to the correct season and year.
  Note: If you are using a Windows Machine:
    * line 30 (filepath= "") should have 2 back slashes after each location (Ex.: filepath= D:\\Dropbox\\Portal\\PORTAL_primary_data\\Plant\\TRANSECTS\\Data_raw\\RawData\\)

*Notes:*  

* You do not need to do all these in one sitting, especially when entering and QAing the plant data. Just make sure you save your changes locally. 
* Follow the steps for QAing the rodent data to create a PR for the new plant data. Again, make sure to write intuitive commit messages every time you push *new* changes. 
* The following tags for commit messages in Github should be used accordingly:  
    - [major] - use if you've made a breaking change (e.g changed the shape of a data table). This will update the version from 1.88.0 to 2.0, for example.  
    - [minor] - use if you're adding new data. This is the default so no tags in a commit message will assume this. This will update the version from 1.88.0 to 1.89.0, for example.
    - [patch] - use if you've made a small change to existing data (eg fixed species or sex, or a typo, etc), without adding new data. This will update from 1.88.0 to 1.88.1, for example.
    - [no version bump] -  This would usually be for changes to code that don't change data at all (eg, adding tests). The version number will not change, and the update won't get archived on Zenodo. 

