## Localization
- Classification outputs if something is present.
- Classification with localization outputs the presence and the location.
- Rather than just having your image give a binary classification or a probability of classification you additionally have it output coordinates.
- For you output you can have a vector where:
 - the first element is the presence of ay of the the objects. If any object is found it's a 1 otherwise 0.
 - the next four are the center coordinates, width, and height.
 - The final elements are a a softmax or hardmax of the classifications you are checking for.
 - If the first value is a zero all other values are None or equivalent
- The loss function could be a summation of the losses for losses for each element of the vector if y[0] = 1 and a mean error of just the first element if y[0] = 0.
  - A good loss function:
    - for the max classifications is a log like.
    - Squared error for the bounding box coordinates.
    - A logistic regression loss for the initial classification.

## Landmark Detection
- Similar to detecting the bounds of an object but instead you might output the coordinates of particular landmarks of a point such as the peak of a mountain, the corner of an eye, the center of the knee etc.

## Object Detection
- The input for an object detection algorithm should be made up of images that are closely cropped to the object that you are trying to
- You then use a sliding window detection system to check small areas of the image to check to see if they contain the object you are looking for.
- You start with a small window and step it across small increments horizontally and vertically. You then increase the size of the window and repeat the process.
- A disadvantage of the sliding windows approach is the computational cost. Using a large step size will decrease the computation cost but will also likely decrease the accuracy of the model.
- You can modify fully connected layers to be convolutional layers by modifying the layers of the filter so that the output is a $1\times1$ with as many channel as you wanted in the fully connected node.
- Instead of using sliding windows you can use a fully convolutional neural network with max pooling and in a single pass calculate perform object detection on all the windows of the image.
- To improve the accuracy of your bounding boxes you can perform the YOLO (You Only Look Once) algorithm:
  - Segment your image into a grid.
  - Perform simple image detection as describe in the beginning of these notes on each grid section.
  - The classification of each object is assigned to the midpoint of the section in which it is located.
  - In practice this is performed by running a CNN that outputs a matrix that has the dimensions of your desired grid and the depth corresponds to your classifications (i.e. if an object is detect, center, height, width, presence of each type of object).
## Intersection Over Union (IOU) Function
- This is the intersection of two areas divided by the union of the two areas.
- It is used as measure of the accuracy of an estimated bound.
- It is typically considered correct if it is greater or equal to 0.5. This threshold can be adjusted depending on your desired accuracy.

## Non-max Suppression
- Is a way of making sure that each object is only detected once.
- The highest probability detection is first selected then all of detections that overlap are suppressed. This repeats until there are no more non-max detections.
- When you are detecting for multiple classes you independently perform non-max suppression on each classification layer.

## Anchor Boxes
- Anchor boxes are a method for detecting multiple classes in the same grid cell.
- You create anchor boxes which are different shaped bounding boxes. Each object detected in a cell is assigned to the cell and bounding box that most closely matches the dimensions of the object. This way if two different object have different bound shapes they can both be detected.
- The output of a such a classifier will be the same multi-layered vector output as seen previously but there will be a series of item in the vector corresponding to each anchor box.
- Anchor boxes don't help if your different objects have the same general bounding box.
- Anchor boxes are typically chosen by hand.
- You can also use a K-means algorithm to group together the types of object shapes that are present in your data.

## YOLO in Practice
- Yolo only requires a single pass of forward propagation.
- The output of the YOLO will be an $numGridCellVert\times numGridCellHoriz \times (numClasses + 5) \times numAnchorBoxes$ matrix. The 5 is for the detection of any of the classes, then center point, and the dimensions of the bounding box.
- You use the inputs to train a ConvNet that outs the correct dimensions.
- Get rid of all prediction that have an IOU below a tolerable threshold.
- You run the results through non-max suppression.
- For each class use non-max suppression to generate final predictions.

## Region Proposal
- This was proposed with am algorithm called the R-CNN.
- The algorithm runs a segmentation algorithm to find regions in the image.
- It classifies one regional at a time.
- Rather than using a moving window algorithm to determine where you run your convnet you run it only on bound that cover the different regions of the image.
- This algorithm is quite slow.
- A later proposal was the "Fast R-CNN" which uses a convolutional implementation of sliding windows to classify all the proposed regions and then the "Faster R-CNN" which uses a CNN to propose regions.
