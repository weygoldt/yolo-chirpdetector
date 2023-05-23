# An even deeper chirpdetector

Why? Relying on a simple ConvNet to detect chirps is not enough due to the great 
variability of chirps. Some are very short, some are very long, some have a very high 
frequency excursion and some have not. This makes it challenging to train a 
ConvNet that can detect all of them: Images that fit big chirps are so large 
that small chirps are hardly visible, and vice versa.

The previous ConvNet was a **classifier**, that I used as the backbone of a 
**detector**. The detector is a sliding window that moves across the spectrogram.
At each position, it extracts a small image and feeds it to the classifier.
If the classifier predicts a chirp, the detector outputs the position and the
size of the window. The contraint is, that the window size must be the same for 
all images, so the classifier can be trained on a fixed input size.

To fix this, instead of reinventing the wheel, I will no try my luck with an 
architecture that is already designed for **detection**: YOLO. 

YOLO means "You Only Look Once". It is a single ConvNet that predicts bounding
boxes and class probabilities for these boxes. It is very fast, because it
only needs to run the network once per image, instead of once per sliding window.
It even runs in real time on a GPU. Lets see if this works for our chirps.