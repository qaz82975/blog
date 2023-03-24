# What is it?

ControlNet adds additional levels of control to Stable Diffusion image composition. Think Image2Image juiced up on steroids. It gives you much greater and finer control when creating images with Txt2Img and Img2Img.

This is for Stable Diffusion version 1.5 and models trained off a Stable Diffusion 1.5 base. Currently, as of 2023-02-23, it does not work with Stable Diffusion 2.x models.

The Auto1111 extension is by Mikubill, and can be found here: https://github.com/Mikubill/sd-webui-controlnet

The original ControlNet repo is by lllyasviel, and can be found here: https://github.com/lllyasviel/ControlNet



Where can I get it the extension?

If you are using Automatic1111 UI, you can install it directly from the Extensions tab. It may be buried under all the other extensions, but you can find it by searching for "sd-webui-controlnet"

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
Installing the extension in Automatic1111

You will also need to download several special ControlNet models in order to actually be able to use it.

At time of writing, as of 2023-02-23, there are 4 different model variants

Smaller, pruned SafeTensor versions, which is what nearly every end-user will want, can be found on Huggingface (official link from Mikubill, the extension creator): https://huggingface.co/webui/ControlNet-modules-safetensors/tree/main

Alternate Civitai link (unofficial link): https://civitai.com/models/9251/controlnet-pre-trained-models

Note that the official Huggingface link has additional models with a "t2iadapter_" prefix; those are experimental models and are not part of the base, vanilla ControlNet models. See the "Experimental Text2Image" section below.

Alternate pruned difference SafeTensor versions. These come from the same original source as the regular pruned models, they just differ in how the relevant information is extracted. Currently, as of 2023-02-23, there is no real difference between the regular pruned models and the difference models aside from some minor aesthetic differences. Just listing them here for completeness' sake in the event that something changes in the future.

Official Huggingface link: https://huggingface.co/kohya-ss/ControlNet-diff-modules/tree/main

Unofficial Civitai link: https://civitai.com/models/9868/controlnet-pre-trained-difference-models

Experimental Text2Image Adapters with a "t2iadapter_" prefix are smaller versions of the main, regular models. These are currently, as of 2023-02-23, experimental, but they function the same way as a regular model, but much smaller file size

The full, original models (if for whatever reason you need them) can be found on HuggingFace:https://huggingface.co/lllyasviel/ControlNet

Go ahead and download all the pruned SafeTensor models from Huggingface. We'll go over what each one is for later on. Huggingface also includes a "cldm_v15.yaml" configuration file as well. The ControlNet extension should already include that file, but it doesn't hurt to download it again just in case.

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
Download the models and .yaml config file from Huggingface

As of 2023-02-22, there are 8 different models and 3 optional experimental t2iadapter models:

control_canny-fp16.safetensors

control_depth-fp16.safetensors

control_hed-fp16.safetensors

control_mlsd-fp16.safetensors

control_normal-fp16.safetensors

control_openpose-fp16.safetensors

control_scribble-fp16.safetensors

control_seg-fp16.safetensors

t2iadapter_keypose-fp16.safetensors(optional, experimental)

t2iadapter_seg-fp16.safetensors(optional, experimental)

t2iadapter_sketch-fp16.safetensors(optional, experimental)

These models need to go in your "extensions\sd-webui-controlnet\models" folder where ever you have Automatic1111 installed. Once you have the extension installed and placed the models in the folder, restart Automatic1111.

After you restart Automatic1111 and go back to the Txt2Img tab, you'll see a new "ControlNet" section at the bottom that you can expand.

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5


Sweet googly-moogly, that's a lot of widgets and gewgaws!

Yes it is. I'll go through each of these options to (hopefully) help describe their intent. More detailed, additional information can be found on "Collected notes and observations on ControlNet Automatic 1111 extension", and will be updated as more things get documented.

To meet ISO standards for Stable Diffusion documentation, I'll use a cat-girl image for my examples.

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
Cat-girl example image for ISO standard Stable Diffusion documentation

The first portion is where you upload your image for preprocessing into a special "detectmap" image for the selected ControlNet model. If you are an advanced user, you can directly upload your own custom made detectmap image without having to preprocess an image first.

This is the image that will be used to guide Stable Diffusion to make it do more what you want.

A "Detectmap" is just a special image that a model uses to better guess the layout and composition in order to guide your prompt

You can either click and drag an image on the form to upload it or, for larger images, click on the little "Image" button in the top-left to browse to a file on your computer to upload

Once you have an image loaded, you'll see standard buttons like you'll see in Img2Img to scribble on the uploaded picture.

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
Upload an image to ControlNet

Below are some options that allow you to capture a picture from a web camera, hardware and security/privacy policies permitting

Below that are some check boxes below are for various options:

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
ControlNet image check boxes

Enable: by default ControlNet extension is disabled. Check this box to enable it

Invert Input Color: This is used for user imported detectmap images. The preprocessors and models that use black and white detectmap images expect white lines on a black image. However, if you have a detectmap image that is black lines on a white image (a common case is a scribble drawing you made and imported), then this will reverse the colours to something that the models expect. This does not need to be checked if you are using a preprocessor to generate a detectmap from an imported image.

RGB to BGR: This is used for user imported normal map type detectmap images that may store the image colour information in a different order that what the extension is expecting. This does not need to be checked if you are using a preprocessor to generate a normal map detectmap from an imported image.

Low VRAM: Helps systems with less than 6 GiB[citation needed] of VRAM at the expense of slowing down processing

Guess: An experimental (as of 2023-02-22) option where you use no positive and no negative prompt, and ControlNet will try to recognise the object in the imported image with the help of the current preprocessor.

Useful for getting closely matched variations of the input image

