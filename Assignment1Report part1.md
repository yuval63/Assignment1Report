


## Line drawing



### Rendering the lines
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



### Drawing the line


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



![image](https://user-images.githubusercontent.com/92427271/137463732-b1ff52d9-8f20-42b9-8eb7-9b7f381d7e21.png)




## Circle drawing
### Rendering the circles

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
### Circle drawing code


/**
*Input st.: point 1 that contains coordinations a glm::ivec2 object, a double named radius which presents the radius of the circle and an adress of glm::vec3 which presents the color
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
![2021-10-14 (3)](https://user-images.githubusercontent.com/92427271/137371286-82f4bef9-ea8a-4ddb-874b-ec2177b0db3e.png)




## Sanity check 1 drawing


### Rendering the sanity check 1

void Renderer::Render(const Scene& scene)
{

	/**Setting the screen size**/
	int half_width = viewport_width / 2;
	int half_height = viewport_height / 2;

	/**Creating the poins**/
	glm::ivec2 p1(100, 600);
	glm::ivec2 p2(800, 600);
	glm::ivec2 p3(400, 600);


	/**Creating the colors**/
	glm::vec3 color(0,100,0);	
	glm::vec3 color2(1, 0, 0);
	glm::vec3 color3(1, 0.5, 0.222324);



	/**Drawing the senity check**/
	sanitycheck(p1, 100, 10, color);	
	sanitycheck(p2, 50, 70, color2);
	sanitycheck(p3, 300, 100, color3);


}
### sanity check 1 code
void Renderer::sanitycheck2(const glm::ivec2& p1, double radius, double a, const glm::vec3& color)
{

	glm::ivec2 firstP(p1.x, p1.y);
	glm::ivec2 newP(p1.x, p1.y);
	int counter = 1;
	firstP.x = p1.x; 
	firstP.y= p1.y+radius;
	newP.x = firstP.x;
	newP.y = firstP.y;
	
	do {
		PutPixel(newP.x,newP.y,color);
		counter++;
		newP.x = (double)(p1.x + (radius * sin(counter*2 * M_PI / (a ))));
		newP.y =(double)( p1.y + (radius * cos(counter*2 * M_PI / (a))));
		cout << "bannana counter: " << counter << " newP.x " << newP.x << " newP.y " << newP.y << " firstP.x " << firstP.x << " firstP.y " << firstP.y <<" sin:" << sin(2 * M_PI / (a * counter)) <<"\n";
	} while (!(newP.x ==firstP.x && newP.y == firstP.y));

}

![image](https://user-images.githubusercontent.com/92427271/137484013-8bf89b6d-9838-42e0-9371-9b38a9476093.png)

## Sanity check 2 drawing


### Rendering the sanity check 2
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


### sanity check 2 code

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





![image](https://user-images.githubusercontent.com/92427271/137465336-ba42fe6d-745d-4ca0-9832-75502f127fd0.png)





## Drawing

![image](https://user-images.githubusercontent.com/92427271/137541499-15504d28-4b4d-468a-a589-bd225045eac5.png)

