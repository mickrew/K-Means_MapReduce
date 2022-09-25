# K-Means_MapReduce
Implementation in Hadoop and Spark of famous K-means algorithm into MapReduce framework. 

K-means clustering algorithm is an iterative algorithm; we can identify three main phases:
- **Initialization**: in this phase we define the input parameters to run the algorithm and we initialize the set of the centroids M choosing them randomly
- **Cluster assignment**: each data point is assigned to the nearest centroid
- **Updating the position of the centroids**: for each cluster we recalculate the centroid (using the points we assigned to it in the previous phase) with the formula 2
Then, the last two phases are repeated until a stop condition is reached. In our case, we decide to use a threshold (that we set from command line when starting the application) for the distance between the centroids of the last two jobs: when all the centroids of the actual job and the previous one, are less than the threshold, we say that we have reached convergence and the application terminates.

Thanks to the possibility of decomposing the problem into several parts, we can think about K-means algorithm into the **Map Reduce framework**. We have structured the Map and the Reduce phases as follows:
- In the **map phase** we have two functions: the setup function and the map function. The setup function reads the file where are stored the centroids and saves them locally. The map function takes a point and assigns it to a centroid.
- In the **reduce phase** we recalculate the new centroids and we check if the convergence condition is verified. In this phase there are two functions: the reduce function and the cleanup function. The reduce function performs the sum of the coordinates of all the points associated to a cluster during the Map phase, the cleanup function computes the calculation of the centroid of the cluster, taking the point calculated as the sum of all the points associated to the cluster and dividing its coordinates by the number of points that belong to the cluster. The cleanup function also takes care to check if the stop condition is reached.
