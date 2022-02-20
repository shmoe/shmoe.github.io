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

In this project are also examples of my proficiency with algorithms. In order to provide smooth camera movement and other interactivity while still rendering the scene to the screen every frame I had to implement an event driven rendering loop. And in creating the models of which to render I created an algorithm I am particularly proud of, which generates the vertices, texture coordinates and surface normals for a soda can of an arbitrary height and radius. The algorithm in question is shown below.

### [SNHU-CS-330-3D-Scene/models.cpp](https://github.com/shmoe/SNHU-CS-330-3D-Scene/blob/master/CS-330%203D%20Scene/models.cpp)
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

  float lengthInv = 1.f / radius;  // multiply x,y,z by this to normalize a vertex vector

  float sector_step = 2 * PI / sector_count;
  float sector_angle;     // theta

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
To enhance this artifact, I had originally intended to implement asynchronous model loading. However, due to the nature of OpenGL contexts not being shared between threads this would have involved a massive refactor of the application that would have negatively affected readability for a negligible decrease in startup time. As such, I instead enhanced the presentation of the repository itself. I added a README markdown document that features the controls of the application as well as showcases its different features, such as the ability to toggle between orthographic projection and perspective projection as well as the ability to change the color of the point light in the scene. In said README I also included screenshots of the application in action. The first screenshot features an angle of the 3D scene that showcases the models and lighting. The second screenshot showcases the wireframe mode. And finally, the third screenshot showcases the ability to change the point lighting and how the diffuse and specular lighting is affected by this change.

## Project 2, on software design and architeture [SNHU-CS-350/CS-350 Thermostat (TI CC3220S)](https://github.com/shmoe/SNHU-CS-350/tree/master/CS-350%207-1%20Project)

To showcase my proficiency with software design and architecture I chose an embedded C project from my Emerging Systems and Architectures class. This is because writing embedded code that is small, efficient and still human readable is a task requiring a proper understanding of software design, architecture and proper documentation. And in developing this project I believe I have achieved just that.

The mentioned project is a small application that runs on embedded systems with a CS3220s architecture. It is a proof of concept for a thermostat application that samples the current temperature, polls for user input in the form of on-board buttons to raise and lower the thermostat’s set temperature and simulates turning the heat off and on depending on how the set temperature compares to the ambient temperature using an LED to represent the heat source. It also sends regular logs over serial using UART.

The main functionalities of the application are split into three distinct tasks: polling for input, poll for the ambient temperature, and update the heat according to new information. These tasks are encapsulated in a task structure and their execution is handled by a task scheduler. All utility functions related to interfacing with board hardware like GPIO and UART are relegated to their own source files.

The primary enhancements done to this artifact was that of presentation and documentation. Embedded systems applications can be very low level and often the software APIs used to interface with hardware are unique to each board. As such it is essential to properly document code such that one unfamiliar with the specific board or with embedded systems development in general can properly gain insights about my abilities from the code.

My biggest challenge in creating this artifact was becoming familiar with the API with which to interface with the hardware. It was a completely new experience for me and the fact that documentation about the API was not easily found online was a new experience to me. Alternatively, my largest challenge in enhancing this artifact was in being consistent with my code style. There is a lot of boilerplate code generated by the IDE that has code style that clashes directly with my own and I had to resolve that.

Below is a snippet of code from the project which features the main event loop and task scheduler. I believe this snippet is an example of simple, clean, and well documented code that exemplifies the project.

### [SNHU-CS-350/CS-350 7-1 Project/gpiointerrupt.c](https://github.com/shmoe/SNHU-CS-350/blob/master/CS-350%207-1%20Project/gpiointerrupt.c)
```C
#define TASKS_NUM 3
```
```C
/* List of tasks to be managed by the task scheduler */
task tasks[TASKS_NUM] = {
                 { 200000, 0, &checkButtons },
                 { 500000, 0, &checkTemp },
                 { 1000000, 0, &update }
};
```
```C
/* Task Scheduler */
while(1) {
    int i;

    /* Iterate over tasks */
    for(i = 0; i < TASKS_NUM; ++i){
        if (tasks[i].elapsedTime >= tasks[i].period) {
            tasks[i].TickFct();
            tasks[i].elapsedTime = 0;
        } // Execute task when period has passed.

        tasks[i].elapsedTime += TIMER_PERIOD;
    }

    /* Wait until next tick */
    while(!TimerFlag) {}
    TimerFlag = 0;
    ++ticks;
}
```

## Project 3, on databases: [SNHU-CS-360 Fitness App (Android)](https://github.com/shmoe/SNHU-CS-360)

The artifact chosen to showcase my proficiency with databases is an Android application created in my Mobile App Architecture and Programming class. The application is a simple weight tracker app that keeps a list of weights on dates associated with each user. It also features a log in screen that checks username and passwords. The app stores data such as weights, dates, user credentials and their relations in a local SQL database. In this way it showcases my ability to utilize databases to not only store data but also utilize the relational nature of SQL to represent associations between data.

![Android Fitness App Screenshot 1](https://github.com/shmoe/SNHU-CS-360/raw/master/res/Screenshot_1.png)

![Android Fitness App Screenshot 2](https://github.com/shmoe/SNHU-CS-360/raw/master/res/Screenshot_2.png)

In reviewing my work on this artifact, I realized that not only are passwords stored on the database in plain text, which makes them vulnerable to data mining, the SQL queries I used are vulnerable to injection attacks. To solve this and showcase my ability to identify and remedy security flaws I first implemented password salting and hashing, which allows passwords to be checked without storing them in plaintext. I then converted all of my SQL queries into stored procedures, which effectively removes the possibility of SQL injection. Finally to improve the presentation of my work in the ePortfolio I tweaked the application’s UI to look more professional and less like placeholder art. I also added screenshots of the application in action to the repository’s README markdown file.

## Informal Code Review

Prior to enhancing my projects in preparation for creating this portfolio I performed an informal code review of my TI CCS3320S project. It can be viewed at the following link: https://youtu.be/jwEd2AS3PZI *Warning: audio is very quiet due to hardware limitations*
