---
layout: page
title: HRSNet
description: A fusion model of HRNet and RESNet with pre-activated residual units.
img: assets/hrsnet/modularized_block.png
importance: 1
category: work
related_publications: false
---
### Authors
Raj Surya, Akshara Madhu Suthanan, Mihir Momaya, Rahul Autade
### Introduction
Deep convolutional neural networks have risen to prominence in numerous computer vision applications, including image classification, object detection, semantic segmentation, and human pose estimation, among others, by achieving state-of-the-art performance. However, some of the previously known algorithms have grappled with the vanishing gradient problem, which occurs when the value of the gradient becomes so small that it has an insignificant effect on the updation of parameters, hindering the ability to scale higher and implement larger layers.

HRNet distinguishes itself from other algorithms by maintaining high-resolution representations throughout the process, utilizing parallel multi-resolution subnetworks that incorporate both downsampling and upsampling in parallel, followed by repeated multi-scale fusions. In contrast, previous algorithms employed an approach of downsampling and then upsampling, resulting in the loss of local information. Although computationally more expensive, the parallel multi-resolution subnetworks utilized in HRNet help overcome this limitation and improve accuracy, potentially unlocking useful applications across various fields of life. This unique capability provided a significant stimulus behind our decision to implement this paper.

The applications of HRNet are not limited to human pose estimation but can also be extended to image classification and semantic segmentation tasks. In the original paper, the algorithm has been successfully employed for pose estimation and semantic segmentation. However, the goal behind our project is to take a step further and train a model that can perform image classification by implementing a fusion model that synergistically combines HRNet and ResNet.

By integrating the high-resolution representational power of HRNet with the residual connections and feature abstraction capabilities of ResNet, we aim to create a robust and versatile model that can effectively tackle the intricate challenges of image classification. This fusion model holds the potential to leverage the strengths of both architectures, potentially yielding superior performance and opening new avenues for exploration in the realm of computer vision.

### Related Work
The evolution of Convolutional Neural Network (CNN) architectures has paved the way for advanced image analysis and understanding, laying a solid foundation for remarkable advancements in the field. Foundational models such as AlexNet, VGGNet, ResNet, and Inception have introduced groundbreaking concepts that have propelled the field forward, including deep layering, residual connections, and inception modules. Unlike older methods that utilize pyramid structures or encoder-decoder designs for tasks like semantic segmentation (for example, PSPNet, DeepLab series, U-Net), the High-Resolution Network (HRNet) stands out as a notable exception, maintaining high-resolution information throughout the network's architecture.

This approach deviates from traditional models that gradually reduce image detail, a process that can inadvertently lead to the loss of crucial information for tasks such as semantic segmentation. In the specific domain of human pose estimation, architectures like Convolutional Pose Machines, Stacked Hourglass Networks, and SimpleBaseline have been meticulously crafted to accurately capture body poses and spatial relationships. These models often involve iterative refinement and multi-stage processing to meet the specific demands of pose estimation tasks, where efficiency and speed are paramount, especially in scenarios where quick responses are essential.

Researchers have dedicated significant efforts to exploring ways to enhance the efficiency of these models, such as employing simpler designs, eliminating unnecessary components, simplifying computational operations, and leveraging faster methods for data processing. These endeavors aim to ensure that advanced models like HRNet can be effectively deployed in real-world situations where rapid analysis is a critical requirement.

While HRNet has demonstrated remarkable prowess in estimating human poses, its capabilities extend far beyond this singular application. Its versatility encompasses a wide range of tasks, including object detection, semantic segmentation, and image classification. This flexibility and effectiveness across diverse domains underscore HRNet's value as a powerful tool in the realm of computer vision, capable of tackling a multitude of challenges by preserving detailed representations throughout its architecture. In contrast to models specifically designed for particular tasks, HRNet's broad abilities highlight its significance as a flexible and robust tool, poised to navigate the ever-evolving landscape of computer vision challenges.

### Baseline Method
The High-Resolution Network (HRNet) stands as a pioneering architecture meticulously designed to maintain high-resolution representations throughout the entirety of the network, departing from the conventional approach of traditional convolutional neural networks that reduce the resolution of feature maps early in their architecture. This distinctive characteristic renders HRNet exceptionally well-suited for tasks that demand intricate spatial information, such as image segmentation, object detection, and even the intricate challenge of human pose estimation.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/hrsnet/residual_block.png" title="bottleneck" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/hrsnet/bottleneck.png" title="bottleneck" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 1 (Left): Residual Unit.
    Figure 2 (Right): Bottleneck design.
