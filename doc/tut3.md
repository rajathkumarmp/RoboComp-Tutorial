#Laser.idsl

##Introduction
This is an interface which specifies the usage of laser in the bot. Used to get the distance of a particular object from the bot or in general to measure the distance. This finds usage in most of the bots to begin with obstacle avoiding bot which you will be learning along this tutorial series.

##Getting Started
First as always create a component which will use the Laser.idsl. Your cdsl should look something like this.

	import "/robocomp/interfaces/IDSLs/Laser.idsl";
	import "/robocomp/interfaces/IDSLs/DifferentialRobot.idsl";
	Component thirdcomp{
		Communications{
			requires DifferentialRobot, Laser;

		};
		language Cpp;
	};

Now generate the code and build the component. In detail explanation about creating a component is previously discussed in this tutorial series.

Now you must understand that Laser.idsl is used to get the distance from an obstacle. Hence we need to declare a vector that would store the distances that are measured. In this example it is called ldata

	RoboCompLaser::TLaserData ldata = laser_proxy->getLaserData();

The above line as discussed above gets the laser data, `getLaserData()` and stores it in `ldata`. Now since keeping into consideration that the bot will be moving and the distance has to be measured continuosly we sort the data that is been collected and stored in ldata by executing the code

```
std::sort( ldata.begin(), ldata.end(), [](RoboCompLaser::TData a, RoboCompLaser::TData b){ return     a.dist < b.dist; }) ;
```

This code would sort and store the code in the first position or ldata.front(). So now to see the obstacle distance all we have to do is use std::cout in other words

	std::cout << ldata.front().dist << std::endl;

This would display the distance in the command window.

##Example

Proceed along if you have already built the component which imports interfaces laser and differentialRobot. 
Open specificworker.cpp and now try implementing laser.idsl. The bot should move in random and display the distance as such on the command window.

The code for this is 

```
void SpecificWorker::compute( )
{


    try
    {
        RoboCompLaser::TLaserData ldata = laser_proxy->getLaserData();
        std::sort( ldata.begin(), ldata.end(), [](RoboCompLaser::TData a, RoboCompLaser::TData b){ return     a.dist < b.dist; }) ;
        
	
	differentialrobot_proxy->setSpeedBase(500, 0); 
  	usleep(1500000);
	std::cout << ldata.front().dist << std::endl;
  	differentialrobot_proxy->setSpeedBase(10, 1.5707);  
  	usleep(1000000);
	std::cout << ldata.front().dist << std::endl;
       	
    }
    catch(const Ice::Exception &ex)
    {
        std::cout << ex << std::endl;
    }

}
```
Here the bot moves in random and you will see the results on the command window. The code for this component `Tutorial-3/thirdcomp` can be found on github [here](https://github.com/rajathkumarmp/RoboComp-Tutorial)

As an exercise you can try building a obstacle avoiding bot using the same interfaces that you have learnt now.
