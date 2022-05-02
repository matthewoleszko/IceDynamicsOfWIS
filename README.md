# Ice Dynamics at Whillans Ice Stream
## Matthew Oleszko
## GPGN599, Spring 2022

Systematic climate change has increased societal concern related to Earth’s systems and changes throughout the world. Issues including sea level rise and shifting ocean currents have heightened the public attention with ice movement and discharge throughout the polar regions [1]. Whillans Ice Stream, West Antarctica (WIS) is a major route ice travels through on its way towards discharge off the Antarctic continent [2]–[4].  WIS is also unique when compared to other ice streams throughout the world as it that moves according to a tidally modulated stick-slip pattern with one to two movement events each day [2]–[4]. I wrote python code to process and visualize data from a multiyear Global Navigation Satellite System (GNSS) array that was located on WIS and collected by Matthew Siegfried [5] for this project. The primary goal of my work was to create a repeatable processing technique that could be used in future studies to understand the patterns and drivers of Whillans Ice Stream movement events. 
## Problem Statement
In this project I sought to create a repeatable python-based procedure to process and understand data from a multiyear GNSS array that was maintained by Matthew Siegfried [5].  
## Methods
At the project’s initiation data from the GNSS array [5] were in a .pos file format. These files recorded location measurements every 15 seconds for each GNSS station throughout the array and included data related to attributes including the station name, time, date, latitude, longitude, height, uncertainty for each measurement. Each individual .pos file recorded data over a 24-hour period for a single station. The GPS array included temporal gaps where measurements were not taken which are believed to be caused by equipment malfunction or insufficient power supply. In this project I generated python functions that complete tasks including the import of each .pos file into a jupyter notebook, cumulation of station measurements into a single pandas dataframe, calculation of the displacement of each measurement, classification of whether a measurement was taken during a movement event, identification the start of the movement events, and visualization of the data results. 
## Importing .pos Files
The first step of the analysis was importing the .pos files into a jupyter notebook. I decided to import the files directly into a pandas dataframe in jupyter notebooks due to the computational efficiency, data searching functions, and visualization capabilities of the Pandas package. The .pos files were formatted similarly to a spreadsheet however columns did not have consistent spacing or delineation between columns. I thus imported the files as a fixed-width spreadsheet by specifying the number of figures that were characteristically in each column. While this import method was successful and produced positive results within the context of this project, it may need to be adjusted for future files if the width of columns in the .pos files change.  
## Cumulating Station Measurements
Each .pos file contained data from a 24-hour period. As GNSS observation stations were often maintained for periods greater than one year, there are hundreds or thousands of .pos files associated with most observation stations. A python function was created to combine the .pos files that were recorded by a single observation station into a cumulated pandas dataframe to observe movement trends and detect WIS movement events throughout the lifetime of each observation station. The resulting pandas dataframe was organized based upon each measurement’s reported data and time, and the indexes of the cumulative dataframe were reset to ensure continuity throughout. 
## Calculating Measurement Displacement
While the .pos files included columns that were designated as the decimal latitude and decimal longitude, analysis of those columns found that they were inconsistent with other data reported for each measurement. As the degrees, minutes, and seconds columns appeared to be more trustworthy, I recalculated the decimal form of the latitude and longitude of each measurement based upon the degree, minutes, and seconds columns. I then calculated the displacement of each measurement relative to the initial GPS measurement for the station using the Haversine Formula. I decided to use the initial station measurement as the reference location as there is uncertainty related to each GNSS measurement. In using a consistent reference location for each measurement, the relative displacement could be calculated without cumulating error from multiple measurements. 
## Detecting Movement Events
I was able to use a simplistic movement detection algorithm due to the stick-slip movement pattern that is characteristic of WIS. This detection algorithm included the exclusion of points that were recorded immediately before or after a temporal gap in the data. These points were excluded to ensure that the increased displacement that occurred with additional time were not reported as false positives in the detection algorithm. I simply calculated the mean location of the 50 previous and 50 following measurements and determined the difference between those two averages to detect movement while minimizing the effect of noise in the measurements. The point would be classified as occurring during a movement event when the difference between the prior mean and following mean exceeded a user inputted threshold and the prior mean was located nearer to the initial station measurement than the following measurement. This detection algorithm was created under the assumption that WIS is consistently moving along a single directional axis.
## Identifying Movement Initiation
The stick-slip movement pattern of WIS was again leveraged to create a very simple algorithm to pick these initial movement measurements. I considered the ten points surrounding each measurement to minimize false positive detections. For a point to be detected as the start of a movement event the five previous points were required to be labeled as occurring during a period of rest, and the point being analyzed, and the following five points were required to be labeled as occurring during movement events.
## Visualization
I visualized the data results in an intuitive manner. I first established a color-blind sensitive color palate that was created by Paul Tol [6]. I then plotted each measurement with color differentiating measurements that occurred during movement or rest periods based upon the decimal day of year since the year of the initial measurement and the total displacement relative to the initial measurement. I also plotted mean of the 50 previous and 50 following measurements for each point to provide evidence of why each point was classified as it was and a star symbol below the start of each movement event (See Figures 1,2). The function that generates each plot can be easily generated with user specification of the dataframe that contains the data being plotted, the initial point reported in the visualization, and the number of subsequent measurements to be reported. 
## Results
The processing and analysis generated a successful result that is consistent with prior observations of WIS’s stick-slip motion with one to two movements per day. The algorithms used did seem to report cases of false-positive detection of slip events. These false-positive classifications are likely due to the uncertainty in measurements causing the algorithm to identify movements when WIS was at rest as measurements that occur during motion (See Figure 2). After using the created functions to analyze two separate observation stations, the functions compile and run successfully on both stations. 
## Conclusion and Future Word
While this project did have a successful result, the detection algorithms can be improved, and the classifications could be optimized to reduce false-positive labeling of points as movement events and the initial points of movement events. The first steps in improving the detection algorithms could include further optimization of the user inputted threshold of the minimum difference between prior and following averages (as described in the Detecting Movement Events section), surrounding measurements when classifying the movement initiation measurements. More advanced detection algorithms could also be used that consider the standard deviation reported in the original .pos files to weight measurements compared to their surrounding neighbors, analyzing the files in the Fourier Domain to identify deeper characteristics of motion, or using a machine learning algorithm to analyze the measurements. Once a thorough evaluation and classification of the datasets is completed, application of this project may have many impacts upon our understanding of WIS movement trends and dynamics over time such as understanding the signature of movement events in the Global Seismic Network (GSN) to consider WIS movement events and trends that exceed the lifetime of this GNSS array.
 
 ![Picture1](https://user-images.githubusercontent.com/104653490/166343004-8e49a012-5aa4-4123-811a-f623fba44a4a.png)

Figure 1 Visualization of station LA02 showing the identification of a single slip event. 

![Picture2](https://user-images.githubusercontent.com/104653490/166343011-540ea36d-11ac-4547-ad35-7b57d8498501.png)

Figure 2 Visualization of a six-day period of station LA16. There are two false positive slip initiation points which are denoted by green (left) and orange (right) arrows. The point indicated with the green arrow was likely due to increased uncertainty in the measurement causing a temporary spike in the relative displacement and the point indicated by the orange arrow was likely due to noise decreasing the relative displacement of a measurements during the slip event causing the classification of two separate events.

## References

[1]	S. Choudhary, S. M. Saalim, and N. Khare, “Chapter 1 - Climate change over the Arctic: impacts and assessment,” in Understanding Present and Past Arctic Environments, N. Khare, Ed. Elsevier, 2021, pp. 1–14. doi: 10.1016/B978-0-12-822869-2.00011-6.
[2]	J. P. Winberry, S. Anandakrishnan, R. B. Alley, D. A. Wiens, and M. J. Pratt, “Tidal pacing, skipped slips and the slowdown of Whillans Ice Stream, Antarctica,” J. Glaciol., vol. 60, no. 222, pp. 795–807, 2014, doi: 10.3189/2014JoG14J038.
[3]	J. P. Winberry, S. Anandakrishnan, R. B. Alley, R. A. Bindschadler, and M. A. King, “Basal mechanics of ice streams: Insights from the stick-slip motion of Whillans Ice Stream, West Antarctica,” J. Geophys. Res. Earth Surf., vol. 114, no. F1, 2009, doi: 10.1029/2008JF001035.
[4]	M. J. Pratt, J. P. Winberry, D. A. Wiens, S. Anandakrishnan, and R. B. Alley, “Seismic and geodetic evidence for grounding-line control of Whillans Ice Stream stick-slip events,” J. Geophys. Res. Earth Surf., vol. 119, no. 2, pp. 333–348, 2014, doi: 10.1002/2013JF002842.
[5]	M. Siegfried, “WIS GNSS Data.” Colorado School of Mines, 2020.
[6]	Paul Tol, “Paul Tol’s Notes,” Paul Tol’s Notes, Aug. 18, 2021. https://personal.sron.nl/~pault/ (accessed Apr. 29, 2022).




## Data Availability
The jupyter notebook referenced in this document and .pos files explained in this project can be located at: https://github.com/matthewoleszko/IceDynamicsOfWIS.git
For additional data, contact Matthew Siegfried at siegfried@mines.edu


