# SNHU Computer Science Portfolio - Griffin Nelson

## Introduction and General Reflection

My name is Griffin Nelson and I was in the Computer Science program at SNHU for about four years. During my time in the program, I learned a lot about computers and various soft and hard skills that will help me in my future career.

Prior to entering the CS program, I was mostly self-taught in terms of programming. While my grasp on technical things like syntax and object-oriented design was solid, I did not have a good understanding of how an organization goes about developing software in a structured and stable way. However in the CS program I gained an understanding of the Software Development Life Cycle (SDLC). This newfound understanding of how software is developed in an professional setting will help me in my future career in the software development field as I will understand how the organization I work for goes about planning, developing and releasing software and I will be able to optimize my workflow accordingly.

I also learned a lot about how to collaborate with my peers with modern version control systems like Git. Git is a powerful tool and there are many different ways to achieve the same outcome. However not all of these ways are conducive to an environment of collaboration. My better understanding of how to use decentralized version control like Git to contribute to larger projects will definitely help me in the enterprise software development world.

Lastly, in working with embedded systems and reverse engineering software I have gained a better understanding of how high-level languages like C and C++ are turned into machine code by the compiler. This better understanding of how high-level concepts like stack variables, heap variables and inline functions are expressed in assembly code will help me with optimizing high-level code and debugging issues caused by compiler optimization.

## Project 1, on data structures and algorithms: [SNHU-CS-330-3D-Scene (OpenGL)](https://github.com/shmoe/SNHU-CS-330-3D-Scene)

To showcase my skills with data structures and algorithms I chose a project that I worked on in my Computer Graphics and Visualization class that I titled "SNHU-CS-330-3D-Scene." This project is written in C++ and makes use of the OpenGL graphics programming API in order to render an interactive 3D scene that implements 3D models, textures, materials and full Phong lighting.

![3D Rendered Scene Screenshot](https://github.com/shmoe/SNHU-CS-330-3D-Scene/blob/master/img/3D%20Scene%20Demo%201.png)

This project is perfect to showcase my proficiency with data structures because in working with OpenGL I had to work with many traditional data structures, such as lists, arrays and matrices, as well as custom data structures, such as the vertex and Model structures I implemented and various GLFW data structures like Vertex Array Objects.

In this project are also examples of my proficiency with algorithms. In order to provide smooth camera movement and other interactivity while still rendering the scene to the screen every frame I had to implement an event driven rendering loop. And in creating the models of which to render I created an algorithm I am particularly proud of, which generates the vertices, texture coordinates and surface normals for a soda can of an arbitrary height and radius. The algorithm in question is shown below but can also be seen in the project file [models.cpp](https://github.com/shmoe/SNHU-CS-330-3D-Scene/blob/master/CS-330%203D%20Scene/models.cpp).


```C++
/**
 * Generate vertices for soda can
 * Adapted from:
 *	http://www.songho.ca/opengl/gl_sphere.html
 */

vector<vertex> vertices;
vertex lid_middle;
vertex bottom_middle;

// change these to change attributes of can
float radius = 1.f;
float bevel_width = 0.2;
float height = 4.f;
int stacks_per_bevel = 3 * 3;
int sector_count = 36;
int stack_count = 36 * 3;

// assing positions to center points for lid and bottom
lid_middle.x = 0.f;
lid_middle.y = height;
lid_middle.z = 0.f;
lid_middle.nx = 0.f;
lid_middle.ny = height;
lid_middle.nz = 0.f;
bottom_middle.x = 0.f;
bottom_middle.y = 0.f;
bottom_middle.z = 0.f;
bottom_middle.nx = 0.f;
bottom_middle.ny = -1.f;
bottom_middle.nz = 0.f;

{
  struct vertex to_push;

  float lengthInv = 1.f / radius;				// multiply x,y,z by this to normalize a vertex vector

  float sector_step = 2 * PI / sector_count;
  float sector_angle;							// theta

  /**
   * Iterate through sectors
   */
  for (int i = 0; i <= stack_count; ++i) {
    to_push.y = height - height * ((float) i / stack_count);

    /**
     * Iterate through sectors of current stack
     */
    for (int j = 0; j <= sector_count; ++j) {
      sector_angle = j * sector_step;			// starts at 0, ends at 2*PI

      // calculate vertex position
      if (i <= stacks_per_bevel) {
        float bevel_radius = radius + (bevel_width * ((float) i / stacks_per_bevel));
        to_push.x = bevel_radius * cosf(sector_angle);
        to_push.z = bevel_radius * sinf(sector_angle);

        lengthInv = 1.f / bevel_radius;
      }										// case: upper bevel
      else if (i < stack_count - stacks_per_bevel) {
        to_push.x = (radius + bevel_width) * cosf(sector_angle);
        to_push.z = (radius + bevel_width) * sinf(sector_angle);

        lengthInv = 1.f / radius;
      }										// case: body
      else {
        float bevel_radius = radius + (bevel_width * ((float) (stack_count-i)  / (stacks_per_bevel)));
        to_push.x = bevel_radius * cosf(sector_angle);
        to_push.z = bevel_radius * sinf(sector_angle);

        lengthInv = 1.f / bevel_radius;
      }										// case: lower bevel

      glm::vec3 vert = glm::vec3(to_push.x, to_push.y, to_push.z);
      glm::vec3 origin = glm::vec3(0.f, to_push.y, 0.f);
      glm::vec3 norm = vert - origin;

      to_push.nx = norm.x * lengthInv;
      to_push.ny = norm.y * lengthInv;
      to_push.nz = norm.z * lengthInv;
/*
      // calculate normal: the vector orthonormal to the vertex (for lighting and physics)
      to_push.nx = to_push.x * lengthInv;
      to_push.ny = to_push.y * lengthInv;
      to_push.nz = to_push.z * lengthInv;
*/
      // calculate texture coordinates
      to_push.s = 1.f - (float)j / sector_count;
      to_push.t = 1.f - (float)i / stack_count;


      vertices.push_back(to_push);
    }
  }

}
```

## Project 2, on software design and architeture [SNHU-CS-350/CS-350 Thermostat (TI CC3220S)](https://github.com/shmoe/SNHU-CS-350/CS-350%207-1%20Project)

## Project 3, on databases: [SNHU-CS-360 Fitness App (Android)](https://github.com/shmoe/SNHU-CS-360)
