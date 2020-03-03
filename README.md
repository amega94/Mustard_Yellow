
First, I will explain to you how Stackdriver Monitoring (SM) graph is constructed for a monitored resource -- in this case for topic/send_message_operation_count (equivalent to the number of messages) [1]. But before doing so, I will explain some important concepts related to Stackdriver Monitoring.

The SM metric model captures measurements in the following form [1] “[(value1, timestamp1), (value2, timestamp2), … ]” known as timeseries points. For your use-case each timeseries point [4] is as follows: (# of messages, timestamp). These samples are basically discrete-time values.
As can be seen in [3], in Metrics explorer, there are 3 options ‘Filter by’, ‘Group by’ and ‘Aggregator.’ For your use case, you need to ‘Filter by’ topic_id to get rid of all other unneeded time-series (plural). The ‘Filter by’ will leave you with a single timeseries that represents your Pub/Sub topic. 

Next is GROUP BY. GROUP BY divides the __set of timeseries__ (when a filter returns more than 1 timeseries. In your case there is only 1 timeseries returned!) into GROUPs to prepare them for the ‘Aggregator.’ The Aggregator aggregates timeseries (plural) data points to produce a single timeseries per GROUP. Since you FILTER leaves you with 1 timeseries, GROUP BY has no effect and for simplicity you should leave it empty (default.)

The Aggregator is used to aggregate multiple timeseries data points per GROUP and implements what is known as a reducing function: such as avg, min, max etc.. Since you only have 1 time series, for most cases the aggregator has no effect. For example avg(1 timeseries) = min(1 timeries) = max (1 timeseries), since there is only 1 datapoint. F for some reducing functions, for example, In For example, if you had 2 timeseries, and you apply ‘mean’ the aggregator will return the mean of the two timeseries in a single timeseries. Again since you only have 1 timeseries, ‘sum’ or ‘avg’ would not make any difference, and for simplicity its best left as default - none. 



So the million dollar question is “what is an aggregated data point”? An aggregated data point is a the abovementioned (value1, timestamp) aggregated over the alignment_period (which is by default 60 seconds) brought together in a single point using the aligner method.




[1] https://cloud.google.com/monitoring/api/metrics_gcp
[2] https://cloud.google.com/monitoring/api/v3/metrics
[3] https://cloud.google.com/monitoring/images/mip-me-instantiated.png
[4] https://cloud.google.com/monitoring/api/v3/metrics#model-intro-timeseries

Lastly, I find that representing discrete-time values in a continuous time graph is a bit confusing for interpretation purposes and therefore I personally recommend that you use XXX.
