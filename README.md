## Overview
Hello there! Welcome to the GitHub page for the JCURA 2024 project "Mountains of Confusion: Evaluating Image Enhancement to Improve AI Landscape Classification" by Larissa Bron. I wanted to expand the condensed format of an academic poster using GitHub in lieu of another published formats to communicate my experimental methods while providing a repository of the code, data, and image sets. 

## Background
The [Mountain Legacy Project](https://mountainlegacy.ca/about) (MLP) works with the largest systematic collection of mountain photographs in the world ([browse the collection](https://explore.mountainlegacy.ca/)). More than 10,000 historic images have been repeated to quantify landscape change over time. 

The pairs of historic and repeat images are used to quantify how the distribution of ecosystems across the landscape have changed over time. The work flow for this comparison begins with pixel-by-pixel mapping of ecosystems in an image. This manual annotation of images is a time-consuming task that has considerably held back scaling up the research team's investigation of the Canadian Rockies. 

To address this bottleneck, the [Python Landscape Classification](https://github.com/scrose/pylc) (PyLC) tool was developed to automate ecosystem classification using semantic segmentation, an Artificial Intelligence (AI) technique. PyLC is a proof-of-concept tool that has a high accuracy when working with images similar to the training data set, though is inconsistent in new image locations. Due to the large size of the image collection and diversity of ecosystems represented, the MLP team is working to improve PyLC to better generalize to the image collection through experimenting in different kinds of updates, such as increasing training data and changing the model's architecture.  

This project began in Summer 2023 when I worked alongside Kristyn Lang on improving PyLC as undergraduate research assistants. While Kristyn approached improving PyLC through updating the training process, I experimented with image enhancements to improve PyLC accuracy, with the hypothesis of "Improving the appearance of ecosystems in images by applying image enhancements will clarify ecosystem characteristics, improving PyLC accuracy for classification." This hypothesis was developed following MLP researcher experience since 2013 in which manually annotating ecosystems in images was made easier by changing image conditions to better visualize the differences between ecosystem types. For the JCURA component, my goal was to gain experience in coding with Python and R for data visualization and analysis. 

## Methods

### 1. Updating the PyLC Test Image Set
PyLC was trained using 95 images from two researcher's previous efforts, [Fortin (2018)](https://dspace.library.uvic.ca/items/0a911eb0-53bf-4a82-a75a-8b6949c28edd) and [Jean et al. (2015)](https://ieeexplore.ieee.org/document/7045940), and then tested with 19 images. The test images were a combination of 11 images from Fortin and Jean withheld from training and 8 images from the Landscapes in Motion project [(Higgs et al., 2020)](https://friresearch.ca/publications/advances-visual-applications-visualizing-quantifying-landscape-change-sw-alberta-using). In summer 2023, we considered whether the test image set was giving a good indication of PyLC accuracy due to the high similarity of the Fortin and Jean testing images to the training set, as well as the different quality of the Higgs et al. landcover reference masks. 

The testing image set was updated to: 1) Incorporate images from more geographic areas to improve understanding of the generizability of PyLC to the MLP collection, and 2) Remove images with reference land cover masks with large errors to assess whether this was impacting PyLC accuracy. The updated "2023 Test Image Set" containing images and land cover masks are [hosted on Zenodo](https://zenodo.org/records/10827942?token=eyJhbGciOiJIUzUxMiJ9.eyJpZCI6ImQxZmJjNThlLTBhYmMtNDFlNC1hNzEyLTRmN2Q5ZDBmYjk0NCIsImRhdGEiOnt9LCJyYW5kb20iOiJiMzBkZDJiOGRiOTE1YjQ3NmQ1YzlmYjE4ZWI0YjhmOSJ9.pIpAVBCVxQtuO7YLUgFzyJqd7uvoYQ80QfVYuiDsXcXl5Kbmhhr6bybNTYg-6S0n2dsBEUZjGR-lR6-2Vr1ZOA). 

### 2. Applying Image Enhancements to the Test Image Set
Three common categories of image enhancements involve adjustments to contrast, sharpness, and noise in images ([Liu, 2022](https://www.sciencedirect.com/science/article/abs/pii/S1051200422001646#se0070); [Pei, 2023](https://www.sciencedirect.com/science/article/pii/S0264127523005014#b0140)). Within these three categories, enhancements were chosen that can be implemented using a Jupyter notebook, require limited computer processing, have available and easy to use code, work on our image collection, and have support from previous research papers. The nine methods I chose were within the many approaches to image enhancements, as reviewed in [Gonzales and Wood (2019)](https://dl.icdst.org/pdfs/files4/01c56e081202b62bd7d3b4f8545775fb.pdf), [Liu (2022)](https://www.sciencedirect.com/science/article/abs/pii/S1051200422001646#se0070), and [Qi (2022)](https://link.springer.com/article/10.1007/s11831-021-09587-6). 

The resulting [Image Enhancement Jupyter notebook](https://github.com/larissaissabron/JCURA_PyLC_2024/blob/main/JCURA2024_ImageEnhancementCode.ipynb) can be used with the [test image set](https://zenodo.org/records/10827942?token=eyJhbGciOiJIUzUxMiJ9.eyJpZCI6ImQxZmJjNThlLTBhYmMtNDFlNC1hNzEyLTRmN2Q5ZDBmYjk0NCIsImRhdGEiOnt9LCJyYW5kb20iOiJiMzBkZDJiOGRiOTE1YjQ3NmQ1YzlmYjE4ZWI0YjhmOSJ9.pIpAVBCVxQtuO7YLUgFzyJqd7uvoYQ80QfVYuiDsXcXl5Kbmhhr6bybNTYg-6S0n2dsBEUZjGR-lR6-2Vr1ZOA). The resulting images that I worked from each image enhancement with are available from MLP in the "PyLCShared2023" folder. 

### 3. Testing Enhanced Images with PyLC
The testing set (unenhanced baseline) and the nine enhanced versions of the testing set were each individually ran through PyLC using grayscale model 2-3 through the command line (Terminal) on a Mac. After running images through, PyLC provides a landcover mask for each image alongside accuracy readouts that compare PyLC's masks to the manually annotated reference masks. Each image set was run through twice to get accuracy readouts that were individual to each image (no flag) and averaged over each image set (--aggregate_metrics flag). 

If you would like to try using PyLC, there are instructions on the [PyLC GitHub](https://github.com/scrose/pylc) for running testing, with some further clarification in [this guide](https://github.com/larissaissabron/JCURA_PyLC_2024/blob/main/Guide_PyLC%20Testing%20and%20Training.pdf) for running PyLC on the remote Compute Canada server or a personal computer. 

### 4. Comparing PyLC Accuracy Between Image Sets
PyLC accuracy metrics were compared at two nested levels with each image: 1) Individual land cover categories, and 2) Averaged over an image. The reported PyLC accuracy metrics for individual images is an un-averaged F1 score for each land cover category and the averaged accuracy metrics considering all land cover categories for the image. The averaged accuracy metrics are Matthew's Correlation Coefficient, Weighted Average F1 score, and an Inverse-Weighted Intersection Over Union. 

I chose to compare the median accuracy for land cover categories and the median averaged accuracy metrics separetly using boxplots in R because my accuracy metrics did not easily fit into a standard statistical test. When I followed an undergrad stats flowchart (ex., [Stats and R Flowchart: What Statistical Test Should I Do?](https://statsandr.com/blog/what-statistical-test-should-i-do/)), I was lead to using the Wilcoxon Sign-Ranked Test, yet I wasn't able to satisfy all of the model assumptions ([Wilcoxon Sign-Ranked Test](https://pythonfordatascienceorg.wordpress.com/wilcoxon-sign-ranked-test-python/)), leading to low confidence in the output. This realization also lined up with re-learning that null hypothesis significance testing might not tell us much about data at this stage (Ex., [Johnson, 1980](https://www.jstor.org/stable/3802789)), so I opted to visualize the output instead. 

Comparing the accuracy metrics required data cleaning where first the .json accuracy metric files from PyLC were converted to .csv files using [this code](https://github.com/larissaissabron/JCURA_PyLC_2024/blob/main/JCURA2024_PyLCAccuracy_JSONtoCSV.ipynb) for each image set. Then, two .csv files were collated and prepared in the "tidy" data format for R: 1) [Averaged accuracy metrics](https://github.com/larissaissabron/JCURA_PyLC_2024/blob/main/image_mcc_wmf1_iwmiou.csv), and 2) [F1 scores for land cover categories](https://github.com/larissaissabron/JCURA_PyLC_2024/blob/main/img_en_lccf1.csv). The datasets were visualized multiple ways using R Studio and then [boxplots were created for the JCURA poster](https://github.com/larissaissabron/JCURA_PyLC_2024/blob/main/JCURA2024_BoxplotsRScript.R) as they most clearly showed how PyLC accuracy fluctuated with each image enhancement. 

## Results
Comparing whether PyLC accuracy changed with each image enhancement involved a qualitative review of the PyLC generated land cover masks and a quantitative assessment of the median accuracy. 

The qualitative review of PyLC land cover masks showed that each enhancement changed how PyLC interpreted each image, however it wasn't clear whether there was an improvement to whole images or any land cover category from the enhancements. 

The quantitative review of the median accuracy supported these findings as each image enhancement only changed each accuracy metric in either a neutral or negative way. For the land cover categories, no image enhancement consistently improved the F1 score which is opposite to the goal of getting each category to achieve F1 scores comparable to Not Categorized and Conifer Forest. As well, even though the averaged accuracy metrics use different weighting to quantify PyLC accuracy, each showed a similar trend that image enhancements either don't affeect or decrease the accuracy of land cover classification. 

## Discussion 
The outcome that image enhancements tended to decrease accuracy yet changed the resulting PyLC land cover mask lends support toward limited model generalization and provides a framework for data augmentation. 

Changing the way images appear may hinder PyLC which was trained on only 95 images [(Rose, 2020)](https://github.com/scrose/pylc), because the model has not "seen" images that have been enhanced by a computer and it may be over-fitted to only the dataset it has reference to [(Karystinos & Pados, 2020)](https://ieeexplore.ieee.org/document/870038). Efforts to improve PyLC accuracy may benefit more from pivoting away from changing how input images appear and toward increasing the number and/or quality of training images [(Foody et al., 2006)](https://www.sciencedirect.com/science/article/pii/S0034425706001234). 

Since the image enhancements affect how PyLC interprets the ecosystems in an image, they could be employed in the PyLC pipeline as additional data augmentation. Data augmentation is a method to artificially increase the amount of training data available by altering the training data set and adding the altered versions to training ([Bansal et al., 2022](https://dl.acm.org/doi/10.1145/3502287); [Shorten & Khoshgoftaar, 2019](https://journalofbigdata.springeropen.com/articles/10.1186/s40537-019-0197-0)). Data augmentation has been shown to increase model generizability while decreasing the number of manually annotated land cover masks needed (ex., [(Clark et al., 2023)](https://www.mdpi.com/2073-445X/12/7/1268). 

## Acknowledgements
This research was supported by the Jamie Cassels Undergraduate Research Awards (JCURA) and supervised by Dr. Eric Higgs from the Mountain Legacy Project. I am grateful for the opportunity to carry out research on WSANEC and Lekwungen lands. Thank you to my supervisor and the MLP lab team for the support, as well as the incredible librarians, researchers, and students at UVic for navigating image enhancements, AI, quantifying AI accuracy, and stats conversations over the past year. 
