
## Brief Description 
* The system comprises two main modules, the `analysis-engine` and the `query-engine`. As their names imply, the first is responsible for the data analysis and statistical information extraction, while the latter is responsible for capturing custom user search criteria(s) and handing them to the analysis engine for processing.
* The `analysis-engine` encapsulates two applications the `SimulationApp` and the `AnalysisApp`. The first generates a sample test data file for 500 Million record and writes them to a file. The name of the file comes from the `application.conf`, while the number of records is statically specified in the `SimulationApp.scala`
  The file size reaches 34GB for the 500 million records and takes good half an hour to be generated.

## Running the Application
* To Run the application, the `SimulationApp` should be kicked-off first in order to generate the sample data file. Currently the number of records is written statically inside the `SimulationApp` and can be changed from there. To make the execution time of the data generation process reasonable, the number of generated records has to be reduced. 
* Copy the generated file and put it under the `analysis-engine` resource directory. The names of the file should be the same for both apps in the the `application.conf`
* Run the `AnalysisApp`
* While the `AnalysisApp` is running, run the `QueryApp` in the `query-engine` module. Follow the instructions on the command line and start passing search arguments to the `query-engine`. 
Any combination of *MaxTemp, MinTemp, MeanTemp, TotalDist, Observatory, Observations* separated by a `|` should be accepted. 
* The required information will be dumped to a file under the project root directory.
* If query is submitted while the `AnalysisApp` is still running *normally will happen if the data size is too large*, the query result would then just reflects the data analysed up until the point when the query has reached the `AnalysisApp` to crunch the data and extract the results. If the same query is resubmitted, a different is expected to show up in the output_file as the `AnalysisApp` will have progressed further, and more data will have already passed the analysis.
* The query submitted has to be in the form `command1|command2|command3` where a command can be one of the following: 
`MaxTemp|MinTemp|TotalDist|Observatory|Observations`. different commands can come in any order, and they have to be separated with a `|`  
 
 
## Assumptions / Simplifications:
* The calculation of the `Distance Travelled` does not take into account the distance between the last record in a batch and the first one in the successive batch.
* The records in each batch are assumed to be preceding *in time*, the records enclosed in the successive batch. 
  That is, records of batch number `n` maps to observations taken before those enclosed in batch `n+1`. 
  While records are considered unordered within the same batch of records, they are assumed ordered compared to the successive batches. 

## TODO
* Sample Feed File, generated by the `SimulationApp`, is created under the root project path. 
  It then has to be copied manually under the `analysis-engine\resources` to be read by the `analysis-engine` module. Ideally, this manual file copy process has to be omitted and the simulation file should be 
  Read and Written from the same place.
* Write Unit and Integration tests for the whole application.
* Implement Requirement Number 3, *Producing a normalised version of the input file*
* Make the desired number of records to be generated by the `SimulationApp` configurable.
* Improve the implementation of the `SimulationApp` to run in different threads to make the data generation time more reasonable, especially for huge data size above 10 million records