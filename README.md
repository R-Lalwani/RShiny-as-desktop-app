# Deploying RShiny app as a desktop application on your client's system
This solution is a set of steps user needs to follow to deploy their RShiny app as a desktop application on someone else's system.
The solution shared here is an  extension of what is provided on 
http://oddhypothesis.blogspot.in/2016/04/desktop-deployr.html and 
http://blog.analytixware.com/2014/03/packaging-your-shiny-app-as-windows.html


## Step 1: Get R Portable from Portableapps.io
You need a software called R Portable to deploy the app. R portable is well, just a portable version of R. It allows the user of your RShiny app need to not have R studio on their system. Once R Portable is installed in your Downloads, copy it and pase it inside your tool's folder

Your tool folder now looks like this:

+- tool/
 |  +- R Portable/
 
## Step 2: Install all package dependancies in R Portable's library
### Step 2.1: Before doing anything, add to the bottom of R-Portable/App/R-Portable/etc/Rprofile.site:

.First = function(){
    .libPaths(.Library)
}

This will force R-Portable to only use its local library

Step 2.2: You need to install all the packages you'd need for your RShiny app in the R portable library. Just open RPortable.exe inside the R Portable folder and write the following

#### Add/remove to the following vector based on your requirements
list_of_packages <- c("RJDBC", "RCurl",  "shinysky", "shiny", "shinythemes", "shinydashboard",
                      "ggplot2", "lubridate", "shinyBS", "dplyr", "markdown", 
                      "shinyWidgets","plotly", "DT", "writexl", "reader", "stringr", "RDCOMClient", "shinyjs", "reshape2",
                      "shinycssloaders", "rstudioapi")

new_packages<- list_of_packages[!(list_of_packages %in% installed.packages()[,"Package"])]
if(length(new_packages)) install.packages(new_packages, repos = "http://cran.us.r-project.org")

## Step 3: Prepare the rest of the folder
I like to keep the ui and server and app file in a sub-folder called shiny. So create a new folder inside your tool folder and name it shiny

Your tool folder now looks like this:

+- tool/
 |  +- R Portable/
 |  +- shiny/
 
Now, keep the ui and server files inside the shiny folder. For my application, I had 2 UI files, called buyer_ui and admin_ui and one server file called server. I had one file call app.R which was sourcing the ui and server files

Your www folder also goes inside the shiny folder

Now your tool folder looks like this:

+- tool/
 |  +- R Portable/
 |  +- shiny/
 |  +- app.R
 |  +- buyer_ui.R
 |  +- admin_ui.R
 |  +- server.R
 |  +- www/
 
 
## Step 4: Prepare the VBS and runShinyApp file




