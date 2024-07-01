# Snippet_Generator
This repository is forked from Brigham Young University's Record Linking Lab. This fork will likely be out of sync with BYU's as the code that is here is authored by me, with the exception of some small bits of code in the test cases. As BYU decides to update or change these tools, I'd like my work to be preserved for people to see as well. :)

This repository was made to be a useful tool to anyone who is using object detection or semantic segmentation models to find and isolate objects in an image, namely boxes (fields) on tabular documents. Given a collection of images and a dataset that defines box points on the images, this code will produce new images that are the "crops" or "snippets" rendered by cropping the image to the bounding box in the dataset. See below for more details. 

## Useful info (Jargon)
This repository expects data to be formatted in the following ways.
    - text data is to be stored in a file that pandas can ingest for a dataframe, .csv or .tsv are common file types. The following columns and dtypes that are to be found in your dataframe: 
        reel_filename: str
        image_filename: str
        snip_name: str
        x1: int
        y1: int
        .
        .
        .
        x4: int
        y4: int
    
    - image data: Any image that is compatible with PIL will work, the generator currently outputs .png files only. Images should be stored in .tar files (This is a common archive format) and should have the following formats: the name of the .tar file will correspond to the reel_filename in the text data. The image filenames in the .tar should correspond to the image_filename column in the text data. 

The snippet generator accepts one pandas df for the text data, and a list to all the .tar files for the images. 

Within the snippet generator, the df is converted into a dictionary. When snippets are solicited from the snippet generator, the tarfiles are streamed and if a tarfile, imagefile and a field with box points is found in the dictionary, the generator will get all the snippets for that image. 

## Getting Started
### Prerequisites
* PIL
* Numpy
* OpenCV

### Usage - Currently pseudocode. Better examples to be built out soon. 
'''
from SnippetGenerator import SnippetGenerator
import pandas as pd

df = pd.read_csv('path/to/csv')

sg = SnippetGenerator(df)

tar_file_paths_containing_images = ['path/to/tar1', 'path/to/tar2', ...]

\# Use case 1: Get batches of crops from the snippet generator
for reel_name, image_names, snippet_names, snippets in sg.get_batches_of_snippets(tar_file_paths_containing_images, batch_size):
    do something

\# Use case 2: save all the snippets out to a .tar or .tar.gz file. Within the tarfile, images will be stored in the following directory structure: reel_name/image_name/snippet_id.png)
tar_file_paths_containing_images = ['path/to/tar1', 'path/to/tar2', ...]
output_directory = 'path to where snippets are to be stored'
outfile = 'name of tarfile that snippets will be archived to'

sg.save_snippets_as_tar(tar_file_paths_containing_images, output_directory, outfile)

\# Use case 3: Save the snippets out to a directory ( a directory structure will naturally be made in the code following this structure: output_dir/reel_name/image_name/snippet_id.png)
tar_file_paths_containing_images = ['path/to/tar1', 'path/to/tar2', ...]

output_directory = 'path to where snippets are to be stored'
save_snippets_to_directory(input_tarfiles, output_directory)



