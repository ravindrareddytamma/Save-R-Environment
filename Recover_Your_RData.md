Recover your R Objects
================
Ravindra Reddy Tamma
30 July 2018

Save your R-Objects Automatically
---------------------------------

Most of the times there happens a scenario where it takes long time to run a particular line of code and assign the result to a variable or R-Object.

When you invoke a command that need more memory than allocated, We generally get an Error stating that Unable to allocate a vector of some GB. So we need to explicitly change the memory limits and re-run all the R Objects again.

To avoid such tedious tasks, a R function was designed to automatically save the all R-Objects in a folder named with latest system timestamp. So whenever there's a crash in R Studio, You can reload all the R-Objects from the folder with latest timestamp

``` r
save_r_objects <- function(interval = 300)
{
  name = gsub(" |:","-",Sys.time())
  dir.create(file.path("E:/RData",name),mode = "0777")
  save.image(file = file.path("E:/RData",name,"/Robject.RData"))
  cat("R Environment saved successfully into below Directory :\n", file.path("E:/RData",name,"/Robject.RData"))
  later::later(save_r_objects,interval)
}  
```

``` r
save_r_objects()
```

    ## R Environment saved successfully into below Directory :
    ##  E:/RData/2018-07-31-01-09-49//Robject.RData

``` r
load_latest_environ <- function()
{
  dirs <- list.dirs(file.path("E:/RData"))
  subdirs <- gsub("/","",gsub(c("E:/RData"),"",dirs))[-1]
  cat("List of Different timestamps of R Enviroment :\n")
  subdirs_time <- timestamp(strptime(subdirs,format = "%Y-%m-%d-%H-%M-%S"))
  subdirs_time <- subdirs_time[order(subdirs_time,decreasing = T)]
  subdirs_latest <- trimws(gsub("##|--|","",subdirs_time[1]))
  cat("Latest R Environment Restored Successfully\n","Latest TimeStamp is:",subdirs_latest)
  load(file.path("E:/RData/",gsub(" |:","-",subdirs_latest),"/Robject.RData"))
}
```

``` r
load_latest_environ()
```

    ## List of Different timestamps of R Enviroment :
    ## ##------ 2018-07-30 23:16:49 ------##
    ## ##------ 2018-07-30 23:18:14 ------##
    ## ##------ 2018-07-30 23:20:36 ------##
    ## ##------ 2018-07-31 00:10:22 ------##
    ## ##------ 2018-07-31 00:11:23 ------##
    ## ##------ 2018-07-31 00:32:20 ------##
    ## ##------ 2018-07-31 00:35:23 ------##
    ## ##------ 2018-07-31 00:39:43 ------##
    ## ##------ 2018-07-31 00:50:24 ------##
    ## ##------ 2018-07-31 00:51:40 ------##
    ## ##------ 2018-07-31 01:03:47 ------##
    ## ##------ 2018-07-31 01:08:12 ------##
    ## ##------ 2018-07-31 01:08:33 ------##
    ## ##------ 2018-07-31 01:08:54 ------##
    ## ##------ 2018-07-31 01:09:15 ------##
    ## ##------ 2018-07-31 01:09:36 ------##
    ## ##------ 2018-07-31 01:09:49 ------##
    ## Latest R Environment Restored Successfully
    ##  Latest TimeStamp is: 2018-07-31 01:09:49

``` r
later::later(save_r_objects,delay = 300)
```
