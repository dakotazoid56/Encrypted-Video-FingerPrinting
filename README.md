# Encrypted-Video-FingerPrinting
Classifying videos based on their streamed network packet transfer. We collected the data from UCSB network nodes using netUnicorn, preprocessed the data with Pandas, Scapy, Numpy, and modeled the data with Sklearn and Keras.

## Description

Replication of Beauty and the Burst: Remote Identification of Encrypted Video Streams (https://www.usenix.org/system/files/conference/usenixsecurity17/sec17-schuster.pdf)
and improved data collection process for model generalizability by using multiple nodes from UCSB PINOT infrastructure instead of one single node. 

### Change Branch to view different aggregation interval results


### DataCollection.ipynb
The DataCollection.ipynb builds a custom netUnicorn Pipeline where you can specify what videos to watch (we used Vimeo for add free content). All our data collection trials are in the dictionaries with labels capture_#. We used capture_4 to represent multi-node collection, and capture_8 to represent single node collecton. Both were 5 vimeo videos, watched for 60 seconds, 50 times per video. 


### DataModelEvaluate.ipynb
The DataModelEvaluate.ipynb file reads all the .pcap files that are created from netUnicorn in the DataCollection file. It then downloads each video .pcap file into a pandas dataframe with the time_interval as rows and 'up_bps', 'down_bps', 'up_pps', 'down_pps', 'up_plen', 'down_plen' as columns. Our graph display each run for the specified video, stacked ontop of each other to view simlilarity. Next, the entire concat df is run through the RandomForestClassifier, then it is run for each time interval, to see which interval has highest accuracy. (Range 20% to 94%). The highest accuracy time interval dataframe is ran on a trustee visualization report to visualize how the model makes decisions. Finally, the list of dfs (optionally cropped for most relevant time intervals) is fed into the Keras Convolutional Neural Network. 


concat: Each video dataframe combined vertically into one dataframe, used for the sklearn 2D RandomForestClassifier

dfs: A list of each dataframe, which is converted to numpy array time series in the Keras Convolutional Neural Network



## Results

As shown in DataModelEvaluate.ipynb, the single-node video streams outpreformed the model accuracy than the multi-node, which was expected. 


|     Collection Type    | RandomForest Accuracy | RandomForest Best Interval | CNN Accuracy |
| ---------------------- | ----------------------| -------------------------- | ------------ |
| Single-Node Collection |           0.64        |   40s-45s   0.9444         |   0.9444     |
| Multi-Node Collection  |           0.60        |   40s-45s   0.9091         |   0.9090     |





## Getting Started

- Must have snl-server access and netUnicorn access at UCSB
- Private information stored in Private.env (not included in repository) as follows

NODE_IP =  
SNL_SERVER_PATH =  
NETUNICORN_ENDPOINT =  
NETUNICORN_LOGIN =  
NETUNICORN_PASSWORD =  
PIPELINE_ENDPATH =  
WEBDAV_USERNAME =  
WEBDAV_PASSWORD =



### Dependencies

- netUnicorn, trustee, pandas, sklearn, keras, dotenv