</div>

The network's architecture starts off with a head, that reduces the input size to 1/4 of its original size and extracts basic features. It is then divided into four distinct stages. The inaugural stage comprises a single high-resolution branch, laying the foundation for subsequent stages. These subsequent stages introduce branches of lower resolutions, with each stage doubling the number of branches from the preceding one, until the final configuration is reached. At the culmination of each stage, a fusion of features from disparate resolutions takes place before transitioning to the next stage. This fusion is accomplished through a meticulous series of upsampling and downsampling operations, ensuring that each branch receives contextual information from all other branches within the network. The resolutions of each branch are 1/4, 1/8, 1/16, and 1/32. The first stage contains 4 residual units where each unit is formed by a bottleneck with the width 64, and is followed by one 3 × 3 convolution changing the width of feature maps to C. The 2nd, 3rd, 4th stages contain 1, 4, 3 modularized blocks, respectively. Each branch in multi-resolution parallel convolution of the modularized block contains 4 residual units. Each unit contains two 3 × 3 convolutions for each resolution, where each convolution is followed by batch normalization and the nonlinear activation ReLU. The widths (numbers of channels) of the convolutions of the four resolutions are C, 2C, 4C, and 8C, respectively. The output head is different for different visual recognition tasks. For image classification, all the layers of different branches are downsampled and concatenated to produce the required number of features maps depending on the number of output classes as shown in Figure 3.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/hrsnet/head.png" title="head" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 3: Head used for Image Classification.
</div>

This ingenious mechanism empowers HRNet to reinforce high-resolution features with contextual cues from lower resolutions throughout its depth, thereby enhancing its representational capacity and enabling it to capture the fine-grained details essential for tasks that demand intricate spatial information.

In contrast, ResNet, one of the most prominent neural network architectures, excels in creating deep networks through the strategic incorporation of residual connections, optimizing feature abstraction at reduced spatial resolutions. This characteristic renders ResNet highly versatile, enabling its successful application across a wide range of tasks, including image classification . HRNet, on the other hand, maintains high-resolution representations throughout the network's architecture, focusing its capabilities on capturing detailed spatial information, a trait that proves particularly beneficial for tasks requiring fine-grained detail, such as semantic segmentation and the intricate challenge of human pose estimation.

### Proposed Method

The primary objective of our project is to meticulously recreate the HRNet architecture as shown in Figure 4, using PyTorch, ensuring an accurate representation of its structure and capabilities. This foundational implementation would serve as the bedrock for our subsequent endeavors. Once the HRNet backbone is firmly established, we plan on augmenting it with various classifiers, rendering it adaptable for diverse computer vision tasks such as object detection, image classification, human pose estimation, and segmentation, thereby realizing the true potential of such a multi-modal model in visual recognition challenges. But due to constrained resources, we analyzed the performance on Image Classification task.

We trained and evaluated HRNet using the CIFAR-100 dataset for high-resolution image classification tasks and assessed the ImageNet dataset which was used in the original paper, was due to limitations in our computational resources. The CIFAR-100 dataset consists of 60,000 images, and consists of 100 classes with each class having 600 images. We split these images into training and testing in the ratio of 5:1. In contrast, the ImageNet dataset is massive, containing millions of images across thousands of categories. As of recent years, it has consisted of around 14 million images, with each image typically having a resolution of 256x256 pixels or higher. The dataset's size and diversity make it a crucial resource for training and benchmarking machine learning models, especially in tasks related to image classification, object detection, and semantic segmentation. However, due to its large size, working with the entire ImageNet dataset requires significant computational resources and storage capacity. 

For the re-implementation of HRNet, we adopted a preactivated residual unit as compared to a normal residual unit used in the original implementation.In contrast to a normal residual unit, which applies activation functions after convolution operations, a preactivated residual unit flips this sequence. In a normal residual unit, the input passes through convolution layers first, and then activation functions like ReLU are applied. This can sometimes lead to the vanishing gradient problem, especially in very deep networks, where gradients diminish as they propagate through many layers. On the other hand, a preactivated residual unit applies activation functions before convolutions. This "preactivation" helps in maintaining stronger gradients throughout the network, aiding in faster convergence and better overall training stability. By addressing gradient-related challenges more effectively, preactivated residual units often yield improved performance and training efficiency compared to their traditional counterparts.

