# Model-View-Delegate（模型-视图-代理）

As soon as the amount of data goes beyond the trivial, it is no longer feasible to keep a copy of the data with the presentation. This means that the presentation layer, what is seen by the user, needs to be separated by the data layer, the actual contents. In Qt Quick, data is separated from the presentation through a so called model-view separation. Qt Quick provides a set of premade views in which each data element is the visualization by a delegate. To utilize the system, one must understand these classes and know how to create appropriate delegates to get the right look and feel.



一旦数据量超出简单的范围，就不再可行在表示层保留一份数据副本。这意味着用户所看到的表示层需要与实际内容的数据层分离。在Qt Quick中，通过所谓的模型-视图分离来实现数据与表示的分离。Qt Quick提供了一组预置的视图，其中每个数据元素通过一个代理来进行可视化。为了利用这个系统，必须理解这些类，并知道如何创建合适的代理以获得正确的外观和感觉。