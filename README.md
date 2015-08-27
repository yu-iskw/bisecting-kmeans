## Bisecting K-Meams Clustering

This is a prototype implementation of Bisecting K-Means Clustering on Spark.
Bisecting K-Means is like a combination of K-Means and hierarchical clustering.

[![Build Status](https://travis-ci.org/yu-iskw/bisecting-kmeans.svg)](https://travis-ci.org/yu-iskw/bisecting-kmeans)

### Scala API

Those are the Scala APIs of Bisecting K-Means Clustering.
`BisectingKMeans` is the class to train a `BisectingKMeansModel`.
You could train a model with `BisectingKMeans.train` method.
And the class has a few parameters.

- `setK`: the number of clusters you want
- `setMaxIterations`: the number of iterations at each step
- `setSeed`: random seed

```
import org.apache.spark.mllib.bisectingkmeans.{BisectingKMeans, BisectingKMeansModel}
import org.apache.spark.mllib.linalg.{Vector, Vectors}

# Prepare for the input data
val localData = (1 to 100).toSeq.map { i =>
  val label = i % 5
  val vector = Vectors.dense(label, label, label)
  (label, vector)
}
val data = sc.parallelize(localData.map(_._2))

# Create an object for this algorithm
val algo = new BisectingKMeans()
  .setK(5)
  .setMaxIterations(20)
  .setSeed(1)

# Train a model
val model = algo.run(data)

# Get trained centers
val centers: Array[Vector] = model.getCenters

# Computes Within Set Sum of Squared Error(WSSSE)
val cost: Double = model.WSSSE(data)

# Convert a cluster tree into an adjacency list
val list: Array[(Int, Int, Double)] = model.toAdjacencyList

# Convert a cluster tree into a linkage matrix
val matrix: Array[(Int, Int, Double, Int)] = model.toLinkageMatrix
```

### Reference

"A comparison of document clustering techniques", M. Steinbach, G. Karypis and V. Kumar. Workshop on Text Mining, KDD, 2000. [pdf](http://cs.fit.edu/~pkc/classes/ml-internet/papers/steinbach00tr.pdf)