Further, in the transition layers used in between different stages, the new branch is only derived using downsampling of the last available branch. But in our implementation, we used a fusion of all branches to create the new branch. This allows the network to capture all the features when creating a new branch.

The HRSNet developed by us uses 4 modularized blocks in each stage as compared to 1,4 and 3 modularized blocks in stages 1,2 and 3 respectively in HRNet.Hence it a deeper model as compared to HRNet and the modularized blocks uses skip connection from the 1st to the last block in each stage. This creates stage independence and hence each stage is responsible for learning only its residual features.

The larger network would allow for better learning of complex data.
However, the downside of this larger network is that, it contains around 43 million trainable parameters as compared to only 13 million trainable parameters of our HRNet implementation for the same value of C.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/hrsnet/modularized_block.png" title="modularised_block" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/hrsnet/modified.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
   Figure 4 (Left): Modified Modularized block with residual connections.
   Figure 5 (Right): Modified Modularized block working(block diagram).
</div>

### Results
#### Quantitative Results

In this research, we thoroughly examined two types of neural network models called HRNet and HRSNet, observing how well they performed over time until they no longer improved, which triggered a stop in their training\cite{Earlystopping}. We employed the Adam, noted for its efficiency with non-convex optimization problems, setting the learning rate at 0.005 and momentum at 0.9. This method is good for solving tough problems where the best solution is hard to find. We set a learning speed and a momentum factor to help the models adjust and learn from the data better. All the images we used were made the same size to ensure they were treated equally by our models. The images used were resized to 224x224 pixels to maintain consistency across inputs. Our experimental results, as depicted in Figure 5, reveal a significant correlation between the complexity parameter C, tested at levels 2, 18, and 30, and the reduction in training loss. Notably, as C increased, both models exhibited faster convergence rates. 

Initially, the HRSNet model displayed a markedly lower training loss at the lower C values when compared to HRNet. However, this advantage diminished, and performance between the two models converged as the parameter C was increased to its highest tested value of 30. This pattern suggests that while higher complexity can accelerate convergence, the marginal gains in performance decrease as the models reach higher levels of complexity.

In the figure 6, the graph depicting the training loss versus epochs for HRNet and HRSNet models across varying complexity levels (parameter C values of 2, 18, and 30) provides a visual representation of the models' learning efficiency. Notably, all curves follow a downward trajectory, indicating a reduction in training loss over time, which is characteristic of successful learning processes in neural networks. The HRNet model, shown in shades of blue, and the HRSNet model, in shades of orange and red, demonstrate differential behaviors depending on the complexity parameter C. For lower complexity (C = 2), the HRSNet model starts with a slightly higher loss than HRNet but quickly descends to a lower loss trajectory, surpassing HRNet in terms of efficiency as the epochs progress.

These findings suggest that making the models more complex can help them learn faster initially, but after a certain point, making them even more complex doesn’t help much more. This means there might be a sweet spot for how complex we should make the models to get the best performance without wasting resources. This information is useful for setting up the models efficiently in future experiments.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/hrsnet/accuracy.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/hrsnet/c_value.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
   Figure 6 (Left): HRNet Accuracy vs HRSNet Accuracy.
   Figure 7 (Right): Training Loss over different C values for HRNet and HRSNet..
</div>

As the complexity increases to C = 18 and C = 30, both models converge towards similar loss values, with HRSNet at C = 18 and C = 30 maintaining a consistently lower loss across the epochs compared to corresponding HRNet configurations. This indicates that the architectural enhancements in HRSNet potentially yield better optimization and faster learning rates under more complex configurations. The nearly parallel decline in loss for C = 18 and C = 30 in HRSNet, especially in the latter epochs, points to a diminishing return on increasing complexity beyond a certain threshold. This graph underpins the observations made in the study regarding the scalability and efficiency of HRNet and HRSNet, highlighting the impact of network complexity on model performance and learning dynamics. It provides a clear, empirical basis for the observed benefits of integrating synergistic architectures in neural network design, especially in tasks requiring rapid learning from complex datasets.

