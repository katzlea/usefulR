useful.commands
================
Lea Katz
`r Sys.Date()`

`{r setup, include=FALSE} knitr::opts_chunk$set(echo = TRUE)`

# Data formats

## Creating dataframes

### Making a df from files in folder

\`\`\`{r dir to df,eval=FALSE} library(tidyverse)

\#creating the dir_to_df function dir_to_df \<- function(files) { files
\<- gsub(“^./”,““,files) df \<- data.frame(column= unlist(files)) %\>%
separate(column, sep=”/“, into=c(”site”,“image”)) } my_df \<-
dir_to_df(files)

    ### Making a df from vectors
    ```{r vec to df}
    # vectors
    employee <- c('John Doe','Peter Gynn','Jolie Hope') #character vector
    salary <- c(21000, 23400, 26800) #numeric vector
    startdate <- as.Date(c('2010-11-1','2008-3-25','2007-3-14')) #date vector
    #combine into df
    employ.data<-data.frame(employee,salary,startdate)
    str(employ.data)
    employ.data2<-data.frame(employee,salary,startdate,stringsAsFactors=TRUE) #employee names become factors

## Dataframe manipulations

### Accessing data

`{r access} #like a list employ.data["employee"] #returns a df employ.data$employee #returns a vector employ.data[["employee"]] #returns a vector employ.data[[1]] #returns a vector #like a matrix  head(employ.data,n=2) #display two rows employ.data[2:3,] #display row 2 & 3 employ.data[employ.data$salary > 21000,] employ.data[1:2,2] #as vector employ.data[1:2,2, drop = FALSE] #as df`

### Modifying 1 value

`{r modif} employ.data[1,"salary"] <- 30; employ.data`

### Turn NAs into 0

`{r NA,eval=FALSE} mydata[is.na(mydata)] = 0`

### Adding columns/rows

`{r adding} employ.data2<-rbind(employ.data,list("Lea Katz",35000,"2021-10-01")) #adds a row employ.data3<-cbind(employ.data2,age=c(35,45,27,26)) #adds a column employ_data3<-employ.data2$age<-c(35,45,27,26) #another way to add a column`

### Removing columns/rows

`{r removing} employ.data3$age <- NULL #removes column employ_data5 <- employ.data2[-1,] #removes first row`

### Change colnames/rownames

`{r names,eval=FALSE} #baseR "start" -> colnames(employ.data)[3] #dplyr iris %>% rename(petal_length = Petal.Length) #from a csv... sometimes not well read area<-header.true(area) #makes first column into title and removes the auto-generated ones`

### Creating subsets

`{r subset} library(dplyr) #filter with a condition employ.subset<-subset(employ.data,subset = salary==21000 ) #select only some variables employ.data %>% select(salary,employee)`

### Find value and change it

\`\`\`{r find and change,eval=FALSE} data\<-as.matrix(data) m \<-
data\[,-1\] rownames(m) \<- data\[,1\]

for (x in 1:nrow(sum)) { i \<-
sum![image_filename\[x\]  j \<- sum](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;image_filename%5Bx%5D%20%20j%20%3C-%20sum "image_filename[x]  j <- sum")label_names\[x\]
m\[i,j\]\<-sum$cover\[x\] }


    ### Group by a variable and only keep last value
    ```{r group & last,eval=FALSE}
    tlog<-aggregate(tlog,by=list(as.character(tlog$unix)), FUN=last)

### Group by a variable and make a sum

`{r group & sum,eval=FALSE} # for 1 variable sum<- area %>%    group_by(image_filename, label_names) %>%    summarize(cover = sum(cover)) # for all variables (if numeric) summed<-mydata %>%    group_by(region.code) %>%    summarise(across(everything(),sum,na.rm=TRUE))`

### not sure what this does anymore…

`{r notsure,eval=FALSE} names(images)<-as.character(unlist(images[1,]))`

### Create new columns with operations

`{r operations} library(dplyr) employ.data %>% mutate(double_salary = salary*2)`

## Merging 2 dataframes

`{r second employee,include=FALSE} age<-c(25,38,27) married<-c("yes","yes","no") spouse<-c("Marie","Joanne",NA) personal.info<-data.frame(age,married,spouse)`

### Binding columns (rows match)

`{r cbind} employ.info<-cbind(employ.data,personal.info) #OR employ.info<-bind_cols(employ.data,personal.info)`

### Matching values for common variables

`{r innerjoin,eval=FALSE} mydata<-inner_join(mydata,metadata, by="transect")`

## Creating Lists

### Making a list from df and vectors

`{r list} age_vec<-c(35,45,27,26) mylist<-list(employees = employ.data, ages = age_vec)`

## Vectors

### Change “numerical vector” into “named numerical vector”

