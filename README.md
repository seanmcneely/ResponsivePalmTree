# ResponsivePalmTree
This JavaScript project allows you to add a fully responsive and interactive palm tree animation to an html webpage! 

The tree is built using p5js, a javascript libary that allows you to create animations by drawing simple shapes. Creating the tree turned out to be far more
challenging than I initially predicted, and was super fun. 

The palm tree exists as an object in JavaScript. To make the tree sway, the root of the tree stays in place, while the horizontal position of the top of the tree varies according to a sine function. In order for the tree to stay the same height while the top sways, it is also necessary to calculate the vertical position of the top of the tree using the pythagorean theorem. Based on theta which increases by some constant step size each iteration of the draw loop, rootX and rootY stay the same, while topX varies with swayWidth * sine(theta), and topY varies with sqrt{treeHeight^2 - (swayWidth * sine(theta))^2}

All curved objects in the animation are created using bezier curves. The bezier curve function in p5 takes as input two reference coordinates and two endpoint coordinates, and a step variable between 0 and 1. The function outputs a coordinate located on a smooth curve between the endpoint coordinates. The step variable determines how far along this curve the output coordinate is located. A step value of zero would output a coordinate directly ontop of one of the endpoint coordinates, as would a step value of one. Step values between zero and one specify points between the endpoints. 

To create the trunk of the tree, we first have to find points on a bezier curve between the bottom of the tree and the top of the tree that define where the pieces of the tree trunk will go. The trunk pieces each have some buffer space below them and some vertical height. To find the correct locations for the top and bottom of each trunk piece, we must find points on the bezier curve that maintain the height and buffer of each trunk piece as the tree sways. This was the biggest challenge of this project! Linear changes in the curve step variable do not correspond to linear distances along the curve! Thus, if we wanted to find the halfway point of the curve, it would not work to use a step of 0.5. Similarly, it is not possible to find the top of a trunk piece from the coordinates of the bottom by adding some constant fraction to the step variable. Doing so would result in non-linear and inconsistent trunk pieces. 

To overcome this and create the tree trunk, I generated several hundred points along the bezier curve and added them to an array. To create a tree trunk, I searched this array of points on the curve to find the closest approximation to where I calculated a tree trunk piece should be. This ensures that the points where I create tree trunk pieces are on the curve and have approximately the correct height. 
