---
layout: post
title: Understanding Signed Distance Fields (SDF) by implementing it in Python
---


## Introduction

Welcome to the world of signed distance fields (SDFs)! In this tutorial, we will delve into the fascinating concept of SDFs and explore their applications in computer vision, computer graphics, and shape representation. Whether you're a beginner curious about 3D reconstruction or an enthusiast looking to expand your knowledge, this tutorial will provide you with a solid understanding of SDFs and their significance.

## Table of Contents

1. Limitations of Triangle-based Representations
2. Why Signed Distance Fields (SDF)?
3. What are Signed Distance Fields?
4. Understanding SDF Hands-On
5. Properties of SDFs
    - Union
    - Intersection
    - Blending
6. Conclusion

## Limitations of Triangle-based Representations

Triangle-based representations are commonly used for rendering three-dimensional (3D) shapes. However, they have some limitations:

1. Complexity: Representing complex shapes using triangles requires a large number of triangles, leading to high computational costs and memory requirements.

2. Mesh Artifacts: Triangle meshes can result in artifacts such as gaps, cracks, or overlapping surfaces when dealing with complex shapes or deformations.

3. Topology Changes: Handling topology changes, such as shape merging or splitting, is challenging using triangle-based representations.

## Why Signed Distance Fields (SDF)?

Signed Distance Fields (SDF) provide an alternative representation for 3D shapes that overcomes the limitations of triangle-based representations. SDFs are efficient and flexible, offering several advantages:

1. Compactness: SDFs represent shapes implicitly by assigning a signed distance value to each point in space, resulting in a compact representation.

2. Efficiency: Evaluating the signed distance value at any point in space is computationally inexpensive compared to triangle-based representations.

3. Robustness: SDFs handle shape deformations, topology changes, and complex shapes without introducing artifacts.

4. Boolean Operations: SDFs enable efficient boolean operations such as union, intersection, and blending, allowing for the creation of complex shapes by combining simpler ones.

## What are Signed Distance Fields?

Signed Distance Fields (SDFs) assign a signed distance value to each point in space relative to the closest surface of a shape. The signed distance can be positive if the point is outside the shape, negative if the point is inside the shape, or zero if the point lies exactly on the surface.

To create an SDF, we need to define a shape's center and radius. We can then calculate the signed distance at each point in a 3D grid using the following formula:

```python
import numpy as np

def calculate_signed_distance(x, y, z, center, radius):
    distance = np.sqrt((x - center[0])**2 + (y - center[1])**2 + (z - center[2])**2)
    return distance - radius
```

Here, `x`, `y`, and `z` represent the coordinates of a point in the grid, `center` is the center of the shape, and `radius` is the radius of the shape. The function calculates the Euclidean distance between the point and the shape's center and subtracts the radius to obtain the signed distance value.

We can create a complete SDF by calculating the signed distance for each point in the grid:

```python
def create_signed_distance_field(grid_size, center, radius):
    sdf = np.zeros(grid_size)
    for x in range(grid_size[0]):
        for y in range(grid_size[1]):
            for z in range(grid_size[2]):
                sdf[x, y, z] = calculate_signed_distance(x, y, z, center, radius)
    return sdf
```

In the above code, `grid_size` represents the size of the 3D grid, and we initialize an array `sdf` with zeros. We then iterate over each point in the grid, calculate its signed distance, and store the value in the `sdf` array.

## Understanding SDF Hands-On

Let's take a hands-on approach to understand SDFs. We'll create a simple 3D shape and visualize it using the matplotlib library in Python. Run the following code:

```python
import numpy as np
import matplotlib.pyplot as plt

# Define the parameters
grid_size = (20, 20, 20)
center = (10, 10, 10)
radius = 8

#

 Create the signed distance field
sdf = create_signed_distance_field(grid_size, center, radius)

# Visualize the 3D shape
fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

x = np.arange(0, grid_size[0]+1, 1)
y = np.arange(0, grid_size[1]+1, 1)
z = np.arange(0, grid_size[2]+1, 1)
X, Y, Z = np.meshgrid(x, y, z)

ax.voxels(X, Y, Z, sdf < 0, facecolors='blue', edgecolor='k')

ax.set_xlabel('X')
ax.set_ylabel('Y')
ax.set_zlabel('Z')

plt.show()
```

