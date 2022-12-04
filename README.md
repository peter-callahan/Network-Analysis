# Network-Analysis

## Dependencies

This analysis uses Python, Jupyter Notebooks, Matplotlib and relies heavily on NetworkX, a library for interacting with networks in Python.  Additionally, a Python module that performs community detection was also valuable in performing this analysis  as it provided a clustering capability out-of-the-box.

## Introduction

The focus of CS-5283 has been on computer networks but the network data structure is present in many other domains.  Some examples include social media networks, geospatial data, process flow modeling, and distributed computing/coupled systems.  Due to the widespread use of this structure an understanding of the various characteristics of networks is essential.  Consider how we would speak about row-based data as having x number of columns or y number of rows or some percentage of null values.  We can describe categorical variables through the concept of cardinality and can cluster row based data, approximate nulls, and calculate summary statistics.  A network can be described in similar ways though characteristics unique to the data structure.  A network can have a shape, it can be directed (move in one direction) or undirected (move in both directions).  Networks can have pathways, clusters (communities), dead ends, and bottlenecks, and none of these are obvious at first glance.  Extracting these characteristics require in-depth analyses and once the patterns are visible we may glean new insights.

## Purpose
This project explores different network clustering methods and seeks to extract hidden properties related to network resiliency.  It relies on academic articles and online resources, cited where appropriate. 

## Data
50 networks, or graphs, of random shapes and sizes were created using a graph generation process.  By randomly assigning connections between nodes based on a fluctuating "probability of connection" value, and randomly selecting the number of nodes in a network, I was able to generate a realistic and diverse series of networks to use in this analysis.  Random generation sometimes resulted in networks which were not connected to each other, a situation that needed to be handled since this analysis assumes a single connected network.  To resolve this issue any network that included unconnected pieces was trimmed of those smaller pieces.  This created a smaller but continuous network to use in the subsequent analysis. 

The "probability of connection" was as important as node quantity when creating synthetic network data.  See the examples below which illustrate the various properties that come with different levels of network connectivity and node quantity.  Networks with many connections have fewer bottlenecks (fig 4.1, 4.2).  Highly interconnected networks with a large number of nodes look more like blobs than a traditional network, and the concept of clustering starts to break down (fig 4.2).  Conversely, a dense network with fewer connections tends to create a daisy chain of single failure points (fig 2.3) along the network.  A sparser network with fewer nodes is what comes to mind for most people and this type is also included (fig 1.3).  

50 networks, or graphs, of random shapes and sizes were created using a graph generation process.  By randomly assigning connections between nodes based on a fluctuating "probability of connection" value, and randomly selecting the number of nodes in a network, I was able to generate a realistic and diverse series of networks to use in this analysis.  Random generation sometimes resulted in networks which were not connected to each other, a situation that needed to be handled since this analysis assumes a single connected network.  To resolve this issue any network that included unconnected pieces was trimmed of those smaller pieces.  This created a smaller but continuous network to use in the subsequent analysis. 

The "probability of connection" was as important as node quantity when creating synthetic network data.  See the examples below which illustrate the various properties that come with different levels of network connectivity and node quantity.  Networks with many connections have fewer bottlenecks (fig 4.1, 4.2).  Highly interconnected networks with a large number of nodes look more like blobs than a traditional network, and the concept of clustering starts to break down (fig 4.2).  Conversely, a dense network with fewer connections tends to create a daisy chain of single failure points (fig 2.3) along the network.  A sparser network with fewer nodes is what comes to mind for most people and this type is also included (fig 1.3).  

<< network_visuals

## Community Detection 

Community detection, or clustering, was performed using a package called CDlib, which stands for Community Discovery Library.  This package supports a number of algorithms which could be used to detect communities inside the network and additional online resources  provided a useful overview of the various algorithms and their use cases.  I explored the various algorithms briefly, noting some similarities between methods of community detection. 

The takeaway from community detection was that algorithm selection is critically important.  Visual inspection shows some algorithms produced starkly different results and so the underlying assumptions of each algorithm including expected use cases, must be well understood before adopting it in practice.  Failure to do this, or flipping casually between algorithms, would have either incomprehensible or undesirable results.

<< community det

## Network Resiliency Analyses

To test the resiliency of a network one method discussed in the literature was to randomly remove one node from the network and check to see if the network disintegrates .  In the context of a computer network, an analysis like this could be used to reveal a breakdown of the system (internet) as a whole.  This would be dependent upon the number and location of nodal failures the system could endure; real work example would include things like hardware failures or network cuts.  A computer network that failed after a single router failure could not be considered resilient.  Contrast that with a more resilient network which continued to function despite the loss of a large portion of its components.

One question emerges quickly, what exactly does it mean to say that a network has disintegrated?  One way is to check if a network breaks into two unconnected networks after a node is lost.  Testing revealed this was an insufficient definition.  While eliminating nodes, if a node near the network edge were lost the network would separate into two parts, one large network and one small network of one or two nodes.  The majority of the network would clearly remain intact while the network would be declared disintegrated prematurely.  See the figure below.  To prevent this I added an additional measure to capture network "mass loss."  This is the total percentage of the network (in nodes) which was lost.  I redefined the notion of network disintegration to mean that a network must break into more than 1 network and lose more than 30% of its mass, as measured from the largest remaining network conglomerate.

<< net disintegration

## Resiliency Analysis of Different Network Types

Expanding the network resiliency analysis to additional network types revealed new insights.  First, highly interconnected networks proved to be more resilient due to the increased number of redundant connections, which prevented network fragmentation.  Second, by running a network resiliency analysis multiple times (since nodes were removed at random) I was possible to locate vulnerabilities in some networks.  Networks that disintegrated when a lower number of nodes were removed had such vulnerabilities.  Even without a sophisticated method to target nodes, which were selected randomly, enough iterations of the network resiliency analysis resulted in identifying weak points to a surprising extent.    

<< net performance

## Conclusions

2000 iterations of the network resiliency analysis were run for each of the networks expressed in the visuals above.  Note in the first example, the average number of nodes removed (per iteration) was 19.8.   However there is a scenario where removing only 6 key nodes reduced the size of the network from 132 to 88, resulting in network disintegration.  Additionally, we can see from the visual above (top left) that of the 6 critical nodes, 2 existed on the network edge and are clearly not critical.  Excluding these results in identification of 4 critical nodes, without which the network cannot continue to operate.  Said another way, this network is exposed to a worst case scenario and contains a substantial weak point.  Contrast this with the second example where 88 nodes were removed from the network in the worst case without the network reaching the disintegration threshold.  The average number of nodes removed per iteration is also 88, indicating no weak point or bottleneck.  The interconnectivity of the second network allows the network to sustain high node loss and maintain continuity. 











