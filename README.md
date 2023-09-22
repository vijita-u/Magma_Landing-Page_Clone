# Landing Page Clone of 'https://thisismagma.com/'

![image](https://github.com/vijita-u/Magma_Landing-Page_Clone/assets/96591032/dbde3a1b-a0ed-4ba5-887e-1eb1da3739a8)


## Overview
This project is a clone of the landing page of 'https://thisismagma.com/' with the goal of learning and implementing various web development technologies and techniques. The project includes HTML5 Canvas animations, scrolling animations using GSAP Scroll Trigger & Locomotive, a carousel using Swiper JS and text animations using TextillateJS.

## Motivation
The primary motivation behind this project was to acquire hands-on experience with the following technologies and concepts:

1. HTML5 Canvas
2. GSAP animations
3. GSAP Scroll Trigger
4. Locomotive
5. SwiperJS
6. TextillateJS

## Live Demo
You can view a live demo of this project at [Magma Live Demo Link]().

## Features

### 1. HTML5 Canvas Animations

#### Implementing a function to take care of all HTML5 Canvas Animations:
  ##### Parameters -
  - **page**: A string representing the page of the HTML element containing the canvas where the animation will be displayed.
  - **data**: A string containing image file paths separated by line breaks (\n). These image files represent the frames of the animation.

  ```
  function canvas(page, data) {
    ...
  }
  ```
  Let's now move on to the description of the code inside the function.

  #### Canvas Initialization and Context Setup -
  - The function starts by selecting the HTML canvas element specified by the page parameter and getting its 2D rendering context.
  - It sets the canvas dimensions to match the window's dimensions, making it responsive to window resizing.

  ```
    const canvas = document.querySelector(`${page} canvas`);
    const context = canvas.getContext("2d");

    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
  ```

  #### Window Resize Event Listener -
  - An event listener is added to the window's resize event, so whenever the window size changes, the canvas dimensions are updated, and the render() function is called to redraw the animation.

  ```
    window.addEventListener("resize", () => {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    render();
    });
  ```
  - We'll talk about the render() function a little later.

  #### Image Loading and Initialization -
  - The **data** parameter is split into an array of image file paths, with each line representing a frame in the animation.
  - An array called **images** is created to store image objects, and an object called **imageSequence** is initialized with a frame property set to 0, representing the current frame of the animation.
  - A loop iterates through the image file paths and creates <img> elements for each image, sets their **src** attributes to the respective file paths, and adds them to the images array.
  
  ```
    const imageFiles = data.split("\n"); // Image array
    const frameCount = imageFiles.length; // Image count

    const images = [];
    const imageSequence = {
    frame: 0,
    };

    for (let i = 0; i < frameCount; i++) {
    const img = new Image();
    img.src = imageFiles[i];
    images.push(img);
    }
  ```

  #### GSAP ScrollTrigger Animation -
  - GSAP (GreenSock Animation Platform) is used to tween (animate) the **imageSequence object's frame** property.
  - The animation changes the frame property from 0 to the total number of frames in the animation (frameCount). It uses the snap option to ensure that the animation only uses whole numbers for frames.
  - This animation is scroll-triggered, meaning it plays as the user scrolls down the page. The ScrollTrigger settings include a scrub of 0.5, which controls the sensitivity of the animation to scrolling, and a     **start** and **end** point within the trigger specified by **${page}**.
  - The **onUpdate** callback function render is called whenever the animation updates to redraw the current frame.

  ```
    gsap.to(imageSequence, {
      frame: frameCount,
      snap: "frame",
      ease: "none",
      scrollTrigger: {
        scrub: 0.5,
        trigger: `${page}`,
        start: "top top",
        end: "250% top",
        scroller: "#main",
      },
      onUpdate: render,
    });
  ```

  #### Rendering Function ( render() ) - 
  -The render function is responsible for rendering the current frame on the canvas.
  - It calls the **scaleImage function** to scale and center the current image frame on the canvas.

  ```
    function render() {
      scaleImage(images[imageSequence.frame], context);
    }
  ```

  #### Image Scaling Function ( scaleImage() ) -
  - The **scaleImage function** calculates the scaling ratio based on the canvas and image dimensions, ensuring the image is centered and scaled appropriately.
  
  ```
    function scaleImage(image, context) {
      let canvas = context.canvas;
      let horizontalRatio = canvas.width / image.width;
      let verticalRatio = canvas.height / image.height;
      let ratio = Math.max(horizontalRatio, verticalRatio);
      let centerShift_x = (canvas.width - image.width * ratio) / 2;
      let centerShift_y = (canvas.height - image.height * ratio) / 2;
      context.clearRect(0, 0, canvas.width, canvas.height);
      context.drawImage(
        image,
        0,
        0,
        image.width,
        image.height,
        centerShift_x,
        centerShift_y,
        image.width * ratio,
        image.height * ratio
    );
  }
  ```

  #### ScrollTrigger Pinning -
  - Lastly, a ScrollTrigger is created to pin the specified page element when it enters the viewport. This pins the content as the user scrolls.

  ```
    ScrollTrigger.create({
		  trigger: `${page}`,
		  pin: true,
		  scroller: "#main",
		  start: "top top",
		  end: "250% top",
	  });
  }
  ```

Overall, this function combines HTML5 Canvas rendering, image loading, GSAP animation with ScrollTrigger, and responsive design techniques to create a visually appealing and scroll-triggered animation on a webpage.

### 2. Scrolling Animations
- In this project, Locomotive Scroll and Scroll Trigger have been merged to combine Locomotive Scroll's smoothness and speed along with GSAP Scroll Trigger's ease of creating animations.
- The above has been made possible by the JS code provided in [this codepen](https://codepen.io/GreenSock/pen/ExPdqKy).

### 3. Carousel using Swiper JS

  #### Getting Started
  - Include the swiperJS's JavaScript and CSS CDN in your index.html file.

    Add link tag inside the <head> tag.
  ```
    <head>
      <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/swiper@10/swiper-bundle.min.css" />
    </head>
  ```
  Add script tag at the end inside the <body> tag.
  ```
    <body>
      ...
      ...
      <script src="https://cdn.jsdelivr.net/npm/swiper@10/swiper-bundle.min.js"></script>
    </body>
  ```

  ### Configure SwiperJS Carousel
  ```javascript

    // Swiper JS Carousel Configuration

    // Create a new Swiper instance targeting the HTML element with the class "mySwiper".
    var swiper = new Swiper(".mySwiper", {
      // Display one slide at a time.
      slidesPerView: 1,
  
      // Add spacing (20 pixels) between each slide.
      spaceBetween: 20,
  
      // Center the currently active slide.
      centeredSlides: true,
  
      // Set the transition speed to 3000 milliseconds (3 seconds).
      speed: 3000,
  
      // Use the "slide" effect for slide transitions.
      effect: "slide",
  
      // Enable loop mode to create an infinite carousel.
      loop: true,
  
      // Start the carousel with the second slide (initialSlide: 1).
      initialSlide: 1,
  
      // Configure autoplay settings.
      autoplay: {
        // Delay between slides (0 milliseconds for immediate transitions).
        delay: 0,
    
        // Reverse direction when reaching the end of the slides.
        reverseDirection: true,
    
        // Do not disable autoplay on user interaction (e.g., clicking or swiping).
        disableOnInteraction: false,
      },
  
      // Define responsive breakpoints for different screen sizes.
      breakpoints: {
        // Screen width 340px and above:
        340: {
          slidesPerView: 2, // Display 2 slides at a time.
          spaceBetween: 30, // Increase spacing to 30 pixels.
        },
    
        // Screen width 640px and above:
        640: {
          slidesPerView: 3, // Display 3 slides at a time.
          spaceBetween: 15, // Reduce spacing to 15 pixels.
        },
    
        // Screen width 768px and above:
        768: {
          slidesPerView: 4, // Display 4 slides at a time.
          spaceBetween: 40, // Increase spacing to 40 pixels.
        },
    
        // Screen width 1024px and above:
        1024: {
          slidesPerView: 5, // Display 5 slides at a time.
          spaceBetween: 50, // Increase spacing to 50 pixels.
        },
      },
    });

  ```
