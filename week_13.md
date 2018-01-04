## Face Recognition
 - Face Verification
   - Verify if the input image is the face of the claimed person.
 - Face Recognition
  - Determine if an image is the face out of multiple options.
## One shot learning.
- A difficult part of face recognition is it is common to have just a single training example of each person.
- To get around this you learn a similarity function that outputs the degree of difference between two images.
## Siamese Network
- You run two neural networks side-by-side and each one outputs a encoding of the image.
- You then measure the difference between the encodings.
- If the difference is small it's the same person. If the difference is large they are different people.
## Triplet Loss
- You look at three images a time.
  - An image of a known person, called the anchor image.
  - An image of a different person known as the negative image.
  - An image of the same person known as the positive image.
- You want the distance between the encoding of the anchor vs the positive image to be small and the distance between the encoding of the anchor image and the negative to be large.
- You typically don't just want to difference to be small but to be at least as large as a parameter, $\alpha$. Also known as the margin.
- The triplet loss function using an (A)nchor, (P)ositive, and (N)egative image is $L(A,P,N)=max(||f(A)-f(P)||^2-||f(A)-f(N)||^2+\alpha)$.
- The cost function is $J=\sum\limits_{i=1}^mL(A^{(i)},P^{(i)},N^{(i)})$
- To train this algorithm you need multiple pictures of the same people.
- To accurately train the algorithm you need to train the algorithm using triplets that are "hard" to train.
- Rather than trying to train a a model from scratch there are good parameters out there from models that have been trained on very large datasets.
## Face Recognition with Binary Encoding
- Another way to perform face recognition is using binary encodings.
- You run parallel networks against two images then process difference between the two outs and feed the vector of the difference to a sigmoid unit.

## Neural Style Transfer
- Neural style transfer is an algorithm that will take a (C)ontent image, and reimagine it as a (G)enerated image in the style of a (S)tyle image.
- You typically start with an existing deep CNN because it has been trained to learn a large number of low-level image features. This is known as transfer learning.
- The earlier (shallower) layers of a ConvNet tend to detect lower-level features such as edges and simple textures, and the later (deeper) layers tend to detect higher-level features such as more complex textures as well as object classes.
- The cost function for neural style transfer is the addition of two separate functions. A cost function of the content and a cost function of the style.
$J(G)=\alpha J_{content}(C,G)+\beta J_{style}(S,G)$
$J_{content}(C,G) =  \frac{1}{4 \times n_H \times n_W \times n_C}\sum _ { \text{all entries}} (a^{(C)} - a^{(G)})^2$
$J_{style}^{[l]}(S,G) = \frac{1}{4 \times {n_C}^2 \times (n_H \times n_W)^2} \sum _ {i=1}^{n_C}\sum_{j=1}^{n_C}(G^{(S)}_{ij} - G^{(G)}_{ij})^2$
where $G^{(S)}$ and $G^{(G)}$ are respectively the Gram matrices of the "style" image and the "generated" image, computed using the hidden layer activations for a particular hidden layer in the network.
- A gram matrix is calculated by multiplying a matrix by its own transposition.
- You can combine the style costs for different layers as follows:
$J_{style}(S,G) = \sum_{l} \lambda^{[l]} J^{[l]}_{style}(S,G)$
where the values for $\lambda^{[l]}$ are given in `STYLE_LAYERS`. This allows the different style feature at different layers to weigh heavier on the final cost.
- The content portion of the cost is calculated using the activations of one of the hidden layers of the network. In practice, you'll get the most visually pleasing results if you choose a layer in the middle of the network--neither too shallow nor too deep.
- The steps of neural style transfer are:
  - Initialize the generated image as static noise.
  - Run forward prop for your content, and generated image and calculate the style cost using the chosen hidden layers for both images.
  - Run forward prop for your style, and generated image and calculate the gram matrices for the activations of the chosen hidden layer for each of the networks. Then calculate the style cost using the gram matrices.
  - Run the style cost process on other layers and blend the costs from multiple layers if desired.
  - Combine the two costs into the total cost.

## Dimensions
- CNN's can be used on 1D and 3D data as well as 2D data.

IDEA: Use 3D CNN on movie data to do neural style transfer from hand-drawn episodes of anime to CD anime.
