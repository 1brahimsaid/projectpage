# Simulation Study: Manufacturing Facility 🏭

## Discrete Simulation/Modeling

**Date:** Finished April 11th, 2021

## [Full Report and Simulink Files HERE](https://github.com/1brahimsaid/simulationstudy)

<p align="center">
  <img src="/img/model.jpeg"/>
</p>

### 1.0  PROBLEM FORMULATION

> At this manufacturing facility three types of products are created (P1, P2, P3), each requiring at least one or more component (C1, C2, C3) for its creation. Each product has a specified workstation associated with it, and only that workstation is responsible for that item. Before assembling the product, each workstation must wait for the required components, utilizing a buffer zone that has a maximum capacity of two for each assembly piece. Before reaching the buffer, two inspectors check the components and place them in the specified workstation only if the buffer zone is not full. Inspector 1 chooses the buffer zone with the least number of components waiting in the queue. In the event of a tie, Workstation 1, 2, then 3 is favored. Looking at this problem, a simulation appears to be the best method of determining the most ideal workflow as the different stations, priority, and components will create a significant amount of potential system states, where the ideal workflow will be difficult to determine mathematically. From the initial appearance it seems very likely that Workstation 1 will have higher rate of production than Workstation 2 or 3 as it only requires one component, disregarded each Workstation’s processing time. In event of a tie, Workstation 1 will possibly be lower in the priority list, as the increased priority will increase the production rate in addition to the already increased production from the one component requirement. 

### 2.0  OBJECTIVE

> The objective of simulating this problem is to identify the ideal behaviour of Inspector 1, and to determine the ideal priority of each Workstation in distributing Component 1 in a manner that benefits all product production. To determine the ideal workflow, Inspector 1’s criteria to distribute Component 1 must be changed, as well as the priority of each Workstation when a tie occurs. 

### 2.0 MODEL CONCEPTUALIZATION


<p align="center">
  <img src="/img/system.jpeg"/>
</p>

#### 1. Entities

**Products:** 
> The resultant output of each workstation, in the defined problem. Three potential products exist (P1, P2, P3).** 

**Components:** 
> The building blocks of each product and the recipe of components for each individual product (P1, P2, P3) differs and production cannot occur without the required components (C1, C2, C3). The supply of components is unlimited and will not slow down the production process. 

**Inspectors:**
> Each component is inspected by two Inspectors, each responsible for their own set of components (Inspector 1 = C1, Inspector 2 = C2, C3).  The inspector is responsible for placing the components into the buffer zones adjacent to the workstations. 

**Workstations:**
> This is where the components are assembled into products. Each product (P1, P2, P3) has a workstation associated with it (W1, W2, W3) where the respective product is assembled.**  

**Buffer Zone:**
> The component queue for the workstations to produce products. Each workstation can only hold a maximum of two components each, a total of five buffer zones exists for each product (P1, P2, P3).** 

#### 2. Essential Features

> The essential features of the system are the ones that limit the rate of production, which we will consider to be the inspectors, workstation, and buffer zones. The time each inspector takes to inspect each component is crucial, the differing times can change how we prioritize components for each workstation. However, while we consider the workstations as essential, they will only compose products at the rate defined in the data and can only be affected by the components supplied. No way exists to increase the rate the workstations process components other than supplying consistent assembly material. If the components are readily available in the buffer, the products will always be produced at the rate defined for each workstation. However, the processing times for each workstation are essential in the priority of component supply they will receive. Since the components are unlimited, we cannot consider it an essential feature or a bottleneck for the system, as it is readily available.  

#### 3. Assumptions 

> The first assumption of the system we make in order to simulate the system is the behaviour of Inspector 2. Since the Inspector 2 is responsible for two components, we expect the Inspector to select between Component 2 or 3 at random or some defined interval stated in the data set. Secondly, if the Inspector is blocked from placing components as the buffer zones are full, we reasonably expect Inspector 2 to move onto another component that is needed or if all buffer zones are filled hold onto the component without resuming a new inspection (this will apply to both inspectors).  

### 4.0  MODEL TRANSLATION

> We have chosen to use the scripting language MATLAB to develop our simulation model. MATLAB has a discrete-event simulation framework (Simulink) that will be used to facilitate the translation of the model into a simulation. It is a common programming language that is often used to machine learning, simulation and designs that require multiple iterations that simply cannot be computed by hand. It is also a language we are familiar with and one we employ in our 4th year projects. Simulink can compute the complex math and simulation required for the modules.  

> The distributions provided for the inspection times, component selection, workstation processing times and will be randomly sampled to create a distinct simulation of random events to test our potential workflow implementation. A script will be used to determine the characteristics of those random events to process them in way that will output statistical information about the quality and efficiency of our choices. Each potential event will be processed sequentially and as they occur. Events will be processed for each component with multiple checks for buffer limitations and workstation production. Events will be generated to indicate that a workstation has completed its product in order to advance to the next randomly generated event. Inspector idle times for when buffer queues are full will also be considered, this event can/will only be changed once a new product has been completed (new event) as that is the only way for buffer queues to clear up. Each event will only occur at the expected start time and our implementation will not attempt to create future events at any time, considering that the buffer queues will stay in the expected range of zero to two as only causal events will be considered. A script or addition will be created to validate the results from the simulation to determine that they are acceptable and not outlandish. This step is important as it will allow us to debug and create a simulation that is as accurate as possible. The simulation in the early stages will have many errors and we are not expecting it to be accurate to start. Identifying those errors and inaccuracies is key to testing and translating the model into a simulation. 

...return to [`projects.ibrahimsaid.ca`](https://projects.ibrahimsaid.ca/) or [`ibrahimsaid.ca`](https://www.ibrahimsaid.ca/).