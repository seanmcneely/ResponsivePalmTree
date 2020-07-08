# ResponsivePalmTree
This JavaScript project allows you to add a fully responsive and interactive palm tree animation to an html webpage! 

The tree is built using p5js, a javascript libary that allows you to create animations by drawing simple shapes. Creating the tree turned out to be far more
challenging than I initially predicted, and was super fun. 

The palm tree exists as an object in JavaScript. The palm tree object has two important members, an array of trunk pieces, and an array of leaf pieces.

## Making the Tree Sway
To make the tree sway, the root of the tree stays in place, while the horizontal position of the top of the tree varies according to a sine function. In order for the tree to stay the same height while the top sways, it is also necessary to calculate the vertical position of the top of the tree using the pythagorean theorem. 

All curved objects in the animation are created using bezier curves. The bezier curve function in p5 takes as input two reference coordinates, two endpoint coordinates, and a step variable between 0 and 1. The function outputs a coordinate located on a smooth curve between the endpoint coordinates. The step variable determines how far along this curve the output coordinate is located. A step value of zero would output a coordinate directly ontop of one of the endpoint coordinates, and a step value of one would output a coordinate ontop of the other endpoint. Step values between zero and one specify coordinates between the endpoints. 

To create the trunk of the tree, we first have to find points on a bezier curve between the bottom of the tree and the top of the tree that define where the pieces of the tree trunk will go. The trunk pieces each have some buffer space below them and some vertical height. To find the correct locations for the top and bottom of each trunk piece, we must find points on the bezier curve that maintain the height and buffer of each trunk piece as the tree sways. This was the biggest challenge of this project! Linear changes in the curve step variable do not correspond to linear distances along the curve! Thus, if we wanted to find the halfway point of the curve, it would not work to use a step of 0.5. Similarly, it is not possible to find the top of a trunk piece from the coordinates of the bottom by adding some constant fraction to the step variable. Doing so would result in non-linear and inconsistent trunk pieces. 

To overcome this and create the tree trunk, I had to make approximations. From the coordinates of the bottom of a tree trunk piece, I calculate the approximate position of the top of the trunk piece by treating the bezier curve as a straight line with some slope, calculating this slope using rise/run of points on the curve, and using the pythagorean theorem to find a point exactly the height of the trunk piece along this straight line. Since I treat the curve as a straight line, my resultant point is likely not on the bezier curve, but is nearby. To find the closest point that is actually on the curve, I generate several hundred points along the bezier curve and add them to an array. I then make sure the point was actually on the curve by searching for the closest point in my array of curve points. 

In the draw loop, I repeat this process to update the positions of each trunk piece. Then, I instruct each trunk piece to draw itself. I subsequently translate each leaf piece to its new location ontop of the tree, and tell each leaf piece to draw itself. 

To make this whole project responsive and interactive, I allowed sliders and window resizes to delete the old palm tree and create a new one using variables that correspond to slider positions and window dimensions. Since only p5 knows where the tree is, I also must create the buttons and sliders in p5 and adjust their locations according to window dimensions when the window is resized. 
