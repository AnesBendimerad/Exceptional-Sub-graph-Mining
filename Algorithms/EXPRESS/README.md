## Introduction
The algorithm EXPRESS is the subgraph sampling approach of exceptional attributed sub-graph mining (Section III-C). 

The folder "Release" contains the runnable JAR file. This can be directly executed. The folder "Code" contains the source code of the algorithm.  The compilation of this algorithm requires some libraries. These libraries are provided in "Code/lib".

## Inputs of EXPRESS
EXPRESS requires two input files. These files must be in the same repository as the runnable JAR. These files are described in what follows.

**1 - The graph file**: This contains the graph the algorithm will mine. It is a JSON file that defines the structure of the graph and the vertex descriptions. The listing below is a simple example of a graph file.
```json
{
	descriptorsMetaData : [
		{
			descriptorName: "Place Types",
			attributesType: "nominal",
			attributesName: ["Health","Tourism","Store","Food","Industry"]
		}
	],
	vertices : [
		{
			vertexId : "V1",
			descriptorsValues :[
				[12,5,4,1,4]
			]
		},
		{
			vertexId : "V2",
			descriptorsValues :[
				[12,4,4,6,6]
			]
		},
		{
			vertexId : "V3",
			descriptorsValues :[
				[4,13,6,6,5]
			]
		}
	],
	edges : [
		{
			vertexId :"V1",
			connected_vertices : ["V2","V3"]
		},
		{
			vertexId :"V2",
			connected_vertices : ["V1"]
		}
	]
}
```
First, this file presents information about the vertex descriptors (descriptorsMetaData). It contains the generic name of the descriptor, and also the name of the attributes. Second, this file defines the vertices and their attribute values. Third, it specifies the edges of the graph.

**2 - Parameters file**: This file contains the values of the algorithm parameters. It must be named "inputParameters.txt". The listing below shows an example of a file that specifies the parameters. If a parameter is not specified, the algorithm will use its default value. 
```
inputFilePath=NYCFoursquareGraph.json
delta=0.005
sigma=1
activateSMinus=false
removeRepetition=true
executionTimeInMS=200
```
These parameters are explained in what follows:
- inputFilePath: The path of the graph file.
- delta: The WRAcc threshold.
- sigma: The subgraph size threshold.
- activateSMinus: This determines whether the algorithm explores the negative contrasts or not.
- removeRepetition: This option allows to remove the repetition of the same subgraphs in the output.
- executionTimeInMS: This parameter specifies the time budget in milliseconds.

## How to launch EXPRESS
In order to execute the algorithm, the files provided in "Release" can be used. Before launching the runnable JAR, please make sure the two required input files are in the same folder as the runnable JAR. Then, the following command line can be used in this folder:
```
java -jar EXPRESS.jar
```

## The result files
When the program finishes the execution, it produces two files.

**1 - resultIndicatorFile.txt**: This file contains some information about the execution, as the execution time.

**2 - retrievedPatternsFile.json**: This is a JSON file that contains the retrieved patterns. The listing below shows an example of "retrievedPatternsFile.json".

```json
{
	"numberOfPatterns" :2,
	"patterns" : [
		{
			"subgraph" : ["V1","V2"],
			"characteristic" : 
			{
				"descriptorName" : "San Francisco Crimes",
				"positiveAttributes" : ["Vandalism","larceny"],
				"negativeAttributes" : ["Vehicle theft"],
				"score" : 0.0154
			}
		},
		{
			"subgraph" : ["V3"],
			"characteristic" : 
			{
				"descriptorName" : "Place Types",
				"positiveAttributes" : ["larceny"],
				"negativeAttributes" : [],
				"score" : 0.0134
			}
		},
	]
}
```