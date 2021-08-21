# EVA6_Capstone
Capstone Project - Panoptic Segmentation

## Part-1 Questions:

We take the encoded image (dxH/32xW/32) and send it to Multi-Head Attention (FROM WHERE DO WE TAKE THIS ENCODED IMAGE?)
We also send dxN Box embeddings to the Multi-Head Attention
We do something here to generate NxMxH/32xW/32 maps. (WHAT DO WE DO HERE?)
Then we concatenate these maps with Res5 Block (WHERE IS THIS COMING FROM?)
Then we perform the above steps (EXPLAIN THESE STEPS)
And then we are finally left with the panoptic segmentation