In this code, we first define the `grid_size` representing the dimensions of the 3D grid. Next, we specify the `center` and `radius` of the shape. We then create the signed distance field using the `create_signed_distance_field` function. Finally, we visualize the 3D shape using voxels, where negative signed distance values are shown in blue.

## Properties of SDFs

Signed Distance Fields possess several useful properties that enable powerful operations on shapes.

### 1. Union

The union operation allows us to combine two shapes to create a new shape that encompasses both. For SDFs, the union operation is performed by taking the minimum signed distance value at each point.

```python
def union(sdf1, sdf2):
    return np.minimum(sdf1, sdf2)
```

In the above code, `sdf1` and `sdf2` represent two SDFs of the same size. The `union` function takes the element-wise minimum of the signed distance values, resulting in a new SDF representing the union of the two shapes.

### 2. Intersection

The intersection operation involves finding the overlapping region between two shapes. In SDFs, we perform the intersection operation by taking the maximum signed distance value at each point.

```python
def intersection(sdf1, sdf2):
    return np.maximum(sdf1, sdf2)
```

Here, `sdf1` and `sdf2` are two SDFs, and the `intersection` function computes the element-wise maximum of the signed distance values, generating a new SDF representing the intersection of the two shapes.
### 3. Blending

Blending is a technique used to smoothly combine two shapes by interpolating their signed distance values. This operation is particularly useful for creating smooth transitions or soft intersections between shapes.

The blend operation is governed by the following equation:

```
blended_sdf = (1 - t) * sdf1 + t * sdf2
```

In the equation, `sdf1` and `sdf2` represent the signed distance fields of the two shapes being blended, and `t` is a blending factor between 0 and 1. The blending factor determines the influence of each shape on the final blended result. When `t` is 0, the blended shape is equivalent to `sdf1`, and when `t` is 1, the blended shape is equivalent to `sdf2`. Intermediate values of `t` create smooth transitions between the two shapes.

By applying the blend equation to the signed distance values at each point in the SDFs, a new blended signed distance field, `blended_sdf`, is generated. This blended SDF represents the combination of the two shapes, incorporating their characteristics in a smooth and seamless manner.

The blending operation in SDFs allows for the creation of complex and intricate shapes by smoothly merging or transitioning between different components. It is a powerful tool for achieving shape variations, deformations, and morphing effects in various applications such as computer graphics, character animation, and procedural modeling.

## Conclusion

Signed Distance Fields (SDFs) offer a compact, efficient, and robust representation for 3D shapes, overcoming the limitations of triangle-based representations. By assigning a signed distance value to each point in space relative to the closest surface of a shape, SDFs enable a wide range of operations such as blending, union, and intersection, allowing for the creation of complex shapes and smooth transitions. 

Understanding SDFs is essential for anyone involved in 3D computer vision, computer graphics, or shape representation. Whether you're exploring 3D reconstruction, object segmentation, or shape analysis, SDFs provide a powerful toolset to tackle complex challenges and achieve realistic and flexible representations.

## Additional Resources

If you're eager to explore further and deepen your understanding of SDFs, here are some additional resources that helped me:

1. [YouTube Video: "Signed Distance Functions"](https://www.youtube.com/watch?v=BNZtUB7yhX4) - A video tutorial explaining the concepts of Signed Distance Functions (SDFs) in a visual and intuitive manner.

2. [Article: "Signed Distance Functions"](https://www.alanzucconi.com/2016/07/01/signed-distance-functions/) - A comprehensive article by Alan Zucconi that provides an explanation of Signed Distance Functions (SDFs), their mathematical foundations, and practical implementations.

These resources will complement your understanding of SDFs and provide additional insights and perspectives to enhance your knowledge in this exciting field. Happy learning!