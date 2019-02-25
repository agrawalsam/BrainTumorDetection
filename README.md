# BrainTumorDetection
This project implements K-Means clustering and Morphological Processing for finding brain tumor in the given image of brain.
Due to difficulty of .m file, code has been uploaded in the following lines with example - 

a=imread('1.jpg'); %1.jpg is given image.Images can be found in the internet. 
figure
imshow(a) %original image display
%Pre-processing laplacian filter locally
sharpened_img = locallapfilt(a,0.4,0.5); %sigma=0.4(all pixel value gets operated below
0.4) and alpha=0.5(sharpening parameter)
figure
imshow(sharpened_img)
%Clustering and Segmentation
segmented = cluster_segment( sharpened_img, 4 );
figure;
subplot(1,4,1), imshow(segmented{1}), title('Cluster 1');
subplot(1,4,2), imshow(segmented{2}), title('Cluster 2');
subplot(1,4,3), imshow(segmented{3}), title('Cluster 3');
subplot(1,4,4), imshow(segmented{4}), title('Cluster 4');
%Erosion Part
se = strel('disk',13);
figure
subplot(1,4,1), imshow(imerode(segmented{1},se)), title('Cluster 1');
subplot(1,4,2), imshow(imerode(segmented{2},se)), title('Cluster 2');
subplot(1,4,3), imshow(imerode(segmented{3},se)), title('Cluster 3');
subplot(1,4,4), imshow(imerode(segmented{4},se)), title('Cluster 4');
function segmented_img = cluster_segment(sharpened_img, nclusters )
 conversionform = makecform('srgb2lab'); %color transformation from rgb to lab color
space.
 sharpened_img = im2double(sharpened_img);% pixel value divide by 255
 lab_img = applycform(sharpened_img,conversionform); %img is changed to lab
 ab_img = lab_img(:,:,2:3); %extracts a and b componen
 [nrows,ncols,~] = size(ab_img);
 ab_img = reshape(ab_img,nrows*ncols,2); %makes it into 1D image
 cluster_idx = kmeans(ab_img,nclusters,'distance','sqEuclidean','Replicates',3);
%replicate === iterations
 cluster_img = reshape(cluster_idx,nrows,ncols); %changes the size of 1D image
 figure, imagesc(cluster_img), title('Clustering results'); %display image with scaled colors
 segmented_img = cell(1,nclusters); %1D array 1 X 4
 disp(size(segmented_img))
 for k = 1:nclusters
 segmented_img{k} = bsxfun( @times, sharpened_img, cluster_img == k );
 end

end

INPUT IMAGE
https://user-images.githubusercontent.com/31516561/53322263-dc47ff80-3900-11e9-9b50-b3a970826092.png

SHARPENED IMAGE
https://user-images.githubusercontent.com/31516561/53322270-e23de080-3900-11e9-9613-8631a2a5a6c7.png

CLUSTERED IMAGE
https://user-images.githubusercontent.com/31516561/53322273-e79b2b00-3900-11e9-853f-b6e40519ff31.png

SEGEMENTED IMAGE
https://user-images.githubusercontent.com/31516561/53322282-ed910c00-3900-11e9-83d0-ac2c0c26261f.png

ERODED IMAGE(MORPHOLOGICAL PROCESSING)
https://user-images.githubusercontent.com/31516561/53322282-ed910c00-3900-11e9-83d0-ac2c0c26261f.png

