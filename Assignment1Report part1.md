# Table of Contents

1. [ Line drawing ](#line-drawing)

<a name="line-drawing"></a>
## Line drawing

### Rendering the lines

```c++
	/**
	*Input st.:  The method gets the scene
	*Output st.: The function returns noting and draws the lines between points 1 to 2-4,6-7
	**/
	void Renderer::Render(const Scene& scene)
	{
		/**Setting the screen size**/
		int half_width = viewport_width / 2;
		int half_height = viewport_height / 2;

		/**Creating the poins**/
		glm::ivec2 p1(100, 100);
		glm::ivec2 p2(300, 300);
		glm::ivec2 p3(400, 100);
		glm::ivec2 p4(100, 400);	
		glm::ivec2 p6(250, 350);
		glm::ivec2 p7(350, 250);
		glm::ivec2 p8(350, 50);
		glm::ivec2 p9(250, 50);

		/**Creating the colors**/
		glm::vec3 color(0,100,0);	
		glm::vec3 color2(1, 0, 0);
		glm::vec3 color3(1, 0.5, 0.222324);
		glm::vec3 color4(0.2222, 0.4234, 0.242);
		glm::vec3 color5(0.131313, 0.4234, 0.242);
		glm::vec3 color6(0.546946995, 0.111, 0.546946995);
		glm::vec3 color7(0.3646946995, 0.9111, 0.146946995);
		glm::vec3 color8(255, 255,255);
		glm::vec3 color9(0.224245525, 0.546946995, 0.00001);


		/**Drawing the lines from point 1 to points 2-4 & 6-9 (idk why I didn't make point 5)**/
		DrawLine(p1, p2, color);
		DrawLine(p1, p3, color2);
		DrawLine(p1, p4, color3);
		DrawLine(p1, p6, color4);
		DrawLine(p1, p7, color5);
		DrawLine(p1, p8, color6);
		DrawLine(p1, p9, color7);
	}
```

### Drawing the line

```c++
	/**
	*Input st.: 2 ivec2 adresses to p1 and p2 that conntain the coordinates of 2 points, adress to vec3 color which will be the color of the line
	*Output st.: The function returns noting and draws the line between the 2 points with the color given
	**/
	void Renderer::DrawLine(const glm::ivec2& p1, const glm::ivec2& p2, const glm::vec3& color)
	{
		int minX=0, minY=0, maxX=0, maxY=0;	/**Most left pixel, most right pixel, bottom pixel, top pixel**/
		double m=0,c=0;

		/**Drawing the first and last pixels**/
		PutPixel(p1.x, p1.y, color);
		PutPixel(p2.x, p2.y, color);

		/**categoryzing the right pixel under maxX, the left pixel under minX,the bottom pixel under the minY and the top pixel under maxY  **/
		if (p1.x < p2.x) {
			maxX = p2.x;
			minX = p1.x;
		}
		else {
			maxX = p1.x;
			minX = p2.x;
		}

		if (p1.y < p2.y) {
			maxY = p2.y;
			minY = p1.y;
		}
		else {
			maxY = p1.y;
			minY = p2.y;
		}

		/**In case the line goes from 2 points with an incline of 0 we're just going to draw a straight line from left to right without going up or down**/
		if (p1.x == p2.x) {
			for (int i = minY+1; i < maxY; i++) {
				PutPixel(minX,i, color);
			}
		}
		else {
			/**In case both edges of the line have the same y, drawing a line from bottom to top without going left or right.**/
			if (p1.y == p2.y) {
				for (int i = minX+1; i < maxX; i++) {
					PutPixel(i, minY, color);
				}
			}

			else {
				/**Calculating the incline and c of the function**/
				m = (double)(p1.y - p2.y) / (p1.x - p2.x);
				c = (double)p1.y-(m*p1.x);

				/**In case the incline is between 1 to -1, going by x calculating the y for each x**/
				if (m >= -1 && m <= 1) {
					for (int i = minX + 1; i < maxX; i++) {
						PutPixel(i, m*(i + 0.5) + c, color);
					}
				}
				else {
					/**In case the incline is not between 1 to -1, going by y calculating the x for each y**/
					for (int i = minY + 1; i < maxY ; i++) {
						PutPixel(((i + 0.5) - c) / m, i, color);
					}
				}
			}
		}
	}
```

![image](https://user-images.githubusercontent.com/92427271/137463732-b1ff52d9-8f20-42b9-8eb7-9b7f381d7e21.png)

## Circle drawing

### Rendering the circles

```c++
	/**
	*Input st.:  The method gets the scene
	*Output st.: The method returns noting and draws the circles on screen
	**/
	void Renderer::Render(const Scene& scene)
	{
		/**Setting the screen size**/
		int half_width = viewport_width / 2;
		int half_height = viewport_height / 2;

		/**Creating 4 poins**/
		glm::ivec2 p1(80,400);
		glm::ivec2 p2(800, 300);
		glm::ivec2 p3(400, 200);
		glm::ivec2 p4(700, 500);

		/**Creating 4 colors**/
		glm::vec3 color(0,100,0);	
		glm::vec3 color2(1, 0, 0);
		glm::vec3 color3(1, 0.5, 0.222324);
		glm::vec3 color4(0.2222, 0.4234, 0.242);

		/**Drawing the 4 circles**/
		DrawCircle(p1, 100, color);
		DrawCircle(p2, 50, color2);
		DrawCircle(p3, 200, color3);
		DrawCircle(p4, 30, color4);
	}
```

### Circle drawing code

```c++
	/**
	*Input st.: point 1 that contains coordinations a glm::ivec2 object, a double named radius which presents the radius of the circle and an adress of glm::vec3 which 		presents the color
	* Output st.: The method returns noting and prints the circle
	**/
	void Renderer::DrawCircle(const glm::ivec2& p1,double radius, const glm::vec3& color)
	{
		/**Since a circle has both Inclines higher than 1 and lower than 1, we're gonna calculate each part seperatly and combine them**/

		/**Drawing the upper and lower part of the circle going from the left side to the right side**/
		for (int i = p1.x-radius; i <= p1.x+radius; i++) {
			PutPixel(i,sqrt(radius*radius-(i-p1.x)* (i - p1.x))+p1.y,color);
			PutPixel(i, (sqrt(radius * radius - (i - p1.x) * (i - p1.x)) + p1.y)-2*((sqrt(radius * radius - (i - p1.x) * (i - p1.x)) + p1.y)-p1.y), color);

		}
		
		/**Drawing the left and right part of the circle going from the up side to the down side**/
		for (int i = p1.y - radius; i <= p1.y + radius; i++) {
			PutPixel( sqrt(radius * radius - (i - p1.y) * (i - p1.y)) + p1.x,i, color);
			PutPixel( (sqrt(radius * radius - (i - p1.y) * (i - p1.y)) + p1.x) - 2 * ((sqrt(radius * radius - (i - p1.y) * (i - p1.y)) + p1.x) - p1.x),i, color);

		}
	}
```

![2021-10-14 (3)](https://user-images.githubusercontent.com/92427271/137371286-82f4bef9-ea8a-4ddb-874b-ec2177b0db3e.png)

## Sanity check 1 drawing

### Rendering the sanity check 1

```c++
	void Renderer::Render(const Scene& scene)
	{
		/**Setting the screen size**/
		int half_width = viewport_width / 2;
		int half_height = viewport_height / 2;

		/**Creating the poins**/
		glm::ivec2 p1(800, 100);
		glm::ivec2 p2(900, 600);
		glm::ivec2 p3(400, 200);

		/**Creating the colors**/
		glm::vec3 color(0, 100, 0);
		glm::vec3 color2(1, 0, 0);
		glm::vec3 color3(1, 0.5, 0.222324);

		/**Drawing the senity check**/
		sanitycheck(p1, 100, 10, color);
		sanitycheck(p2, 50, 70, color2);
		sanitycheck(p3, 300, 100, color3);
	}
```

### sanity check 1 code

```c++
	void Renderer::sanitycheck(const glm::ivec2& p1, double radius, double a, const glm::vec3& color)
	{
		glm::ivec2 firstP(p1.x, p1.y);
		glm::ivec2 newP(p1.x, p1.y);
		int counter = 1;
		firstP.x = p1.x; 
		firstP.y= p1.y+radius;
		newP.x = firstP.x;
		newP.y = firstP.y;

		do {
			DrawLine(p1, newP, color);
			newP.x = (double)(p1.x + (radius * sin(counter*2 * M_PI / (a ))));
			newP.y =(double)( p1.y + (radius * cos(counter*2 * M_PI / (a))));
			counter++;
		} while (!(newP.x ==firstP.x && newP.y == firstP.y));
	}
```

![image](https://user-images.githubusercontent.com/92427271/137589538-2e9a9d1a-d29d-43b9-89b9-ec2372ddceac.png)

## Sanity check 2 drawing

### Rendering the sanity check 2

```c++
	void Renderer::Render(const Scene& scene)
	{
		/**Setting the screen size**/
		int half_width = viewport_width / 2;
		int half_height = viewport_height / 2;

		/**Creating the poins**/
		glm::ivec2 p1(100, 100);
		glm::ivec2 p2(800, 300);
		glm::ivec2 p3(400, 600);

		/**Creating the colors**/
		glm::vec3 color(0,100,0);	
		glm::vec3 color2(1, 0, 0);
		glm::vec3 color3(1, 0.5, 0.222324);

		/**Drawing the sanity check**/
		sanitycheck2(p1, 100, color);
		sanitycheck2(p2, 200, color2);
		sanitycheck2(p3, 50, color3);
	}
```

### sanity check 2 code

```c++
	/**
	*Input st.: point 1 that contains coordinations a glm::ivec2 object, a double named radius which presents the radius of the circle where all the ponits are
	*and an adress of glm::vec3 which presents the color
	* Output st.: The method returns noting and prints every line between point 1 to the circle
	**/
	void Renderer::sanitycheck2(const glm::ivec2& p1, double radius, const glm::vec3& color)
	{
		glm::ivec2 p2(0,0);	/**A new point that will repesent every possible point on the circle**/

		/**Drawing the lines between p1 and every single point on the top and bottom of the circle**/
		for (int i = p1.x - radius; i <= p1.x + radius; i++) {
			p2.x = i;
			p2.y=sqrt(radius * radius - (i - p1.x) * (i - p1.x)) + p1.y;
			DrawLine(p1, p2, color);
			p2.y=(sqrt(radius * radius - (i - p1.x) * (i - p1.x)) + p1.y) - 2 * ((sqrt(radius * radius - (i - p1.x) * (i - p1.x)) + p1.y) - p1.y);
			DrawLine(p1, p2, color);
		}
		
		/**Drawing the lines between p1 and every single point on the left side and right side of the circle**/
		for (int i = p1.y - radius; i <= p1.y + radius; i++) {
			p2.y = i;
			p2.x=sqrt(radius * radius - (i - p1.y) * (i - p1.y)) + p1.x;
			DrawLine(p1, p2, color);
			p2.x=(sqrt(radius * radius - (i - p1.y) * (i - p1.y)) + p1.x) - 2 * ((sqrt(radius * radius - (i - p1.y) * (i - p1.y)) + p1.x) - p1.x);
			DrawLine(p1, p2, color);
		}
	}
```

![image](https://user-images.githubusercontent.com/92427271/137465336-ba42fe6d-745d-4ca0-9832-75502f127fd0.png)

## Drawing

### Code

```c++
	void Renderer::Render(const Scene& scene)
	{
		/**Setting the screen size**/
		int half_width = viewport_width / 2;
		int half_height = viewport_height / 2;
		glm::vec3 color(0,0,252);	
		drawing(color);
	}

	/**
	* Input st.: The color of the picture
	* Output st.: Running over each line drawing the picture
	**/
	void Renderer::drawing(const glm::vec3& color) {

		/**Creating the poins**/
		glm::ivec2 p1(100, 600);
		glm::ivec2 p2(1200, 600);

		/**Drawing each line 20 times for bigger picture, dawing 26 lines**/
		for (int i = 0; i < 20; i++) {
		
			/**Line 1 (out of 26)**/
			
			p1.x = 12 * 20;
			p1.y = 700-(100 + i);
			p2.x = 34*20-1;
			p2.y = 700-(100 + i);
			DrawLine(p1,p2,color);

			/**Line 2 (out of 26)**/
			
			p1.x = 6 * 20;
			p1.y = 700-(120 + i);
			p2.x = 41 * 20-1;
			p2.y = 700-(120 + i);
			DrawLine(p1, p2, color);

			/**Line 3 (out of 26)**/
			
			p1.x = 3 * 20;
			p1.y = 700-(140 + i);
			p2.x = 6 * 20-1;
			p2.y = 700-(140 + i);
			DrawLine(p1, p2, color);

			p1.x = 19 * 20;
			p2.x = 25 * 20-1;
			DrawLine(p1, p2, color);

			p1.x = 35 * 20;
			p2.x = 50 * 20-1;
			DrawLine(p1, p2, color);

			/**Line 4 (out of 26)**/
			
			p1.x = 2 * 20;
			p1.y = 700 - (160 + i);
			p2.x = 5 * 20 - 1;
			p2.y = 700 - (160 + i);
			DrawLine(p1, p2, color);

			p1.x = 6 * 20;
			p2.x = 18 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 19 * 20;
			p2.x = 22 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 25 * 20;
			p2.x = 48 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 5 (out of 26)**/
			
			p1.y = 700 - (180 + i);
			p2.y = 700 - (180 + i);

			p1.x = 2 * 20;
			p2.x = 9 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 15 * 20;
			p2.x = 21 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 22 * 20;
			p2.x = 44 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 6 (out of 26)**/
			
			p1.y = 700 - (200 + i);
			p2.y = 700 - (200 + i);

			p1.x = 4 * 20;
			p2.x = 8 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 9 * 20;
			p2.x = 10 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 13 * 20;
			p2.x = 14 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 16 * 20;
			p2.x = 25 * 20 - 1;
			DrawLine(p1, p2, color);


			p1.x = 33 * 20;
			p2.x = 42 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 7 (out of 26)**/
			
			p1.y = 700 - (220 + i);
			p2.y = 700 - (220 + i);

			p1.x = 7 * 20;
			p2.x = 8 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 9 * 20;
			p2.x = 15 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 19 * 20;
			p2.x = 22 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 25 * 20;
			p2.x = 28 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 25 * 20;
			p2.x = 28 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 30 * 20;
			p2.x = 33 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 37 * 20;
			p2.x = 44 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 8 (out of 26)**/
			
			p1.y = 700 - (240 + i);
			p2.y = 700 - (240 + i);

			p1.x = 15 * 20;
			p2.x = 25 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 27 * 20;
			p2.x = 30 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 35 * 20;
			p2.x = 48 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 9 (out of 26)**/
			
			p1.y = 700 - (260 + i);
			p2.y = 700 - (260 + i);

			p1.x = 14 * 20;
			p2.x = 22 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 24 * 20;
			p2.x = 27 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 32 * 20;
			p2.x = 35 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 36 * 20;
			p2.x = 54 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 10 (out of 26)**/
			
			p1.y = 700 - (280 + i);
			p2.y = 700 - (280 + i);

			p1.x = 2 * 20;
			p2.x = 14 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 22 * 20;
			p2.x = 24 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 25 * 20;
			p2.x = 27 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 29 * 20;
			p2.x = 35 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 36 * 20;
			p2.x = 58 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 11 (out of 26)**/
			
			p1.y = 700 - (300 + i);
			p2.y = 700 - (300 + i);

			p1.x = 2 * 20;
			p2.x = 3 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 8 * 20;
			p2.x = 10 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 14 * 20;
			p2.x = 15 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 16 * 20;
			p2.x = 21 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 24 * 20;
			p2.x = 25 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 27 * 20;
			p2.x = 30 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 33 * 20;
			p2.x = 35 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 36 * 20;
			p2.x = 59 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 12 (out of 26)**/
			
			p1.y = 700 - (320 + i);
			p2.y = 700 - (320 + i);

			p1.x = 9 * 20;
			p2.x = 15 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 18 * 20;
			p2.x = 24 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 25 * 20;
			p2.x = 29 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 30 * 20;
			p2.x = 32 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 33 * 20;
			p2.x = 36 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 37 * 20;
			p2.x = 55 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 13 (out of 26)**/
			
			p1.y = 700 - (340 + i);
			p2.y = 700 - (340 + i);

			p1.x = 21 * 20;
			p2.x = 23 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 24 * 20;
			p2.x = 32 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 33 * 20;
			p2.x = 36 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 37 * 20;
			p2.x = 53 * 20 - 1;
			DrawLine(p1, p2, color);
			
			/**Line 14 (out of 26)**/

			p1.y = 700 - (360 + i);
			p2.y = 700 - (360 + i);

			p1.x = 4 * 20;
			p2.x = 21 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 23 * 20;
			p2.x = 30 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 32 * 20;
			p2.x = 36 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 37 * 20;
			p2.x = 52 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 15 (out of 26)**/
			
			p1.y = 700 - (380 + i);
			p2.y = 700 - (380 + i);

			p1.x = 6 * 20;
			p2.x = 8 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 18 * 20;
			p2.x = 29 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 30 * 20;
			p2.x = 37 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 38 * 20;
			p2.x = 54 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 16 (out of 26)**/
			
			p1.y = 700 - (400 + i);
			p2.y = 700 - (400 + i);

			p1.x = 8 * 20;
			p2.x = 15 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 20 * 20;
			p2.x = 28 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 29 * 20;
			p2.x = 37 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 38 * 20;
			p2.x = 55 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 17 (out of 26)**/
			
			p1.y = 700 - (420 + i);
			p2.y = 700 - (420 + i);

			p1.x = 15 * 20;
			p2.x = 18 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 24 * 20;
			p2.x = 26 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 28 * 20;
			p2.x = 37 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 38 * 20;
			p2.x = 57 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 18 (out of 26)**/

			p1.y = 700 - (440 + i);
			p2.y = 700 - (440 + i);

			p1.x = 18 * 20;
			p2.x = 24 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 27 * 20;
			p2.x = 36 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 27 * 20;
			p2.x = 36 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 37 * 20;
			p2.x = 58 * 20 - 1;
			DrawLine(p1, p2, color);
			
			/**Line 19 (out of 26)**/
		
			p1.y = 700 - (460 + i);
			p2.y = 700 - (460 + i);

			p1.x = 22 * 20;
			p2.x = 23 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 26 * 20;
			p2.x = 35 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 36 * 20;
			p2.x = 52 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 54 * 20;
			p2.x = 59 * 20 - 1;
			DrawLine(p1, p2, color);
			
			/**Line 20 (out of 26)**/
			
			p1.y = 700 - (480 + i);
			p2.y = 700 - (480 + i);

			p1.x = 21 * 20;
			p2.x = 22 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 25 * 20;
			p2.x = 34 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 35 * 20;
			p2.x = 50 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 21 (out of 26)**/
			
			p1.y = 700 - (500 + i);
			p2.y = 700 - (500 + i);

			p1.x = 20 * 20;
			p2.x = 21 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 24 * 20;
			p2.x = 28 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 34 * 20;
			p2.x = 49 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 22 (out of 26)**/
			
			p1.y = 700 - (520 + i);
			p2.y = 700 - (520 + i);

			p1.x = 19 * 20;
			p2.x = 20 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 23 * 20;
			p2.x = 26 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 42 * 20;
			p2.x = 52 * 20 - 1;
			DrawLine(p1, p2, color);
		
			/**Line 23 (out of 26)**/
			
			p1.y = 700 - (540 + i);
			p2.y = 700 - (540 + i);

			p1.x = 19 * 20;
			p2.x = 20 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 22 * 20;
			p2.x = 25 * 20 - 1;
			DrawLine(p1, p2, color);

			/**Line 24 (out of 26)**/
			
			p1.y = 700 - (560 + i);
			p2.y = 700 - (560 + i);

			p1.x = 18 * 20;
			p2.x = 19 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 22 * 20;
			p2.x = 23 * 20 - 1;
			DrawLine(p1, p2, color);
			
			/**Line 25 (out of 26)**/
			
			p1.y = 700 - (580 + i);
			p2.y = 700 - (580 + i);

			p1.x = 17 * 20;
			p2.x = 18 * 20 - 1;
			DrawLine(p1, p2, color);

			p1.x = 22 * 20;
			p2.x = 23 * 20 - 1;
			DrawLine(p1, p2, color);
			
			/**Line 26 (out of 26)**/
			
			p1.y = 700 - (600 + i);
			p2.y = 700 - (600 + i);

			p1.x = 17 * 20;
			p2.x = 22 * 20 - 1;
			DrawLine(p1, p2, color);
		}
	}
```

### Result

![image](https://user-images.githubusercontent.com/92427271/137588715-281cbaaf-174d-40a0-9be8-d332f40f9c42.png)

Original picture for comparison:
Berdly from deltarune

![image](https://user-images.githubusercontent.com/92427271/137589226-ec771449-1848-43de-86c6-fad53952a392.png)
