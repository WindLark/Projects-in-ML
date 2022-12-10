This is for the final project of Projects in ML. 

Report for 12/6/2022 is included at Report.md

The Final Report and Code are all included in the Notebook "Final_Project_Textual_Embeddings_for_Specific_Use_Cases_of_Diffusion_Models.ipynb". Due to the notebook's size there may be display issues - [in that case, the Notebook can be viewed at this link](https://colab.research.google.com/drive/1swJf1dqd_cgkXV3CqVK5mEgXLrVTY3CE?usp=sharing).

[Google Slides Presentation Link](https://docs.google.com/presentation/d/1HBJzE3sBF__nWo_nvdz2x2-s_ws9i6HC/edit?usp=sharing&ouid=108357174084582086995&rtpof=true&sd=true)


#Introduction

The recent developments in image generation models like Dall-E, MidJourney, and StableDiffusion have led to people exploring if they can be used for purposes such as generating prototype UI components and concept art for fictional characters. However, there are limitations to a model that has not been trained beyond its base configuration. As seen in Figure 1, a model might generate extraneous, incorrect, or unhelpful features that may not fit a user's intended use case (in this case, generating a mockup of a website UI). The result is that a user may waste additional time trying to find a suitable prompt to have the AI model generate their desired content, or give up on using the model entirely. We attempt to train a text embeddings for individual UI components and concepts a user might specifically want to see in Stable Diffusion to determine whether it would be possible to generate a modular series of components that could be added or removed in prototyping a website as needed.

For my final project, I attempted to implement and train a Stable Diffusion model with the following objectives:

1) To create an implementation of Stable Diffusion that would train specifically on UI element or web design images, and qualitatively assess the success of the results. As mentioned above, the goal is to help a user specifically prototype website UI or similar design elements (e.g. app icons).

2) To create a Google Colab notebook that would be accessible by an interested user looking to prototype UI images without being limited by their device or budget. A graphic designer may be working off a device like a notebook or similar that lacks the necessary GPUs to run an AI model like Stable Diffusion offline - our interest lies in making sure this is not a limitation for them so that they can quickly prototype UI designs for further personal and professional use. Furthermore, Google Colab space and GPU memory is extremely limited, both by the GPU itself and Google's own usage limits. This creates two issues - one, large ML models such as Stable Diffusion may not be able to run and two, any moderately long use of it can cause a session to be terminated prematurely due to the usage limit, costing the user any image output or trained embeddings they might have been generating.

2) To understand how the implementation of Stable Diffusion and similar computer vision models works from a practical standpoint. I hoped to gain a better understanding of txt2img models as they are a large part of ongoing computer vision research.

The report portion of this colab notebook will primarily be split into sections that comment on each step of the process. This consists of a segment at the beginning that covers the research done in selecting the model and selecting/processing the dataset, indiviudal sections to comment on specific aspects of Stable Diffusion I learned while attempting this implementation, and a closing section after the code that demonstrates and discusses the results, issues, and next steps such a project could take.

#Model and Dataset Research

A recurring problem in previous approaches to the txt2img and image generation problems primarily motivated by GANs was having to deal with complex networks or flawed generations with a large number of image artifacts [1]. Without additional implementations to guide it on a specific topic, GANs struggled to capture context specific information such as limb segmentation and even more general problems such as separating foreground and background objects from each other. As a result, GAN based models could not generalize well and a new implementation had to be built for each specific problem.

In 2021, OpenAI released the DALL-E txt2img model, which demonstrated that when trained with large amounts of data, a sufficeintly robust machine learning model could successfully translate text tokens to images. This is accomplished via the use of transformers, which used stacked encoders and decoders to impelment multi-head attention, which concatenates successive scores based on an input sequence to determine the context of a word at any given position relative to any other words [2]. DALL-E first uses a variable autoencoder to perform dimensionaltiy reduction by compressing 256x256 images to 32x32 grids of tokens representing the image [1]. Then, a transformer is passed text tokens embeddings and the image tokens embeddings to generate hidden states result in an RGB cahnnel distribution of what the expected image should look like. The result generalizes well and is applicable to a wide variety of images.

However, one of the biggest challenges regarding models like DALL-E is that their developers typically do not release the source code for their models, which can limit accessibility because the model cannot be run offline or in any desired context, only through appropriate apps and cloud services. We thus turn to StableDiffusion, which was released in 2022 by Rombach et al. The model consists of the following components:

1) A language model such as BERT that takes a string and breaks it down into tokens. This model is typically frozen as we have no interest in training on it

2) A UNet that is a stacked series of Resnet blocks that performs dimensionality reduction on the corresponding image. It also generates and repeatedly denoises the image as it processes it in relation to the text tokens it was given.

3) A VAE Decoder that decodes the image representation to generate the final image. [3]

This transformer based architecture is reliant on using text embeddings to generate and denoise an image into a specific result based on a learning a Reverse Markov Chain [3]. We can take advantage of this by training a new word token embedding that corresponds to a new set of images such as app icons or website GUI that we then use in conjunction with the model. If trained correctly, the model should able to incorporate the new token into its probability distributions and denoise accordingly.

One other notable aspect of Stable Diffusion observed by Rombach et. al is that becuause the Unet is biased towards denoising an image with similar relevant context (aka inductive bias), this lets Stable Diffusion use more 2D convolution layers (which are less comutationallty intensive) as opposed to transformers [3].

#References

1) Ramesh A, Pavlov M, Goh G, Gray S, Voss C, Radford A, Chen M, Sutskever I. Zero-shot text-to-image generation. InInternational Conference on Machine Learning 2021 Jul 1 (pp. 8821-8831). PMLR.

2) Vaswani A, Shazeer N, Parmar N, Uszkoreit J, Jones L, Gomez AN, Kaiser Ł, Polosukhin I. Attention is all you need. Advances in neural information processing systems. 2017;30.

3) Rombach R, Blattmann A, Lorenz D, Esser P, Ommer B. High-resolution image synthesis with latent diffusion models. InProceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition 2022 (pp. 10684-10695).

[Report is continued on the Colab Notebook.](https://colab.research.google.com/drive/1swJf1dqd_cgkXV3CqVK5mEgXLrVTY3CE?usp=sharing).
