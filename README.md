# computer_vision_practice
pricticing kernels, edge detection, corner detection, blob detection and SIFT
Presentation Outline: Image Feature Matching and Object Localization

1. Introduction: The Challenge
Problem Statement: The goal was to detect and precisely locate a specific cell (from a cropped image) within a larger, original blood cell image.

2. Key Techniques
A. SIFT (Scale-Invariant Feature Transform) for Robust Feature Detection
What it is: SIFT is a powerful algorithm for detecting and describing local features in images. These features are invariant to image scale, rotation, and partially invariant to illumination changes, making them robust.

Steps:
Grayscale Conversion: Both the cropped_img and the original_img were converted to grayscale. SIFT primarily operates on grayscale images to simplify calculations and focus on intensity gradients.

Keypoint Detection & Descriptor Computation: SIFT identified unique and distinctive points (keypoints) in both images. For each keypoint, a 128-dimensional descriptor was computed, encapsulating its local appearance.

Result: cropped_img yielded 10 keypoints, and original_img yielded 51 keypoints.

B. Brute-Force Matching with Lowe's Ratio Test
Purpose: To find correspondences between the keypoints of the two images.

Method:
Brute-Force Matcher (BFMatcher): A brute-force matcher was used to compare all descriptors from the cropped_img with all descriptors from the original_img.

knnMatch (k-Nearest Neighbors Matching): For each descriptor in the cropped_img, the two closest descriptors in the original_img were found.

Lowe's Ratio Test: This critical filtering step eliminated ambiguous matches. By comparing the distance of the best match to the second-best match (ratio of 0.80), only truly distinctive and robust matches were retained.

Result: Out of 10 potential matches, 6 robust 'good matches' were identified, ensuring high-quality correspondences.

Visualization: We visualized these good matches with lines connecting corresponding keypoints between the cropped and original images, demonstrating successful feature association.

C. Homography Estimation with RANSAC
What it is: A homography is a 3x3 transformation matrix that maps points from one plane to another. In our case, it maps the coordinates from the cropped_img to its corresponding location in the original_img.

Steps:
Keypoint Coordinate Extraction: The (x,y) coordinates of the 6 good matches were extracted from both the cropped and original images.

cv2.findHomography: This function computed the homography matrix (M).
RANSAC (Random Sample Consensus): Integrated into findHomography, RANSAC is an algorithm used to robustly estimate model parameters (like the homography matrix) from a set of observed data containing outliers. It ensures that only consistent matches contribute to the final transformation, making the estimation reliable even with some incorrect matches.

Result: A 3x3 homography matrix M was successfully computed, along with a mask indicating the inlier matches.

D. Object Localization and Visualization
Purpose: To clearly show the detected region of the cropped_img within the original_img.

Steps:

Define Cropped Image Corners: The four corner coordinates of the cropped_img were defined.
Perspective Transform: The computed homography matrix M was used with cv2.perspectiveTransform to transform these four corner points to their corresponding positions in the original_img's coordinate system.

Draw Bounding Box: A quadrilateral (bounding box) was drawn on the original_img using these transformed corner points.

Result: A green bounding box was accurately drawn around the detected cell in the original image, visually confirming the successful localization.

Additional Note: To test robustness, the original image was rotated 90°, and SIFT was able to successfully detect and localize the cell even in the rotated image, demonstrating the algorithm’s rotation invariance