The weight and guidance sliders determine how much influence ControlNet will have on the composition.

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
ControlNet weight and guidance strength

Weight slider: This is how much emphasis to give the ControlNet image to the overall prompt. It is roughly analagous to using prompt parenthesis in Automatic1111 to emphasise something. For example, a weight of "1.15" is like "(prompt:1.15)"

Guidance strength slider: This is a percentage of the total steps that control net will be applied to . It is roughly analogous to prompt editing in Automatic1111. For example, a guidance of "0.70" is tike "[prompt::0.70]" where it is only applied the first 70% of the steps and then left off the final 30% of the processing

Resize Mode controls how the detectmap is resized when the uploaded image is not the same dimensions as the width and height of the Txt2Img settings. This does not apply to "Canvas Width" and "Canvas Height" sliders in ControlNet; those are only used for user generated scribbles.

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
ControlNet resize modes

Envelope (Outer Fit): Fit Txt2Image width and height inside the ControlNet image. The image imported into ControlNet will be scaled up or down until the width and height of the Txt2Img settings can fit inside the ControlNet image. The aspect ratio of the ControlNet image will be preserved

Scale to Fit (Inner Fit): Fit ControlNet image inside the Txt2Img width and height. The image imported into ControlNet will be scaled up or down until it can fit inside the width and height of the Txt2Img settings. The aspect ratio of the ControlNet image will be preserved

Just Resize: The ControlNet image will be squished and stretched to match the width and height of the Txt2Img settings

The "Canvas" section is only used when you wish to create your own scribbles directly from within ControlNet as opposed to importing an image.

The "Canvas Width" and "Canvas Height" are only for the blank canvas created by "Create blank canvas". They have no effect on any imported images

Preview annotator result allows you to get a quick preview of how the selected preprocessor will turn your uploaded image or scribble into a detectmap for ControlNet

Very useful for experimenting with different preprocessors

Hide annotator result removes the preview image.

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
ControlNet preprocessor preview

Preprocessor: The bread and butter of ControlNet. This is what converts the uploaded image into a detectmap that ControlNet can use to guide Stable Diffusion.

A preprocessor is not necessary if you upload your own detectmap image like a scribble or depth map or a normal map. It is only needed to convert a "regular" image to a suitable format for ControlNet

As of 2023-02-22, there are 11 different preprocessors:

Canny: Creates simple, sharp pixel outlines around areas of high contract. Very detailed, but can pick up unwanted noise

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
Canny edge detection preprocessor example



Depth: Creates a basic depth map estimation based off the image. Very commonly used as it provides good control over the composition and spatial position

If you are not familiar with depth maps, whiter areas are closer to the viewer and blacker areas are further away (think like "receding into the shadows")

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
Depth preprocessor example



Depth_lres: Creates a depth map like "Depth", but has more control over the various settings. These settings can be used to create a more detailed and accurate depth map

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
Depth_lres preprocessor example



Hed: Creates smooth outlines around objects. Very commonly used as it provides good detail like "canny", but with less noisy, more aesthetically pleasing results. Very useful for stylising and recolouring images.

Name stands for "Holistically-Nested Edge Detection"

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
Hed preprocessor example



MLSD: Creates straight lines. Very useful for architecture and other man-made things with strong, straight outlines. Not so much with organic, curvy things

Name stands for "Mobile Line Segment Detection"

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
MLSD preprocessor example



Normal Map: Creates a basic normal mapping estimation based off the image. Preserves a lot of detail, but can have unintended results as the normal map is just a best guess based off an image instead of being properly created in a 3D modeling program.

If you are not familiar with normal maps, the three colours in the image, red, green blue, are used by 3D programs to determine how "smooth" or "bumpy" an object is. Each colour corresponds with a direction like left/right, up/down, towards/away

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
Normal Map preprocessor example



OpenPose: Creates a basic OpenPose-style skeleton for a figure. Very commonly used as multiple OpenPose skeletons can be composed together into a single image and used to better guide Stable Diffusion to create multiple coherent subjects

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
OpenPose preprocessor example



Pidinet: Creates smooth outlines, somewhere between Scribble and Hed

Name stands for "Pixel Difference Network"

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
Pidinet preprocessor example



Scribble: Used with the "Create Canvas" options to draw a basic scribble into ControlNet

Not really used as user defined scribbles are usually uploaded directly without the need to preprocess an image into a scribble



Fake Scribble: Traces over the image to create a basic scribble outline image

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
Fake scribble preprocessor example



Segmentation: Divides the image into related areas or segments that are somethat related to one another

It is roughly analogous to using an image mask in Img2Img

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
Segmentation preprocessor example



Model: applies the detectmap image to the text prompt when you generate a new set of images

r/StableDiffusion - A1111 ControlNet extension - explained like you're 5
ControlNet models

The options available depend on which models you have downloaded from the above links and placed in your "extensions\sd-webui-controlnet\models" folder where ever you have Automatic1111 installed

Use the "🔄" circle arrow button to refresh the model list after you've added or removed models from the folder.

Each model is named after the preprocess type it was designed for, but there is nothing stopping you from adding a little anarchy and mixing and matching preprocessed images with different models

e.g. "Depth" and "Depth_lres" preprocessors are meant to be used with the "control_depth-fp16" model

Some preprocessors also have a similarly named t2iadapter model as well.e.g. "OpenPose" preprocessor can be used with either "control_openpose-fp16.safetensors" model or the "t2iadapter_keypose-fp16.safetensors" adapter model as well

As of 2023-02-26, Pidinet preprocessor does not have an "official" model that goes with it. The "Scribble" model works particularly well as the extension's implementation of Pidinet creates smooth, solid lines that are particularly suited for scribble.