The overall lower loss of HRSNet is due to the fact that HRSNet is inherently more complex that HRNet, hence it is able to fit better on the dataset. We also analyzed the loss of original HRNet implementation vs our implementation and observed that our implementation leads to lower overall loss, which is because of the use of preactivated residual units.

Although the overall loss is very low, the validation loss for higher C values are higher, which implies that the model is overfitting. The accuracy values also converge at higher C values because the model starts to overfit and early stopping is implied. The fact that our dataset image is upsampled to 224 * 224 and is lower in number as compared to ImageNet is the potential reason for this overfitting. We firmly believe that by the use of more complex and larger dataset, HRSNet and our implementation fo HRNet will perform better as these models are designed for complex high resolution images. But due to resource constraints, we are unable to perform these analysis.

#### Qualitative Results

In our qualitative evaluation of the HRNet and HRSNet models, we observed significant differences in their ability to classify images across various categories. These models were particularly adept at recognizing simpler objects, such as wardrobes and sunflowers, demonstrating high accuracy. However, they struggled with more complex image categories like 'girl' and 'otter', where accuracy rates were considerably lower. This discrepancy highlights a need for improvements in our training methodologies to achieve consistent performance across all types of images. Addressing this issue involves several strategies. Firstly, enhancing the diversity of the training dataset for underperforming categories is crucial; this includes adding a greater variety of images to reflect different backgrounds, poses, and interactions. Secondly, applying sophisticated image augmentation techniques—such as rotation, scaling, and color variation—can help models generalize better under varied conditions. Thirdly, targeted retraining sessions could be employed for categories with low accuracy to refine the models’ ability to distinguish these more challenging classes. Additionally, implementing advanced feature extraction techniques may improve classification accuracy by capturing more detailed features essential for accurate identification. Finally, conducting a thorough error analysis to understand the specific reasons for misclassifications can provide insights that drive further improvements. By adopting these comprehensive approaches, we can enhance the overall reliability and accuracy of the models, ensuring they perform well across all categories, irrespective of complexity.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/hrsnet/high_acc.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/hrsnet/low_acc.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
   Figure 8 (Left): Classes with highest accuracy.
   Figure 9 (Right): Classes with lowest accuracy.
</div>

### Conclusion
Our project primarily centered on the replication and subsequent enhancement of the HRNet model from its foundational principles. We aimed to augment the performance of the existing HRNet architecture by implementing specific modifications and leveraging insights from other convolutional neural networks, notably ResNet. 

In our proposed method, we have modified the modularized block structure from the original HRNet model and utilized preactivated ResNet layers. During the process of pre-activating the ResNet layer, we added the activation function (in our case, ReLU) before the convolution layer, deviating from the conventional approach of adding it after the convolutional layer. This resulted in overall lower loss compared to original HRNet. We incorporated four modularized blocks in each stage, interconnected through residual connections. The major focus here was to develop a model capable of performing image classification tasks as a baseline method, other computer vision tasks could then be built upon the same foundational architecture. The newer HRSNet lead to lower loss values than HRNet and gave better accuracy as well.

Our experiments revealed that the hybrid HRSNet approach demonstrated superior performance when we used a lower number of feature maps (parameter C). However, as we increased the number of feature maps, the complexity also increased, resulting in similar loss values for both HRNet and HRSNet. Consequently, we conclude that the less complex hybrid model outperformed the original HRNet backbone model and this effect could be attributed to the presence of modified modularized blocks and pre-activated ResNet layers. The performance of model with smaller C value is because of the use of a smaller dataset and a higher C value would be required for larger datasets\cite{high_c_value}. This also shows the versatality of the model for being able to adapt to different sizes of datasets and perform well in them.

### Future Work
There are several avenues of exploration that we can go through on our project topic. Firstly, there is a great need to go deeper into the parameter optimization field, to achieve more refined results. Testing with different parameters, such as learning rates, batch sizes, and architecture, holds promise to further increase model efficiency and robustness. Additionally, we plan on benchmarking the performance of our model across diverse datasets to establish the generalizability and versatility of our proposed method HRSNet.  Furthermore, we plan to extend the proposed method going beyond what is currently available to include other computer vision tasks offers an exciting avenue for future research. We can address a broad range of challenges, thus expanding its utility and impact on the computre vision domain. Through these insightful processes, our project can continue to evolve, and contribute to the existing Computer Vision knowledge base.