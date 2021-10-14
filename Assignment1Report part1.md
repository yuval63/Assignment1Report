
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

