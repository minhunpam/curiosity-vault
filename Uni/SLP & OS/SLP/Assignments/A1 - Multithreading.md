## Todo1:
- create threads for
	- aliens
	- aesteroids
	- player
	- projectile handler

## Todo2: Implement `init_enemies`
### 1. `pthread_attr_setdetachstate(&enemy_attr, PTHREAD_CREATE_JOINABLE)`
- `PTHREAD_CREATE_DETACHED`: whichever thread created using attr with this state will be in a detached state
	- detached state means a thread can never joined
- `PTHREAD_CREATE_JOINABLE`: whichever thread created using attr with this state will be in a joinable state
	- joinable state means a thread can be joined

### 2. Why do we have to create the `enemy_params` on the heap, but not stack?
- If we declared `enemy_params` on the stack inside the loop, the `enemy_params` data on a previous stack frame will be overwritten by that from the next iteration or invalidated
- Besides, the `pthread_create`isn't necessarily executed in its iteration, but normally when the iteration advances, so it would reuse the same stack slot from the previous one, which could lead to unwanted behavior or crash
- Allocating on the heap gives each thread its own state block that stays valid until the thread explicitly frees it

## Todo3: Spawn aesteroids correctly on in the map
- takes the coordinates from the parameter
- free allocated memory of parameter on Todo2
- draw aesteroids on the map

## Todo4: Handling with aliens
- Set alien position correctly based on the received coordinates from parameter
- Free allocated memory of parameter on Todo2
- Make sure the alien dies ***IMMEDIATELY*** when the projectile hits it or game ends
	- The term "immediately" == the thread can be disappear any time = be cancelled any time
		- `pthread_setcanceltype(int type, int* oldtype)` sets the cancelability type of the callilng thread to the value given in `type`
			- `PTHREAD_CANCEL_DEFERRED` - a cancellation request is deferred until the thread next calls a function that is a cancellation point
			- `PTHREAD_CANCEL_ASYNCHRONOUS` - **can be cancelled at any time**. (Typically, it  will  be canceled  immediately  upon  receiving a cancelation request, but
              the system doesn't guarantee this.)

## Todo 5: 
- If player wants to shoot, we must go through every single slot of `active_projectiles` and check if a slot is free, then we can create an instane of `projectile` on that slot
- The position of the newly created projectile is right above the position of the player

## Todo6:
- Find out if an alien got hit by a projectile by checking if the position of alive aliens and the position of the projectile are matching
- Kill the hit alien = `pthread_cancel` its thread and because on Todo4, we already set the cancel  type of alien threads to asynchronous, it will respond the cancellation request ***IMMEDIATELY***
- The set that hit alien to be dead, clear the game map and increase the points for player

## Todo7: When an alien is dead, spawn a new one
- The replacing alien
	- has a new random position
	- inherits the spot of the dead alien
		- index in alien_positions 
		- alien_tids


## Todo8: Termination of threads
- Terminate all running threads
	- Those threads which are able to return on their own should terminte on their own
		- Asteroids
		- Projectile handler
	- Those which aren't able to will receive cancellation requests from `pthread_cancel`
- Give back the corresponding return value to matching arrays via `pthread_join()`



