# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program
   

### Reflection of Path Planning

##### 1. Use of spline.h
* Here simulator is giving previous path. First I am getting last two points from it and adding it to `ptsx` and `ptsy`.
* Then I am creating three new waypoints to generate continuous path.
```
getXY(car_s+30,2+4*lane,map_waypoints_s,map_waypoints_x,map_waypoints_y)
getXY(car_s+60,2+4*lane,map_waypoints_s,map_waypoints_x,map_waypoints_y)
getXY(car_s+60,2+4*lane,map_waypoints_s,map_waypoints_x,map_waypoints_y)
```
<br> Here lane is current lane of the car.<br>
This will generate x and y coordinates, before pushing this points to `ptsx` and `ptsy` I am transforming it to car coordinates from global coordinates.
* Then I am fitting these points using spline.
* Then I am pushing all the remaining waypoints from previous path to `ptsx` and `ptsy`.
* Once that is done I am creating new points based on velocity.
 <br> ` double N = (target_dist/(.02*velocity/2.24)); `
* These new points will be added to `next_x_vals` and `next_y_vals` in addition to previous points.

#### 2. Smooth start and stop
* To start smoothly I am taking initial velocity as 0, and incrementing it additively, and same goes while stopping.
* For example,

```
if(velocity<49){
	velocity += 0.5;
}
```

#### 3. Using sensor fusion data
* Simulator is passing x and y components of velocity of other cars, I am using this data to check if our trajectory is overlapping with any other trajectory in same lane. If that is the case then I am reducing velocity.
* Also I am setting a flag `too_close` for all cars ahead of our car and setting `too_close_back` flag for cars behind our car
* These flags will be used for lane change.

#### 4. Lane change
* To change lane I am using flags created using sensor fusion data.
* When there is the a car ahead of our car, I am checking adjacent lanes and switch lanes if possible.
* If lane change is not possible or safe then reduce speed and keep in the same lane. 