`{r namedvec} ages<-c(35,45,27,26) names<-employ.data$employee names(ages)<-names #gives names to numerical values of the vector`

# Simple things

## Paste

\`\`\`{r paste,eval=FALSE} \#in a string dive\<-“20220601-DIV1”
campaign\<-“zeeland” my_path =
paste0(“data/”,campaign,“/”,dive,“/images”)

\#in a function
file\<-read.csv(paste0(“tlog/csv-files/”,dive,“-gps.csv”))


    # Plots
    ```{r grahamdata,include=FALSE}
    library(ggplot2)
    library(tidyverse)
    graham <- readr::read_csv("caribous_graham.csv",
              col_types = cols(season=col_factor(),sex=col_character()))
    tibble::glimpse(graham)
    ##for just 1 individual
    graham_ind <- graham %>%
      dplyr::filter(animal_id == "GR_C12") %>%
      dplyr::select(-animal_id,-sex)
    tibble::glimpse(graham_ind)

## Spatial data

### Position points

`{r geompoint} ggplot(graham_ind,aes(date,tree_cover)) +   geom_point() +   theme_light()`

### Tracked position

`{r geompath} ggplot(graham_ind,aes(longitude,latitude)) +   geom_path() #in order of dataset, use geom_line for ascending order`

### Full positioning plot with all the labels and titles

`{r full position} ggplot(graham_ind,aes(longitude,latitude)) +   #geom_point() + # order is important, best if points are on top of path   geom_path(color="grey67") +   geom_point(color="pink") +   geom_rug(sides="rb", alpha = .05) + #rb=right+bottom, alpha=transparency   #xlab("Longitude") +   #ylab("Latitude")  + #labels   #ggtitle("Trajectory of Caribou C12") but "labs()" is better   labs(     x = "Longitude", #use NULL if you don't want any label     y = "Latitude",     title = "Trajectory of Caribou C12",     subtitle = "Map of 2,562 locations of female C12 tracked between 2001 and 2003 in Graham, Canada",     caption = "Data: B.C. Ministry of Environment & Climate Change",     tag = "Fig. 1"   )`

### Coordinate system

\`\`\`{r coordinates} \#linear and non-linear
ggplot(graham_ind,aes(longitude, latitude)) + geom_path() +
coord_cartesian(  
ylim = c(56.8, NA) \#you can set the min and max values of both axis )
\#THIS WILL NOT REMOVE POINTS OUTSIDE THE PLOT AREA, just zoom \#if you
want to really remove the points do like this :
ggplot(graham_ind,aes(longitude, latitude)) + geom_path() +
scale_y\_continuous(limits = c(56.8, NA))

\#another features ggplot(graham_ind,aes(longitude, latitude)) +
geom_point() + coord_cartesian( expand = FALSE, \#to have à (0,0) origin
clip = “off”, \#so points/labels don’t get cut on the axis lines ylim =
c(56.8, NA) )

\#coord_fixed ggplot(graham_ind,aes(longitude, latitude)) +
geom_point() + coord_fixed( ratio = 5 \#default is 1 if you put nothing
) + scale_y\_continuous( breaks = seq(56,58, by = .04) )

\#coord_polar : makes straight lines into curves!
ggplot(graham_ind,aes(longitude, latitude)) + geom_path() +
coord_polar()





    ## Statistics
    ### Boxplot
    ```{r geomboxplot}
    ggplot(graham_ind,aes(season,tree_cover)) +
      geom_boxplot() +
      theme_linedraw()

## Assigning, saving, rendering, etc.

### Assign plot to object

`{r assign,eval=FALSE} #g<-ggplot(graham_ind, aes(longitude,latitude)) +   #geom_point() #g (g<-ggplot(graham_ind, aes(longitude,latitude)) +     geom_point()) #to print it directly !! use parentheses g +   geom_path(color="purple") +   geom_rug(side="rb",alpha= .08) # you can add layers to your plot object later... can aboid typos if you cant to change things?`

### Save

\`\`\`{r save,eval=FALSE} ggsave(filename=“myggplot.png”, \# raster
graphic width = 10, height = 7, dpi = 700) \# pixel intensity to 600 or
higher!

ggsave(filename=“myggplot.pdf”, \#vector graphic =\> best width = 10,
height = 7, device=cairo_pdf) \# doesn’t work?


    ## Different types of plots
    ### Discrete variable
    ```{r aes discrete}
    ggplot(graham_ind,aes(
      longitude, latitude,
      color = season, #if color is a variable in the dataset, put inside "aes()"
      shape = season
      )) + 
      geom_point()

### Continuous variable

