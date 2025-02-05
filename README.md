# Demonstration of a generic hands-on deep learning method for anomaly detection in log data

## Overview

The `log_data_analyzer.ipynb` notebook demonstrates how unexpected subsequences (anomalies) can be found in sequential text data. The example implementation uses [software logs](https://en.m.wikipedia.org/wiki/Logging_(computing)).

Logs can be very helpful in understanding the behavior of a new system, application or environment we have only recently started to work with. The latter situation involves some incremental learning process - from human and ... possibly machine standpoint. That is, machine learning (ML) can be utilized as a powerful tool in log data analysis.

Log messages, in general, are very specific to the activities being logged and thus can contain numeric data, so anomaly detection methods based on [natural language processing (NLP)](https://en.wikipedia.org/wiki/Natural_language_processing), including the [bag-of-words (BoW)](https://en.wikipedia.org/wiki/Bag-of-words_model) model, can be applied only to a limited extent. For the purpose of genericity, the most important *component* of a log to be analyzed by this notebook's method is the *sequence* of log messages. So, a log is converted to a sequence of *message types* rather than messages.

|  Log             |
|------------------|
|  Message type 1  |
|  ...             |
|  Message type n  |

**In particular, this notebook demonstrates a hands-on, generic in its simplicity, method for finding anomalies in a potentially large amount of logs by utilizing [k-means clustering](https://en.wikipedia.org/wiki/K-means_clustering), regular expressions and [recurrent neural network (`RNN`)](https://en.wikipedia.org/wiki/Recurrent_neural_network).**

The implementation includes three main tasks:
1. application of an [unsupervised](https://en.wikipedia.org/wiki/Unsupervised_learning) clusterization method via `k-means clustering`, based on [singular value decomposition (`SVD`)](https://en.wikipedia.org/wiki/Singular_value_decomposition) on potentially high-dimensional sparse data;
1. transformation of the clustered log message data into unique per-cluster regular expression patterns that represent each log message as input to the `RNN` model;
1. application of a [semi-supervised](https://en.wikipedia.org/wiki/Weak_supervision) anomaly detection method via `RNN`, based on the following step-by-step algorithm:
    1. training process to learn possible log message sequences in already available normal log data (the supervised part);
    1. next log message predictions on new log data;
    1. [optional] if in step 2 a next log message cannot be predicted based on what is learned in step 1, a human intervention is needed to determine whether this is an anomaly or just a new log message to be learned (the unsupervised part);
    1. [optional] if in step 3 are found new normal log messages, go to step 1, else: go to step 2.
