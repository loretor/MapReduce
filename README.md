# Simulation of the MapReduce Paradigm in a distributed system though OMNeT++ 🖥️📤

## 🧑‍💻 Components of the team 
- Lorenzo Torri
- [Valentina Villa](https://github.com/valentinavilla)
- [Giorgio Vergona](https://github.com/GioVergona)

## 📌📂 Description 
Implementation through [OMNET++](https://omnetpp.org/) of the [MapReduce Paradigm](https://static.googleusercontent.com/media/research.google.com/it//archive/mapreduce-osdi04.pdf), which handles also the possible failure ❗ of the Worker Nodes.
- The [MapReduce](/MapReduce) folder is the OMNeT++ project  
- It is also available a .pptx [presentation](/Presentation.pptx) to show with animations the functioning of the algorithm.
This README file represents only a small view of what is explained inside the presentation.

## 👨‍🏫 Explanation of the Project
The idea of the project is to implement a failure safe distributed system, in order to process tuples of key and value.  
The input data (the list of tuples to be processed) is stored inside a [CSV file](/MapReduce/src/InputCoordinator/test.csv) where each line represents a <k,v> tuple.
The operators that can be applied to the list of all tuples are the following:
- map(f: int→int): input <k,v>, output <k,f(v)> 
- changeKey(f: int→int): input <k,v>, output <f(v),v> 
- reduce(f: list<int>→int): input list<v> of a single k, output <k,f(v)>   

where inside the map and the changeKey one of this functions can be written:
- ADD(int)
- MUL(int)
- SUB(int)
- DIV(int)

The sequence of operations is expressed in a [JSON format](/MapReduce/src/InputCoordinator/test.json) alongside the number of partitions in which the input data must be splitted into.

In this project we opted for an implementation similar to [Apache Spark](https://it.wikipedia.org/wiki/Apache_Spark), with a scheduling approach. Each operator can start its execution only when all the tuples have been processed by one of the Workers Nodes with the operator before.

## 🔎 Assumptions
- Workers may fail, Coordinator is reliable
- Links reliable
- Workers rely on local durable storages only for input data whereas intermediate results are lost if the Worker fails
- System relies on a durable non-faulty storage where the latest version of data is stored
- Workers may fail only after receiving an heartbeat from the Coordinator and before replying with an ACK
- Coordinator stops sending heartbeats to the Workers, until the recovery of a failed Worker has finished
- Final operator of the sequence must always be a reduce

## Explanation of the Algorithm
The explanation step by step of the algorithm and of the failure management❗ is very well documented and showed with some pptx animations in the presentation
