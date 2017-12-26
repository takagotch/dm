SAMPLE


# DataMining

DataMining is a little collection of several Data-Mining-Algorithms.
Since is written in pure ruby and does not depend on any extension,
it is platform independent.

## Alogrithms

#### Already implemented
1. Density Based Clustering (DBSCAN)
2. Apriori
3. PageRank
4. k-Nearest Neighbor Classifier

#### Coming soon
4. k-Means
5. Naive Bayes
6. ...

## Installation

  $ gem install data_mining

## Usage

#### For Density Based Clustering

```ruby
  require 'data_mining'

  #
  # Point with id 'point1', x-value 1 and y-value 2:
  # [:point1, [1, 2]]
  #
  input_data = [
                [:point1, [1,2]],
                [:point2, [2,1]],
                [:point3, [10,10]]
               ]
  radius = 3
  min_points = 2
  dbscan = DataMining::DBScan.new(input_data, radius, min_points)
  dbscan.cluster!

  dbscan.clusters # gives 1 cluster found containing point1 and point2

  dbscan.outliers # gives point3 as outlier
```

#### For Apriori

```ruby
  require 'data_mining'

  transactions = [
                    [:transaction1, [:product_a, :product_b, :product_e]],
                    [:transaction2, [:product_b, :product_d]],
                    [:transaction3, [:product_b, :product_c]],
                    [:transaction4, [:product_a, :product_b, :product_d]]
                  ]
  min_support  = 2
  apriori      = DataMining::Apriori.new(transactions, min_support)
  apriori.mine!

  apriori.results
  # gives the following array:
  # => [ [[:product_a], [:product_b], [:product_d]],
  #      [[:product_a, :product_b], [:product_b, :product_d]]
  #    ]
  # where position 0 in the array, holds an array of all single items which
  # satisfy the min_support. position 1, holds an array of all pair items
  # satisfying the min_support and so on as long as min_support is satisified.

  # Perhaps an easier way to get an item_set immediately:
  apriori.item_sets_size(2)
  # gives the following array, representing all item sets of size two, satisfying
  # the min_support:
  # [[:product_a, :product_b], [:product_b, :product_d]]
```

#### For PageRank

```ruby
  require 'data_mining'

  graph   = [
              [:node_1, [:node_2]],
              [:node_2, [:node_1, :node_3]],
              [:node_3, [:node_2]]
            ]

  pagerank  = DataMining::PageRank.new(graph)
  # we can also pass a damping factor, default is 0.85
  # DataMining::PageRank.new(graph, 0.90)
  # as well as the iterations to calculate the pagerank, default
  # is 100
  # DataMining::PageRank.new(graph, 0.85, 1000)
  pagerank.rank!

  pagerank.ranks
  # gives the following hash:
  # => {:node_1 => 0.2567567634554257, :node_2 => 0.4864864730891484,
  #     :node_3 => 0.2567567634554257}
  # where the key stays for the node and the value for the calculated
  # pagerank
```

#### For K-Nearest Neighbor Classifier

```ruby
  require 'data_mining'

  data  = [
            [:class_1, [1, 1]],
            [:class_1, [2, 2]],
            [:class_2, [10, 10]],
            [:class_2, [11, 12]],
            [:class_3, [12, 12]]
          ]
  k     = 2

  knn   = DataMining::KNearestNeighbor.new(data, k)

  knn.classify([:unknown_class, [2, 3]]) # gives :class_1 back

  # Since the given point of :unknown_class has the coordinates
  # (2, 3) for (x, y) and has therefore the following two points
  # as his 2 (k=2) nearest neighbors:
  #   [:class_1, [1, 1]]
  #   [:class_1, [2, 2]]
  #
  # And since all neighbors are of the same class (:class_1), the
  # majority of the k-nearest-neighbor classes is obviously also :class_1
```

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## License

(The MIT License)

