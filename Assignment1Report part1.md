



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

