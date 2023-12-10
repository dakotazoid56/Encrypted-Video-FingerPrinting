# Encrypted-Video-FingerPrinting
Classifying videos based on their streamed network packet transfer. We collected the data from UCSB network nodes using netUnicorn, preprocessed the data with pandas and wireShark, and modeled the data with Sklearn and PyTorch.

## Description

The DataCollection.ipynb builds a custom netUnicorn Pipeline where you can specify what videos to watch (we used Vimeo for add free content). Additionally, you can customize the capture time, the number of nodes streamed from, the number of streams per node, the endpoint folder, the run number (for multiple runs for same capture_#), and the endpoint save folder where the .pcaps get saved to. For our single-node model, we used Capture_8 which represents 5 vimeo videos, streamed for 60 seconds, on 1 single node. For our multi-node model, we used Capture_4 which represents 5 vimeo videos, streamed for 60 seconds, across 50 random nodes (20 then 30).

The DataModelEvaluate.ipynb file reads all the .pcap files that are created from netUnicorn in the DataCollection file. It then downloads each video .pcap file into a pandas dataframe with the time_interval as rows and 'up_bps', 'down_bps', 'up_pps', 'down_pps', 'up_plen', 'down_plen' as columns. The video dataframes are then all set to the same length, then displayed in graphs. One graph displays each run for the specified video, stacked ontop of each other to view simlilarity. This is done for each streamed video, and a column must be choosen ('down_bps' was what we chose). Then the data is divided for the different models as follows:

concat: Each video dataframe combined into one dataframe, used for the sklearn 2D RandomForestClassifier

dfs: A list of each dataframe, which is the .pcaps processed in memory, used for the 3D pyTorch Convolution Neural Network

*** The list of dfs, because of the 3D complexity must be processed in memory, but the concat data can be stored as .csv and once processed, accessed quickly ***


Next, the concat df is run through the RandomForestClassifier. The first classifier run is the overall accuracy of the model, then it is run for each time interval, to see which 5 second time intervals are most usefull. (Range 20% to 94%). We took the highest accuracy time interval dataframe, and ran the trustee visualization report to see how the model was making decisions. 


Finally, the list of dfs (optionally cropped for most relevant time intervals) is fed into the pyTorch Convolutional Neural Network. 

## Results

As shown in DataModelEvaluate.ipynb, the single-node video streams outpreformed the model accuracy than the multi-node, which was expected. 


|     Collection Type    | RandomForest Accuracy | RandomForest Best Interval | CNN Accuracy |
| ---------------------- | ----------------------| -------------------------- | ------------ |
| Single-Node Collection | Row 1 Column 2        | Row 1 Column 3             |              |
| Multi-Node Collection  | Row 2 Column 2        | Row 2 Column 3             |              |





## Getting Started

- Must have snl-server access and netUnicorn access at UCSB
- Private information stored in Private.env (not included in repository) as follows

NODE_IP = ''
SNL_SERVER_PATH = ''
NETUNICORN_ENDPOINT = ''
NETUNICORN_LOGIN = ''
NETUNICORN_PASSWORD = ''
PIPELINE_ENDPATH = ''
WEBDAV_USERNAME = ''
WEBDAV_PASSWORD = ''

### Dependencies

- netUnicorn, trustee, pandas, sklearn, pyTorch, dotenv


