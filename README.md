# EVA6_Capstone
Capstone Project - Object Detection and Panoptic Segmentation on Custom Construction Dataset

Unlike traditional computer vision techniques, DETR approaches object detection as a direct set prediction problem. It consists of a set-based global loss, which forces unique predictions via bipartite matching, and a Transformer encoder-decoder architecture. Given a fixed small set of learned object queries, DETR reasons about the relations of the objects and the global image context to directly output the final set of predictions in parallel. Due to this parallel nature, DETR is very fast and efficient.
## Part-1 Questions:

1. We take the encoded image (dxH/32xW/32) and send it to 2. Multi-Head Attention (FROM WHERE DO WE TAKE THIS ENCODED IMAGE?)
3. We also send dxN Box embeddings to the Multi-Head Attention
4. We do something here to generate NxMxH/32xW/32 maps. (WHAT DO WE DO HERE?)
5. Then we concatenate these maps with Res5 Block (WHERE IS THIS COMING FROM?)
6. Then we perform the above steps (EXPLAIN THESE STEPS)
And then we are finally left with the panoptic segmentation

![Architecture Diagram](./arch.png)

1. The Input image size is 3xWxH where H and W is the Height and width of the encoded image passed, 3 is for the RGB colour dimensions of the image. Before the image goes through the encoder-decoder model it is passed through the backbone of the ResNet50 model which generates a lower resolution activation map of size H/32, W/32, 2048.This image is then encoded using positional and patch embeddings and then fed to Multi-Head Attention in encoder part. The result of the encoder generated is passed to the decoder. The output of encoder is encoded image dxH/32xW/32 and this image is sent to Multi-Head Attention in Panoptic Segmentation block.

2. The two inputs to the Decoder are the encoded image which is passed from the encoder and the object queries or the BBox embeddings. We get Object Embeddings(Bounding Boxes) for a image from Decoder. Each Attention Map will be of dimension NxMxH/32xW/32 where:
- N is the number of these Box embeddings generated
- M is D/N where D is the channels passed from the encoded image
- H is nothing but the Height of the original image
- W is nothing but the width of the original image

3. The activation map generated from the Resnet50 block is stored. The activation heat maps are extracted from the layer 2,3,4 and 5 of the Resnet block and the encoded images or the attention heat maps gets sampled from NxMxH/32xW/32 to NxH/4xW/4 by passing it continuously through these Resnet layer. The activation maps helps to generate the segmentation scale for particular class based on the bounding box.


## Approach:
- Collect the dataset images
- Pass the images through the model to get predictions for stuff categories.
- Combine the predicted annotations and prepare the dataset.
- Train model for BBOX detection on the prepared things dataset along with other stuff categories.
- Train DETR model for Panoptic segmentation on the above dataset.