2018.10.9 Xin Kong
Analysis of TNet:
1. Source_data direcoty: restore the trained model, run evaluate.py to get data: input_pts(input), TNet(transform1), transformed_pts(ExpandDims), pts dimension: BatchxPoints_numx3(4x1024x3) TNet dimension: Batchx3x3(4x3x3)
2. Convert input and ExpandDims files to pcd. 1-1024 rows -> pcd_0;1025-2048 rows -> pcd_1;...; 1-3 rows in TNet file -> 3x3 (pcd_0);4-6 rows in TNet file -> 3x3 (pcd_1); ...  
3. Input*TNet = ExpandDims is verified. Data is OK.
4. Visualized by pcl_viewer: The points are rotated and scaled to a cube, which is around 1m³. 
5. The author wanted the transformation matrix to be close to orthogonal matrix in the pointnet paper. But the TNet(3x3) I get is not close to orthogonal matrix and still get a large loss in this iterm: L = ||1-AA'||.
6. TNet is invertible. Does TNet have other special properties in linear algebra?
7. I train the model without the TNet1 and get a little worse result than the original.

with TNet1(original):(Namespace(batch_size=32, decay_rate=0.7, decay_step=200000, gpu=0, learning_rate=0.001, log_dir='log', max_epoch=250, model='pointnet_cls', momentum=0.9, num_point=1024, optimizer='adam'))
eval mean loss: 0.498235
eval accuracy: 0.891234
eval avg class acc: 0.861447 

without TNet1:
(Namespace(batch_size=32, decay_rate=0.7, decay_step=200000, gpu=0, learning_rate=0.001, log_dir='log_kx_without_T1', max_epoch=250, model='pointnet_cls_without_T1',momentum=0.9,num_point=1024, optimizer='adam'))
eval mean loss: 0.777766
eval accuracy: 0.868912
eval avg class acc: 0.843538
