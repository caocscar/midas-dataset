# MIDAS Two Month Dataset Notes

Date Range: Mar 1, 2013 (TripStart 41334) to Apr 30, 2013 (TripStart 41394)

Description|Count|
---|---
Vehicles|106
Trips|26425
AVI Videos|488 GB
BsmP1|26 GB
DataDas|49 GB
Relations Table|18 GB

~|BSM|No BSM|Total
---|---:|---:|---:
Video|16807|9339|26146
No Video|25|254|279
Total|16832|9593|26425

~|DAS|No DAS|Total
---|---:|---:|---:
Video|26102|44|26146
No Video|173|106|279
Total|26275|150|26425

# 4Vehicle Dataset

The 4 vehicles are: 10204, 10205, 10207, 10502

Description|Count (Size)
---|---
Vehicles|4
Videos|11030
AVI Videos|242 GB


# How to Construct AVI Videos from IVBSS viewer video
1. Find existing forward video bin file on UMTRI server using the **device and trip id**. You will need server access for this step.
2. Use Mich's `MakeAviFile.exe` Windows executable to convert this customized file to standard AVI file (manual step). You will need to install the software first on a Windows machine. The program will construct an AVI file from the individual frames in the file with the timestamp you specified in fps. 
3. Query *IndexForward* table in *SpFot* database to find closest `VideoTime` to given timestamps (centiseconds). You will need server access for this step.
4. Find associated `ForwardCount` (frame number).
5. Subtract offset frame number (assuming it doesn't start at frame 0).
6. Knowing the fps and the frame number, we can specify the appropriate timeframe in seconds as the offset frame number divided by 10 for the `ffmpeg` command. You will need to have `ffmpeg` installed beforehand.

For example, the following command specifies a window from 779.1 to 783.7 seconds since we know the AVI was created at 10 fps and we are interested in frames 7791 to 7837 (which corresponds to `ForwardCount` 7800 and 7846, respectively).
`ffmpeg -i Forward_10148_1496.avi -ss 779.1 -to 783.7 -c copy Forward_10148_1496_78180_78650.avi`

**Note**: Frame numbers will usually not align with VideoTime (cs).
