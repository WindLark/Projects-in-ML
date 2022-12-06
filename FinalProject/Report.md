## Presentation Report

https://paperswithcode.com/dataset/ui5k

My initial research consisted of reviewing and briefly experimenting with the unlabeled mobile screenshot dataset shown above. I decided to reject this and similar datasets for the purposes of the project as its scope was too broad - there are many different types of user interface screenshots and my goal was to train a model in a manner such that someone such as a web designer could explicitly prototype a specific icon or detail that they wanted, such as a search bar or an app icon featuring a computer.

https://www.kaggle.com/datasets/testdotai/common-mobile-web-app-icons

I decided to use this kaggle dataset of common mobile webapp icons as it had been mostly preprocessed to 200x200 images and separated by intended label such as "bookmark", "calculator", or "computer". StableDiffusion works by taking text input and processing it into tokens via a frozen text encoder such as BERT or CLIP that is then turned into embeddings. These embeddings processed by a Unet to generate noise residuals that a VAE decoder then turns into an image. My intention was to train a new text token and embedding that corresponded to a specific app icon, and then observe the results of using it in conjunction with a prompt such as "An <app icon> on a web site." (<>'s are recommended by Huggingface to distinguish from existing tokens) If a design wished to have multiple different forms of new app icons or UI symbols, they could use the token corresponding to a specific embedding to add it to their AI-generated mockup. I found that choosing wordy text-based tokens such as "<computer-app-icon>" generally confused the application as it would attempt to process the existing words (e.g. "icon" producing a music icon on the image) and decided to use a token named "<XYZ>" instead for the initial computer token to minimze possible overlap. I selected 200 images from the "computer" set of images, and used them as the training set. 

Currently I have an embedding that has been trained up to 3000 epochs with the results at 1000-epoch intervals demonstrated in the slides, as well as StableDiffusion setup that can save and load embeddings for training and image generation. My general observation so far is that the model overfit to generating app icons only and is not particularly compatible. The hyperparameters might need to be changed a bit or more training time might need to be given. With the time available, I am considering moving onto a different dataset (e.g. Returning to a previously considered screenshots dataset) and trying to observe if the same overfitting problem occurs there, as it it did not occur when I tried it with the sample images provided by stablediffusion (the "cat toy" example is provided in the slides for comparison). StableDiffusion articles tend to claim 5-20 images are sufficient for training - I may also experiment with giving the model less training images and observing the results (though I believe this will ultimately result in more overfitting).

The end goal is to have trained 2-3 word embeddings, each from a different dataset or subset of one dataset, and determine how capable StableDiffusion is at using these word embeddings in conjunction with other word embeddings from a txt2img prompt. As our current embedding is not particularly successful in this regard I hope to train on different datasets with different parameters to determine an optimal approach.

## Additional Resources:
  
https://huggingface.co/blog/stable_diffusion
https://huggingface.co/blog/annotated-diffusion
The blog posts that were primarily references as part of my research (as well as their included notebooks).

https://www.kaggle.com/datasets/colinmorris/favicons 
https://www.kaggle.com/datasets/danhendrycks/icons50

Above are two other app icon datasets I may use to try and train different or comparable icons to observe what happens when I try to generate images featuring multiple embeddings.

https://www.kaggle.com/datasets/splcher/animefacedataset 
https://www.kaggle.com/datasets/soumikrakshit/anime-faces

We hold the anime (cartoon character) face data sets in reserve in the event we want to use a drastically different dataset - many mobile games made for Android and iOS phones tend to feature a cartoon character as an app icon, so we could explore if training on a series of cropped anime faces is sufficient to to generate an embedding representing an app icon that can be used in conjunction with other prompts.