\`\`\`{r aes cont} ggplot(graham_ind,aes( longitude, latitude, color =
tree_cover)) + geom_point()

ggplot(graham_ind,aes( longitude, latitude, size = tree_cover )) +
geom_point( color = “darkgreen”, \#should not go in aesthetics because
it is for the geom_points shape = 1 )


    ### Scales
    ```{r scales}
    ggplot(graham_ind,aes(
      date, tree_cover,
      color = season)) + 
      geom_point() +
      scale_x_date() + #tells ggplot that the x axis is a date
      scale_y_continuous() +   #tells ggplot that the y axis is continuous
      scale_color_discrete()

`{r scales2} ggplot(graham_ind,aes(   date, tree_cover,   color = season)) +    geom_point() +   scale_x_date(     expand = c(0,0), #removes the "padding" on the sides)     date_breaks = "3 months", #can use english to specify date breaks     date_labels = "%m/%y", #to shorten the dates to desired abbreviation     name = NULL   ) +   scale_y_continuous(     labels = scales::percent_format(scale=1), #our numbers were already 75, but if it was 0.75, we could do scale = 100     sec.axis = dup_axis(name = NULL), #duplicate the y axis for easier reading     name = "Tree Cover"   ) +      scale_color_discrete(     type = c("#AE79F0","#EEAF5C"), #either use normal color names or hexcodes     name = "Season:"   )`

### Color gradients

`{r color gradients} ggplot(graham_ind,        aes(longitude,latitude,            color = yday)) +   geom_point () +   scale_color_continuous (     type = "viridis" # with existing palette, install more palette packages :)   ) ggplot(graham_ind,        aes(longitude,latitude,            color = yday)) +   geom_point () +   scale_color_gradient (     low = "#AE79F0", high = "#EEAF5C" # custom gradient   )`

### heatmap

`{r heat} ggplot(graham_ind,aes(longitude,latitude,)) +   geom_hex (     aes(fill=..count..), # is useful to put inside the geom if there are several geoms     bins = 15,     color = "white", #white outlines draws your eye to global extremes     size = 0.5    ) +   scale_fill_continuous(type = "viridis") +   scale_color_continuous(type = "viridis", guide = "none") +   labs (     x = "Longitude",     y = "Latitude",     title = "Heatmap of C12",     caption = "Data source : blablabla"   )`

### heatmap without the outlines

`{r heat2} ggplot(graham_ind,aes(longitude,latitude,)) +   geom_hex (aes(color =..count..), bins = 15) +   scale_fill_continuous(type = "viridis",                         name = "Count:",                         #breaks = c(50,100,150,200,250))                          #breaks = seq(50, 250, by = 50))                         breaks = 1:5*50 #from 1 to 5 and multiply each by 50                         ) +   scale_color_continuous(type = "viridis", guide = "none") +   labs (     x = "Longitude",     y = "Latitude",     title = "Heatmap of C12",     caption = "Data source : blablabla"   ) #this is another way to do it   ggplot(graham_ind,aes(longitude,latitude,)) +   geom_hex (bins = 15, color = "white", size = 1) +  #bins = resolution => always try several   rcartocolor::scale_fill_carto_c(palette="SunsetDark",                                   name = "Observations:",                                   breaks = 1:5*50)`

### Mutliple plots together (facets)

`{r facets} g <- ggplot(graham_ind,aes(longitude,latitude)) +   geom_path(color="grey67") +   geom_point(aes(color=factor(year))) + #aes here only colors the points!   scale_color_discrete(     name = "Year",     #labels = c("year 2001", "year 2002", "year 2003")     labels= function(x) paste("Year", x)) g + facet_wrap(~ season) #tilde ~ is option + N g + facet_wrap(~factor(year)) #to add ylabs to mini-plots again => lemon package`

### to arrange the plots differently

\`\`\`{r arrange} g + facet_wrap( \~ factor(year), ncol = 2 )

\#two pariables =\> GRID g + facet_grid( factor(year) \~ season )


    ## Aestethics 
    ```{r aes libraries,include=FALSE}
    library(RColorBrewer)

### Color palettes

`{r rbrewerpalettes,eval=FALSE} display.brewer.all(type="seq") # "Sequential" : from light to dark display.brewer.all(type="div") # "Diverging" : dark-light-dark display.brewer.all(type="qual") # "Qualitative" : visual differences between groups display.brewer.all(colorblindFriendly=TRUE)`

### Using a palette in ggplot

`{r use the palette} ggplot(graham_ind,aes(longitude,latitude,color=factor(year))) +   geom_path(color="grey67") +   geom_point() + #aes here only colors the points!   scale_color_brewer(     palette="YlOrRd",     name = "Year",     #labels = c("year 2001", "year 2002", "year 2003"),     labels = function(x) paste("Year", x)   )`

# Maps

`{r libraries,include=FALSE} library(sf) library(mapview)` \## Mapview
`{r mapview} caribou <- st_as_sf(graham_ind, coords = c("longitude", "latitude"), crs = 4326, remove = FALSE) mapview(caribou)`
