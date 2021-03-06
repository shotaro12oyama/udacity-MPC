## MPC Project Writeup ##


[image1]: ./equation_model.png "equation model"
[video1]: ./project_result.mov "project result"

----
### Project Overview ###

>In this project you'll implement Model Predictive Control to drive the car around the track. This time however you're not given the cross track error, you'll have to calculate that yourself! Additionally, there's a 100 millisecond latency between actuations commands on top of the connection latency.

the video of the resulte of my project can be found [here.](https://github.com/shotaro12oyama/udacity-MPC/blob/master/project_result.mov)

### Rublic Points ###

#### The Model ####

As udacity's learning course, the model implemented at this time consists of the vehicle's x and y coordinates(xt, yt), orientation angle (psi), velocity (vt), cross-track error (cte) and psi error (epsi). 

Also, actuators are acceleration and delta (steering angle). 
The equations of this model is as below.

(equation model)
![alt text][image1]

#### Timestep Length and Elapsed Duration (N & dt) ####

I set 20 & 0.1 for N and dt. I tried with other values such as (10 and 0.1), (50 and 0.02), (100 and 0.01) and so on, however, they did not work well. As a result, 20 & 0.1 is appropriate timeframe and steps for predicting better trajectory.

(update)
##### Why smaller dt is better? : #####
The smaller dt can allow MPC to predict in higher resolution, and to get better optimization result because there are more timing to modify throttle/steering_angle.

##### Why larger N isn't always better? (computational time): #####
If N is too large, it will take time to complete the prediction, and it cause additional latency for throttle/steering_angle, as a result more cte.

##### How does time horizon (N*dt) affect the predicted path?:
It time horizon is too long, the cost from the difference between vehicle and center of the line will provide more impact to the MPC. Thus, it make vehicle to be better along with the cetner of line, but the speed may be trade-off.

#### Polynomial Fitting and MPC Preprocessing ####

I transformed `ptsx`, `ptsy` (The global x, y positions of the waypoints) from car's perspective, to make the calculation easier, then use them for polyfit function. (#116-132 line, in main.cpp)

#### Model Predictive Control with Latency ####

I try to predict 0.1 sec feature state as initial, (#150-157 line, in MPC.cpp) in order for the vehicle to decide steer_angle and throttle based on the predicted waypoint after 0.1s.

(update)
Originally I took x, y, psi and v only into account, but I added the update for cte and epsi.
