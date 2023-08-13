
# Potholes

The simulated potholes were made to mimick potholes that would be present at the Intelligent Ground Vehicles Competition 2022. The rules specified a **"Solid white circle with 2' diameter"**.

## Generating Synthetic Data

Using this technique of data creation was highly efficient for two reasons. Firstly, I was able to reuse a dataset of 1.6k images that were initially used to train a U-Net model to detect lane lines. By using the same dataset for potholes, I can save time on collecting data. Secondly, the YOLOv4 model requires a label around each object in the format of <code>\<x> \<y> \<width> \<height></code>. I can easily create a label for each pothole while generating them, saving time on labelling.


- In the <code>data</code> directory, add folders containing the input images. The script will then iterate through each of the images.
- For each image, a random number of potholes are generated (1, MAX_POTHOLES).
- The script finds a rectangular patch in the image, where the average color is grey (rgb value range from 15 to 180).
    - Only specific parts of the image are iterated through: Between 20%-80% on the horizontal axis and between the 70%-90% on the vertical axis.
    - These numbers were picked to guess where asphalt in a lane image is more likely to appear.
    - It also makes sure that potholes are not overlapping.

- After finding a suitable patch, an ellipse is generated with a grey color fill.
    - The greyness depends on the rgb values in the patch (Darker asphalt will have darker potholes).
- The location of the potholes in YOLOv4 format is then appended to a .txt containing all the potholes for that image.
    - The labelled image is stored in <code>processed/data/</code>.
    - The label is stored in <code>processed/label/</code>.

### Example Images

Randomely generated potholes in Toronto streets.\
![Randomely Generated Potholes in Toronto](https://github.com/Ammar-V/Potholes/blob/main/Images/Lane_Input_2773.png?raw=true)

Potholes with metaball noise to act as shadows.\
![Randomely Generated Potholes in Toronto with noise](https://github.com/Ammar-V/Potholes/blob/main/Images/Lane_Input_4195.png?raw=true)

Potholes with fine salt-pepper noise.\
![Randomely Generated Potholes in Toronto](https://github.com/Ammar-V/Potholes/blob/main/Images/Lane_Input_2234.png?raw=true)



## Splitting the data into train/test/val

(train, test, val) = (70%, 10%, 20%)

- Currently, the script simply takes data from <code>dataset/data/</code> and <code>dataset/label/</code> and moves it to <code>dataset/train/</code>, <code>dataset/test/</code>, or <code>dataset/val/</code>.
    - The images are chosen at random and therefore the images are shuffled.
